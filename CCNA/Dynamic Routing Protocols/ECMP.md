If multiple paths to a destination have equal cost, the router will enter all the routes in the routing table and will load balance between them.

*EIGRP* is the only path that can do Unequal Cost Multi Path, but it needs to be manually configured to support this.

Let's assume we have the following network topology:
![[Pasted image 20230409220220.png]]

Then to go from R4 to R1:
- half the paths will take R4 -> R3 -> R2 -> R1
- half the paths will take R4 -> R5 -> R6 -> R1

*Note*: Load-balancing in this case means that all the packets that come from a particular source will go through a given path, and packets from another source will go through another path. This is done to ensure in-order arrival of the packets, because if packets from a single source were load-balanced between different paths, they may end up arriving out of order if the latency of a link in one path is high.