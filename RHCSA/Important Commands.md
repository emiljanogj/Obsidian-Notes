- Set the SELinux context for all the files in a directory
```
semanage fcontext -a -t {context} /{directory_name}(/.*)?
```
- To restore the context of all the files in a directory we use the following command
```
restorecon -Rv /{directory-name}
```
- To set a boolean we use the following command:
```
setsetbool -P {boolean} {on|off}
```
- To add a repo we use the following config file in `/etc/yum.repos.d`
```
[repo-name]
name={repo-name}
baseurl={url}
enabled=true
gpgcheck=false
gpgkey={file:///{complete-file-path}}
```
We then add the repo using the command `dnf config-manager --enable {repo-name}`

- To recover the root password during the bootloader phase we use the following steps:
```
1. Enter rd.break at the end of linux file
2. remount root fs with rw permissions mount -o remount,rw /sysroot
3. chroot /sysroot
4. passwd root
5. touch /.autorelabel
```

- To set the IP and default gateway for a given interface we use the following commands:
```
IP -> ip address add {IP/subnet-mask} dev {if-name}

GW -> ip route add {GW-IP} dev {if-name}
```

 - To check all the SELinux-related errors in audit logs we use the following commands:
```
sealert -a /var/log/audit/audit.log
```

- To set the size of a LV using extents as unit we use the following command:
```
lvcreate -n {lv-name} -l {number_of_extents} /dev/{vg-name}
```

- To sort all processes by CPU consumption we use the following command:
```
ps -aux --sort=pcpu
```
- To print info about a process we use the following command
```
ps -o pid,nice,com {PID}
```

- To compress a whole directory with gunzip we use the following command after `cd` to the destination:
```
tar -cvzf {name.tgz} {folder_to_be_compressed}
```

- To decompress we use the following command:
```
tar -xvzf {name.tgz}
```
