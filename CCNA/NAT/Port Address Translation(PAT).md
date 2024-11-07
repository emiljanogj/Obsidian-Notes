itn g0**Note**: The problem with **Dynamic NAT** is that we may run out of public IPs, and all the requests after this point will be automatically terminated. To avoid this problem we use **PAT**
- PAT is an extension to NAT that permits multiple devices to be mapped to a single public IP address. Each host is mapped to a different port number.
- We configure PAT exactly the same as NAT but we put the `overload` keyword in the last command as follows:
```
ip nat inside source list 1 pool Flackbox overload
```

#### Practical Scenario

Assume a small office accesses the internet, but hasn't bought any public IP => Whenever one of the hosts tries to access the internet, its private IP will be mapped to a public IP assigned dynamically by DHCP. In this case, we configure PAT in exactly the same way as above, but instead of translating to an IP range, we translate to the outside interface as follows:

```
R1(config)# int f1/0
ip nat inside

R1(config)# int f0/0
ip nat outside

access-list 1 permit 10.0.2.0 0.0.0.255

ip nat insde source list 1 interface f0/0 overload (+)
```

*Note*: The only difference from the previous config is the last command(+), where in the `Dynamic NAT` case we used the following:
```
ip nat pool {pool-name} {start-IP} {end-IP} netmask {subnet-mask} overload
```
If the company is small and has not purchased any public IP address, it will get a public IP address from the DHCP server => We replace the line where we allocate the pool with the line in (+), where `f0/0` is the outside interface rather than a pool of addresses. ˜∫
