#NAT
***

Assume we have the following network topology:
![[Pasted image 20230814222934.png]]

And we have the following requirements:
Configure the network as follows:

1) Configure the router to get an IP address via DHCP from your ISP

2) Configure the router to allocate IP addresses via DHCP to clients in your network:

- Network = 10.1.1.0/24

- Default Gateway = 10.1.1.254

- DNS = 8.8.8.8

3) Configure the router so internal hosts can access the internet servers using PAT (router IP address)


To solve this, we do the following:
1. Get an IP address for the interface `Gig0/0/1`  
```
Router1(config)# int g0/0/1
Router1(config-if)# ip address dhcp -> this interface gets assigned its IP from the ISP(in this case it received the IP = 8.8.8.100)
```

2. Assign an IP address to the interface `Gig0/0/0`
```
Router1(config)# interface Gig0/0/0
Router1(config-if)# ip address 10.1.1.254 255.255.255.0
```

3. Configure Router1 as DHCP server
```
Router1(config)# ip dhcp pool {pool-name}
Router1(dhcp-config)# network 10.1.1.0 255.255.255.0
Router1(dhcp-config)# default-router 10.1.1.254
Router1(dhcp-config)# dns-server 8.8.8.8
Router1# show ip dhcp binding
```

4. Define the PAT mapping
```
Router1(config)# access-list 1 permit 10.1.1.0 0.0.0.255
Router1(config)# int g0/0/1
Router1(config-if)# ip nat outside
Router1(config)# int g0/0/0
Router1(config-if)# ip nat inside
Router1(config) ip nat inside source list 1 interface g0/0/1 overload -> PAT
```

5. Make sure to change the PC configs to enable DHCP
