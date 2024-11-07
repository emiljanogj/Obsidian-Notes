
![[Pasted image 20230416010513.png]]

- If SALES PC1 sends broadcast traffic to the Switch, the Switch will forward this traffic to all the ports, except for the port where it came from. However, this ignores the security rules implemented at L3 since switches operate at a lower level(L2) => This presents security concerns + overflows the links and the routers with unnecessary traffic.
- The above problem is solved by introducting VLANs, which segment the LAN into separate broadcast domain at L2.
- Usually a one-to-one mapping bw an IP subnet and a VLAN
- In the above example, we create 2 VLANs as follows
![[Pasted image 20230416011134.png]]
