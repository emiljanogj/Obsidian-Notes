- Network command works the same as in [[EIGRP]]. 
- The difference is that when we don't specify the network, it defaults to the classful boundary

Assume we have the following network command:
```
R1 (config-router)# network 10.0.0.0 0.0.255.255 area 0
```
This instructs the router to do the following:
1. Look for interfaces that fall within that range
2. Enable OSPF on those interfaces, send out and listen to **Hello** packets, and form adjacencies.
3. After forming adjacencies, the router will advertise the network and mask  configured on the interfaces it found on step 1.

