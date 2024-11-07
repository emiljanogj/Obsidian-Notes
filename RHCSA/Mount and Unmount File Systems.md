
- The following command lists the devices, their UUID, the mount points and the partition's file-system type:
```
lsblk -fp
```

- To list all the processes that are using a certain mount point, we use the following command:
```
lsof {mount-path}
```

- To mount a partition to a mountpoint, we use the following command:
```
lsblk -fp -> list all partitions + their UUID
mount UUID="{UUID of our partitions}" {mountpoint}
```

- The Master Boot Record(MBR) partitioning scheme is the standard on systems that run the BIOS firmware. It permits up to 4 primary partitions. In Linux systems, with extended and logical partitions, we can have up to 15 partitions of up to 2 TB. 
![[Pasted image 20230609201058.png]]

- The above limitations of MBR are fixed by the GPT partition scheme, which allows up to 128 partitions up to size of 8 ZiB.
![[Pasted image 20230609201234.png]]

### Managing Partitions

- To display the partition table on a specific disk(/dev/vda in this example), we use the following command:
```
parted /dev/vda print
```

- To partition a disk(using either MBR or GPT), we use the following commands:
```
parted /dev/vda mklabel msdos -> MBR
parted /dev/vda mklabel gpt -> GPT
```

- After the partition is created, we need to apply a filesystem to it as follows:
```
mkfs.{xfs,ext4} {partition}
```

- A filesystem mount is not persistent across system reboots. To create persistent fs mounts, we modify the `/etc/fstab` file as shown below:
```
[root@host ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Thu Apr 5 12:05:19 2022
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=a8063676-44dd-409a-b584-68be2c9f5570   /        xfs   defaults   0 0
UUID=7a20315d-ed8b-4e75-a5b6-24ff9e1f9838   /dbdata  xfs   defaults   0 0
```

where:
- defaults - specifies a list of default options
- 0 - is used by the dump command to back up the device
- 0 - is used to specify the order in which the fsck fs utility will run. The `fsck` utility is run at system boot to verify that the fs are clean. The `xfs` fs don't use `fsck` but for `ext4`, we specify the value 1 for the root filesystem(to be checked first) and 2 for all the others(to be checked after the root filesystem).

- After changing the `/etc/fstab` file, we need to do a `systemctl daemon-reload`. 

- After creating a new partition, we should wait for the systems to register it before proceeding as follows:
```
udevadm settle
```

