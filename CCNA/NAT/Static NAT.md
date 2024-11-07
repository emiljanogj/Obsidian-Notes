3 different NAT Types:

- Static NAT - one-to-one mapping bw a private and public IP address. Used for servers that accept incoming connections.
- Dynamic NAT - a pool of public addresses offered on a first come first serve basis. Usually used for internal hosts that need to connect to the internet but do not accept incoming connections
- PAT - allows the same public IP address to be reused

- Assume we have the following network topology:

![[Pasted image 20230501145000.png]]

In router R1, we want to configure the mapping between the private IP address for **INT-S1**(network 10.0.1.0/24) and its public IP address(203.0.113.0/28). To do that, we use the following commands:

```
R1(config)# int f0/0
ip nat outside

int f1/0
ip nat inside

ip nat inside source static 10.0.1.10 203.0.113.3
```

#### NAT Translations

- **Inside local address** - IP address actually configured on the inside host's OS(`10.0.1.10` in th e above example)
- **Inside local address** - The NAT'd address of the inside host as it will be reached by the outside networ(`203.0.113.3` in the above example)
- **Outside local address** - IP address of the outside host as it appears to the inside network(`203.0.113.20` in our example)
- **Outside global address** - IP address assigned to the host on the outside network by the host's owner(`203.0.113.20` in the example above)

#### 2-Way NAT

In most scenarios, the **outside local address** and **outside global address** will be the same. However assume we have the following scenario:

![[Pasted image 20230501152227.png]]

Company A and B have merge together but they haven't had the time to readjust their IP addresses => A1 and B1 will have the same address, which is bad news. Hence we need to NAT the address on the left and the address on the right also =>
- When we send a packet A1 -> B1, R1 will translate the source address(`10.10.10.10`) to `10.10.20.10`.
- The destination address also needs to be address from `10.10.30.10` to `10.10.10.10`. In this case the above variables in A1, will be as follows:
	- Inside local address = 10.10.10.10
	- Inside global address = 10.10.20.10
	- Outside local address = 10.10.30.10
	- Outside global address = 10.10.10.10

*Note*: For one-way NAT outside local and outside global are reported as being the same(public IP address of the other host). That's not necessarily the truth, since the other host has its own private IP.

#### Lab

- To check the NAT translations we use the following command:
```
show ip nat translations
```

