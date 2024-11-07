#VLAN
***
#### Broadcast Domains

![[Pasted image 20230826170228.png]]

Consider the above topology:
- If PC1 sends a broadcst packet to SW1, SW1 will forward it to the router, which will then drop it since routers don't forward traffic => PC1, PC2, SW1 and R1 form a broadcast domain. Same for PC3 + PC4 + PC5 + SW2 + R1 and PC6 + PC7 + PC8 + SW3 + R2.
- Furthermore, if R1 sends a broadcast packet to R2, this traffic will only be received by R2 => R1 + R2 also form a broadcast domain.

#### No VLAN case
![[Pasted image 20230826172205.png]]

Assume we have the following network topology where no VLAN is configured.

- PC1 tries to send traffic to PC2 as follows:
![[Pasted image 20230826172415.png]]

- Since PC2 is in another subnet => PC1 will send it to its default gateway(R1), which will send it to PC2 as follows:
![[Pasted image 20230826172723.png]]

- On the other hand, if PC1 sends a broadcast traffic the switch will forward it to all its interfaces as follows:
![[Pasted image 20230826174000.png]]

The above scenario is inefficient, because it is not necessary to send the broadcast traffic to all the interfaces. To avoid this scenario, we use VLANs as follows in the next section.

#### With VLAN configuration
- For unicast traffic, it will be the same scenario as above.
- For multicast traffic, the following scenario will happen:
![[Pasted image 20230826174432.png]]

*Note*: The Switch does not perform inter-VLAN routing, so it still needs to send the traffic via the router even if PC1 and PC2 were in the same subnet, since they are in different VLANs:
![[Pasted image 20230826174618.png]]

 
#### Trunk VLANs

- The industry standard today is the 802.1Q, which appears in the Ethernet header as follows:
![[Pasted image 20230826180637.png]]
- The tag is 4 bytes in length and consists of 2 main fields
	- Tag Protocol Identifier(TPID)
	- Tag Control Information(TCI)
 ![[Pasted image 20230826180754.png]]

The components of the 802.1Q are as follows:
- TPID - 2 bytes in length. Always set to 0x8100 to indicate that the frame is 802.1Q-tagged
- PCP - 3 bits in length. Used for Class of Service(CoS), which prioritizes important traffic in congested networks
- DEI - 1 bit. Indicates whether the frame can be dropped in congested networs
- VID - 12 bits => Can take values from 0 - 4095(0 and 4095 are reserved). Used to indicate the VLAN number the frame belongs to.



#### Router on a Stick(ROAS)
![[Pasted image 20230826182452.png]]

- Used to route between multiple VLANs using a single interface on the Switch and Router
- The switch interface is configured as a regular trunk
- The router interface is configured using subinterfaces. Each subinterface is configured with the VLAN tag and IP address
- The router will behave as if frames arriving with a certain VLAN tag have arrived on the subinterface  configured with that VLAN tag
- The router will send the frames on each subinterface with the VLAN tag configured on the subinterface

Assume a PC in VLAN 10 on the right wants to send a packet to one of the PCs in VLAN 30. Then the following takes place:

![[Pasted image 20230826183254.png]]

2 methods of configuring the native VLAN on a router:
- Using the following command in the router's subinterface
```
encapsulation dot1q {vlan-id} native
```
- Configuring the IP address for the native VLAN on the router's physical interface


#### L3 Switches
- Supports all the features of routers including inter-VLAN routing via Switch Virtual Interfaces(SVIs)

**Inter-VLAN Routing via SVI**

- **SVIs** are the virtual interfaces we can assign IP addresses to in a multilayer switch
- Configure each PC to use the SVI as their gateway address
- To send traffic to different subnets/VLANs, the PCs will send traffic to the switch, and the switch will route the traffic

![[Pasted image 20230827132737.png]]



