- Example Usage(no proxy)
```
zabbix_sender -vv -z 127.0.0.1 --host vtcl-bacula-qa-1.sharedtcs.net -k "tci.zabbix.autoregistration.messages" -o "Average: test3"
```

- Example Usage(with proxy)
```
zabbix_sender -vv -z 127.0.0.1 --host vtcl-zbxprxmgmt-qa-1.sharedtcs.net  -k "tci.zabbix.autoregistration.messages" -o "Average: With Proxy"
```

