g- RIP uses hop count as shown below
![[Pasted image 20230409173715.png]]

In the network topology above, if R4 want to reach 10.0.1.0/24 subnet it will go through R4->R5->R1. We see however that this is not a good idea in this example since the R4->R5 connection has low bandwidth.

Let's see how the routers advertise their routes in the above network topology. Assume R5 has already updated its routing table. R1 will now send the following message to R2:
![[Pasted image 20230409175245.png]]


![[Pasted image 20230409175406.png]]

R3 will then advertise to R4 etc. One of the limitations of this approach is that the path with lowest bandwidth may end up being often used. This problem is solved by *OSPF*, which uses bandwidth when calculating the cost.
*IS-IS* is another protocol that can be used. In this case the cost of the paths needs to be set manually, or otherwise it defaults to using the hop count. 
*EIGRP* uses the bandwidth + delay to compute the cost.  A fixed delay is computed based on the interface bandwidth, but it can be set manually also if we would like to manipulate the path.