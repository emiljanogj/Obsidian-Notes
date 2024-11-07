![[Pasted image 20230417120728.png]]

*Note*: In this case there's no race for the interface, but we need separate interfaces for each VLAN => we may run out of interfaces

#### Router Configuration
```
R1(config)# interface FastEthernet 0/1
R1(config-interface)# ip address 10.10.10.1 255.255.255.0
R1(config)# interface FastEthernet 0/2
R1(config-interface)# ip address 10.10.20.1 255.255.255.0
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.2

SW1(config)# interface FastEthernet 0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config)# interface FastEthernet 0/2
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 20
```
