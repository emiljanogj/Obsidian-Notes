#IPv6 
***

### **Shortening IPv6 addresses**
![[Pasted image 20230902130138.png]]

### **Finding the IPv6 prefix**
- Typically, an enterprise requesting IPv6 addresses from their ISP will receive a 148 block.
- Typically, IPv6 subnets use a /64 prefix length.
- That means an enterprise has 16 bits to use to make subnets.
- The remaining 64 bits can be used for hosts.

![[Pasted image 20230902130733.png]]

- For multiples of 4 it's straightforward
![[Pasted image 20230902130921.png]]

- For non-multiples of 4
![[Pasted image 20230902131126.png]]

### Configuring IPv6 Addresses
- First we enable IPv6 routing with the following command:
```
ipv6 unicast-routing
```

- Then we set an IP address
```
ipv6 address 2001:db8:0:0::1/64
```

### EUI-64

- It's a method of converting a MAC address(48-bits) into a 64-bit interface identifier, which can then become the `host portion` of a `/64` IPv6 address
- Conversion method is as follows:
![[Pasted image 20230902133808.png]]
- To instruct the router to use `EUI-64` to generate an IPv6 address we use the following command:
```
ipv6 address 2001:db8:0:2::/64 eui-64
```

### IPv6 Address Types
**Global Unicast Addresses**
 - Public addresses => must register to use they and they must be globally unique
 - Originally defined as `2000::/3` but now defined as addresses which aren't reserved for other purposes
![[Pasted image 20230902135210.png]]

**Unique Local Addresses**
- Private Addresses which cannot be used over the internet(*Note*: It's good practice to use a random global ID in case 2 companies merge)
- Uses the address block FCO0:: /7 (FCO0: : to FDFF: FFFF: FFFF: FFFF: FFFF: FFFF: FFFF: FFFF)
- However, a later update requires the 8th bit to be set to 1, so the first two digits must be FD.
- Global ID(randomly generated) are the 40 bits after `FD`
- *Note*: They are routable
![[Pasted image 20230902135905.png]]

**Link Local Addresses**

- Link-local IPv6 addresses are automatically generated on IPv6-enabled interfaces.
- To enable IPv6 on an interface we use the following command:
```
ipv6 enable
```
- Uses the address block `FE80::/10 (FE80:: tO FEBF: FFFF: FFFF: FFFF: FFFF: FFFF: FFFF: FFFF)`
- However, the standard states that the 54 bits after `FE80/10` should be all 0, so you won't see
link local addresses beginning with `FE9`, `FEA`, or `FEB`, only `FE8`.
- The interface ID is generated using `EUl-64` rules.
- Link-local means that these addresses are used for communication within a single link (subnet).
- *Note*: Link-local addresses are not routable

*Note*: Routers will not route packets with a link-local destination IPv6 address.
- Common uses of link-local addresses:
- routing protocol peerings (OSPFV3 uses link-local addresses for neighbor adjacencies)
- next-hop addresses for static routes
- Neighbor Discovery Protocol (NDP, IPv6's replacement for ARP) uses link-local addresses to
function

**Multicast Addresses**
- One source to multiple destinations(that have joined the specific multicast group)
- Uses range `FF00::/8`
- *Note*: `IPv6` doesn't use broadcast
![[Pasted image 20230902141301.png]]

**IPv6** multicast scopes are divided into several categories:
- **Interface local(FF01)** - The packet doesn't leave the local device. Can be used to send traffic to a service within the local device
- **Link-local(FF02)** - The packet remains in the local subnet => Routers will not route the packet between subnets
- **Site-local(FF05)** - The packet can be forwarded by routers but within a single physical location
- **Organization-local(FF08)** - The packet can be forwarded within an entire company/organization
- **Global(FF0E)** - No boundaries

**Anycast addresse**
- One-to many
- Multiple routers are configured with the same `IPv6` address
- They use a routing protocol to advertise the address
- When hosts send packets to the destination address, routers will forward it to the neares router configured with that IP address
- To configure a router with anycast address we use the following command:
```
R1(config-if)# ipv6 address 2001:db8:1:1::99/128 anycast
```


#### IPv6 Header
![[Pasted image 20230903103525.png]]

- Traffic class
	- 8 bits in length
	- Used for QoS to indicate high-priority traffic
- Flow label
	- 20 bits in length
	- Used to identify a communication stream
- Payload length
	- Length of the payload in bytes
	- Length of header is not included since it's always 40 bytes
- Next header
	- 8 bits in length
	- Indicates the protocol used
- Hop Limit
	- 8 bits in length
	- Same as IPv4 TTL field
- Source/Destination Address
	- 128 bits in length

#### Solicited-Node Multicast Address

- Used for the Neighbor Discovery Protocol(NDP)
- Generated from the unicast address as follows:
![[Pasted image 20230903104157.png]]

#### Neighbor Discovery Protocol(NDP)

- Used to replace ARP for `IPv6`
- 2 message types used
	- Neighbor Solicitation(NS) == ARP Request
	- Neighbor Advertisement == ARP Reply

**Neighbor Soliciation**
![[Pasted image 20230903104620.png]]
*Note*: The `Destination IP (R2 solicited-node multicast address)` is computed from the unicast address as described above. R1 knows the unicast address from the ping command.

**Neighbor Advertisement**
![[Pasted image 20230903104804.png]]
*Note*: `R2` knows the destination MAC from the `Neighbour Solicitation` packet.


#### RS/RA Messages

- **Router Solicitation(RS)**
	-  Router Solicitation (RS) = ICMPv6 Type 133
	- Sent to multicast address FF02::2 (all routers).
	- Asks all routers on the local link to identify themselves.
	- Sent when an interface is enabled/host is connected to the network.
- **Router Advertisement (RA)**
	- Router Advertisement (RA) = ICMPv6 Type 134
	- Sent to multicast address FF02::1 (all nodes).
	- The router announces its presence, as well as other information about the link.
	- These messages are sent in response to RS messages.
	- They are also sent periodically, even if the router hasn't received an RS.


#### SLAAC

- Stands for **Stateless Address Auto-configuration**
- Hosts use the RS/RA messages to learn the IPv6 prefix of the local link and then automatically generate an IPv6 address
- This helps us avoid using the `ipv6 address prefix/prefix-length eui-64` to manually enter the prefix
- Instead we use the following command:
```
ipv6 address autoconfig
```
- The device will use `EUI-64` to generate the interface ID

#### Duplicate Address Detection(DAD)
- Allows hosts to check if other devices on the local link are using the same IPv6 address
- Any time an IPv6 interface initializes(with `no shutdown`) or an IPv6 address is configured on an interface it performs DAD
- It uses `NS` and `NA` messages
- The host will send an `NS` to its IPv6 address. If it doesn't get an `NA`, then its address is unique


#### Static Routing
- IPv6 is disabled by default and must be enabled with `ipv6 unicast-routing `
- There are 3 types of routes we can configure:
```
#Network route
RI (config)# ipv6 route 2001:db8:0:3::/64 2001: db8:0:12::2 
```
```
#Host route
R2(config)# ipv6 route 2001: db8:0:1::100/128 2001: d68:0:12:1
R2(config)# ipv6 route 2001:db8:0:3::100/128 2001: d68:0:23:2
```
```
#Default route
R3 (config)# ipv6 route :: /0 2001: db8:0:23::1
```
