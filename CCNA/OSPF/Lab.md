#OSPF_Routing 
***

**Important Reminder**: Remember to use the inverse of the subnet mask in the `network` command as follows:
```
network 10.0.0.0 0.255.255.255 area 0
```

- To find the router ID we use the following command:
```
sh ip protocols
```

*Note*: This is usually set to the IP of the loopback0 interface in case such an interface exists, or to the highest IP otherwise.

- To print the interface cost we use the following command(i.e cost if FastEthernet0/0 in the case below):
```
sh ip ospf interface f0/0
```

- For multi-area configurations, if we split the routers in multiple areas, each route will denote the inter-route areas by `IA`, however it will not perform automatic summary. To perform automatic summary, we need to define the range for each are, for example:
```
area 0 range 10.0.0.0 255.255.0.0
```

- ![[Pasted image 20230713000840.png]]

Assume in the example above that we don't want to send any OSPF info to the other AS. In this case we add a static router from the router in red to the external router, and redistribute this route to OSPF. The router in red is called ASBR.

- To find the priority of a router we use the following command:
```
sh ip ospf interface {interface-number}
```

- To set the priority:
```
config t
interface {interface-number}
ip ospf priority {priority-num}
```

The router with the highest priority will be set as the Designated Router. Otherwise, the router with the highest loopback interface number.

*Note*: After changing the priority, we need to go back to the main terminal and run the following command:
```
clear ip ospf process
```