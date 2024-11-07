#Linux_PrivilegeEscalation 

---

- To enumerate all hidden files we use the following command:

```bash
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null
```

- To enumerate hidden directories
```bash
find / -type d -name ".*" -exec ls - l {} \; 2>/dev/null
```