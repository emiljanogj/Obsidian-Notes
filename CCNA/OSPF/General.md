It works as follows:
- Each router will discover its neighbors and forms adjacencies
- They will then flood the Link State Database(LSDB), and they will learn all the possible routes
- The router will then compute the shortest path to each destination using Dijkstra and add them in the routing table
- The router will then listen and respond to network changes

###### OSPF Packet Types

- **Hello** - send out and listen for Hello packets when OSPF is enabled, and form the adjacencies.
- **DBD Database Description** - Adjacent routers will tell each other about the networks they know with this packet
- **LSR(Link State Request)** - If a router is missing info about a network, it will ask its neighbor for more info via an **LSR**. 
- **LSA(Link State Advertisement)** - This is the packet the neighbor sends out when a router is looking for more info.
- **LSU(Link State Update)** - A list of LSA. When a major change is made to the network, i.e.  a new link is added, this change needs to be propagated through the network via an **LSU**. 
- **LSAck** - Receiving routers acknowledge LSAs.