The **systemd-journald** service collects logs from the following sources:
	- System kernel
	- Errors during the boot process
	- Standard errors and outputs from daemons
	- Syslog events
- It organizes this logs into a structured format and sends them to **rsyslog**
- **rsyslog** then organizes them into persistent log files in **/var/log** or forwards them to other processes.
- Several **/var/log** directories:
	- **/var/log/messages** - most of the syslog messages are stored here except the ones that follow
	- **/var/log/secure** - stores the syslog messages about security and authentication events
	- **/var/log/cron** - stores syslog messages about scheduled cron jobs
	- **/var/log/maillog** - stores syslog messages about the mail server 
	- **/var/log/boot.log** - stores non-syslog console messages about startup
- Syslog Facilities:
![[Pasted image 20240124163445.png]]

- Syslog Priorities:
	- 0 - Emergency
	- 1 - Alert
	- 2 - Critical
	- 3 - Error
	- 4 - Warning
	- 5 - Notice
	- 6 - Informational
	- 7 - Debugging
- Config for facility and priority is done in `/etc/rsyslog.conf` or in any file under `/etc/rsyslog.d`
- For example, the following rule:
```
authpriv.*                /var/log/secure
```
will send all the messages that are sent to the *authpriv* facility with any priority to `/var/log/secure`. If we specify a priority, it will send all the logs with that priority or higher.

- To send emergency logs to all the users we use the following configuration:
```
.emerg                                         :omusrmsg:
```

- To send syslogs manually we use the following syntax:
```
logger -p {facility}.{priority} "message"
```

- We can filter `journalctl` logs based on a given timeline:
```
journalctl --since "{start_date} {start_time}" --until {end_date end_time}
```

- `journalctl` logs can also be filtered on other criteria which can be checked using the `journalctl -o verbose` command
- The common filtering criteria are:
	- **\_COMM** - the command name
	- **\_EXE** - the path to the executable file
	- **\_PID** - the PID of the process
	- **\_UID** - the UID of the user that started the process 
	- **\_SYSTEMD\_UNIT** - systemd unit that started the process

- Logs are not persistent by default, but we can make them persistent by changing the `/etc/systemd/journald.conf` file to set one of the following values:
	- **persistent** - logs persist across reboots and are stored in `/var/log/journal`. If `/var/log/journal` does not exist it will be created by `systemd-journald` service
	- **volatile** - logs are stored in `/run/log/journal` which is temporary => they are not persistent across reboots
	- **auto** - If `/var/log/journal` exists, logs are stored there and are persistent. Otherwise they are not persistent. This is the default
	- **none** - No logs are stored but they can be forwarded
- *Note*: After changing the configuration `systemd-journald` should be restarted
- If system was rebooted many times and we'd like to check the logs from a specific boot we use the command:
```
journalctl --list-boots

journalctl -b {boot-number}
```


### Maintain Accurate Time

- To get an overview of the time-related system settings we use the command `timedatectl`
- To list the timezones we use the command `timedatectl list-timezones`
- To set the timezone we use the command
```
timedatectl set-timezone {timezone}
```
- and to set the time we use the command
```
timedatectl set-time {time}
```
- To turn on/off NTP time synchronization we use the command
```
timedatectl set-ntp {true | false}
```
*Note*: The above command enables/disables the `chronyd` service

- NTP server can be configured in `/etc/chrony.conf` as follows:
```
server {DNS/IP} iburst
```
*Note*: `iburst` is an option that instructs `chronyd` service to take 4 measurements for a more accurate initial time configuration
- To check the NTP sources we use the command:
```
chronyc sources -v
```

### Final Lab

**Important Note:** Don't forget to restart the `rsyslog.service` after making a change to `/etc/rsyslog.d` config files.

- To get the logs from the last 30 minutes we use the command below:
```
journalctl --since "30 minutes ago"
```
