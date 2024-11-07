- Will do equal cost load balancing on up to 4 paths by default, but can be increased to 16
- Can be configured to also do unequal cost load balancing
- The network command uses a wildcard mask which is the inverse of the subnet mask => to calculate the subnet mask from the wildcard mask, we subtract the wildcard from 255.255.255.255 as follows
```
router eigrp 100
network 10.0.0.0 0.0.255.255
```
If we do not enter a wildcard mask, it will default to the wildcard mask for that specific class.

Example: Assume we have the following network topology:
![[Pasted image 20230410230004.png]]

Assume we configure the eigrp routing protocol as follows:
```
R1(config-router) # network 10.0.0.0 0.0.255.255
```
This implies the following"
- 10.0.1.0/24 will be advertised
- 10.0.2.0/24 will be advertised
- 10.1.0.0/24 will not be advertised(not in the networking range)
- 10.0.0.0/16 will not be advertised(only the specific interfaces are advertised and not the network itself)

- EIGRP routers identify themselves using an EIGRP router id which is in the form of an ip address. Since this needs to be constant there are 2 options:
	- use a Loopback
	- manually set the router id

- EIGRP updates are sent via `224.0.0.9`