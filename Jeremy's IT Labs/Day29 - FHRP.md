#FHRP
***

### HSRP

- Cisco proprietary
- 2 versions available
- Multicast IPV4 addresses for the gratuitous ARP replies(when the active router goes down)
	- v1 - `224.0.0.2`
	- v2 - `224.0.0.102`
- Virtual MAC addresses:
	- v1 - `0000.0c07.acXX`(XX is the HSRP group number)
	- v2 - `0000.0c9f.fXXXX`(XXX is the HSRP group number)
- Can use different active routers for every subnet

### VRRP

- Very similar to **HSRP** with the following differences:
![[Pasted image 20230901180040.png]]

#### GLBP

- Cisco proprietary
- Load balances among multiple routers within a single subnet
- An **AVG(Active Virtual Gateway)** is elected + up to 4 **AVFs(Active Virtual Forwarders)** are then assigned by the **AVG**
- Each **AVF** acts as the default gateway for a portion of the hosts in the subnet
- Multicast IPv4 address = `224.0.0.102`
- Virtual MAC address = `007.b400.XXYY`(XX - GBLP number YY - AVF number)
- *Note*: **HSRP/VRRP** can load balance between different subnets while **GLBP** load balances between hosts in the same subnet.


**Gratuitous ARP**
![[Pasted image 20230929151831.png]]

- When the active router goes down in an **FHRP** scenario, the backup router sends a **Gratuitous ARP** to all the switches to update their MAC address table to the new active router. They are send without being requested(broadcasted to `FFFF.FFFF.FFFF`)


![[Pasted image 20230929153817.png]]

### IPv6
- To enable IPv6 we use the command `ipv6 unicast-routing`
- To configure an IPv6 address we use the command `ipv6 address {address}/{prefix-length}`
- If the U/L  bit of a MAC address is 0 => it is a UAA
- To enable IPv6 without manually assigning an IP address to the interface we use the command `1pv6 enable`
- *Note*: IPv6 doesn't use broadcast addresses
- 