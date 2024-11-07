- They are configured in the switch => Switches only allow traffic within the same VLAN
- A device can only communicate with another device in the same VLAN
- The commands to configure a VLAN are as follows:
```
vlan 10
name ENg
interface range FastEthernet 0/1 - 3
switchport mode access
switchport access vlan 10
show vlan brief
```
- To print info about VLAN on a specific interface we use the following command:
```
show interface FastEthernet 0/1 switchport
```
