- Several theoretical standards
![[Pasted image 20230916151134.png]]

- 3 main types of **service sets**:
	- **Independent** - 2 or more wireless devices  directly connect without using an AP. Can be used for file transfer but not scalable
	- **Infrastructure** - a kind of infrastructure in which clients connect to each other via an AP and are identified by a **BSSID(MAC address of the AP's radio)**. The area of an **AP** where its signal is usable is called a **BSA(Basic Service Area)**.
![[Pasted image 20230916152144.png]]
	- To create larger **wireless LANs** beyond the range of a single AP, we use an **ESS(Extended Service Set)**
	- **APs** with their own **BSS** are connected by a Switch
		- Same SSID
		- Different BSSID
		- Different channel to avoid interference
	- Clients can pass from one AP to another without losing connectivity
![[Pasted image 20230916152533.png]]


- **Mesh**
	- Is used in situations where it's difficult to run an Ethernet connection to every AP
	- 2 radios, 1 to provide a **BSS** to clients and 1 to form a backhaul network that bridges traffic from AP to AP
	- At least 1 AP connected to the wired network and it's called **RAP(Root Access Point)**
	- Other APs are called **MAPs(Mesh Access Points)**

#### Distribution Systems

- Most wireless networks aren't standalone networks, but just a way for wireless clients to connect to wired network infrastructure 
- Each wireless **BSS** or **ESS** is mapped to a different VLAN
![[Pasted image 20230916201947.png]]


#### Additional AP Operational Modes

- **Workgroup Bridge**
![[Pasted image 20230916203400.png]]
- WGB is a Cisco-proprietary version of the 802.11 standard that allows multiple wired clients to be bridged to the wireless network
- In the example above PC1 is connected to **WGB** via wires which is connected to the AP via wireless


- **Outdoor Bridge**
	- Can be used to connect networks over long distances without a physical cable connecting them
	- Point-to-point or Point-to-multipoint

![[Pasted image 20230916203922.png]]


*Note*: AP should connect to the switch via a trunk