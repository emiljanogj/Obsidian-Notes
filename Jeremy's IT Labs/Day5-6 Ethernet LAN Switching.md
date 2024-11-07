#Ethernet
***
***Ethernet Frame***
![[Pasted image 20230825173924.png]]

- **Preamble**
	- 7 bytes in length
	- Alternating 1s and 0s(10101010 x 7)
	- Allows the devices to sync their receiver clocks
- **SFD**
	- 1 byte in length(10101011)
	- Marks the end of the preamble and start of the rest of the frame
- **Destination MAC Address** (6 bytes)
- **Source MAC Address** (6 bytes)
- **Type/Length**(2 bytes)
	- Length - a value of 1500 or less
	- Type - a value >= 1536
- **FCS**
	- Frame Check Sequence
	- 4 bytes in length
	- Detects corrupted data by running a CRC algorithm

*Note*: Dynamic MAC Addresses are removed from a Switch's mac address table after 5' of inactivity

**OUI** - First 24 bits of a MAC address

=> Ethernet packet has a min size of 64 bytes
		18 bytes for the header(Source + Destination MAC,  Type/Length, FCS)
		48 bytes min for the payload(If payload is < 48 bytes, it is padded with 0s)