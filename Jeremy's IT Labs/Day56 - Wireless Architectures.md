#Wireless 
***

![[Pasted image 20230917084148.png]]

**802.11** packet fields:
- **Frame Control** - Provides info such as the message type and subtype
- **Duration ID** - Can indicate either the time in microseconds dedicated to the transmission or an identifier for the connection depending on the message type
- **Addresses** - up to 4 addresses present as follows:
	- *Destination Address* - Final recipient of the frame
	- *Source Address* - Original sender of the frame
	- *Receiver Address* - Immediate recipient of the frame
	- *Transmitter Address* - Immediate sender of the frame
- **Sequence Control** - Used to reassemble fragments and eliminate duplicates
- **QoS** - Used for quality control
- **HT Control** - Used to enable high throughput operations:
	- *802.11n* - also known as 'High Throughput' Wi-Fi
	- *802.11ac* - also known as 'Very High Throughput' Wi-Fi
- **FCS** - used to check for errors in the frame


#### 802.11 Association Process
- 3 connection states:
	- Not authenticated, not associated
	- Authenticated, not associated
	- Authenticated and associated

- 2 ways a station can scan for a **BSS**:
	- Active scanning - the station sends for probe requests and listens for a probe response from the AP
	- Passive scanning - the station listens for beacon messages from the AP
![[Pasted image 20230919093901.png]]


### Lightweight APs

**Split MAC Architecture**

- Two tunnels are created between each **AP** and **WLC**
	- *Control Tunnel* - UDP port 5246. This tunnel is used to configure the APs and control/manage the operations. All traffic in this tunnel is encrypted by default
	- *Data Tunnel* - UDP port 5247. All traffic from wireless clients is sent through this tunnel to the WLC and then to the wired network. Not encrypted by default but  we can enable **DTLS(Datagram Transport Layer Security)**


#### Traffic Flow(Lightweight/Autonomous APs)

![[Pasted image 20230919101515.png]]

- Several things to note in the picture above
	- In **Lightweight APs** the traffic is always sent to the WLC(via an access port link). Even the traffic destined to the PC within the same network(denoted with red arrows) also goes through the WLC
	- In **Autonomous APs** the traffic to a PC within the same network is sent directly without going through the wired network


#### Modes of Operation of Lightweight APs

- Modes of operation:
	- **Local** - AP offers a BSS(multiple BSS) for the clients to associate with
	- **FlexConnect** - Same as **Local**, but it also offers the flexibility of switching between the wired and wireless connection if the tunnels to the WLC go down
	- **Sniffer** - The AP does not offer a BSS for clients. It is dedicated to just capture the wireless traffic and send it to a software like Wireshark
	- **Monitor** - The AP does not offer a BSS for clients but it is just dedicated to receive `802.11` frames to detect rogue devices.
	- **Rogue Detector** - It does not even use its radio. It listens to traffic on the wired network and also receives a list of suspected rogue clients and AP MAC addresses from the WLC
	- **SE-Connect** - It does not offer a BSS to the clients but it's dedicated to RF spectrum analysis on all channels.

**Cloud-based APs**
- Only management traffic is sent to the cloud while data traffic is sent directly to the wired network like when using **Lightweight APs**


#### WLC Deployments

- In a split-MAC architecture, there are four main WLC deployment models:
	- **Unified**: The WLC is a hardware appliance in a central location of the network.
	- **Cloud-based**: The WLC is a VM running on a server, usually in a private cloud in a data
	   center. This is not the same as the cloud-based AP architecture discussed previously.
	- **Embedded**: The WLC is integrated within a switch.
	- **Mobility Express**: The WLC is integrated within an AP.


#### 802.11 Message Types

- **Management** - used to manage the BSS.
	- **Beacon**
	- **Probe request, probe response**
	- **Authentication**
	- **Association request, association response**
**Control** - Used to control access to the medium (radio frequency).
and data frames.
		- **RTS** (Request to Send)
		- **CTS** (Clear to Send)
		- **ACK**
• **Data**: Used to send actual data packets.
