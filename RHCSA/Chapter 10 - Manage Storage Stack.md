
1- To partition the disk we use the command from the previous chapter:
```
parted /dev/vdb mklabel gpt mkpart primary {start} {end}
```
- To set the lvm flag to the partition we use the following command:
```
parted /dev/vdb set 1 lvm on
```

- To wait for the new partition to be recognized we use the command
```
udevadm settle
```

- To label 2 partitions as PVs that are ready to create VGs we use the following command:
```
pvcreate /dev/vdb1 /dev/vdb2
```

- To create a VG from the physical volumes we use the command:
```
vgcreate {vg-name} /dev/vdb1 /dev/vdb2
``` 

- To create an LV from the available PEs in a VG we use the following command:
```
lvcreate -n lv01 -L 300M vg01 
```

### Create a Logical Volume with Deduplication and Compression

- **VDO** - provides in-line block-level deduplication, compression and thin storage provisioning. 
- It has 2 components:
	- **VDO pool LV** - stores, deduplicates and compresses data and sets the size of the VDO volume backed by the physical volume.
	- **VDO LV** - Provisioned on top of the **VDO pool LV**  and sets the logical size of the VDO volume before deduplication and compression
- To create a VDO LV we use the following command:
```
lvcreate --type vdo --name {vdo-name} --size {size} {volume-group}
```

- To extend a volume group with a PV(i.e `/dev/vdb3`) we use the following command:
```
vgextend vg01 /dev/vdb3
```

- To extend a LV we use the following command:
```
lvextend -L +500M /dev/vg01/lv01
```
- After extending the logical volume as shown above, we need to resize the filesystem mounted on the LV also. To extend an XFS FS we use the following command:
```
xfs_growfs {filesystem}
```
- To extend an EXT4 filesystem we use the following command:
```
resize2fs {lv-name}
```

*Note*: The argument to `xfs_growfs` is the mountpoint, while  `resize2fs` takes the LV name. We can resize EXT4 fs both up and down, while XFS can only be extended up.



### Extending Swap Space

- To extend swap space LV, we need to turn the swap memory off using the following command:
```
swapoff -v /dev/vg01/swap
```
- We then extend the LV using the following command:
```
lvextend -L +300M /dev/vg01/swap
```
- To format the LV as swap space we use the command:
```
mkswap /dev/vg01/swap
```
- To turn on the swap space we use the following command:
```
swapon /dev/vg01/swap
```


### Reduce Volume Group Storage

- Reducing a VG involves removing unused PV from the VG.
- It is done using the command `pvmove` to move the data from extents on one PV to extents on another PV within the same VG
- Using `pvmove` with the `-A` option backs up the metadata of the VG. This option uses the `vgcfgbackup` command
- To remove a PV from a VG we use the command
```
vgreduce {vg-name} {pv-name}
```
- To remove an LV we do the following:
	- Unmount the filesystem mounted on the LV with the command `umount {filesystem`}
	- Remove the LV with the command `lvremove {LV-name}`

### Storage Stack

![[Pasted image 20240121100313.png]]

**Stratis Storage Management**

- Uses a concept called **thin provisioning**
	- **thin provisioning** - Instead of immediately allocation physical storage to the filesystem when you create it, it dynamically allocates storage from a storage pool as we store more data in it.
![[Pasted image 20240121104917.png]]

- We use stratis with the following commands:
```
dnf install stratis-cli stratisd

systemctl enable --now stratisd
```

- To create a pool we use the following command:
```
[root@host ~]# stratis pool create pool1 /dev/vdb  
[root@host ~]# stratis pool list  
Name Total Physical Properties UUID pool1 5 GiB / 37.63 MiB / 4.96 GiB ~Ca,~Cr 11f6f3c5-5...
```

- To extend a pool with a physical volume, we use the following command:
```
stratis pool add-data {pool-name} {pv-name}
stratis blockdev list {pool-name}
```

- To create a filesystem from a pool we use the following command:
```
stratis filesystem create {pool-name} {fs-name}
stratis filesystem list
```

- To create a snapshot of the filesystem we use the following command:
```
stratis filesystem snapshot {pool-name} {fs-name} {snapshot-name}
```
*Note*: 560 MB are allocated initially to store the system's journal. To access the snapshot we simply have to mount it to a filesystem.

- To mount a Stratis filesystem persistently we do the following:
	- Find the UUID of the fs using the following command
```
lsblk --output=UUID /dev/stratis/{pool-name}/{fs-name}
```
 - Modify `/etc/fstab` as follows:
```
UUID={UUID} {mountpoint}    xfs   defaults,x-systemd.requires=stratisd.servce  0  0      
```

- To copy 2GB of random data to a file we use the following command:
```
dd if=/dev/urandom of={output-file} bs=1M count=2048
```
*Note*: `bs` specifies the block size(`1M`), so in this case we have `2048` blocks of size `1M` which equals to `2GB`.


### Final Lab

- To specify a certain unit(default is `MB`) with `parted` we use the following command:
```
parted /dev/vdb unit MiB print
```
- After creating the partition with the following command:
```
parted /dev/vdb mkpart primary 514MiB 1026MiB
```
- and setting the label to `lvm` with the following command:
```
parted /dev/vdb set 2 lvm on
```
- we need to initialize it as a PV with the following command:
```
pvcreate /dev/vdb2
```

*Note*: When extending a logical volume, don't forget to resize the filesystem mounted on top of it also with the following command:
```
xfs_growfs {mountpoint}
```
