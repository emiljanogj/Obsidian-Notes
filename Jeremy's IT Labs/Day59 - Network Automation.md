#Automation 
***
### Data Plane

- All tasks involved in forwarding user data/traffic from one interface to another are part of the
data plane.
- A router receives a message, looks for the most specific matching route in its routing table,
and forwards it out of the appropriate interface to the next hop.
- It also de-encapsulates the original Layer 2 header, and re-encapsulates with a new
header destined for the next hop's MAC address.
- A switch receives a message, looks at the destination MAC address, and forwards it out of
the appropriate interface (or floods it).
- This includes functions like adding or removing 802.1q VLAN tags.
- NAT (changing the src/dst addresses before forwarding) is part of the data plane.
- Deciding to forward or discard messages due to ACLs, port security, etc. is part of the data
plane.
- The data plane is also called the 'forwarding plane'.


### Control Plane

- How does a device's data plane make its forwarding decisions?
- routing table, MAC address table, ARP table, STP, etc.
- Functions that build these tables (and other functions that influence the data plane) are part
of the control plane.
- The control plane controls what the data plane does, for example by building the router's
routing table.
- The control plane performs overhead work.
- OSPF itself doesn't forward user data packets, but it informs the data plane about how
packets should be forwarded.
- ST itself isn't directly involved in the process of forwarding frames, but it informs the data
plane about which interfaces should and shouldn't be used to forward frames.
ARP messages aren't user data, but they are used to build an ARP table which is used in
the process of forwarding data.


### Management Plane

- Like the control plane, the management plane performs overhead work.
- However, the management plane doesn't directly affect the forwarding of messages in the
data plane.
• The management plane consists of protocols that are used to manage devices.
- SSH/Telnet, used to connect to the CLI of a device to configure/manage it.
- Syslog, used to keep logs of events that occur on the device.
- SNMP, used to monitor the operations of the device.
- NTP, used to maintain accurate time on the device.

### Logical Planes

- The operations of the Management plane and Control plane are usually
managed by the CPU.
- However, this is not desirable for data plane operations because CPU
processing is slow (relatively speaking).
- Instead, a specialized hardware ASIC (Application-Specific Integrated
Circuit) is used. ASICs are chips built for specific purposes.
- Using a switch as an example:
	- When a frame is received, the ASIC (not the CPU) is responsible for  the switching logic.
	- The MAC address table is stored in a kind of memory called TAM (Ternary Content-Addressable Memory). *Another common name for the MAC address table is CAM table.
	- The ASIC feeds the destination MAC address of the frame into the TCAM, which returns the matching MAC address table entry.
- The frame is then forwarded out of the appropriate interface.
• Modern routers also use a similar hardware data plane: an ASIC designed for forwarding logic, and tables stored in TAM.



### Southbound Interface

- The SBI is used for communications between the controller and the network devices it controls.
- It typically consists of a communication protocol and API (Application Programming Interface).
- APIs facilitate data exchanges between programs.
- Data is exchanged between the controller and the network devices.
- An API on the network devices allows the controller to access information on the devices, control their data plane tables, etc.
- Some examples of SBIS:
	- OpenFlow
	- Cisco OpFlex
	- Cisco onePK (Open Network Environment Platform Kit)
	- NETCONF