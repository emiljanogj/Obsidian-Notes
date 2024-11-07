There are three components that work closely together:

- Physical Volumes

To list all the physical volumes we use one of the following commands:
```
sudo pvs
sudo pvscan
sudo pvdisplay
```

To list all the block devices that can be used to create physical volumes we use the following command:
```
sudo lvmdiskscan
```

A block device cannot be initialised as physical  volume if it is already mounted on the machine, so we first need to unmount it and then create the physical volume using the following commands(Assuming we are using `/dev/sdc`):
```
sudo umount /dev/sdc
sudo pvcreate /dev/sdc
```

- Volume Groups

We now proceed to create a volume group from the physical volume we just created. First we list all the existing volume groups using one of the following commands:
```
sudo vgs
sudo vgdisplay
```

We then proceed to create the volume group(`vg01`) as follows:
```
sudo vgcreate vg01 /dev/sdc
```

To deactivate/activate a volume group, we use the following commands:
```
sudo vchange -a n vg01
sudo vgchange -a y vg01
```


- Logical Volumes

To list all the logical volumes, we use one of the following commands
```
sudo lvs
sudo lvscan
sudo lvdisplay
```

To create a logical volume of size 10GB in the vg01 volume group we use the following command:
```
sudo lvcreate -L 10G -n lv01 vg01
```
*Note*: The above command creates the logical volume `/dev/vg01/lv01` .

### Filesystems

After creating a logical volume, we need to mount a filesystem on top of the logical volume. We will assume we have created the logical volume in the previous steps(`/dev/vg01/lv01`). 
```
sudo mkfs.ext4 /dev/vg01/lv01
```

After creating the filesystem, we mount it in a directory to access it
```
sudo mkdir /a/b
sudo mount /dev/vg01/lv01 /a/b 
```

To mount the filesystem automatically on reboot, we add the entry for the filesystem in the `/etc/fstab` file as follows:
```
/dev/vg01/lv01 /a/b ext4 defaults 0 0
```

After creating and mounting the filesystem, we use one of the following commands to display the filesystem:
```
sudo lsblk | grep lv01

sudo df -h | grep lv01

sudo fdisk -l | grep lv01
```


![[Pasted image 20230622171339.png]]

**Note**: Filesystems cannot be shrunk. In order to do this, we backup the fs and mount them to a new logical volume of different size.

- To list the storage partition table, we use the following command:
```
df -h
```

- To list all the physical volumes we use the following command:
```
pvdisplay
```