![[Pasted image 20230416101722.png]]

- Enables devices from different VLANs to communicate with each-other through many switches.
- Access Ports([[VLAN Access Ports]]) carry traffic for one specific VLAN while Trunk Ports carry traffic for different VLANs(used for inter-switch communication).
- When a Switch forwards traffic to another switch it tags the L2 **Dot1Q(Trunk Port Protocol)** with the correct VLAN number. This header is then removed when the last switch sends traffic to the end host.
- The forwarding Switch modifies the Ethernet frame as follows
![[Pasted image 20230416102412.png]]

- We configure a Trunk Port as follows
```
interface FastEthernet 0/24
description Trunk to SW2
switchport trunk encapsulation dot1q(+)
switchport mode trunk
switchport trunk native vlan {native-vlan-number}
show interface FastEthernet 0/24 switchport
```
*Note*: (+) is only used for older switches that support both Dot1q and ISL. We don't need to do this in the newer switches which only support Dot1q.
- If a switch is not connected to any device from a particular VLAN, we can limit the allowed VLANs on that switch so that traffic destined to that VLAN never goes through this switch. This saves bandwidth on the links bw switches. Ex: traffic destined to Sales PCs in the image below should never go through the upper switch.
![[Pasted image 20230416104149.png]]
To restric VLANs we use the following commands:
```
interface GigabitEthernet 0/1
switchport trunk allowed vlan 10,30
```

### Lab

- To check the switchport config in a particular interface we use the following command:
```
show interface {interface-number} switchport
```
