#Linux_PrivilegeEscalation 

- To find all the write directories, we use the following command:
```
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
```


### Cheatsheet

![[Linux_Privilege_Escalation_Module_Cheat_Sheet.pdf]]