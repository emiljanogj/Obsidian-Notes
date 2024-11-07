- OID Structure
![[Pasted image 20230906223756.png]]

- Port Numbers
	- SNMP Agent - UDP 161
	- SNMP Manager - UDP 162


#### Configuring SNMP Agent
```
R1(config)# snmp-server community {community-string} ro
R1(config)# snmp-server community {community-string} rw

R1(config)# snmp-server host {host-IP} version 2c {community-string}

R1(config)# snmp-server enable traps snmp linkdown linkup
R1(config)# snmp-server enable traps config
```