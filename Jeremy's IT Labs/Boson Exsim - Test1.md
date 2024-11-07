
### Q32
![[Pasted image 20231011004704.png]]

### Q34
![[Pasted image 20231011005025.png]]

### Q37
![[Pasted image 20231011005526.png]]

### Q43
![[Pasted image 20231011005723.png]]

### Q44
![[Pasted image 20231011005905.png]]

### Q47
![[Pasted image 20231011010127.png]]

### Question 60
![[Pasted image 20231011215822.png]]

**Answer**: The path with the longest prefix will be selected. However note that the subnet `10.20.0.0/28` contains the IP range `10.20.0.0` - `10.20.0.15` so `10.20.0.17` is not actually included in this subnet => The `10.20.0.0/24` path will be selected => the next hop will be `192.168.10.3`.
**Reminder**: AD of different routing protocols:
![[Pasted image 20231011220341.png]]

### Q62
![[Pasted image 20231011222022.png]]

- HostA cannot ping HostB and HostC cannot ping HostD since each pair resides in different VLANs on the same switch. The correct answer is that HostD can ping HostA
**Explanation**: When HostD pings HostA, SwitchB will recognize that HostD belongs to VLAN 111 which is the native VLAN in SwitchB => The traffic will be sent untagged. When SwitchA receives untagged traffic, it will assume that the traffic belongs to the native VLAN which is configured as VLAN 11 in SwitchA. Since HostA belongs to VLAN 11 => HostA will received traffic from HostD.

### Q65
![[Pasted image 20231011222612.png]]
**Explanation**: **MST** can configure multiple spanning trees for the same VLAN => multiple traffic-forwarding paths => failover capabilities

### Q66
![[Pasted image 20231011223027.png]]
**Explanation**: The steps to enable **SSH** in a router are as follows:
1. Configure the router with a hostname other than Router
2. Configure the router with a domain name using the `ip domain-name` command
3. Generate an RSA key pair for the router by issuing the `crypto key generate rsa` command
4. Configure the VTY lines to use SSH by issuing the `transport input ssh command` from the line configure mode

