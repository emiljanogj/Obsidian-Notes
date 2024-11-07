*Useful Note*:

![[Pasted image 20230825162117.png]]


#### IPv4 Header

![[Pasted image 20230826135135.png]]

- **IHL** - Identifies the length of the header in 4-byte increments => value of 5 implies 5x4=20 bytes header length. In the interval [5, 15]
- **DSCP** - Used for QoS
- **ECN**
	- Provides e2e notification of network congestion without dropping packets
	- Optional feature that requires support from both endpoints
- **Total Length** - Indicates the total length of the packet(L3 + L4 header). Measured in bytes and has a max value of 2^16 - 1. Min value of 20 bytes = min length of the header
- **Identification** - identifies which packet the fragment belongs to(if the packet is fragmented). 
	- Packets are fragmented if larger than the MTU(usually 1500 bytes)
	- Fragments are re-assembled by the receiving host
- **Flags** -Used to control/identify fragments
	- Bit 0 - reserved(always set to 0)
	- Bit 1 - DF bit, used to indicated that a packet should not be fragmented
	- Bit 2 - MF bit, set to 1 if there are more fragment in the packet or 0 for the last fragment
- **Fragment Offset** - Indicates the position of the fragment in the packet. Used if the fragments arrive out of order.
- **TTL** - Time to Live
- **Protocol**:
	- 6 - TCP
	- 17 - UDP
	- 1 - ICMP
	- 89 - OSPF
- **Header Checksum** - checks for errors in the header