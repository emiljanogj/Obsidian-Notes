### Add Partitions, File Systems, and Persistent Mounts

**MBR**
- The **Master Boot Record(MBR)** is the standard partitioning scheme on systems that run the BIOS firmware.
- It supports up to 4 primary partitions, but in Linux systems with extended and logical partitions we can create up to 15 partitions.
- Each partitions has a size of 32-bits => a disk can have a size of up to 2 TB.
![[Pasted image 20240117130045.png]]

**GPT**
- It is the standard in systems that run the UEFI firmware and it addresses the limitations of BIOS.
- Supports up to 64 partitions and each partitions is 64-bits => it can have a size of up to 8 ZiB.
- **GPT** is redundant => there is a backup partition at the end of the disk for each primary partition at the beginning.
- It further uses checksums to detect corrupted partitions
![[Pasted image 20240117130335.png]]
- To set the partitioning scheme to be used we use the following command:
```
parted /dev/vdx mklabel {gpt|msdos}
```
- To create a primary or extended partition we can also use the `mkpart` command.
- To list the supported file-system types we use the following command:
```
parted /dev/vdb help mkpart
```
- To specify the starting sector we need to put an s at the end, i.e. `2048s`. Otherwise the unit defaults to `MiB`. 
- To wait for the system to detect the new partition and create the corresponding device file we use the command:
```
udevadm settle
```
- The full command is as follows:
```
parted /dev/vdb mkpart userdata xfs 2048s 1000MB
```

- To delete a partition we use the command `parted /dev/dbb`, then `print` to identify the partition we want to remove and then `rm {partition-number}`. 
- After the block device is created, we then create a fs on top of it using the command `mkfs.{fs-type} {block-device}`, i.e `mkfs.xfs /dev/vdb1`
- To mount a fs persistently we need to modify the `/etc/fstab` file which has the following format:
```
UUID=7a20315d-ed8b-4e75-a5b6-24ff9e1f9838   /dbdata  xfs   defaults   0 0
```
where:
	- 1st field - UUID or device name i.e `/dev/vdb1`
	- 2nd field - mount point
	- 3rd field - fs type
	- 4th field - options to apply to the fs, usually set to defaults
	- 5th field - Used by the `dump` command to backup the device
	- 6th field  - Defines whether the `fsck` utility should run to the file-system status. It's always set to 0 for `xfs` systems, 1 for the root fs and 2 for the other fs. => It processes the **root** fs first and then the other filesystems concurrently
- An error in `/etc/fstab` makes the system unbootable => We should verify `/etc/fstab` by using the command `findmnt --verify`.
- After changing `/etc/fstab` we must run `systemctl daemon-reload`
- *Note*: UUID is recommended instead of the block device name since the name might change depending on the underlying storage layer or the order of loading of the devices, but the UUID remains the same.
- To retrieve the block UUID we use the command `lsblk --fs`
- The size of a sector is usually `512 bytes` => for a sector of 2048 we have `2048 * 512 = 1048576 = 1049 kB = 1 MB`. => To have a drive with size `1 GB` we need to specify the ending to be `1001 MB`.


### Manage Swap Space

- Swap space recommendations are as follows:
![[Pasted image 20240117181629.png]]
 *Note*: The swap space is used in hibernation mode and the whole RAM is copied there => Swap space must be at least the size of RAM.

- To create a swap space we must do the following:
	- Create a partition of type **linux-swap**
	- Place a swap signature on the device
- To apply a swap signature to a device we use the command `mkswap` and to activate the swap space we use the command `swapon`
- To activate the swap space permanently we add the following line to `/etc/fstab`
```
UUID={uuid}       swap    swap    defaults    0   0
```
*Note*: The 2nd line is usually the mount point but since the swap spaces are not accessible through the directory structure we use a simple placeholder(`swap`).
*Note*: The defaults option includes the auto mount option which activates the swap space at boot time. They don't require any `dump` or `fsck` => the last 2 fields are set to 0
- Usually the swap spaces are used in round robin mode but we can set a priority in `/etc/fstab` as shown below:
![[Pasted image 20240117191053.png]]
- To enable all the swap entries in `/etc/fstab` we use the command `swapon -a`


### Notes from the Final Lab
- I tried using the `parted` command without using the `mklabel` command first and I got an error saying **unrecognised disk label**. To avoid this we have to run the following command first(in this case for a gpart partition):
```
parted /dev/vdb mklabel gpt
```
- When we create a swap partition with the command:
```
parted /dev/vdx mkpart {partition-name} linux-swap {start} {end}
```
the partition doesn't get a UUID until we apply a swap signature with the command:
```
mkswap /dev/vdx
```
- Only then we can copy the UUID and add it to `/etc/fstab` to make the swap partition persistent
- *Note*: When using parted the fs type for swap spaces is `linux-swap`, while in `/etc/fstab` is simply swap