- To create a primary partition(i.e in `/dev/vdb') and label it as LVM we use the following command:
```
1. parted /dev/vdb mklabel {gpt|msdos} -> sets the partitioning scheme(GPT or MBR)

2. parted /dev/vdb mkpart primary 512MB 1024MB


3. parted /dev/vdb2 set 2 lvm on
```
- To initialize the partition as physical volume we then use the following command:
```
pvcreate /dev/vdb2
```
- To extend a volume group with a PV we use the following command:
```
vgextend {vg-name} {pv-name}
```

- To extend a LV we use the following command:
```
lvextend -L {size} {lv-name}
```
- To extend the FS mounted on this LV we use the following command:
```
xfs_growfs {directory}
```
- To create a FS from an existing LV we use the following command:
```
mkfs -t xfs /dev/{vg-name}/{lv-name}
```
