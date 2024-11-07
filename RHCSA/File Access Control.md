
- To set the ownership of a file, we use the following command:
```
chown {user}:{group} {filename}
```

#### Special Permissions

- **u+s(suid)** - the file executes as the user that owns the file rather than the user that ran the file
- g+s(sgid) - Files in this directory inherit the group owner from the directory
- o+t(sticky) - users with write access to this directory can only modify the files they owne

![[Pasted image 20230504111258.png]]
