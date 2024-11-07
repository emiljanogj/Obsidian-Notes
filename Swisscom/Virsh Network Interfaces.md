- Attach network interface:
```
 virsh attach-interface --domain vtcl-ftps-zbl-101.sharedtcs.net --type bridge --source br50 --model virtio
```

- List the existing network interfaces
```
virsh domiflist vtcl-container-zbl-101.sharedtcs.net
```


- List all the available logical interfaces
```
ls -l /sys/class/net
```


 virsh attach-interface --domain vtcl-ftps-zhh-101.sharedtcs.net --type bridge --source br50 --model virtio