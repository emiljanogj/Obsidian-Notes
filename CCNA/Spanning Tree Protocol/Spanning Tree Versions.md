- **802.1D STP** - The original protocol. Very low convergence + 1 ST for all the VLANS => no support for load balancing.
- **802.1w Rappid Spanning Tree Protocol(RSTP)** - Much faster convergence time, but still 1 ST for all the VLANs => no support for load balancing
- **802.1s Multiple Spanning Tree Protocol(MSTP)** - Enables grouping and mapping VLANs to different spanning tree instances for load balancing


#### MSTP Load Balancing Example

Assume we have the following loop in our network topology:
![[Pasted image 20230422121812.png]]

- Acc3 has PCs attached in multiple VLANs. 
- CD1 is made the Root Bridge for VLANs 10-19 and CD2 is made the Root Bridge for VLANs 20-29 => Traffic from VLAN 10-19 is forwarded on the link to CD1 and blocked on the link to CD2 while traffic from VLANs 20-29 is forwarded on the link to CD2 and blocked on the link to CD1.

##### Cisco Proprietary Protocols

![[Pasted image 20230422122339.png]]
*Note*: With MSTP we can group multiple VLANs in the same ST instance, but RPVST+ doesn't support that.