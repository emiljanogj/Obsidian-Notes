#IPv6
***

- A Cisco router can generate full IPv6 addresses for itself when given the interface and /64 network to use.
- The IPv6 address is generated by injecting`FF:FE` in the middle of the MAC address(48 bits) and inverting the 7th bit.


**EUI-64 Address Configuration**

```
R1(config)# int f0/0
R1(config-if)# ipv6 address 2001:db8:0:1::/64 eui-64
R1(config)# int f2/0
R1(config-if)# ipv6 address 2001:db8:0::/64 eui-64
```

*Note*: On router interfaces, it is recommended to use a memorable address such as `2001:db8:0:1::1` rather than the `EUI-64`.