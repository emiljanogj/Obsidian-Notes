![[Pasted image 20230422132904.png]]

Assume we have the above network topology, where the Warehouse switch was selected as the Root Bridge, since it was the oldest switch, i.e. the one with the lowest MAC address. If we would like to send traffic from a PC connected to Access1 to another PC connected to Access3, the path(after removal of blocked links) would be as follows:
![[Pasted image 20230422133128.png]]

To set a switch as the Root Bridge for a particular VLAN we use the following command:

```
spanning-tree vlan {VLAN-number} root primary
```

Setting Core1 switch as the Root Bridge, will enable the traffic in the above example to follow this route:
![[Pasted image 20230422134220.png]]

This route is much more optimal(5 hops) compared to the previous route(7 hops).

- To set a backup Root Bridge, we use the following command:
```
spanning-tree vlan {VLAN-number} root secondary
```

