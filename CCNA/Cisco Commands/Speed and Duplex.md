To set to full duplex and 100 Mbps we use the following commands:
```
interface FastEthernet 0/1
duplex full
speed 100
```

#### Verification Commands(in switch SW1)

```
show running-config
show ip interface brief
show run interface vlan 1
show interface vlan 1
show version
```

**Duplex Full** - The interface can simultaneously transmit and receive data, allowing for bidirectional communication.
**Duplex Half** - The interface can either transmit or receive data at a given time but not both. Usually used in older or lower speed Ethernet connections