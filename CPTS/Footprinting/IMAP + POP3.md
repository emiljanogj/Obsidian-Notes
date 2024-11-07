#Footprinting 

---

**Important IMAP Commands**
![[Pasted image 20241022225135.png]]

**Important POP3 Commands**
![[Pasted image 20241022225202.png]]

- To perform an **Nmap** on an **IMAP/POP3** server we use the following command
```bash
sudo nmap {IP} -sV -p110,143,993,995 -sC
```

- To then connect to **IMAP** server via **curl** we use the following command:
```bash
curl -k 'imaps://{IP}' --user {username}:{password}
```

- We can also connect to **IMAP** server with **openssl** using the following command:
```bash
openssl s_client -connect {IP}:imaps
```

- To connect to **POP3** server with **openssl** we use the following command:
```bash
openssl s_client -connect {IP}:pop3s
```


## After Connect to IMAPs

- After connecting to an **IMAP** server we use the following command to login:
```bash
A1 LOGIN {username} {password}
```

- To list all the folders we use the following command:
```bash
a list "" *
```

- To then select a specific folder we use the following command:
```bash
a select {FOLDER_NAME}
```

- To fetch the an email we use the following command:
```
a fetch <id> all
```
which yields something like the following:
```bash
a fetch 1 all
* 1 FETCH (FLAGS (\Seen) INTERNALDATE "08-Nov-2021 23:51:24 +0000" RFC822.SIZE 167 ENVELOPE ("Wed, 03 Nov 2021 16:13:27 +0200" "Flag" (("CTO" NIL "devadmin" "inlanefreight.htb")) (("CTO" NIL "devadmin" "inlanefreight.htb")) (("CTO" NIL "devadmin" "inlanefreight.htb")) (("Robin" NIL "robin" "inlanefreight.htb")) NIL NIL NIL NIL))
a OK Fetch completed (0.001 + 0.000 secs).

```

- And to fetch the body, we use the following command:
```bash
a fetch <id> body[]
```
