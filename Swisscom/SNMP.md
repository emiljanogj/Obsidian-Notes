- **SNMP Walk(v3)**
```bash
snmpwalk -v3 -l authPriv -u ro_snmp -a SHA -A 'xxx' -x AES -X 'yyy' vtcl-avictrl-bil-c1-v1.sharedtcs.net 1.3.6.1.2.1.1.5.0 -Dlcd
```

- **SNMP Walk(v2)**
```bash
snmpwalk -v 2c -c hds-public 10.167.8.67 1.3.6.1.2.1.25.4.2.1.
```

