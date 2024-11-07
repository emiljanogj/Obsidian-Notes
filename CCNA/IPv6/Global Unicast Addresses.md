#IPv6
***

3 types of address:
- Global unicast - similar to IPv4 public addresses. Assigned from the range 2000::/3
- Unique local
- Link local

#### Global Unicast Addresses

- Similar to IPv4 addresses
- Assigned from the range 2000::/3
- 64 bits for the subnet + 64 bits for the host
- If a company is assigned a /48 address => that leaves 16 bits for internal subnets.
- ExampleL If a company is assigned 2001:10:10::/48 => it can assign subnets 2001:10:10:0::/64 - 2001:10:10:ffff::/64 to its internal network segments

##### Global Unicast Address Configuration

```
R1(config)# ipv6 unicast-routing
R1(config-ig)# int f0/0
R1(config-if)# ipv6 add 2001:db8:0:1::1/64
R1(config-if)# int f2/0
R1(config-if)# ipv6 add 2001:db8:0:0::1/64
```

*Note*: The global unicast addresses need to be unique as opposed to link local addresses which we will take a look at latter.
