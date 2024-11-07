#SQL_Injection 

---

- To use comments for SQL injection we use the following syntax:
```sql
admin'-- 
```

which yields the following query:

![[Pasted image 20241028212508.png]]

- If the syntax uses a parenthesis, we have to close it or otherwise we get a syntax error:
```sql
admin')-- 
```

which yields the following query:

![[Pasted image 20241028212617.png]]

**Note**: There's a space after `-- `
