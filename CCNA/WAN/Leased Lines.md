- A dedicated physical connection bw 2 locations, which is not shared with anyone else.
- It can be used to establish connections bw a corporate office and:
	- another corporate office => providing point to point connectivity
	- a data center that's connected to the company's existing WAN => providing multipoint connectivity bw offices
	- a data center connected to the internet => providing internet connectivity
- Example of a leased line is given below:
![[Pasted image 20230726233443.png]]


- 2 offices may be connected over internet using VPN via leased lines as follows:
![[Pasted image 20230726233623.png]]

- Or we can also have a dedicated leased line between the 2 offices and normal leased lines(no VPN) for connections to the internet as follows:
![[Pasted image 20230726233747.png]]

- Leased lines use a serial connection requiring the correct interface card in the router(they do not use the Ethernet port)
- Common bandwidth options are given below:
![[Pasted image 20230726233858.png]]

- T1 and E1 links were commonly used for connections to the Public Switched Telephone Network(PSTN):
	- T1 is capable of carrying 24 concurrent TDM calls
	- E1 is capable of carrying 30 concurrent TDM calls
- VoIP using SIP(Session Initiation Protocol) signalling over Ethernet WAN connections are common today
- A common PSTN topology is given below:
![[Pasted image 20230726234254.png]]

- Calls between offices over WAN go through the T1 Leased Line(SW1 -> R1 -> R2 -> SW2) using only Ethernet ports(no special network interface cards required) as follows:
![[Pasted image 20230726234402.png]]