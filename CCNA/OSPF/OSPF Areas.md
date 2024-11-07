- OSPF supports a hierarchical design which segments large networks into smaller areas
- Each router maintains full info about its area and only a summary of other areas.

A two level hierarchy is used as shown below:
![[Pasted image 20230412145343.png]]

- Area 0(Transit area/Backbone) - Does not generally contain end users. All the traffic goes through it
- Area 1(Non-backbone) - Contains the end users and is used to connect them with Area 0.
- A router can only form adjacencies with its neighbors on the same area.

Routes received from other routers in the same area appear as internal OSPF routes:
![[Pasted image 20230412145946.png]]

A router may have interfaces in multiple areas. These routers are called Area Border Routers(**ABRs**)

![[Pasted image 20230412150058.png]]
An **ABR** has the following characteristics:
- It separates LSA flooding zones => if a link goes down in one area, it will only flood the routers in that area.
- It becomes the primary point for area address summarization. For example, in the image above, 1st ABR contains the full info about routers in Area 1 and Area 0, but only a summary of the routes in Area 2 and vice versa.
- It functions usually as the source for default routes
- They don't automatically summarize => If we don't configure summarization, all routes are flooded everywhere => The network will behave basically as a single-area network.

To configure summarisation, we use the following commands:
![[Pasted image 20230412154136.png]]
If the route 10.0.0.0/24 goes down, the route 10.0.0.0/16 is still unchanged so R1 doesn't need to do anything and the change is only propagated in the routers in area 1(R3 + R4).

###### Autonomous System Boundary Routers
![[Pasted image 20230412164112.png]]
If we are using another routing algorithm ain Area AS besides OSPF, i.e. EIGRP, we may use an **ASBR** to redistribute the paths to the OSPF neighbors => ASBR is running both EIGRP + OSPF

### Lab

