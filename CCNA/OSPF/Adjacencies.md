#OSPF_Routing
***

- As discussed previously, L3 IP header contains the following fields:
![[Pasted image 20230412221737.png]]
The protocol is an integer denoting the following:
- 6: TCP
- 17: UDP
- 89: OSPF

#### OSPF Packet

If **OSPF** is used as the protocol, the packet contains the following fields:
![[Pasted image 20230412222326.png]]

where the fields are as follows:
- Version Number: OSPFv2 or OSPFv3
- Type: 
	- Hello
	- LSR
	- LSU
	- LSAck
	- DBD(Database Descriptor)
- Authentication Type:
	- 0 - No Password
	- 1 - Plain text password
	- 2 - MD5
- Authentication: authentication string if auth is used

#### Hello Packets
- Used by OSPF routers to discover each-other and form adjacencies
- They send Hello packets out each interface where OSPF is enabled except [[Adjacencies and Passive Interfaces#^3600a6]]
- Multicast to 224.0.0.5 every 10s to make sure that a router is not down
- A Hello packet contains the following fields:
	- *Router ID*: 32-bit number that uniquely identifies each OSPF router
	- *Hello Interval*: How often the router will send Hello packets(default = 10s)
	- *Dead Interval*: How long the router will wait before declaring the neighbor out of service(default = 4 x Hello Interval)
	- Neighbors: A list of adjacent OSPF router that this router has received a Hello packet from
	- Area ID
	- Stub Area Flag: Stub areas have a default route to their ABR rather than learning routes outside their area => if the dst is in another area, they just send that packet to the ABR and let ABR handle the rest

#### When do 2 routers form adjacencies?
 The following fields from the Hello packet must match for the router to form adjacencies:
 - Must be in each other's neighbor list(this is first established between routers when a router first joins the network)
 - Hello and Dead intervals
 - Area ID
 - IP Subnet
 - Authentication Flag
 - Stub Area Flag

#### Illustration of how an adjacency is formed

1. Each router sends its router IDs(Usually a router has a loopback interfaces as its ID. If no loopback interface exists like in this scenario, it picks the IP of the highest interface)
![[Pasted image 20230412225834.png]]

2. The routers start the exchange
![[Pasted image 20230412225958.png]]

3. Each router sends a summary of its Link State Database(LSDB) with a DBD packet
![[Pasted image 20230412230127.png]]

4. If a router sees from DBD packet that it's missing some info about its neighbor's neighbors, it sends an LSU packet as follows:
![[Pasted image 20230412230331.png]]

5. The router that received the LSU sends an acknowledgement(LSAck) that it received the info
![[Pasted image 20230412230433.png]]

#### Lab

To debug issues with **OSPF** we use the following command:
```
debug ip ospf adj
```

To check the MTU for mismatch we use the command:
```
show ip interface {interface-name}
```

To set the MTU we use the commands:
```
config t
int {interface name}
ip mtu {mtu-number}
```
