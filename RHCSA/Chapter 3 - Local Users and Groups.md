- Printing local user information
```
[user01@host ~]$ cat /etc/passwd
user01:x:1000:1000:User One:/home/user01:/bin/bash
```
where:
- **user01** - username for the user
- **x** - just a placeholder that used to hold the encrypted password of the user
- **1000** - uid
- **1000** - gid
- **User One** - description
- **/home/user01** - home directory
- **/bin/bash** - login shell

- Printing local group information
```
[user01@host ~]$ cat /etc/group
...output omitted... 
group01:x:10000:user01,user02,user03
```
where:
- **group01** - group name
- **1000** - gid
- **user01...** - A list of users that are members of this group as a supplementary group


### Gain Superuser Access
`su` starts a *non-login* shell, i.e. it keeps the original user's env settings while `su -` starts a *login shell*, i.e as if it is a new login.
![[Pasted image 20231217143754.png]]

- `/etc/sudoers` is the main config file for the sudo command. It should be modified by the `visudo` command. An example of `/etc/sudoers` line is given below:
```
%wheel       ALL=(ALL:ALL)      ALL
```
- In the line above:
	- `%wheel` - specifies the name of the group
	- 1st ALL - on any host with this file
	- 2nd ALL - users in the `wheel` group can run commands as any other user
	- 3rd ALL - users in the `wheel` group can run commands as any other group
	- 4th ALL - the above rules apply to all the commands
- We can do the same as above by adding the file `/etc/sudoers.d/wheel` with the same content as above
- To enable users in the `game` group to run the `id` command as the `operator` user we use the following command:
```
%game        ALL=(operator) /bin/id
```

### Manage Local Users

- The main command here is `usermod` with the following options:
![[Pasted image 20231217152342.png]]
![[Pasted image 20231217152358.png]]


### Group Membership

- To modify the primary group membership of a user we use the following command:
```
usermod -g {group-name} {user-name}
```
- To add a user to the supplementary group we use the following command:
```
usermod -aG {group-name} {user-name}
```
- We can switch to a new group with the following command:
```
newgrp {group-name}
```
*Note*: The new group must be in the list of supplementary groups

### Manage User Passwords
- Encrypted passwords are moved to `/etc/shadow` which is only accessible by `root`
- A password in `/etc/shadow` has the following format:
```
[root@host ~]# cat /etc/shadow  
...output omitted... user03:$6$CSsXsd3rwghsdfarf:17933:0:99999:7:2:18113:
```
- The fields are as follows:
	- **user03** - name of the user
	- **$6CSsXsd3rwghsdfarf** - encrypted password of the user
	- **17933** - the days from the epoch on which the password was changed
	- **0** - min days since the last change before another change is allowed
	- **99999** - max number of days without a change until password expires
	- **7** - number of days before password expiration when to send a notification to the user
	- **2** - number of days without activity since the password expired before an account is locked
	- **18113** - The days when the password expires since the epoch
- The password format is as follows
```
$6$CSsXcYG1L/4ZfHr/$2W6evvJahUfzfHpc9X.45Jc6H30E
```
where:
	- 6 - hashing algorithm(SHA512). 5 - SHA256  1 - MD5
	- `CSsXcYG1L/4ZfHr/` - randomly generated salt
	- `2W6evvJahUfzfHpc9X.45Jc6H30E` - hashed password + salt
- The import parameters for password aging are described in the following diagram:
![[Pasted image 20231218211757.png]]
- An example of the command that uses the above parameters is:
```
chage -m 0 -M 90 -W 7 -I 14 {username}
```
- To set the expiration date of a password we use the following command:
```
chage -E $(date -d "+30 days" +%F) {username}
```
- To check that the command worked as expected we use the following command:
```
chage -l {username} | grep "Account expires"
```
- To lock an account after a certain date we use the following command
```
usermod -L -e {date} {username}
```
- To disable login shell for a user we use the following command:
```
usermod -s /sbin/nologin {username}
```
- To force a user to change their password we simply set the last change date to 0 as follows:
```
chage -d 0 {username}
```

*Note*: Important file for password management is `/etc/login.defs`


To format the date we use the following command:
`date -d "+30 days" +%F`