#Cisco-Security 
***
- Unused services should be disabled
```
R1(config)#no ip http server
R1(config)#no cdp run
```

#### Time Synchronisation

- All servers and infrastructure devices in your network should be synchronised to the same time
- This helps in logging, but is required by several security features such as Kerberos authentication and digital certificates
- An NTP server should be used to ensure all devices have the same time
- A Cisco Router can be used both as an NTP server/client
![[Pasted image 20230728164814.png]]


