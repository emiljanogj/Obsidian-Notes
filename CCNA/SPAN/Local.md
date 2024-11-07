#SPAN
***
- **Local SPAN** captures traffic on a Switch and sends a copy to a local port connected to a traffic analyzer.
- The packet capture source can be:
	- One or more distinct switch ports
	- VLAN
	- Port Channel or EtherChannel
- Multiple VLANs/interfaces can be specified by using comma-separated values or hyphen-separated for ranges.


#### Configuration using Interface
```
SW1(config)# monitor session {session-id} source interface {interface-number} {tx|rx|both}

SW2(config)# monitor session {session-id} destination interface {interface-number} 
```


#### Configuration using VLAN
```
SW1(config)# monitor session {session-id} source interface vlan {vlan-number} {tx|rx|both}
```


