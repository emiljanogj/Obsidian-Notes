#Linux_PrivilegeEscalation 

---  

**Example of logrotate config file**

```bash
cat /etc/logrotate.conf


# see "man logrotate" for details

# global options do not affect preceding include directives

# rotate log files weekly
weekly

# use the adm group by default, since this is the owning group
# of /var/log/syslog.
su root adm

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
#dateext

# uncomment this if you want your log files compressed
#compress

# packages drop log rotation information into this directory
include /etc/logrotate.d

# system-specific logs may also be configured here.
```


**Logrotate Exploit**
- Needs 3 conditions:
	- `logrotate` must run as privileged user or as `root`
	- we need `write` permissions on the log file directory(this is the directory where we will redirect the logs via `symlink`)
	- the `logrotate` config file should include a directive to create new files after rotating the old ones
	- `logrotate` must be in one of the following versions:
		- 3.8.6
		- 3.11.0
		- 3.15.0
		- 3.18.0

**Example of vulnerable logrotate config**:
```bash
/tmp/log/pwnme.log {
    daily
    rotate 12
    missingok
    notifempty
    size 1k
    create
}
```

