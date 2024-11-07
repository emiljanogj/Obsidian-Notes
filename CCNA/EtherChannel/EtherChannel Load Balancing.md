Assume we have the following network topology:

![[Pasted image 20230423161515.png]]

- Then PC1 will send its first packet via the first link, 2nd packet via the same link etc. => Loadbalancing in this case doesn't mean that packets from PC1 use different links as this would cause the packets to arrive out of order. However, packets from PC1 use the first link, packets from PC2 use the 2nd link etc. Only if the link goes down, will the packets change their route.