#Linux 

***
**Timeout**
sudo credentials are cached for a certain amount of time as determined by `timestamp_timeout` variable.

**Sessions**
sudo will honor TTY segregation by default => if we run sudo in one terminal and try again in a separate one, we would have to provide the password again. The `tty_tickets` can be used to persist variables between multiple sessions, i.e. it can be used to exploit sudo caching. The `tty_tickets` variable should be left enabled to ensure that sudo cache records are segregated on a per-session basis.




