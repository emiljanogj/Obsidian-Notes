- The `-X` option is used to set permissions symbolically. It allows us to set the execute(search) permissions on directories so that their contents can be accessed without changing file permissions.
- Assume we have the following command:
```
[root@host opt]# chmod -R g+rwX demodir
```
*Note*: It sets the execute permission on the `demodir` only it's a directory or if it's a file and it has already execute permissions for some user.

### Special Permissions

![[Pasted image 20231224000832.png]]

- To print the `umask` value, we use the following command:
```
[user@host ~]$ umask 
0022
```
*Note*: In the above example, the `umask` clears all the group and write permissions of newly created files.

- The umask applies both to files and directories
- The umask can be changed in `/etc/profile.d/local-umask.sh`. An example is given below:
```
# Overrides default umask configuration asda sda  
if [ $UID -gt 199 ] && [ "`id -gn`" = "`id -un`" ]; then
    umask 007
else
	umask 022 fi
```
*Note*: The above change only applies to interactive non-login shells. To change the umask for login shells we do the modification in `/etc/login.defs`
