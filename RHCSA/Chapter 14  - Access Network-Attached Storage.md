  -r- To mount an NFS Filesystem we use the following command:
```
mount -t nfs -o rw,sync server:/export /mountpoint
```
*Note*: This is not persistent. To make it persistent we again need to modify `/etc/fstab` as follows:
```
vim /etc/fstab  
...  
server:/export /mountpoint nfs rw 0 0
```
- To check which process is keeping a mountpoint busy we use the command `lsof /mountpoint`. If there's a process using the mountpoint, doing `umount /mountpoint` will fail.

### Mount NFS Exports with the Automounter
- **Automounter** mounts/unmounts NFS exports on demand
- Mounted+idle fs and unmounted fs both use very little resources but the advantage of unmounting fs with **automounter** is that the fs cannot be corrupted.
- When mounting a filesystem the **autofs** uses the most current configuration unlike an `/etc/fstab` that uses the initial configuration


### Autofs

- First we need to install the `autofs` library as follows:
```
dnf install autofs
```

**Indirect Mapping**
- First we need to create a master map as follows:
```
sudo vim /etc/auto.master.d/demo.autofs
```
- In this master map we include the following line:
```
/shares  /etc/auto.demo
```
- The file `/auto.demo` itself has the automount properties as follows:
```
[user@host ~]$ sudo vim /etc/auto.demo

work  -rw,sync  serverb:/shares/work
```
*Note*: The **sync** option indicates that the server is synchronized immediately during write operations.
*Note*: In this case the mount point is derived from `/demo.autofs` + `/etc/auto.demo` => the mountpoint is `/shares/work`. This has the same name as the server's directory but this doesn't have to be the case
- If serverb exports 2 or more directories we use the following:
```
*   -rw,sync  serverb:/shares/&
```
=> When a user attempts to access `/shares/work` the * replaces & and `/serverv:/shares/work` is mounted
- 

**Direct Mapping**
- To use direct mapping, we don't specify anything in the master map(in `/etc/auto.maser.d/demo.autofs`):
```
/-        /etc/auto.demo
```
- and then in `/etc/auto.demo` we use the full path as follows:
```
/mnt/docs   -rw,sync   serverb:/shares/docs
```

- After changing the configs we need to restart the `autofs` service as follows:
```
systemctl enable --now autofs
```
