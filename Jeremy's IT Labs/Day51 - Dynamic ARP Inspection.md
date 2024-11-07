	1p[ppppp- DAI inspects the sender MAC and sender IP fields of ARP messages received on untrusted ports and checks that there is a matching entry in the DHCP snooping binding table.
	- If there is a matching entry, the ARP message is forwarded normally
	- Otherwise, ARP message is discarded
- **DAI** doesn't inspect messages received on trusted ports
-  **ARP ACLs** can be manually configured to map IP addresses/MAC address for DAI to check
	- Useful for hosts that don't use DHCP
- Supports rate-limiting to prevent attackers from overwhelming the switch with ARP messages


#### DAI Configuration
```
SW2(config)# ip arp inspection vlan {vlan-number}
SW2(config)# interface {interface}
SW2(config-if)# ip arp inspection 
SW1# show ip arp inspection interfaces
SW1(config)# interface {interface-number}
SW1(config-if)# ip arp inspection limit rate burst interval 2
```
*Note*: Rate limit limits the number of packets received on an interface, not the ones sent.

- To disable an interface that is err-disabled by ARP inspection we use the following command:
```
SW1(config)# errdisable recovery cause arp-inspection
```

- We can enable further ARP inspections with the following command:
```
SW1(config)# ip arp inspection validate {dst-mac | ip | src-mac}
```
The IP option simply checks for unexpected IP addresses like 0.0.0.0, 255.255.255.255.
![[Pasted image 20230913175309.png]]
The `src-mac` and `dst-mac` options above check the highlighted 2 fields and make sure that the destination and source MAC addresses in both fields are the same

- For statically configured MAC IP addresses that don't have an entry in the switch's DHCP snooping binding table we use **ARP ACLs**:
First we create the ACL
```
SW2(config)# arp access-list ARP-ACL-1
SW2(config-arp-nacl)# permit ip host {ip-address} mac host {mac-address}
```
Then we need to apply it to the proper VLAN as follows:
```
SW2(config)# ip arp inspection filter ARP-ACL-1 vlan 1
```


