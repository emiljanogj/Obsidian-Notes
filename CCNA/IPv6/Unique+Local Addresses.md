#IPv6 
***

#### Unique Local Addresses

- Similar to IPv4 private addresses
- Not publicly reachable
- Assigned from the range `FC00::/7`


### Link Local Addresses

- Valid for communications on that link only
- Assigned from the range `FE80::/10 - FEB0::/10`
- Hosts should be assigned /64 addresses
- Example: Assume we have the following networking topology:
![[Pasted image 20230725231639.png]]
In the example above we have the following connectivities:
- A <-> B via `FE80::/64` and `FE80::2/64`
- A <-> C and B <-> C via `FE80::1/64` and `FE80::3/64` and `FE80::2/64` and `FE80::3/64` respectively. 


- Link local addresses are used for communications which should not be forwarded beyond the local link like routing protocol hello packets and updates.
- Link local addresses are automatically generated with EUI-64 addresses on IPv6 enabled Cisco router interfaces(it can be ridden with manual configuration)

#### Link Local Address Configuration

```
R1(config)#int f0/0
R1(config-if)#ipv6 address fe80::1 link-local
R1(config-if)# int f2/0
R1(config-if)ipv6 address fe80::1 link-local
```

#### Assigning multiple address to an interface with IPv4 and IPv6
- When we assign multiple address to an IPv4 interface as follows:
![[Pasted image 20230725233004.png]]
we see that only the latest address is save. However, we can assign at most 2 IPv4 addresses to an interface using the secondary keyword as follows:
![[Pasted image 20230725233137.png]]

- With IPv6 however we can assign multiple addresses without using any special keyword as follows:
![[Pasted image 20230725233248.png]]

**Final Notes**:
- Link local addresses are mandatory on IPv6 addresses while unique local addresses and global unicast addresses are optional
- We can have multiple addresses in the same interface
- Usually one link local address for the routing protocol + one global unicast address for normal routing is typical.