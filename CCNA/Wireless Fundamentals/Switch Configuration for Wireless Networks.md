#Wireless 
***
- Assume we have 2 WLANs configured on an Autonomous AP, where the WLAN with SSID Guest is mapped to VLAN 22 in the Switch and the WLAN with SSID Corporate is mapped to VLAN 21 as follows:
![[Pasted image 20230805135304.png]]

#### Autonomous AP Configuration
***

```
Switch(config)# vlan 21
Switch(config-vlan)# name Corporate
Switch(config)# vlan 22
Switch(config-vlan)# name Guest
Switch(config)# interface GigabitEthernet1/0/1
Switch(config-if)# switchport trunk encap dot1q
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 21,22
```

#### Lightweight AP Configuration
***
In this case we need to add 2 extra VLANs to the Switch: 1 for the admin to manage the `WLC`, and 1 for the `WLC` to manage the `APs`. However, these can be one and the same.
![[Pasted image 20230805140031.png]]

```
Switch(config)# vlan 21
Switch(config-vlan)# name Corporate
Switch(config)# vlan 22
Switch(config-vlan)# name Guest

Switch(config)# vlan 10
Switch(config-vlan)# name Management
Switch(config)# vlan 11
Switch(config-vlan)# name AP-Management

Switch(config)# interface GigabitEthernet1/0/2
Switch(config-if)# switchport trunk encap dot1q
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,11,21,22

Switch(config)# interface GigabitEthernet1/0/1
Switch(config-if)# switchport mode access
Switch(config-if) switchport access vlan 11
```

*Note*: The packets in this case are tagged by the `WLC`. The `AP` doesn't tag any of the packets but always sends them to via the Management VLAN(`vlan 11`), as we first need to login to the `WLC` first. 