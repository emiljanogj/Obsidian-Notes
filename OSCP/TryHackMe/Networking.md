**NMAP Service Engine:** https://nmap.org/nsedoc/

- Useful nmap command:
```
nmap -A -T4  -v {IP}
```

## SMTP

![[Pasted image 20240430233958.png]]

**JOHN**
- Assume we have the following hashed password:
```
carl:*EA031893AA21444B170FC2162A56978B8CEECE18
```
- To crack the password we use the command `john hash.txt`

### Hash
The format of a hash is as follows: `$format$rounds$salt$hash`.
