- **EtherChannel** is a solution introduced to avoid the problem of blocking ports in the Spanning Tree Protocol. For example, in the example below:
![[Pasted image 20230423170923.png]]
In Acc3, only the interface F0/24 is forwarding while all the other interfaces are blocked. After configuring **EtherChannel**, we get the following:
![[Pasted image 20230423171021.png]]
Now we have F0/23 and F0/24 in a forwarding state, which is better than before, but still the interfaces F0/21 and F0/22 are blocked. To solve this issue, we introduce the concept of **Multi-chassis EtherChannel**
- Spanning Tree** only sees one interface in this case so no loops are detected
- Full load balancing and redundancy across all interfaces is supported
- Technologies that enable **Multi-chassis EtherChannel** are:
	- StackWise
	- VSS
	- vPC