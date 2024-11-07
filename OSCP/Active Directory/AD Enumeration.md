- To authenticate using a specified domain we use the following command:
```Bash
ssh za.tryhackme.com\\<AD Username>@thmjmp1.za.tryhackme.com
```
where `za.tryhackme.com` is the AD domain and `thmjmp1.za.tryhackme.com` is the host we're login in.

**Note**
- When we use the following domain:
```
dir \\za.tryhackme.com\SYSVOL
```
=> we're using **Kerberos**
- and with the following:
```
dir \\<DC IP>\SYSVOL
```
=> we're using **NTLM**. **NTLM** is easiest to bypass detection

