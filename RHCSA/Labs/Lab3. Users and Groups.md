- To force new users to change their passwords every X days we set `PASS_MAX_DAYS` in `/etc/login.defs`
- To create a user that belongs to a certain group we use the following command:
```
useradd -G {group-name} {username}
```
- To force a user to change their password on the first login we use the following command:
```
chage -d 0 {username}
```
