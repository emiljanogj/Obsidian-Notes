### Q1
![[Pasted image 20231012232922.png]]
**Explanation**:
- **CAT 5e** - 1000 Mbps
- **CAT 5** - 100 Mbps
- **CAT 6** - 10 Gbps
- **CAT 3** - 10 Mbps


### Q5
![[Pasted image 20231012233544.png]]
**Explanation**: The format of an Ethernet header is as follows:
![[Pasted image 20231012233625.png]]

### Q7
![[Pasted image 20231012234103.png]]
**Explanation**: If we used `enable secret 5`, then we would have to provide the MD5 hash of the password.

### Q10
![[Pasted image 20231012235330.png]]
**Explanation**: 
- **EPG** - Endpoint Groups
- **APIC** - Implements the network application policies. Also connects to the Leaf nodes only. 


### Q17
![[Pasted image 20231013002447.png]]
**Explanation**: There are six HSRP states: init, learn, listen, speak, standby and active. Hello messages are sent by routers in the active, standby and speak states. HSRP routers in the speak state participate in the election of active and standby routers. After the election, all routers are placed in the listen state, i.e. they listen for Hello messages. However, only routers in the standby state monitor hello messages from the active router.


### Q19
![[Pasted image 20231013002952.png]]

### Q20
![[Pasted image 20231013003225.png]]
**Explanation**: IPSec with GRE works in the following 4 steps:
1. The sending device combines a session key(encryption key) with the data to be transported over a tunnel. It then uses the session to encrypt both the data and this new key that it formed.
2. The sending device encapsulates the encrypted data and session key into a packet with a VPN header and new IP header which contain source/dest
3. New packet is sent
4. Receiving device decrypts it using the session key


### Q21
![[Pasted image 20231013135232.png]]

![[Pasted image 20231013135250.png]]
**Explanation**: The first command overloads the pools => it will use the addresses in the range `172.16.1.2` - `172.16.1.21`. This is not the minimal amount of IP addresses since the 2nd command will only use the serial interface's IP. The last one is wrong(Note the outside keyword).

### Q22
![[Pasted image 20231013135544.png]]
**Explanation**: The other networks are set to `30` and `120` for the hello and dead timer intervals respectively.

### Q23
![[Pasted image 20231013135743.png]]

### Q25
![[Pasted image 20231013141425.png]]
**Explanation**: Beacon frames contain the following information about the wireless networks:
- SSID
- timestamp 
- authentication
- data transfer speed
- vendor-specific proprietary info

**Association requests** is a request sent by the wireless client to the AP to request access to the network after the client has been authenticated by the AP or an authentication server.


### Q27
![[Pasted image 20231013154052.png]]
**Explanation**:
- `Please define a hostname other than Router` - This is a warning we will receive when we issue the command `crypto key generate rsa`
- `Please define a doman first` - This is a warning we would receive if we issued the command `crypto key generate rsa` and we had set a valid hostname(i.e. hostname other than Router)
- `Please enable SSH as a transport mode` - Not a valid Cisco router warning


### Q30
![[Pasted image 20231013154620.png]]**Explanation**:
- A is not correct since the **Local mode** is the default operating mode for lightweight AP. A lightweight AP operating in local mode is capable of providing multiple basic server sets(BSS) on a single channel. AP and WLC manage connectivity to the same WLAN. Latency between AP and WLC cannot exceed `20ms` => An administrator is advised to configure the physical network interface on a local mode AP to operate in access mode and enable PortFast on it.
- **FlexConnect** - Enables a failsafe if the **CAPWAP** connection between the AP and WLC fails. Enables a lightweight AP to switch traffic between an SSID and a VLAN
- **Bridge Mode** - Operates as a dedicated connection between 2 networks. The connection is point-to-point or point-to-multipoint.


### Q34
![[Pasted image 20231013155923.png]]

### Q37
![[Pasted image 20231013160338.png]]
**Explanation**: The default holdtime is 120s not 180s. The command `lldp timer 120` sets the LLDP update frequency, not the holdtime.

### Q38
![[Pasted image 20231021005225.png]]
**Explanation**: The remote engine ID is a unique ID used to identify an SNMP server. It's comprised of the enterprise number and the default MAC address
- The command
```
snmp-server user boson boson remote 192.168.51.1 v3 auth md5 80$0n!
```
is used to access the remote SNMP server `192.168.51.1` using SNMPv3, user `boson` that belongs to group `boson` and to authenticate with the password whose MD5 hash is `80$0n!`
*Note*: We didn't specify a remote engine in RouterB. The above commands would have worked if we also issued the following command before the above command in RouterB:
```
RouterB(config)# snmp-server engineID remote 192.168.51.1
```

