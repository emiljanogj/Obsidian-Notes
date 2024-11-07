 Assume we have the following network topology:

![[Pasted image 20230425002721.png]]

Assume PC1 sends an ARP request to resolve the MAC address of its DG. This request will also be forwarded to the attack which responds as. follows:

`I am 10.10.10.1, my MAC address is 3.3.3.3`.

- To prevent this, we use DHCP snooping. The switch is between DHCP server and PC, so the switch keeps track of the mapping MAC Address -> IP address.
- If invalid ARP traffic passes through(invalid mapping MAC->IP) the switch just drops the traffic.
- We configure DHCP spoofing as follows:
```
int {interface-name}
ip arp inspection trust (+)
ip arp inspection vlan 10
```

(+) This command is used on devices that do not get their IP from a DHCP server(routers, firewalls, switches), since the switch doesn't have a mapping IP -> MAC in that case.