### Q68
![[Pasted image 20231011223510.png]]
**Explanation**:
In this scenario, the port will restart and a log message will appear on the console when an
attached powered device (PD attempts to draw more than its allocated amount of power from the configured interface. Because sending an electrical current to a device that does not support
Power over Ethernet (Po) could potentially damage the receiving device, power-sourcing
equipment (PSE), such as a PoE-capable switch, will first apply a small voltage to a PoE-enabled
port to determine whether a PD is attached to the port. The Institute of Electrical and Electronics
Engineers (IEEE Pot standards require a PD to provide a measurable resistance of approximately
25 kilo Ohms (kohms) when it is probed by a PSE. If the PSE detects a PD, the PSE can then send a signal with a higher voltage to determine the class of the PD. When an IEEE standards-compliant PD receives this higher-voltage signal from a PSE, its response will inform the PSE about the PD's power requirements. The PSE will categorize the PD into an appropriate class, if possible, and will then guarantee a minimum amount of power relative to the class of the PD. If the PSE cannot identify the appropriate class for a PD, the PD will be categorized into the default class and will receive the default amount of power.
Power policing is a Cisco feature that enables a switch to monitor the current draw of connected
devices and to take action if the draw exceeds the amount allocated to the PD in accordance with its negotiated power class. The allocated maximum power draw is referred to as the cutoff power value. You can issue the power inline police command from interface configuration mode to enable power policing with the default settings. When power policing is enabled with the default settings for a PoE-capable interface, the interface will enter an error-disabled state, effectively shutting down the port, when an attached PD attempts to draw more than the cutoff power from the configured interface. A log message describing the event will also be sent to the console. An interface in an error-disabled state will remain shut down until it is manually reset by an administrator issuing the shutdown and no shutdown commands in sequence for the interface) or until the error-disabled auto recovery mechanism timer expires and the port is automatically reset. Although error-disable detection for inline power is enabled by default on Cisco PoE-capable switches, error-disable auto recovery for inline power is not enabled by default. Therefore, a port that has been placed into an error-disabled state by an inline power event will not automatically reset by default. You can issue the errdisable recovery cause inline-power command from global configuration mode to enable error-disable auto recovery for inline power. You can issue the power inline police action log command to change the default power policing behavior. When the log action is configured, a PoE-enabled interface will restart and send a log message to the console when an attached PD attempts to draw more than the cutoff power from the configured interface. This will typically cause the PD to reboot and to renegotiate its power requirements

### Q70
![[Pasted image 20231011224318.png]]

### Q71
- **WLC** commands to display information about the connected **APs**
![[Pasted image 20231011224412.png]]


### Q73
![[Pasted image 20231011230403.png]]
**Explanation**: There are five OSPF network types:
- Broadcast
- Nonbroadcast
- Point-to-point
- Point-to-multipoint broadcast
- Point-to-multipoint nonbroadcast

- Brodcast networks
	- Enabled by default on Ethernet and FDDI connections
	- DR and BDR elections are performed 
	- Multicast updates are sent => manual configuration of neighbors with `ip ospf neighbor` command is not required
	- Hello timer = 10s
	- Dead timer = 40s
	- Enabled with the command `ip ospf network broadcast`
- Nonbroadcast networks
	- Enabled by default on **Frame Relay** and **X.25** connections
	- Multicasts are not allowed => manual configuration of neighbour routes with `ip ospf neighbor` command is required
	- Hello timer = 30s
	- Dead timer = 120s
	- Enabled with the command `ip ospf network non-broadcast`
- Point-to-point
	- Enabled by default on **High-Level Data Control(HDLC)** and **Point-to-Point Protocol(PPP)** serial interfaces
	- DR and BDR elections are not performed
	- Multicast updates are sent => manual config of neighbour routes not required
	- Hello timer = 30s
	- Dead timer = 120 s
	- Configured with the command `ip ospf network point-to-point`
- Point-to-multipoint
	- DR and BDR elections are not performed
	- Multicast updates sent => manual config of neighbor routes is not required
	- Hello timer = 30s
	- Dead timer = 120s
	- Enabled with the command `ip ospf network point-to-multipoint`
- Point-to-multipoint nonbroadcast
	- DR and BDR elections are not performed
	- Multicast not allowed => manual config of neighbor routes is required
	- Hello timer = 30s
	- Dead timer = 120s
	- Enabled with the command `ip ospf network point-to-multipoint non-broadcast`


### Q75

- **ARP Poisoning Attack**
	- The attacker sends a **Gratuitous ARP** message to a host
	- The attacker's MAC address is associated with the IP address of a valid host on the network => All the packets intended for the valid IP go through the attacker
- **VLAN Hopping Attack**
	- The attacker double tags `802.1Q` frames
	- Can be prevented by disabling the **DTP** protocol, by changing the native VLAN and configure user-facing ports as access ports
- **DHCP Spoofing Attack**
	- An attacker installs a DHCP server on the network in an attempt to intercept DHCP requests
	- The rogue DHCP server responds to the DHCP requests with its IP address as the default gateway
	- Prevented by DHCP snooping

### Q76
![[Pasted image 20231011235858.png]]

**Explanation**: It should be assigned a `/30` since a point-to-point interface only needs 2 available IP addresses. `192.168.0.130` is not the start of a `/30` subnet.


### Q77
![[Pasted image 20231012000229.png]]
**Explanation**: The `enable secret` command is followed by the encryption level which is one of the following:
- 0 - password in plaintext
- 4 - SHA256 encrypted string
- 5 - MD5 encrypted string
- 8 - Password-Based Key Derivation Function 2 with SHA256 hashed secret
- 9 - scrypt hashed secret


### Q83
![[Pasted image 20231012000620.png]]
**Explanation**: If we specify a static route in which the the destination network, outbound interface and next-hop IPv6 address are all configured directly
3 other route types:
- **Directly attached static route** - specifies the destination IPv6 network and the outbound interface, i.e `ipv6 route 2001:db8:a::/32 fe0/1`
- **Recursive static route** - Specifies the destination IPv6 network and the next-hop address, i.e `ipv6 route 2001:db8:a::/32 2001:db8:a::1`


### Q86
![[Pasted image 20231012001601.png]]

- Routing is done in the control planes, **SNMP** and **Syslog** are used for management traffic.


### Q87
![[Pasted image 20231012001913.png]]

### Q88
![[Pasted image 20231012002040.png]]
**Explanation**: **FlexConnect ACLs** are rules that permit or deny traffic. They are defined in the AP VLAN interface if the lightweight AP is operating in **FlexConnect** mode. They are supported on the native VLAN if the VLAN config is not inherited from a **FlexConnect** group..
- **FlexConnect ACLs** are applied per AP + VLAN not per AP + interface.

### Q89
![[Pasted image 20231012004809.png]]

### Q91
![[Pasted image 20231012005726.png]]
**Explanation**:
- **802.11k** - provides assisted roaming in a wireless network => a wireless client can request a list of known APs that might be potential candidates for transition, i.e. are in the same wireless band with which the client is associated
- **802.11r** - provides support for fast transition roaming(not assisted roaming). The client and AP perform the **Pairwise Master Key** calculation in advance. These pre-calculated PMK keys used after the client sends its reassociation request to the new AP and the AP responds with its reassociation response => This enables the client to move to a new AP without the need to re-authenticate. Supports either 802.1x or PSK authentication
- **802.11w** - provides management frame protection(**MFP**), which prevents the management frames from being compromised by a malicious user. Management frames are used to manage the connection between an AP and a wireless client and includes the following
	- beacon
	- probe requests + probe responses
	- (re)association requests + (re)association responses
 *Note*: These frames are not encrypted and are used by attacks in **DoS** attacks
- **802.11v** - provides network assisted power savings => a client can remain in standby/sleep state longer to improve the battery life. 802.11v provides a **DIrected Multicast Service(DMS)** which enables a client to request that certain multicast messages be delivered as unicast messages => some multicast messages are ignored and unicast messages are received at a greater rate.


### Q93
![[Pasted image 20231012124555.png]]
**Explanation**: **IPSec** operates in two modes:
- **transport mode** - only the payload is encrypted not the header. Less compatible with NAT than the tunnel mode.
- **tunnel mode** - the whole packet is encrypted including the header. The encrypted packet is then encapsulated in a new packet that includes a new IP header => requires additional headers. L3 forwarding devices are not capable of decrypting the encrypted packet => a new IP header encapsulates the encrypted packet for routing.


### Q94
![[Pasted image 20231012135113.png]]
**Explanation**:
- **802.1X** - requires an external RADIUS server for AAA
- **CCKM** - Cisco Centralized Key Management method. Cisco proprietary fast-rekeying method that enables a wireless client to roam from one AP to another without requiring intervention from WLC.
- **802.1X + CCKM** - enables wireless clients to roam between different APs without needing to re-authenticate to the RADIUS server.
- **PSK** - correct answer. This supports an ASCII passphrase bw 8-63 characters or a key of 64 hex values.

### Q96
![[Pasted image 20231012140952.png]]
**Explanation**: Terms to memorize:
- **giant** - A frame that exceeds 1518 bytes(MTU including the Ethernet header + CRC check) and has a bad Frame Check Sequence(FCS)
- **jumbo** - A frame that is up to 9216 bytes in size
- **baby giant** - A frame that is up to 1600 bytes in size
- **runt** - A frame that is less than 64 bytes and has a bad FCS


### Q97
![[Pasted image 20231012141426.png]]
**Explanation**:
- **Northbound interfaces** - enables an SDN controller to communicate with the applications in the application plane
	- **REST**
	- **OSGi** 
- **Southbound interfaces** - enables the SDN controller to communicate with the data plane
	- **NETCONF** - Uses XML + RPCs to configure network devices
	- **OpFlex** - Issues generic instructions which allow the network devices to make implementation decisions
	- **OpenFlow** - Sends detailed instructions
	- **OnePK** - Cisco proprietary that uses TLS to encrypt communication with the network devices in the data plane


### Q98
![[Pasted image 20231012142520.png]]
**Explanation**: 
- **NTP broadcast client** - listens on the configured interface for NTP broadcasts from any NTP server
- **static client mode** - listens for NTP broadcasts in the configured interface from the server configured with the command `ntp server {server-IP}`
- **authentication** - enabled on the client with the following set of commands:
```
ntp authenticate
ntp authentication-key key-number md5 key
ntp trust-key key-number
ntp server ip-address key key-number
```
enabled on the server with the following commands:
```
nt authenticate
ntp authentication-key key-number md5 key
```

- **server mode** - enabled with the command `ntp master {stratum}` where stratum is a value from 1-15.
- **symmetric active** - enabled with the command `ntp peer {peer-ip-address}`. NTP peers have the same stratum values and are used as a fallback in case the communication with the server fails.
**Example**: Assume we have two servers at stratum 1 and two routers(R1,R2) at stratum 2 as follows:
![[Pasted image 20231012143710.png]]
Then we configure them as follows:
```
R1(config)# ntp server 10.10.10.1
R1(config)# ntp peer 192.168.20.1

R2(config)# ntp server 10.10.20.1
R2(config)# ntp pseer 192.168.10.1
```

### Q99
![[Pasted image 20231012143848.png]]

### Q101
![[Pasted image 20231012144101.png]]
**Explanation**: Info provided from IP phone to a Catalyst switch using CDP is as following:
![[Pasted image 20231012144503.png]]
*Note*: The switch is providing `6.3W` of power to the IP phone


### Q102
![[Pasted image 20231012144937.png]]
- **Explanation**: We do not need to configure the NTP packet source IP address as long as the address of the outgoing interface can be used as destination for NTP replies. If that's not the case, we can configure an alternate source address using the command `ntp source {interface}`


### Q104
![[Pasted image 20231012145722.png]]
**Explanation**:
- An SFP module is a hot-pluggable device that enables a switch, router or other device to accept connections from Fibre Channel or Gigabit Ethernet cables. Cisco devices do not support the use of third-party SFP module.


### Q105
![[Pasted image 20231012145854.png]]

### Q92
We have the following network topology:
![[Pasted image 20231012150051.png]]
We have the above network and the following tasks:
![[Pasted image 20231012151348.png]]
**Explanation**: We first use the command `show interface status` to check which of the ports are active access ports:
![[Pasted image 20231012152321.png]]

![[Pasted image 20231012152923.png]]

![[Pasted image 20231012153008.png]]

![[Pasted image 20231012153022.png]]

![[Pasted image 20231012153044.png]]

![[Pasted image 20231012153112.png]]

![[Pasted image 20231012153127.png]]


### Q6
![[Pasted image 20231012154001.png]]
**Explanation**: 3 modes of VLAN Trunking Protocol(VTP):
- **server mode**
	- stores configuration information in NVRAM so if the switch is powered off the VLAN config will be retained
- **client mode**
	- Cannot create/modify/delete VLANs
	- Can send its VLAN config information to other switches
	- In version 1 and 2 clients do not store VLAN info in NVRAM
	- In version 3 they store config info in VLAN
- **transparent mode**
	- Can forward VTP advertisements to other switches
	- Stores VLAN info in NVRAM
	- Forward a VTP advertisement only if the VTP domain and version number on the switch match that of VTP advertisement

### Q8
![[Pasted image 20231012161328.png]]
**Explanation**: NAT is configured on the global level, not interface level.

### Q12
![[Pasted image 20231012161550.png]]
**Explanation**: RFC 1918 defines 3 classes of private addresses:
- **Class A** - 10.0.0.0 - 10.255.255.255
- **Class B** - 172.16.0.0 - 172.31.255.255
- **Class C** - 192.168.0.0 - 192.168.255.255


### Q13
![[Pasted image 20231012162249.png]]
**Explanation**: By default an IP phone is configured to override the CoS priority value  assigned by the host and reassign the lowest CoS priority value of 0 to the data packets. To prevent this we use the command `switchport priority extend trust`, which will instruct the phone to just forward the data packets to the switch without changes to the CoS value.
`mls qos trust cos` - moves the trust boundary from switch to the IP phone, which lets the switch accept the IP phone voice traffic as having come from a trusted source.

**Appendix: Difference between CoS and QoS**
- CoS operates at L2 while QoS operates at L3
- CoS defines priority levels while QoS manipulates traffic according to these priority levels
- Quality of Service is the capability of a network to provide better service to the selected network traffic by giving priority including dedicated bandwidth, controlled jitter and latency.
- Where Class of Service is the value given in Layer2 frame to DSCP to  differentiate the traffic to give differential QoS treatment
- CoS is a form of QoS limited to L2 Ethernet and it uses 3-bits of the 802.1Q tag to differentiate traffic => no trunking implies no CoS.
- DSCP is the most commonly used value for QoS at L3 and is found in 6-bits of the IP header => 64 values but only 14 are used. The values are of the form AFxy where x is 1-4 and refers to the precedence and y is 1-3 and refers to the drop probability
- *Note*: To prevent a device for setting the QoS bits to 0 we have to use the command `mls qos trust cos` for CoS and `mls qos trust dscp` for DSCP.

### Q14
![[Pasted image 20231012164859.png]]
**Explanation**: ARP table maps an IP address to the MAC address while the CAM table maps the MAC address to the physical port.
*Note*: The **FIB** table contains a subset of the entries in the routing table but it is designed for faster packet forwarding. 

### Q20
![[Pasted image 20231012171127.png]]
**Explanation**:
- The default max number of equal cost paths that can be inserted in the routing table is 4. We can change this by using the `maximum-paths {max-path-number}` command.
- The `variance` command is used to determine whether the `EIGRP` feasible successors can be used for unequal cost load balancing.


### Q21
![[Pasted image 20231012171350.png]]

### Q24
![[Pasted image 20231012171510.png]]
Explained above in **Q73**.

### Q26
![[Pasted image 20231012171814.png]]
**Explanation**: WLC contains 2 main types of interfaces:
- **dynamic interfaces** - WLC can contain up to 512 dynamic interfaces. Function similarly to VLANs, i.e. are used to segment the traffic in a WLC
- **static interfaces** - include several types of interfaces
	- management interface - used for L2 communications between the APs and the controller. Also used to communicate with other WLCs in the network
	- service port interface - used for out-of-band maintenance purposes on WLC. It's a physical interface on WLC which can be used to recover the WLC in case of failure
	- console port - asynchronous interface accessed with a serial cable or a USB to RJ-45 adapter

### Q27
![[Pasted image 20231012172418.png]]
**Explanation**: An **Identity Service Engine(ISE)** is used for the appropriate QoS-level and VLAN-Tag attribute for each user. 
*Note*: The Radius server can be used to modify/terminate an already authenticated session only if its CoA feature is enabled.

**Q15**
![[Pasted image 20231013000534.png]]
**Explanation**: `172.16.17.20` is the network address so we cannot assign it to a router => We use the 2 IPs in the `172.16.17.16/30` network, i.e. `172.16.17.17` and `172.16.17.18`. `172.16.17.16` is the network address and `172.16.17.19` is the broadcast address.

### Q17
![[Pasted image 20231013001715.png]]
