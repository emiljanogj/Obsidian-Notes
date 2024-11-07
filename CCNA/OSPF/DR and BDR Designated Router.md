
Assume we have the following network topology:
![[Pasted image 20230412235509.png]]

- It is inefficient for all the router to form a mesh, i.e. to send updates to all the other routers. A more efficient solution is to send updates to a master(DR) and let the master distribute it to the other router. To make this solution more robust, we need a backup(BDR) for the master. 
- The router with the highest priority become DR and the one with the 2nd highest priority becomes BDR.
- The DR and BDR establsh FULL neighbor state with all the routes on the network segment, while the other routers only establish 2-Way state and they don't exchange routes with one another. When a change occurs on a router connected to a multiaccess segment, it sends an LSU to 224.0.0.6 which is the address where the designated routers listen to. The DR/BDR will then multicast it to all the other routers in the segment that have OSPF enabled(multicast at 224.0.0.5)

#### Lab

- If several routers have the same priority, the one with the highest router ID will be selected as DR and the one with the 2nd highest router ID will be selected as BDR.
- To find check the router ID we use the following command:
```
sh ip protocols
```
- To change the reference bandwidth in the cost calculation we use the following command:
```
auto-cost reference bandwidth 100000
```
- To print the cost of a route that uses OSPF we use the following command
```
sh ip ospf interface {interface-number}
```
- To print the adjancies, we use the following command:
```
show ip ospf neighbor
```
- **Important note**: To change the cost of the link we need to change the cost of the interfaces in both side.
- To propagate a static route to all the routers in a network we use the following command:
```
router ospf 1
default-information originate
```
- *Important Note*: Even if we use segmented networking, we won't get its benefits(smaller routing table sizes) without enabling the route summary in the Area Border Routes.

- In a single-area network, we ensure that a router becomes DR by setting its priority to highest with the following command:
```
ip ospf priority {priority-number}
```

![[Pasted image 20230416001544.png]]

Routers establish Full connections with the DR + BDR, and only 2WAY connections with the other routers => they share their full paths with the DR + BDR and only a summary of the paths with the other routers.