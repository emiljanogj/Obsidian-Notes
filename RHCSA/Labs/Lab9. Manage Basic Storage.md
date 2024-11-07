- To create a partition we use the following command:
```
1. parted /dev/vdb mklabel gpt

2. parted /dev/vdx mkpart {partition-name} {partition-type} {start-sector} {size}
```

- For example:
```
1. parted /dev/vdb mklabel gpt



2. parted /dev/vdb mkpart backup xfs 2048s 2GB

```

- To format the newly created partition and get its ID we use the following command
```
1. mkfs.xfs /dev/vdb1


2. lsblk --fs /dev/vdb1
```


**Swap Partitions**
- To initialise a swap partition we use the following command:
```
parted /dev/vdX mkpart {partition-name} linux-swap {start} {end}
```
- To format the partition and set it on we do the following:
```
1. mkswap /dev/vdX


2. swapon -a

3. swapon --show
```

	