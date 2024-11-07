- Logical interfaces that allow us to assign an IP address to a router or an L3 switch
- Not tried to a physical interface => they cannot go down
- Assigned /32 subnets => no IP addresses are wasted with these interfaces
- Same loopback interface is usually used for multiple tasks, i.e management and BGP.

Assume we are working with the following network topology:

![[Pasted image 20230410000435.png]]

And we'd like to go from 10.1.2.10 to 10.0.0.1. To go to 10.0.0.1, we may take one of the following paths:
	- R4 -> R3 -> R2 -> R1
	- R4 -> R5 -> R1

- If the upper path goes down, then we cannot connect to R1(10.0.0.1)
- If the lower path goes down, we cannot connect to R1 interface 10.0.3.1

In order to be able to connect to R1 if either of the paths go down, we define a loopback interface in R1 as shown below:

![[Pasted image 20230410000937.png]]

R4 will then learn the 2 paths to get to *Loopback0*, and will pick the path with the lowest cost. =>  R4 will still connect to R1(loopback 0) even if one of the paths go down. 

**Lab**

To create a loopback interface, we do the following:
1. Create the loopback interface
```
interface loopback 0
```

2. Give it an ip address in /32 subnet
```
ip address 192.168.1.1 255.255.255.255
```

3.  Enable the routing protocol in the loopback interface
```
do sh run | sec eigrp
router eigrp {NUMBER}
network 192.168.1.1 0.0.0.0
```

Important note from the lab:
![[Pasted image 20230410131810.png]]
=> Floating static routes defined within a subnet != /24 will still appear in the routing table even if we define the route with a lower AD([[Administrative Distance]]) than EIGRP.