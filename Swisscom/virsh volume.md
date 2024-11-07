- Creating a volume using virsh
```
virsh vol-create-as --pool vtcl-container-zbl-101 vtcl-container-zbl-101_data2.qcow2 400G
```

- Attach disk to the VM
```
virsh attach-disk --domain vtcl-container-zbl-101.sharedtcs.net --source /var/lib/libvirt/images/vtcl-container-zbl-101/vtcl-container-zbl-101_data2.qcow2 --target vdc

virsh domblklist vtcl-container-zbl-101.sharedtcs.net --details
```

- Created logical volume
```
[root@vtcl-container-zbl-101]:~# lvdisplay --maps /dev/vdatavg/rkt_jenkins_lv 
  --- Logical volume ---
  LV Path                /dev/vdatavg/rkt_jenkins_lv
  LV Name                rkt_jenkins_lv
  VG Name                vdatavg
  LV UUID                RtDdIk-R49x-qsCM-35Aw-V8tV-6RFq-yqu1Qz
  LV Write Access        read/write
  LV Creation host, time vtcl-container-zbl-101.sharedtcs.net, 2023-08-28 15:54:28 +0200
  LV Status              available
  # open                 1
  LV Size                400.00 GiB
  Current LE             102400
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:13
   
  --- Segments ---
  Logical extents 0 to 102398:
    Type                linear
    Physical volume     /dev/vdc1
    Physical extents    0 to 102398
   
  Logical extents 102399 to 102399:
    Type                linear
    Physical volume     /dev/vdb1
    Physical extents    31616 to 31616
```
