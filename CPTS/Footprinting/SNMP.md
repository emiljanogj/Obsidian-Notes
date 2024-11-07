#Footprinting 

---

- To bruteforce the community string for an SNMP server we use the following command:
```bash
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt {IP}
```

- After finding the community string, we can then do an **snmpwalk** as follows:
```
snmpwalk -c {community-string} {IP} -v{1|2c|3}
```


