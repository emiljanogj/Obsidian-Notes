#Footprinting 

---

## Useful Commands

![[Pasted image 20241019190342.png]]

## Important Ports

- Ports `111` and `2049`

## Nmap Scan

- To do an extensive **nmap** scan we use the following command:
```bash
nmap -sC -sV -Pn {IP} -p111,2049 --script nfs* {IP}
```

- To show the exports of a given server we use the following
```bash
showmount -e {IP}
```

- To mount an external NFS share in a local file we use the following command:
```bash
mount -t nfs {IP}:{folder} {local-folder} -o nolock
```
