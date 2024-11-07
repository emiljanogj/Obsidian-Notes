#RIP
***

- RIP will automatically summarise routes to the classful boundaries by default. For example: 192.168.10.1/30 will be advertised as 192.168.10.0/24 since it's a class B network and class B networks have a subnet mask of 255.255.255.0. ^37af4f
- This auto-summary feature is not desirable, so when enabling RIP we do the following
```
router rip
no auto-summary
```
- However, address summary is still desirable as explained in [[Dynamic Routing]], so we enable it with the following command(assume the network topology is the one shown below):
![[Pasted image 20230410220613.png]]
```
R2(config-router): interface f1/0
R2(config-router): ip summary-address rip 10.0.0.0 255.255.0.0
```

*Note*: We're configuring the summary in the interface where we're sending the update out of.

To check if a route was received via RIP, we use the following command:
```
ip rip database
```

We can add a default static route as follows(assume the following network topology):
![[Pasted image 20230410221928.png]]

```
ip route 0.0.0.0 0.0.0.0 203.0.113.2
router rip
default-information originate
```
*Note*: The static route is only added in the outbound router(R4 in the above topology)instead of adding it in every router.

**LAB**

To configure a router using the RIP routing protocol, we use the following commands:
```
router rip
version 2
no auto-summary (1)
network 10.0.0.0 (2)
```

(1) is used to avoid the issue described in [[#^37af4f]]
(2) is used to define which network the router will advertise. Exceptions are made in the case of passive interfaces as described in [[Adjacencies and Passive Interfaces#^3600a6]].
