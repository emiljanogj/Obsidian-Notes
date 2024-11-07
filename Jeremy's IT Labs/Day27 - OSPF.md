
- One more option to change the OSPF cost of an interface is to change the bandwidth of the
interface with the bandwidth command.
- The formula to calculate OSPF cost is reference bandwidth / interface bandwidth
- Although the bandwidth matches the interface speed by default, changing the interface
bandwidth doesn't actually change the speed at which the interface operates.
- The bandwidth is just a value that is used to calculate OSPF cost, EIGRP metric, etc.
- To change the speed at which the interface operates, use the speed command.
- Because the bandwidth value is used in other calculations, it is not recommended to change
this value to alter the interface's OSPF cost.


### Modifying the OSPF Cost
3 methods to modify the cost:
- Change the reference bandwidth
```
R1(config-router)# auto-cost reference bandwidth megabits-per-second
```

- Manual configuration
```
R1(config-if)# ip ospf cost cost
```

- Change the interface bandwidth
```
R1(config-if)# bandwidth kilobits-per-second
```


### OSPF Neighbors

There are several states that the neighbouring process goes through

1. **Down**
	- OSPF is activated on R1's G0/0 interface
	- It sends an OSPF hello message to `224.0.0.5`
	- It doesn't know of any neighbors yet so the state is `Down`
![[Pasted image 20230901002452.png]]

2. **Init**
	- R2 receives the Hello packet and it will add an entry for R1 to its OSPF neighbor table
	- R2's ID is not in the Hello packet as we saw above that it has an initial `RID` of `0.0.0.0`
![[Pasted image 20230901004911.png]]

3. **2-way**
	- R2 will send a Hello packet containing the RID of both routers
	- R1 will insert R2 into its OSPF neighbour table in the 2-way state
	- R1 will send another message containing the RID of router R2
	- R2 will insert R1 into its OSPF neighbour table in the 2-way state
	- Now all routers are in the same state
![[Pasted image 20230901005311.png]]

4. **Exstate**
	- The two routers will now prepare to exchange info about their LSDB
	- First they have to pick a `Master` and a `Slave`
	- The router with the higher RID becomes the **Master** and initiates the exchange
![[Pasted image 20230901005602.png]]

5. **Exchange**
	- The routers exchange **DBD**s which contain a list of LSAs in their LSDBs
	- The **DBD**s do not contain detailed information, just basic stuff
	- The routers compare the info in the **DBD** they received to the info in their own **LSDB** to determine which LSAs they must receive from their neighbors
![[Pasted image 20230901010011.png]]

6. **Loading**
	- Routers send **Link State Request(LSR)** messages to request that their neighbors send them any **LSA**s that they don't have
	- **LSAs** are sent in **Link State Update(LSU)** messages
	- The routers send **LSAck** messages to acknowledge that they received the **LSAs**
![[Pasted image 20230901010245.png]]

7. **Full**
	- The routers have a full OSPF adjacency and identical **LSDBs**
	- They continue to send and listen for Hello packets(every 10s)
	- When a hello packet is received the `Dead` timer is reset(40s). If the timer goes to 0, the neighbor is removed

*Note*: The `network` command simply tells the router on which interface to activate the routing protocol. We can also do this in the interface itself as follows:
```
R1(config)#int g0/0
R1(config-if)#ip ospf 1 area 0
```

