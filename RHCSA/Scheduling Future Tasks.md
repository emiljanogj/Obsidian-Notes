- To manage scheduled jobs, we use the `crontab` command as follows:
```
crontab -l -> lists the jobs for the current user
crontab -r -> removes the jobs for the current user
crontab -e -> edits the jobs for the current user
crontab filename -> replaces the jobs with the jobs from this filename
```

- The fields in the crontab file appear in the following order:
	- Minutes
	- Hours
	- Day of the month
	- Month
	- Day of the week
	- Command

It executes the command at the given day of the month or day of the week.
Example:
```
15 12 11 * Fri command
```

The above crontab executes `command` every 11th day of the month and every Friday at 12:15.

#### Schedule Recurring System Jobs

- We can also schedule a recurrent service run by setting the Timer field in the `service.timer` file  as follows:
```
[Timer]
OnCalendar=*:00/10
```

#### Manage Temporary Files

- The `systemd-tmpfiles-clean` service configuration files are located in 3 locations:
	- `/etc/tmpfiles.d/*.conf`
	- `/run/tmpfiles.d/*.conf`
	- `/usr/lib/tmpfiles.d/*.conf`

The above files are listed in the priority order.

Assume we have the following line in `/etc/tmpfiles.d/tmp.conf`

```
d /run/momentary 0700 root root 30s
```

This line ensures that the /run/momentary directory exists with root user and group permissions and it purges all the files that are unused within the last 30s.

- The directories for the jobs that run daily/weekly/monthly are defined in `/etc/anacrontab`. If we do `cat /etc/anacrontab`, we get the following:
![[Pasted image 20230622232726.png]]
