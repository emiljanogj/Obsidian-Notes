#Linux_PrivilegeEscalation 

- If we're in a shell that only allows to execute the `ls` command, we can bypass it as follows:
```Bash
ls -l `pwd`

```

- If for example only the `echo` command is allowed, we can read a file's content using the following command:
```Bash
echo "$(<{filename.txt})"
```

