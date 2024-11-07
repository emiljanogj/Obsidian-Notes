*Connected Routes* - only show the subnet for an interface, but not the actual IP, as shown in the image below:
![[Pasted image 20230409153031.png]]

*Local Routes* - shows the actual IP for the interface. However, it always show the subnet as /32 despite of the actual subnet, as shown below:
![[Pasted image 20230409153047.png]]
**Static Routing**
Adding routes manually in the routing table if an interface doesn't exist as shown in the diagram below:
![[Pasted image 20230409153121.png]]

**Static Routing Example with 3 Routers**
![[Pasted image 20230409153151.png]]

**Route Summarization + Default Routes**
***Route Summarization***
Assume we have the following network topology:
![[Pasted image 20230409153216.png]] ^19c92f

We define the following routes in R1:
```
ip route 10.1.0.0 255.255.255.0 10.0.0.2
ip route 10.1.1.0 255.255.255.0 10.0.0.2
ip route 10.1.2.0 255.255.255.0 10.0.0.2
```

The static routes above enable us to reach the R2 and R3 interfaces from R1. However instead of defining the 3 routes separately, we can also do sth like this:
`ip route 10.1.0.0 255.255.0.0 10.0.0.2`

**Longest Prefix Match**
![[Pasted image 20230409153256.png]]

Assume we want to reach 10.1.3.2 from R1. There are 2 paths, we can take:
1. R1 -> R2 -> R3 -> R4 -> R5
2. R1 -> R5

The corresponding IP rules are defined as follows:
```
ip route 10.1.0.0 255.255.0.0 10.0.0.2
ip route 10.1.3.0 255.255.255.0 10.0.3.2
```
In this case, the packet will pick the route with the longest prefix, i.e. in this case the 2nd route.

**Load Balancing**
Assume now that both routes have the same prefix, as follows:
![[Pasted image 20230409153322.png]]
In this case, the router will load balance between the routes, i.e., pick the route with the smallest load and send the packet through it.

**Default Route**
![[Pasted image 20230409153339.png]]

Assume now we have the network topology defined above. We have defined in R1 the following rules:
```
ip route 10.1.0.0 255.255.0.0 10.0.0.2
ip route 10.1.3.0 255.255.255.0 10.0.3.2
```
Now we need a rule to reach the internet. For this, we add a default gateway as follows:
`ip route 0.0.0.0 0.0.0.0 203.0.113.2`
In this case, every packet that doesn't match the first two rules, will be routed via this rule.