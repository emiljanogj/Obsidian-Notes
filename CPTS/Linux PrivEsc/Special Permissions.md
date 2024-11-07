#Linux_PrivilegeEscalation 

- The `setuid` bit allows a user to execute a program or a script with the permissions of the owner of the script. To find all the files that have the `setuid` bit set we use the following command:
```Bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

- The `setgid` allows us to execute a binary as if we were a part of the group that created it. To find the binaries that have the `setgid` bit set we use the following command:
```Bash
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