### Q78
![[Pasted image 20231021121523.png]]
**Explanation:**
- `1000BaseLX` - Single-mode fiber cable
- `1000BaseCX` - twinaxial cable
- `1000BaseSX` - multi-mode fiber
- `1000BaseZX` - single-mode fiber but Cisco proprietary.

### Q82
- Steps to configure **SSH**:
	1. Set a hostname other than `Router`
	2. Set domain name using the command `ip domain-name`
	3. Generate RSA key using the command `crypto key generate rsa`
	4. Enable SSH with the command `transport input ssh`


### Q83
![[Pasted image 20231021122032.png]]
**Explanation**:
- A **successor** is the best next-hop route to a destination
- A **feasible successor** is a loop-free path to the destination. The feasible successors can be viewed with the command:
```
R1# show ip eigrp topology
```
For each feasible successor we can see the **Feasible Distance(FD)** and the **Administrative Distance(AD)**


### Q87(Etherchannel configuration)
![[Pasted image 20231021122800.png]]

### Q91(DTP Configuration)
![[Pasted image 20231021124232.png]]

### Q97
![[Pasted image 20231021130707.png]]
**Explanation**: The `ip helper-address` command is used to configure a DHCP relay agent and is used to send the DHCP requests to the interface issuing the IP addresses

### Q100
Assume we have the following network topology:
![[Pasted image 20231021131644.png]]
and the following requirements:
![[Pasted image 20231021131712.png]]

The configuration is done as follows:
```
SwitchA(config)# vtp mode server
SwitchA(config)# vtp domain boson
SwitchA(config)# vtp password NetSimX -> note the password
SwitchA(config)# interface range fastethernet 0/1 - 2
SwitchA(config-if-range)# channel-protocol pagp
SWitchA(config-if-range)# channel-group 1 mode desirable
SwitchA(config)# interface port-channel 1
SwitchA(config-if)# switchport mode trunk
SwitchA(config-if)# switchport trunk encapsulation dot1q
SwitchA(config-if)# switch mode trunk
SwitchA(config-if)# switchport nonegotiate
```

### Q102
![[Pasted image 20231021134011.png]]
**Explanation**: `ADD1`, `ADD2`, `ADD3` contain the source, destination MAC address and the `BSSID` respectively. `ADD4` is configured only for frames passing between different APs, which is not the case here is the destination is a host on a wired network.
The 4 address fields contain the following information:
- `ADD1` - Receiver Address. This is the MAC address of the intended recipient of the frame
- `ADD2` - Transmitter Address. This is the MAC address of the wireless station that transmitted the frame
- `ADD3` - BSSID. This is the MAC address of the AP
- `ADD4` - Source Address. This is the MAC address of the wireless station that originated the frame.

- All these different scenarios are illustrated in the picture below:
![[Pasted image 20231021155027.png]]



### Q104
![[Pasted image 20231021134240.png]]

**Explanation**:
![[Pasted image 20231021134317.png]]

*Note*: In the most significant bytes there are some special fields:
![[Pasted image 20231021134435.png]]

### Q105
![[Pasted image 20231021134540.png]]


### IPsec with GRE
- **GRE** is a tunneling protocol developed by Cisco that allows the encapsulation of a wide variety of network protocols inside point-to-point links as follows:
![[Pasted image 20231021160930.png]]
*Note*: The problem in the image above, is that the packets sent are not encrypted, and that's why we use **IPsec**.


### Q103-3
![[Pasted image 20231029191331.png]]
**Explanation**:
- Voice traffic from the IP phone is tagged with an IEEE 802.1Q header that contains an IEEE 802.1p CoS value.  This priority value is trusted by the switch in order to prioritize voice traffic. Data traffic however is tagged with a 802.1Q header that contains a CoS value that is not trusted by the switch. To move the trust boundary to the phone, i.e. trust both the voice and data traffic we use the following command on the switch:
```
SW1(config)# mls qos trust cos
```
- `switchport priority extend cos` - configures the IP phone to override the priority of the data packets it receives from the host. This command prevents the host from exploiting a high-priority data queue
- No commands can be entered on the IP phone => B and C are incorrect