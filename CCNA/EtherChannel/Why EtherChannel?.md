
- Assume we have connected several PCs to a Switch in our Access Layer. Usually these PCs don't all send requests at the same time => We can afford to have a single upward link from the Access Layer to the Distribution Layer(usually the ratio is 20:1).
- Assume we have a 48 port 1Gbps Switch with 2 10Gbps upward links => The ratio is 48/20 = 2.4:1
- However there is a problem when using the Spanning Tree algorithm.  Assume we have the following network topology:
![[Pasted image 20230423160413.png]]

As discussed in [[How STP Works?]] , STP doesn't support load balancing => Access1 will always select the link T0/1 to connect to its Root Bridge => T0/2 link is blocked.
- To avoid the issue above, we introduce the concept of EtherChannel, which is simply grouping several physical interfaces into a single logical interface. 
- Hence, the topology in Fig.1 now becomes as follows:
![[Pasted image 20230423160639.png]]

In this case, if either of the uplinks from Access1 -> Dist1 fails, the other link will take over, so we will be using the full capacity.