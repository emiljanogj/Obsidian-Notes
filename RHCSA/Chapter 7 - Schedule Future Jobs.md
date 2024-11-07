
![[Pasted image 20240114154258.png]]

- The format of a crontab line is as follows:
![[Pasted image 20240114154959.png]]

- If the first field is `*/5`, this means that the command will be executed every 5 minutes
- To wait until the file is created we use the following command:
```
while ! test -f {filename}; do sleep 1s; done
```
- `crontab -e` creates a cron job for a specific user while the jobs under `/etc/cron.d` are system-wide
- Cron jobs can also be defined under `/etc/anacrontab` with the following format:
```
# period in days  delay in minutes  job-identifier       command
```
*Note*:
 - For `period in days` we can also use macros such as `@daily`, `@weekly`, `@monthly`
 - `delay in minutes` - defines the delay in minutes that the `crond` daemon must wait before the job starts
 - An example is as follows:
```
@daily 1 5  cron.daily  nice run-parts /etc/cron.daily
```
*Note*: The above command runs all the jobs in the `/etc/cron.daily` directory, every day after 5 minutes.
- The `run-parts` command is a script that runs all the executable files under a given directory and `nice` runs a job with the default niceness value(10)
- System unit files defined under `/etc/systemd/system` take precedence over the system unit files under `/usr/lib/systemd/system`
- After changing a timer unit, the `systemd` daemon needs to be reloaded with the following command:
```
systemctl daemon-reload
```
- To active the timer unit, we use the following command:
```
systemctl enable --now {unitname}.timer
```
- `/etc/anacrontab` ensures that jobs are not skipped because the system is hybernating or powered off.
### Manage Temporary Files

- At boot time, the `systemd-tmpfiles-setup` service is launched.
- It calls the `systemd-tmpfiles --create` command which reads the following config files in the order of priority:
	- `/etc/tmpfiles.d/*.conf`
	- `/run/tmpfiles.d/*.conf`
	- `/usr/lib/tmpfiles.d/*.conf`
- To prevent the system from overflowing from a large number of temporary files, there is a `systemd-tmpfiles-clean.timer` unit which has the following format:
```
[user@host ~]$ systemctl cat systemd-tmpfiles-clean.timer # /usr/lib/systemd/system/systemd-tmpfiles-clean.timer  
# SPDX-License-Identifier: LGPL-2.1-or-later  
#

#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
[Unit]
Description=Daily Cleanup of Temporary Directories
Documentation=man:tmpfiles.d(5) man:systemd-tmpfiles(8)
ConditionPathExists=!/etc/initrd-release

[Timer]

OnBootSec=15min
OnUnitActiveSec=1d
```
where:
	- **OnBootSec** - runs the unit 15 min after the system is booted
	- **OnUnitActiveSec** - runs the unit 1 day after the last run
- This timer unit calls the `systemd-tmpfiles-clean.service`
 - As usual, after updating the above unit we need to reload the systemd daemon as follows:
```
systemctl daemon-reload
```
- `systemd-tmpfiles --clean` reads the same config files and purges the files that were not accessed/modified/created lately(earlier than a defined max age)
- For example the following command creates a directory(`/run/systemd/seats`) if it doesn't already exist
```
d /run/systemd/seats 0755 root root -e
```
- An example of a command in `/usr/lib/tmpfiles.d/tmp.conf` is given below:
```
q /tmp 1777 root root 10d
```
*Note*: The 1 stands for the sticky bit, which prevents the users from deleting other users' files in /tmp

The command above creates the directory(`/q` stands for queue) with `rwxrwxrwx` permissions and sets its owner to root and the max age to 10 days.
*Note*: The 1 in front of `777` is the sticky bit which specifies that only the owner(root in this case) can rename or delete files inside `/tmp`.
- If we create a file inside `/etc/tmpfiles.d`, i.e. `/etc/tmpfiles.d/momentary.conf` and we want to test it we can use the command:
```
systemd-tmpfiles --create /etc/tmpfiles.d/momentary.conf
```


![[Pasted image 20240427215145.png]]

### DIfference between Anacrontab and Crontab

- Crontab schedules precise jobs, and if a system is down the job will not be executed
- Anacrontab schedules less precise jobs, but they are scheduled even if a system is down
- Anacrontab configuration is defined as follows:
```
7   15  myjob  /path/to/my/script.sh
```

- 7 - period in days, i.e the task should be executed once in every 7 days
- 15 - The job will wait for 15 minute after system startup before it's run
- myjob - an identifier for the job to be run
- /path/to/my/script.sh - script to be executed


### Timers
- Similar to cronjobs but more persistent because they are executed even if the system is sleeping at the time they are scheduled.
- The timer and its corresponding service are both defined under **/etc/systemd/system/{name}.{service|timer}**

**Example**

- **Service Configuration(/etc/systemd/system/hello-world.service)**
```
[Unit]
Description="Hello World script"

[Service]
ExecStart=/usr/local/bin/helloworld.sh
```
- **Timer Configuration(/etc/systemd/system/hello-world.timer)**
```
[Unit]
Description="Run helloworld.service 5min after boot and every 24 hours relative to activation time"

[Timer]
OnBootSec=5min
OnUnitActiveSec=24h
OnCalendar=Mon..Fri *-*-* 10:00:*
Unit=helloworld.service

[Install]
WantedBy=multi-user.target
```
- Timers can be 2 types:
	- **real-world**: Execute at a given time. Specified above with the **OnCalender** field
	- **monotonic**: Execute based on specific events, i.e. after system boot(**OnBootSec**) or after the last execution(**OnUnitActiveSec**)
- To manage timers we use the following commands:
```
systemctl start {timer}.timer

systemct restart {timer}.timer

systemctl stop  {timer}.timer
```
- To list timers we use the following commands:
```
systemctl list-timers

systemctl list-timers --state={STATE}
```
