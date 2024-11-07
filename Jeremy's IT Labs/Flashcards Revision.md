  **Ethernet Header Frame**
![[Pasted image 20231015113712.png]]

**ARP  Code - `0x0806`**

**IPv4 Address classes**
![[Pasted image 20231015114515.png]]


**Interface Status**
- **Status** - shows  the L2 status of the interface
- **Protocol** - shows the L1 status of the interface

**Ethernet Terminology**
- **runts** - frames that are smaller than the min size of 64 bytes
- **giants** - frames that are greater than the max size of 1518 bytes
- `show interfaces` error counters:
	- **frame** - counts frames that have an incorrect/illegal format
	- **output frames** - counts frames that the switch tried to send but was unable to

**IPv4 Header**
![[Pasted image 20231015122541.png]]

*Note*: Max length of an **IPv4** header is 60 bytes depending on the length of the options field
- **DF(Don't Fragment) Flag** - Used to indicate whether the packet can be fragmented or not
- **MF(More Fragments) Flag** - Used to indicate if more fragments follow the current fragment

- **Fragmented Packets** - If a packet exceeds the max size it will be fragmented into several packets. When the receiving device receive a fragment, it uses:
	- **Identification** - to identify the original packet the fragment belongs to
	- **MF** - to check if more fragments are coming
	- **Fragment Offset** - to determine the position of the fragment in the original packet in case the fragments come out of order

**VLANs**
- Several default VLANs in a Cisco Switch: 1, 1002, 1003, 1004, 1005
- To display all the trunk VLANs we use the command `show interface trunk`
- **Extended VLAN** range is `1006 - 4094`
- To configure the VLAN number on a router subinterface we use the command `encapsulation dot1q {vlan-number}`
- **TPID** field is always set to `0x8100` => 16 bits in length
- **PCP** - 3 bits in length and is used for CoS
- **DEI(Drop Eligible Identifier)** - 1 bit in length and can be used to indicate whether the frame can be dropped in congested networks
- 802.1Q tag is inserted after the **Source MAC address** in the Ethernet frame header. It is **4 bytes** in length
- To reset an interface to its default configuration we use the command `default interface {interface-number}`
- To configure the native VLAN in a router subinterface we use the command `encapsulation dot1q {VLAN-ID} native`
- To configure a switch interface as a routed port, we use the command `no switchport`


### DTP/VTP
- Each time a change is made to the VLAN table in a VTP server, the revision number is increased
- `dynamic auto + trunk => trunk`
- Interfaces on old Cisco switches default to `switchport mode dynamic desirable` while in new switches they default to `switchport mode dynamic auto`
- VTPv1 + VTPv2 do not support the dynamic VLAN range `1006 - 4094`
- ISL is the Cisco proprietary protocol that is favoured in DTP
- VTP transparent maintains its VLAN database in NVRAM
- VTPv1/v2 client does not maintain the VLAN database in the NVRAM while VTPv3 client does


### STP
- *Note*: One designated port per collision domain
- 3 fields in the STP Bridge ID
	- Bridge Priority(4 bits)
	- Extended VLAN ID (12 bits)
	- MAC Address (48 bits)
- The root port are selected using these 3 criteria(in order of importance)
	- Root cost
	- Neighbour bridge id
	- Neighbour port id
- Interfaces in **Learning** state do not send/receive regular traffic
- To enable **BPDU Guard** on all **Portfast** enabled interfaces we use the following command:
```
SW(config) spanning-tree portfast bpduguard default
```
- **Loop Guard** shuts down an interface if it stops receiving BPDUs
- Interfaces in a blocking state keep receiving BPDUs, but they do not send BPDUs
- If a BDPU in a switch has exceeded the max age timer(default = 20s) it will be considered as stale and discarded
- **PVST+ BPDUs** are sent to the MAC address `0100.0ccc.cccd`, while **Standard STP BPDUs** are sent to the MAC address `0180.c200.0000`
- **Forward delay timer** - the time it takes for an interface to transition from the **Listening/Learning** state to the **Forwarding** state
- **STP BPDUs** are only forwarded in Designated ports
- **MAC Addresses** - Interfaces in **Learning state** learn MAC addresses, while the ones in **Listening state** do not.
- `spanning tree vlan {vlan-id} root primary` sets the priority to `24576` while `spanning tree vlan {vlan-id} root secondary` sets it to `28672`
- To set the link type we usually use the following command `SW1(config-if)# spanning tree link-type {point-to-pont|shared}`. However note that for the edge type, which is the same as a portfast-enabled interface we use the command `SW1(config-if)# spanning-tree portfast`
- A neighbour is considered lost if the switch misses 3 BPDUs
- In **RSTP** all switches originate the **BPDUs** while in the classic **STP** only the **Root Bridge** originates BPDUs
- 3 builtin features in **RSTP** that allow ports to move directly to a forwarding state
	- UplinkFast
		- Used by storing one or more backup roots on non-root switches. These are additional paths to the root bridge and can lead to faster convergence in case the link to the root bridge fails
	- BackboneFast
		- Sends its neighbor a proposal agreement in case it has lost path to the root bridge
	- PortFast
		- Same as in STP, i.e. sends the access ports directly to the forwarding state 
- **RSTP** is `802.1w`


### EtherChannel
- To configure a L3 **Etherchannel** we use the following commands:
```
SW1(config-if)# no switchport
SW1(config-if)# channel-group {group} mode {mode}
```
- To view the current EtherChannel load-balancing method we use the command `show etherchannel load-balance`
- **LACP** is `802.3ad`


### Dynamic Routing
- **EGPs** are used to share routes between different AS
- Two types of **IGP**
	- **Link State**
	- **Distance Vector**
	- Path Vector
- **Path Vector**
	- **BGP**
- **Link State Routing Protocols**
	- **OSPF** - cost is computed using the interface's bandwidth
	- **IS-IS** - all links have the same cost(10) by default
- **Distance Vector Routing Protocols**
	- **RIP**
	- **EIGRP**
- **Distance Vector** - A router sends the following info:
	- List of all known networks
	- Its cost to reach these networks
- **Link State**
	- Each router sends a list of its interfaces to its neighbours
	- Each list is passed unchanged so a router has a map of the global network topology

### OSPF
- To prevent an OSPF router from sending OSPF messages out of an OSPF-enabled  interface we use the command `R1(config-router)# passive-interface {interface-id}`
- To reset the OSPF process on the local router we use the command `clear ip ospf process`
- To configure the max number of paths an OSPF router will use to perform ECMP load balancing we use the following command `maximum-paths {number-of-paths}`
- OSPF LSAs have a default aging timer of 30' 
- Can't perform unequal cost load balancing
- **OSPF** messages are broadcast to `224.0.0.5`
- OSPF cost of a loopback interface is 1
- Goes through 7 stages
	1. Down
	2. Init
	3. Exstart
	4. 2-Way -> DR and BDR elected in this stage
	5. Exchange
	6. Loading
	7. Full
- OSPF Master/Slave determined during the Exstart Phase
- Default Dead Timer for Ethernet connections is 40s, while default **Hello timer** is 10s
- OSPF Message Types
	- 1 - Hello
	- 2 - DBD(Database Description)
	- 3 - LSR(Link State Request)
	- 4 - LSU(Link State Update)
	- 5 - LSAck
- To configure the reference bandwidth for OSPF we use the following command:
```
auto-cost reference-bandwidth {megabits-per-second}
```
- We can enable **OSPF** directly in an interface with the command:
```
R1(config-if)# ip ospf {process-id} area {area-id}
```
- To configure the OSPF type of an interface we use the following command:
```
ip ospf network {network-type}
```
- To enable OSPF authentication on an interface we use the following command:
```
R1(config-if)# ip ospf authentication
```
- In a **Serial** connection the **DCE** side specifies the clock rate. The speed is configured with the following command:
```
clock rate {bits-per-second}
```
- To check if an interface is **DCE** or **DTE** we use the following command:
```
show controllers {interface-id}
```
- If the MTU settings of 2 routers don't match, they can become OSPF neighbours but won't move to the Full state
### Distance Vector Protocols(RIP + EIGRP)
- **RIPng(RIP Next Generation)** is the RIP protocol used for IPv6
- **EIGRP** messages are broadcast to `224.0.0.10`
- **RIP** messages are broadcast to `224.0.0.9`
- To enable RIP on all the interfaces in a specified range we use the command `R1(config-router)# router rip`
- To configure the **EIGRP** router id we use the command `eigrp router-id a.b.c.d`
- The command to enter **EIGRP** is `R1(config-router)# router eigrp {as-number}`. *Note*: As opposed to **OSPF** where the command was `R1(config-router)# router ospf {process-id}` where the process ids between neighbor didn't have to match, here the as numbers for neighbours need to match
- *Note*: Note that when we use the `network` command to enable RIP on the interfaces, we don't enter a subnet/wildcard mask, since it will use the default mask for that network class


### FHRP
- To active **HSRPv2** we use the command `standby version 2`
- HSRP active router is determined based on the following criteria(sorted by order of importance)
	- Highest priority
	- Highest IP address
- **VRRP** multicast address is `224.0.0.18`


### IPv6
- To configure an IPv6 address using `EUI-64` we use the following command:
```
R1(config-if)# ipv6 address {prefix}/{prefix/length} eui-64
```
- In a directly attached static route, only the exit interface is specified
- To view the IPv6 equivalent of an ARP table we use the command `R1# show ipv6 neighbor`
- A recursive route is defined using the command `ipv6 route destination/prefix-length next-hop`
- **Link-local IPv6 addresses** are not routable => we can define a link-local address as the next hop only in **full-specified static routes**.


### Access List
- To configure a remark for a standard numbered ACL we use the following command:
```
access-list {number} remark {remark}
```
- To view all ACLs we use the following command:
```
R1# show ip access-lists 
```
- Standard ACLs are number in the ranges `1-99` and `1300-1999`. 


### LLDP + CDP
- To show detailed information for a **CDP** neighbor we use the following command
```
show cdp entry {hostname}
```
- Default **LLDP** holdtime is 120s and message timer is 30s
- To enable **CDP** v2 we use the command `cdp advertise-v2`
- **LLDP** messages are sent to MAC address `0180.C200.000E` while CDP messages are sent to MAC address `0100.0CCC.CCCC`
- To enable **CDP** globally we use the command `R1# cdp run` while to enable it on an interface we use the command `R1(config-if)# cdp enable`
- To show information about the sent/received LLDP frames we use the command `show lldp traffic`
- *Note*: A device can run CDP+LLDP at the same time
- LLDP can't share **VTP** information while CDP can.
- The reinitialization timer is set by default to 2s and is used in case of link flapping so that the LLDP doesn't reinitialize and consume CPU power each time a link is flapping


### NTP
- To create an NTP authentication key we use the following command:
```
ntp authentication-key {key-number} md5 {hashed-key}
```
- Protocol is `UDP:123`
- To set the software clock we use the command
```
clock set hh:mm:ss day month year
```
- To configure **symmetric active** mode, where a client peers with another client to use its clock in case the NTP master fails, we use the command `ntp peer {IP}`
- To specify the NTP authentication key to use with the NTP server we use the following command:
```
ntp server {server-IP} key {key-number}
```
- To show a list of NTP servers we use the command `show ntp associations`
- To set the timezone we use the following command:
```
clock timezone {name} {hours-offset} {minutes-offset}
```
*Note*: Only UTC is supported => we need to specify the hours and minutes offset from UTC

### DNS
- To enable a router to perform DNS lookup we use the command `ip domain lookup`
- To configure a router to act as a DNS server we use the command `ip dns server`
- To display DNS in a Windows machine we use the command `ipconfig /displaydns` and to clear it we use the command `ipconfig /flushdns`
- DNS usually uses UDP but it can also use TCP for messages > 512 bytes
- To configure an entry in the router's host table we use the command `ip host {hostname} {ip-address}`
- To configure the DNS server we use the command `ip name-server {DNS-IP-Address}`


### SNMP
- To configure traps to send to the NMS we use the following command:
```
R1(config)# snmp-server enable traps {trap-types}
```
- To configure contact information for a device we use the following command
```
R1(config)# snmp-server contact {contact-info}
```
- SNMP agents use `UDP:161` while SNMP managers use `UDP:162`
- To configure the NMS address + version + community string we use the following command:
```
R1(config)# snmp-server host {NMS-IP} version 2c {community-string}
```
- Several security features:
	- **Access** - can be used to define an ACL to limit the managed device to access the NMS only
	- **Context** - can be used to specify which VLANs are accessible by SNMP
	- **Views** - used to limit what info is available to the NMS
		- If no view is specified => all MIB objects can be read
		- If no view is specified => no MIB object can be written to

### Syslog
- Uses UDP port 514
- To configure the level of Syslog messages sent to an external server we use the command `logging trap {level}`
- To enable the timestamp for **Syslog** messages, we use the following command:
```
service timestamp log datetime
```
- To configure logging to an external address we use the command `logging host {ip-address}`
- To enable sequence numbers we use the command `service sequence-numbers`
- To configure Syslog monitoring to VTY lines we use the following command:
```
R1(config)# logging monitor {level}
```
- To configure synchronous logging we use the command `logging synchronous`
- To display Syslog messages for the current session when connected via **Telnet**, we use the command `R1# terminal monitor`

### FTP + TFP
- **TFTP** servers listen on UDP port 69
- **FTP** data connections use TCP port 20, while control connections use TCP port 21
- 2 modes
	- passive - the client initiates the data conneciton
	- active - the server initiates the data connections
- Several FS
	- disk - storage devices such as flash memory
	- opaque - used for internal functions
	- nvram - Non-Volatile RAM. Startup config is stored there
	- network - Represents external file systems(i.e. FTP/TFTP servers)
	- To display the file systems we use the command `show file systems`
- To copy a file from **TFTP** server to the router's flash we use the command `copy tftp: flash:`
- 3 phases of **TFTP** file transfer:
	- Connection
	- Data Transfer
	- Connection Termination


### NAT
- To configure **Dynamic NAT** we use the following command:
```
R1(config)# ip nat inside source list {ACL} pool {pool-name}
```
*Note 1*: **ACL** is used to specify the range of private IP addresses that will be translated by **NAT**.
*Note 2*: **PAT(NAT Overload)** can be configured the same as **NAT** but adding the **overload** keyword at the end of the command 

- To configure a **pool** for dynamic NAT we use the following command:
```
R1(config)# ip nat pool {pool-name} {start-ip-address} {end-ip-address} [prefix {prefix-length} | netmask {subnet-mask}]
```


### QoS
- To configure an error policy for **PoE** we use the following commands
	- `power inline police action err-disable` - the interface will be put into `err-disable state` and can be reinstated using the `shutdown` + `no shutdown` commands
	- `power inline police action log` - the interface is not `err-disabled`, but it just sends err messages to Syslog


### Security Fundamentals
- **RADIUS** uses UDP ports `1812` and `1813` while **TACACS+** uses TCP port 49


### Port Security
- *Note*: Before any configuration is done, we need to ensure that port-security is enabled on an interface with the following command
```
SW1(config-if)# switchport port-security
```
- In **shutdown** mode, Syslog/SNMP messages are generated
- Default **errdisable** recovery time is 300s. To enable the recovery we use the following command:
```
SW1(config)# errdisable recovery cause psecure-violation
```
- To configure the recovery interval we use the following command:
```
SW1(config)# errdisable recovery interval {seconds}
```
- In **protect** mode, no Syslog/SNMP messages are generated. That is the difference with the **restrict** mode
- To configure the port security violation mode we use the following command
```
SW1(config)# switchport port-security violation {shutdown | restrict | protect}
```
- Port security can't be enabled on an interface configured with `switchport mode dynamic desirable` or `switchport mode dynamic auto`
- Port security static address aging is disabled by default. It can be enabled as follows:
```
SW1(config)# switchport port-security aging static
SW1(config)# switchport port-security aging interval {minutes}
```
- To show **errdisable** recovery information we use the command `SW1# show errdisable recovery`
- Default aging type is `absolute`


### DHCP Snooping
- To enable DHCP snooping we use the following command:
```
SW1(config)# ip dhcp snooping vlan {vlan-number}
```
- To configure a DHCP snooping trusted port we use the following command
```
SW1(config-if)# ip dhcp snooping trust
```
- To show the DHCP snooping binding table we use the following command:
```
SW1# show ip dhcp snooping binding
```
- To enable errdisable recovery for DHCP rate limiting violations we use the following command:
```
SW1(config)# errdisable recovery cause dhcp-rate-limit
```
- **DHCP relay agent** listens for broadcast messages from the client and forwards them to the server encapsulating them in a unicast packet. To configure a router as a **DHCP relay agent** we use the following command:
```
Router(config-if)# ip helper-address {DHCP-Server-IP}
```
- **Option 82** - when a **DHCP relay agent** receives a DHCP request from a client it adds the client's network location to this request so that the DHCP server knows what IP address range to assign the IP from. This prevents attacks such as **DHCP Spoofing** also. 


### Dynamic ARP Inspection(DAI)
- We can use ACL to specify the IP addresses that will be inspected using the following command:
```
SW1(config)# arp access-list {ACL}
```
- To map an IP to a MAC address in an `ARP ACL` we use the following command:
```
SW1(config-arp-nacl)# permit ip host {IP} mac host {MAC}
```
- We can configure a rate limit to limit the number of ARP packets received on an interface as following:
```
SW1(config-if) ip arp inspection limit rate {packets-per-second} [burst interval {burst-interval-seconds}]
```
*Note 1*: This only limits the packets received, and has no effect on the sent packets
*Note 2*: Default rate is `15 packets/s`
- To apply an ACL to DAI we use the following command:
```
SW1(config)# ip arp inspect filter {ACL} vlan {VLAN-name}
```

### Wireless Architecture
- **Leased Lines**
	- T1 - 1.544 Mbps
	- T2 - 6.312 Mbps
	- T3 - 44.736 Mbps
	- E1 - 2.048 Mbps
	- E2 - 8.448 Mbps
	- E3 - 34.368 Mbps

- In L3 MPLS, CE and PE form routing protocol peerings, while in L2 MPLS, CE form peerings with one-another and the provider layer acts as a big switch
- Terminology:
	- **Single-Homed** - 1 connection to 1 ISP
	- **Dual-Homed** - 2 connections to  1 ISP
	- **Multi-Homed** - 1 connection to 2 ISPs
	- **Dual Multi-Homed** - 2 connections to 2 ISPs(4 connections in total)


### VRF
- To create a **VRF** we use the following command:
```
ip vrf {vrf-name}
```
- It acts in a similar way to VLAN but for Layer 3 traffic => Any device in a specific VRF can be L3 directly routed to another device in the same VRF but cannot directly reach a device in another VRF(unless **VRF Leaking** is configured)
- It allows a router to build multiple routing tables => Allows to create separate VPNs for different customers, hence the name(VPN Routing and Forwarding(VRF))
- Similarly to `802.1q` VLAN trunks, it uses `802.1q` trunks, GRE tunnels or MPLS tags to extend and tie the VRFs together
- To assign an interface to VRF we use the following command:
```
R1(config-if)# ip vrf forwarding {vrf-name}
```

![[Pasted image 20231019172054.png]]


- To configure VRF in the router above we use the following command:
```
R1(config)# ip vrf RED
R1(config-vrf)# ip vrf BLUE
R1(config-vrf)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip vrf forwarding RED
R1(config-if)# ip address 10.0.1.1 255.255.255.0
R1(config-if)# no shutdown

R1config)# interface GigabitEthernet0/1
R1(config-if)#  ip vrf forwarding BLUE
R1(config-if)# ip address 10.0.2.1 255.255.255.0
R1(config-if)# no shutdown

R1(config)# interface GigabitEthernet0/2
R1(config-if)# description Export to Flow collector
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1config-if)# no shutdown

R1(config)# interface GigabitEthernet0/3
R1(config-if)# no ip address
R1(config-if)# exit
```

### Wireless
- Several categories:
	- `802.11` - works in 2.4 GHz. Max rate is `2 Mbps`
	- `802.11n` - works in 2.4/5 GHz. Aka WiFi 4
	- `802.11ax` - 2.4/5/6 GHz. Max rate is `4*6.93 Gbps`
	- `802.11ac` - works in `5 GHz`
	- `802.11a` - works in `5 GHz`

- Depending on where **WLC** is placed in the topology we have the following 4 types:
	- **Unified** - Placed in a central position. Supports up to 6000 APs
	- **Cloud-based**  - WLC is placed in the cloud. Supports up to 3000 APs
	- **Embedded** - **WLC** is integrated within a switch. Supports up to 200 APs
	- **Mobility Express** - **WLC** integrated within an AP. Supports up to 100 APs

- **CAPWAP** tunnel between the **WLC** and the **APs**.
	- UDP port 5247 for the data traffic
	- UDP port 5246 for the control traffic

![[Pasted image 20230917084148.png]]

- *Note*: **Lightweight APs** connect to a switch via access ports while **Autonomous APs** connect via trunk ports.
- `802.1X` Enterprise Mode basically uses an AAA server(usually RADIUS) for authentication:
![[Pasted image 20231020144914.png]]

- DHCP `option 43` can be used to tell APs the IP address of their **WLC**
- **WLCs** can only support static **LAG**(Pagp or LACP are not supported )
- **CAPWAP** tunnels are formed from the Management interface
- WLC distribution system ports are the standard network ports that connect to the switched network and are used for data traffic.


### Software-Defined Networking(SDN)

- Several Cisco solutions:
	- **ACI** - Cisco solution to automate data centers automation
	- **SD-Access** - Cisco solution for campus LAN automation
	- **SD-WAN** - Cisco solution for automating WANs

- 3 switch types
	- Edge switches - connect to end-hosts
	- Control switches - use LISP to perform various control operations
	- Border switches - connect to domains outside SD-Access



### Wireless - Official Guide Material

- **Meraki** is Cisco's cloud based solution
- APs are managed centrally from a controller located in the cloud
![[Pasted image 20231106213301.png]]

**WLC Deployment Methods**
![[Pasted image 20231106214740.png]]


### Wireless Intro
- **Basic Service Area(BSA)** - AP serves as a single point of contact for every device that wants to use the BSS, i.e. it advertises the existence of the BSS so the clients can find and join it
- To advertise the existence of the **BSS**, the **AP** uses a unique **BSS** identifier(**BSSID**) that is based on the AP's own radio MAC address
![[Pasted image 20231106215802.png]]

- Basically, an **AP** is in charge of mapping a **VLAN** to an **SSID**
![[Pasted image 20231106220347.png]]

*Note*: Different SSID provide increased security not increased distribution as the coverage area is still the same
![[Pasted image 20231106220653.png]]

- **Extended Service Set** - Clients in different geographic locations are connected by a switched infrastructure called an **Extended Service Set** to provide seamless roaming between different areas
![[Pasted image 20231106220905.png]]

- We can extend the coverage area of an AP using a **Repeater** as follows:
![[Pasted image 20231106222733.png]]

**Workgroup Bridge**
- If a client only supports wired connections we can use a **Workgroup Bridge** to connect to a wireless network as follows:
![[Pasted image 20231106223025.png]]

**Outdoor Bridge**
- Two APs are configured as bridges to enable two LANs to connect as follows:
![[Pasted image 20231106223644.png]]
*Note*: The signal is strengthened since the antennas point in 1 direction only

**Mesh Topology**
- Each mesh AP usually leverage dual radios, one using a channel in one range of frequencies and the other in a different range. The BSS is maintained in 1 channel and used for clients to connect while the other channel is used for the backhaul network as follows:
![[Pasted image 20231106224432.png]]

### Channel Layouts for 2.4 vs 5 GHz
**2.4 GHz**
![[Pasted image 20231106230257.png]]

**5 GHz**
![[Pasted image 20231106230315.png]]

*Note*: In 2.4 GHz, the channels are overlapping while in 5 GHz they are not

![[Pasted image 20231106230550.png]]


### Cisco AP Modes
- **Local** - Offers one ore more functioning BSS on a specific channel. During times that is not transmitting, the AP will scan the other channels to measure the level of noise, interference, discover rogue devices etc
- **Monitor** - No transmission but the receiver is enabled to act as a dedicated sensor for IDS events, rogue access points etc
- **FlexConnect** - **AP** configured at this mode can switch between wireless and wired traffic in case the **CAPWAP** tunnel between the **AP** and **WLC** goes down
- **Rogue detector** - detects rogue devices by correlating the MAC addresses heard on the wired network with those heard over the air
- **Bridge** - AP operates in Bridge mode, i.e used to connect 2 VLANs separated by a distance
- **Flex + Bridge** - FlexConnect operation enabled on a mesh AP
- **SE-Connect** - Dedicates its radios to spectrum analysis on all channels


### Wireless Security

### Privacy and Integrity Methods

- **TKIP** was developed during the **WEP** time and it adds the following features:
	- **MIC** - Message Integrity Code
	- **Time stamp** - Prevents replay attacks
	- **Source MAC Address** - **TKIP sequence counter** provides a record of frames sent by a unique MAC address to prevent frames from being replayed
	- **Key mixing algorith** - It uses a unique 128-bit **WEP** key for each frame
	- **Long IV** - **IV** increased from 24 to 48 bits

![[Pasted image 20231107001957.png]]

- 2 Authentication modes
	- Personal - using a pre-shared key(PSK)
	- Enterprise(802.1X) - Using the EAP methods

**Personal Mode**
- a 4-way handshake between AP and client to establish the PSK
- In WPA and WPA2 this was vulnerable to MITM but was solved in WPA3 using **SAE** where both the client and AP can initiate the authentication process

![[Pasted image 20231107002400.png]]

*Note*: Autonomous AP needs a trunk link to the Switched LAN while a Lightweight AP needs an access link as shown below:
![[Pasted image 20231107005005.png]]


### WLC Interfaces
- **Management Interface** - Used for management traffic such as RADIUS user authentication, WLC-to-WLC communication, SNMP, NTP, Syslog + CAPWAP packets
- **Redundancy Interface** - Used for redundancy when 2 WLCs are used
- **Virtual Interface** - IP address facing wireless clients when WLC is relaying DHC requests, performing authentication or supporting client mobility
- **Service port interface** - used for OOB management
- **Dynamic interface** - connects a VLAN to WLAN


### QoS - Official Guide
- Several methods of classification shown below:
![[Pasted image 20231109230025.png]]
- Old version: **IPP(3 bits)** + 5 bits unused -> **Type of Service** = 1 byte in the IPv4 header
- New version: **DSCP(6 bits)** + **EN(2 bits)** -> Type of Service = 1 byte in the IPv4 header


- Another useful header exists in the 802.1Q header in the 3rd byte of the 4-byte **802.1Q** field as shown below:
![[Pasted image 20231109231026.png]]
*Note1*: This is known as the **CoS** or **PCP** field and it's 3 bits
*Note2*: This field is only included in the Ethernet frames that are transmitted in links that use the **802.1Q** trunking protocol as shown below:
![[Pasted image 20231109231337.png]]

- A summary table is given below:
![[Pasted image 20231109231655.png]]

**Assured Forwarding**
![[Pasted image 20231109234912.png]]
- **AF** defines 12 different values
- Packets with AF1x would go into 1 queue, the ones with **AF2x** would go into another etc
- In the **AF2x** queue, **AF23** is treated the worst


**Class Selector**
- Previously **IPP**, and now it's known as **DSCP**
- The conversion is $CSX$ is equal to $DSCP8X$ as follows:
![[Pasted image 20231109235236.png]]

**Guidelines for DSCP Marking Values**
- **EF(Expedited Forwarding)** - Voice traffic
- **AF4x** - Interactive video
- **AF3x** - Streaming video
- **AF2x** - High priority(low-latency) data
- **DF/CS0** - Best Effort


### Queuing
- **Class-Based Weighted Fair Queuing(CBWFQ)** guarantees a bandwidth percentage even when the link is congested as shown below:
![[Pasted image 20231110000150.png]]

**Low-Latency Queuing(LLQ)**
- **LLQ** was introduced to address the following scenario
Assume we have defined the following Queues:
![[Pasted image 20231110001025.png]]
Although the voice queue is guaranteed $50\%$, assume a voice packet arrives as soon as we move to the next queue. Then, although it is guaranteed a certain bandwidth, it then needs to wait for the classifier to go through all the other queues.

- To avoid the above issue, **LLQ** tells the scheduler to treat one ore more queues as a special priority queue as follows:
![[Pasted image 20231110001335.png]]
*Note*: The solution above may give rise to another problem. If the interface speed is X bits per second but more than X bits/s come into the priority queue, no other queues will be serviced. This is known as the **queue starvation**. This is solved by **shaping** and **policing** policies where a max percentage(instead of the min defined in **CBWQ**) of bandwidth is defined 

### Summary
1.  Use a round-robin queuing method like CBWFQ for data classes and for noninterac-
tive voice and video.
2. If faced with too little bandwidth compared to the typical amount of traffic, give data
classes that support business-critical applications much more guaranteed bandwidth
than is given to less important data classes.
3. Use a priority queue with LLQ scheduling for interactive voice and video, to achieve
low delay, jitter, and loss.
4. Put voice in a separate queue from video so that the policing function applies sepa-
rately to each.
5. Define enough bandwidth for each priority queue so that the built-in policer should
not discard any messages from the priority queues.
6. Use Call Admission Control (CAC) tools to avoid adding too much voice or video to
the network, which would trigger the policer function.


### Shaping and Policing
- **Shaping** - If packets in **LLQ** exceed the max allocated bandwidth, delay them.
- **Policing** - If packets in LLQ exceed the max allocated bandwidth, discard them or mark them differently

#### Policing

*Note*: We are allowed to configure a burst period as shown below:
![[Pasted image 20231110083903.png]]

**Key Features of Policing**
- It measures the traffic rate over time compared to the configured policing rate
- It allows for a burst of data after a period of inactivity
- It is enabled on an interface on any direction but usually ingress
- It can drop the packet or re-mark it so that the packet is a candidate for a more aggressive discarding in the other devices in the transmission chain
	- *Note*: When re-marking the packet, the scheduler does the following:
		1. It remarks the packet but lets it in the network anyway
		2. If other devices in the network are experiencing congestion, they will apply a more aggressive dropping policy to this packet
		3. Otherwise, the packet will be let through



#### Shaping

- Shapers measure the traffic rate over time for comparison to the configured shaping rate.
- Shapers allow for bursting after a period of inactivity.
- Shapers are enabled on an interface for egress (outgoing packets).
- Shapers slow down packets by queuing them and over time releasing them from the queue at the shaping rate.
- Shapers use queuing tools to create and schedule the shaping queues, which is very important for the same reasons discussed for output queuing.

**Congestion Avoidance**
- **Windowing** - When a sender uses TCP, a window size is selected to define the number of bytes that a sender can send unacknowledged before it simply stops and waits
- Note that with windowing, the transmission rate is now defined by the receiver. The faster the response(larger the window size), the greater the transmission rate



### LAN Architecture

**Small Office/Home Office**
![[Pasted image 20231111004030.png]]
- **AP** acts anonymously rather than with a **WLC**


### WAN Architecture

**Metro Ethernet Physical Design and Topology**
![[Pasted image 20231111010635.png]]
- **UNI** refers to *user network interface*, where the network term is the network managed by the SP.  All the users see is a big switch and the rest is managed by the **SP**. 
- **UNI** may refer to any of the standards below
![[Pasted image 20231111010922.png]]

![[Pasted image 20231111111831.png]]


**Ethernet Line Service(Point-to-Point)**

![[Pasted image 20231111112159.png]]

- In the above topology the routers R1 and R2 would:
	- Use physical Ethernet interfaces
	- Configure IP addresses in the same subnet
	- Their routing protocols would become neighbors and exchange routes
- Another common E-Line topology is using different lines different sites as follows:
![[Pasted image 20231111113211.png]]

**Ethernet LAN Service(Full Mesh)**
![[Pasted image 20231111114458.png]]
*Note*: All the routers are in the same subnet in the WAN

**Ethernet Tree Service(Hub and Spoke/Partial Mesh/Point-to-Multipoint)**
![[Pasted image 20231111114717.png]]



### Multiprotocol Label Switching(MPLS)
![[Pasted image 20231111125314.png]]
*Note*: In the above topology, an access link refers to the link between a CE and PE. Since **MPLS** adds its own header to the packet and discards incoming data-link headers => it supports a lot of data-link protocols as shown below
![[Pasted image 20231111125635.png]]

**L3 with MPLS VPN**
![[Pasted image 20231111130215.png]]
In the above topology the following takes place:
- A CE router becomes neighbor with the PE router on the other end of the access link
- A CE router does not become neighbor with other CE routers
- However, the CE routers all learn how to reach one another via the PE routers. This happens via route distribution as shown below:
![[Pasted image 20231111130814.png]]

**DSL**
![[Pasted image 20231111132435.png]]

**Cable Internet**
![[Pasted image 20231111132522.png]]


### VPN
![[Pasted image 20231111133104.png]]

**Site-to-Site VPN with IPSec**
![[Pasted image 20231111145353.png]]



### Cloud Architecture
![[Pasted image 20231111151927.png]]

**Data Center Network**
![[Pasted image 20231111152442.png]]
*Note*: **ToR** = **Top of the Rack switches**

**Virtualized Data Center**

- The OS is decoupled from the hardware on which it runs, so that the OS, as a VM, can run on any server in a data center that has enough resources to run the VM.
- The virtualization software can automatically start and move the VM between servers in the data center.
- Data center networking includes virtual switches and virtual NICs within each host (server).
- Data center networking can be programmed by the virtualization software, allowing new VMs to be configured, started, moved as needed, and stopped, with the networking details configured automatically.


**Cloud**
- **Infrastructure as a Service(IaaS)**
![[Pasted image 20231111163359.png]]
*Note*: Infrastructure(OS + Storage + CPU + RAM + Network) is managed by the cloud provider

- **Software as a Service(SaaS)**
![[Pasted image 20231111163537.png]]
*Note*: An example could be Microsoft Exchange email server software as a service

**Platform as a Service(PaaS)**
![[Pasted image 20231111163855.png]]

We have 2 options to connect to the public cloud:
- **via Internet**
	- Pros
		- Agility - can get started immediately without having to setup the networking
		- Flexibility - flexibility when switching to another provider
		- Distributed users
	- Cons
		- Security - Liable to MITM attacks
		- No QoS
		- No SLA - ISP provides no SLA agreement over the performance


**Private WAN and VPN Access to Public Cloud**
![[Pasted image 20231111165140.png]]

![[Pasted image 20231111170056.png]]


### Software-Defined Networking(SDN)
![[Pasted image 20231111171936.png]]
**Control Plane** has the following functionalities:
- Routing Protocols
- IPv4 ARP
- STP
- Switch MAC Learning

**Data Plane** devices then make all decisions, for example where to forward the traffic, by using the above info from the **Control Plane** -> **Separation of Concerns**. The data plane is computationally expensive so it happens in an *Application Specific Integrated Circuit(ASIC)* and the MAC address table is stored in a special type of storage called *TCAM(Ternary Content Addressable Memory)* as shown below:
![[Pasted image 20231111203324.png]]

**ACI**
- Cisco's SDN solution for data centers
- The centralized controller is called **Application Policy Infrastructure Controller(APIC)**
- Uses Intent-Based Networking
![[Pasted image 20231111213325.png]]


**Cisco Software-Defined Access(SDA)**
- **DNA Center** is the controller for SDA networks
- Several components:
	- **Overlay** - The mechanisms to create VXLAN tunnels between SDA switches which are used to transport traffic from one fabric endpoint to another
	- **Underlay** - The network of devices and connections
	- **Fabric** - **Overlay** + **Underlay**
- The SDA underlay configuration requires us to think and choose different SDA roles filled by each device. The roles can be as follows:
	- **Fabric edge node** - connects to endpoint devices
	- **Fabric border node** - connects to devices that do not belong to the SDA underlay, for example switches that connect to WAN routers or to an ACI data center
	- **Fabric control node** - A switch that performs several control functions for the underlay(LISP) => it requires more CPU and memory than the other switches
- The traditional access layer design is given below:
![[Pasted image 20231112000227.png]]


- DNA-Center uses a *routed access layer* as follows:
![[Pasted image 20231112000630.png]]
The above topology has the following properties:
- All nodes are L3-switches => no STP or HSRP is needed
- Routing protocol used is IS-IS

**SDA Overlay**
- VXLAN encapsulation is used The underlay uses a separate IP address space compared to the rest of the enterprise, while overlay uses the enterprise's address space
![[Pasted image 20231112001747.png]]

**LISP for Overlay Discovery and Location(Control Plane)**
- Discovery Process in SDA works as follows:
- Fabric edge nodes—SDA nodes that connect to the edge of the SDA fabric—learn the location of possible endpoints using traditional means, based on their MAC address, individual IP address, and by subnet, identifying each endpoint with an endpoint identifier (EID).
- The fabric edge nodes register the fact that the node can reach a given endpoint (EID) into a database called the LISP map server.
- The LISP map server keeps the list of endpoint identifiers (EIDs) and matching routing locators (RLOCs) (which identify the fabric edge node that can reach the EID).
- In the future, when the fabric data plane needs to forward a message, it will look for and find the destination in the LISP map server's database.
![[Pasted image 20231112005103.png]]

- When an ingress fabric edge node(ingress tunnel router) receives a packet, it need to know how to reach that end host. It uses the following procedure:
	- It sends a message to the LISP map server asking if it knows how to reach it
	- If it does, it forwards the packet to the node listed as the RLOC
	- For example, assume in the image above that the RLOC is SW3. Then SW1 needs to encapsulate the packet before it forwards it to SW3 via VXLAN as shown below:
![[Pasted image 20231112005543.png]]

**SDA Security Based on User Groups**

![[Pasted image 20231112010731.png]]
- The policies above create groups identified by Scalable Group Tags(SGT) and set the relation of each group with the other groups as follows:
![[Pasted image 20231112010937.png]]
- Each user is mapped to their corresponding groups by the **Identity Service Engine(ISE)**
- If **DNA-Center** identifies a permit rule bw the source and destination, it allows the edge node to create the VXLAN tunnel where the src/dst SGTs are included in the header as follows:
![[Pasted image 20231112011222.png]]

- **Cisco PI** is a tranditional Cisco Management Platform but it doesn't support **SDA**


### IPv6 - Official Guide

**IPv6 Header**
![[Pasted image 20231113202946.png]]

![[Pasted image 20231113211710.png]]

- An IPv6 packet has the following structure:
![[Pasted image 20231113212541.png]]

- An example of a global unicast IPv6 address is given below:
![[Pasted image 20231113213023.png]]
*Note*: The global routing prefix(Prefix ID) is not unique. It is unique in combination with the Subnet ID => this allows for $2^{16}$ IPv6 addresses

**Unique Local Unicast Addresses**
![[Pasted image 20231113214426.png]]
*Note*: The prefix reserved for unique local unicast addresses is $FC00::/7$ however RFC 4193 requires the 8th bit to be set to 1 => in practice it actually is $FD00::/8$

*Note*1: If we assign a global unicast address to an interface, it is also implicitly assigned a link-local address using **EUI-64**.

*Note2*:  IPv6 is disabled by default. To enable it we use the following commands:
```
ipv6 unicast-routing
```
on the global level and
```
ipv6 address
```
on an interface level


**Link-Local Addresses**
- Are used by some overhead protocols(i.e. NDP)
- When using the `show ipv6 route`, the address listed is the link-local address. Same goes for the default gateway as shown below:
![[Pasted image 20231113224035.png]]
- Packets sent to a link-local address do not leave the local data link. They are also **automatically generated**
- Mostly used for overhead protocols + as the **next hop address in IPv6 routes**
- It has the following format:
![[Pasted image 20231113224406.png]]

*Note*: If an interface only needs a link-local address and no global unicast address, we use the command `ipv6 enable`

**Solicited-Node Multicast Addresses**
![[Pasted image 20231113230808.png]]

**Anycast Addresses**
- Two or more routers are configured with the same IPv6 address => We can just route a packet to that address and the routing protocol will pick the best address to reach it
- It is configured as follows:
```
R1(config-if)# ipv6 address {IPv6-address} anycast
```


**Summary**
![[Pasted image 20231113232015.png]]

![[Pasted image 20231114141343.png]]


### WLC vs AP

**AP**
- encryption
- beacon/probe response
- sending/receiving 802.11 traffic
- packet prioritization

**WLC**
- authentication
- security management
- RF management
- Lightweight AP configuration management
- key exchange
- data encapsulation
- client association requests
- client load balancing


### OSPF

- Several message types:
	1. Hello
	2. DBD
	3. LSR
	4. LSU
	5. LSAck

- To configure the OSPF reference we use the following command:
```
R1(config-router)# auto-cost reference bandwidth {Mbps}
```


# Ethernet Header

![[Pasted image 20231209132725.png]]


### enable secret vs enable password

- **Enable password**
The syntax is as follows:
```
enable password [level level] {password | [encryption-type] encrypted-password}
```
*Note*: The only allowed encryption type is 7, which indicates that the password was previously encrypted by a Cisco router

- **Enable secret**
The syntax is as follows:
```
enable secret [level level] { [0] unencrypted-password | encryption-type encrypted-password}
```

Instead of 0(cleartext) we can use any of the following:
- 4 - For SHA-256 hashed text
- 5 - For MD5 hashed text
- 8 - Specifies a Password-Based Key Derivation Function 2 (PBKDF2) with SHA-256 hashed secret
- 9 - Scrypt hashed secret

*Note*: More info can be found here: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/security/d1/sec-d1-cr-book/sec-cr-e1.html#wp3884449514

### Network Types

- Broadcast
	- DR and BDR election is performed
	- Hello=10s Dead=40s
	- No manual configuration of neighbours 
	- Enabled on Ethernet + FDDI interfaces
- Non-broadcast
	- DR and BDR election is performed
	- Hello=30s Dead=40s
	- Enabled on Frame Relay + X.25
	- Manual configuration of neighbors with the `neighbor` command is required
- Point-to-point
	- DR and BDR election is not performed
	- Hello=10s Dead=40s
	- Enabled on PPP and HDLC
	- Manual configuration of neighbors with the `neighbor` command is not required
- Point-to-multipoint
	- DR and BDR election is not performed
	- Hello=30s Dead=120s
	- Manual configuration of neighbors is not required with the `neighbor` command
- Point-to-multipoint non-broadcast neighbors
	- DR and BDR election is not performed
	- Hello=30s Dead=120s
	- Manual configuration of neighbors with the `neighbor` command is required

*Note*: The command we use in all the cases to set the network type is:

```
ip ospf network {network-type} i.e
ip ospf network point-to-multipoint non-broadcast
```


### Virtual and Dynamic Interfaces in Wireless

- **Virtual interface** 
	- Provide an IP address that is the same across multiple controllers => enable seamless roaming. 
	- When web authorization has been enabled the user is redirected to the IP address of the virtual interface. 
	- If DHCP relay has been enabled on the controller, the vIP will serve as the IP address of the DHCP server on the wireless stations.
- **Dynamic Interfaces**
	- Up to 512
	- Used for client data
	- Act in the same way as VLANs, i.e. used to segment user traffic

### WLAN Configuration

4 steps to configure a WLAN:
- Select the WLAN type. There are 3 WLAN types:
	- Normal WLAN
	- Guest WLAN
	- Remote WLAN - Used for wired ports on the WLC
- Enter a 32-character or less profile name in the **Profile Name** field
- Enter a 32-character or less **Service Set Identifier(SSID)**
- Choose a WLAN ID
	- A max of 512 WLANs are supported but only 16 can be actively configured

### CCKM
- Cisco proprietary fast-rekeying method that enables clients to roam from one AP to another without requiring the intervention of the WLC => making the transition seamless


### Southbound Interfaces

- **NETCONF** - uses XML for formatting + protocol messages and RPC calls to networks devices
- **OpFlex** - declarative language
- **OpenFlow** - imperative language
- **onePK**
	- Cisco-proprietary API 
	- It uses Java, C or Python to configure network devices
	- Can use either SSH or TLS

### NTP Authentication

- Client side
```
ntp authenticate
ntp authentication-key {key-number} md5 {key}
ntp trusted-key {key-number}
ntp server {server-ip} key {key-number}
```

- Server side
```
ntp authenticate
ntp authentication-key {key-number} md5 {key}
```

*Note*: NTP uses **UDP port 123**


### L2 Security
![[Pasted image 20231212170526.png]]

### PPOE
- When a PD attempts to draw more current than the cutoff current from the PSE, the port will be placed into an errdisabled state.
- In `errdisabled` state, we need to do `shutdown` + `no shutdown` to restart the interface
- To enable automatic recovery we use the following command:
```
errdisable recovery cause inline-power
```

*Note*: The default recovery time is `300s` but can be manually set to a value bw `30s` and `86400s`


- **Multicast MAC addresses** - The first 24 bits are in the range `01-00-5E-00-00-00` to `01-00-5E-7F-FF-FF`


### AP Configuration

```
show ap config global
```
- The above command displays the Syslog server settings for every AP joined to the AP

```
show ap config general {ap-name}
```
- Displays general information about the AP such as: DNS, default gateway, MAC address, IP, Cisco AP Name
```
show ap core-dump {ap-name}
```
- Displays the memory core dump of the AP

```
show ap crash-file {ap-name}
```
- Displays a list of crash dump files of the AP

### CDP IP Phone - Catalyst Switch Output
![[Pasted image 20231213001058.png]]

### VTP
![[Pasted image 20231213002519.png]]


### HSRP States

- **Init** - HSRP does not run yet
- **Learn** - The router has not yet seen a Hello message from the active router so it doesn't know the vIP yet
- **Listen**
	- The router knows the vIP
	- It is neither the standby or the active router
	- It listens for Hello messages from either of these routers
- **Speak**
	- The router sends hello messages and actively participates in the election of the active/standby router
- Standby
- Active

*Note*: Hello and dead timers are 3 and 10 seconds respectively


### IP Protocol Numbers
- ICMP - 1
- TCP - 6
- **UDP** - 17
- IPv6 - 41
- GRE - 47
- ESP - 50
- AH - 51
- 88 - EIGRP
- 89 - OSPF
- 112 - VRRP