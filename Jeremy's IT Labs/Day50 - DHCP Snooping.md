#DHCP 
***
- Filters DHCP traffic on untrusted ports
- All ports are untrusted by default
	- **uplink** ports are configured as trusted ports and **downlink** ports remain unstrusted
![[Pasted image 20230912152601.png]]

-  **DHCP Server** messages received on an untrusted port are always rejected
- **DHCP Client** messages are further inspected

- Messages sent by DHCP Servers:
	- OFFER
	- ACK
	- NAK = Opposite of ACK, used to decline a client's REQUEST

- Messages sent by DHCP Clients:
	- DISCOVER
	- REQUEST
	- RELEASE = Used to tell the server that the client no longer needs its IP address
	- DECLINE = Used to decline the IP address offered by a DHCP server

#### DHCP Snooping Configuration

```
SW2(config)# ip dhcp snooping
SW2(config)# ip dhcp snooping vlan {vlan-number} -> Need to enable DHCP snooping for all the vlans
SW2(config)# interface g0/0
Sw2(config-if)# ip dhcp snooping trust -> By default DHCP is untrusted
```

- To limit the DHCP rate we use the following config
```
SW1(config-if)# ip dhcp snooping limit rate 1
```

- To recover from DHCP rate limit error we use the following command
```
SW1(config)# errdisable recovery cause dhcp-rate-limit
```
