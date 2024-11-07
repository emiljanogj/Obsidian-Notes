 
#Automation 
***

![[Pasted image 20230924235119.png]]


### SD-Access

![[Pasted image 20230924235509.png]]

*Note*: **DNA Center** is the controller.


The **Fabric** consists of:

- The **underlay** - is the underlying physical network of devices and connections
- The **overlay** - is the virtual network built on top of the physical access network. **SD-Access** uses **VXLAN** to build tunnels
- The **fabric** is the combination of **underlay** and **overlay**


#### SD-Access Underlay

- The underlay's purpose is to support the VXLAN tunnels of the overlay.
- There are three different roles for switches in SD-Access:
	- Edge nodes: Connect to end hosts
	- Border nodes: Connect to devices outside of the SD-Access domain, ie. WAN routers.
	- Control nodes: Use LISP (Locator ID Separation Protocol) to perform various control plane functions.
- You can add SD-Access on top of an existing network (brownfield deployment) if your network hardware and software supports it.
	- Google 'Cisco SD-Access compatibility matrix' if you're curious.
	- In this case DNA Center won't configure the underlay.
- A new deployment (greenfield deployment) will be configured by DNA Center to use the optimal SD-Access underlay:
	- All switches are Layer 3 and use IS-IS as their routing protocol.
	- All links between switches are routed ports. This means STP is not needed.
	- Edge nodes (access switches) act as the default gateway of end hosts (routed access layer).

**Traditional LAN**
![[Pasted image 20230925094209.png]]


**SD-Access Underlay**
![[Pasted image 20230925094303.png]]



#### SD-Access Overlay

- LISP provides the control plane of SD-Access.
- A list of mappings of EIDs (endpoint identifiers) to RLOCs (routing locators) is kept.
- ElDs identify end hosts connected to edge switches, and RLOCs identify the edge switch which can be used to reach the end host.
- There is a LOT more detail to cover about LISP, but I think you can see how it differs from the traditional control plane.
- Cisco TrustSec (CTS) provides policy control (QoS, security policy, etc).
- VXLAN provides the data plane of SD-Access.

![[Pasted image 20230925105506.png]]

#### Cisco DNA Center

- CISCO DNA Center has two main roles:
	- The SDN controller in SD-Access
	- A network manager in a traditional network (non-SD-Access)
• DNA Center is an application installed on Cisco UCS server hardware.
• It has a REST API which can be used to interact with DNA center.
• The SBI supports protocols such as NETCONF and RESTCONF (as
well as traditional protocols like Telnet, SSH, SNMP).
- DNA Center enables Intent-Based Networking (IBN).
- The goal is to allow the engineer to communicate their intent for
network behavior to DNA Center, and then DNA Center will take care
of the details of the actual configurations and policies on devices.



### Network Automation Tools

![[Pasted image 20230925113552.png]]