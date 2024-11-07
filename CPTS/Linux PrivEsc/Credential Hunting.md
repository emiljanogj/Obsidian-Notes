#Linux_PrivilegeEscalation 

- To find config files we use the following command:
```bash
htb_student@NIX02:~$  find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
```

