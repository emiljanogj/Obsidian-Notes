Assume we have the following network topology:

![[Pasted image 20230420001414.png]]

Assume in the above scenario that half the PCs use R1(`10.10.10.1`) as their default gateway and half use R2(`10.10.10.2`). In case one of them goes down, we need to change the default gateway in half of the PCs and this is not optimal. To avoid this scenario, we use a VIP as the default gateway. the Virtual IP is negotiated between R1 <-> R2.

First Hop Redundancy Protocols include:
- HSRP: Deployed in active-standby mode => R1 is active and R2 is in standby mode or vice versa. Hence, all the traffic will be routed via R1, but when R1 fails it will instead be routed via R2.
- VRRP: Similar to HSRP but open standard while HSRP is Cisco proprietary
- GLBP: Supports active-active load balancing across multiple routers