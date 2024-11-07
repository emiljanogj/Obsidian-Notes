- To print users that belong a given group we use the following command:
```
getent group {grpname} | cut -d: -f4
```
- We use the following command:
```
chmod 2770 /home/techdocs
```
The option `2` at the beginning is the `setgid(sgid)` option, which means that all the files created in that directory will inherit the group ownership of the directory rather than the primary group of the user who created the file
