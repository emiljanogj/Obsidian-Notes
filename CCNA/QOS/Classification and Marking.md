#QoS
***

- As discuss previously, one of the main goals of `QOS` is to mark different traffic types, with different priority level
- Before the traffic is marked, it needs to be classified first. Several common ways to classify traffic:
	- Class of Service(COS) - There is a 3-bit field in the Layer 2 802.1q frame header which is used to carry the CoS QoS marking. Can take a value 0-7(0 - Best Effort, 7 - Highest Priority). Signalling traffic is marked as CoS 3 and the voice payload as CoS 5.
	- **DSCP(preferred)** - The Type of Service(TOS) byte in L3 IP header is used to carry the DSCP QoS marking. Only 6 bits are used => 64 possible values(0 - lowest priority, i.e. Best Effort). Signalling traffic is marked as 24(CS3) and the voice payload as 46 (EF).
	- ACL
	- NBAR - Can be used to recognize traffic based on L3-L7 info. Signatures are download from Cisco and loaded into our router to detect well-known traffic.
- We may think of the case when a user changes the DSCP/CoS bit by themselves to prioritize certain traffic. To prevent this we define a trust boundary, where the markings from the IP phone are trusted, but mark traffic from the PC is given priority 0 in the following example:
![[Pasted image 20230730195815.png]]

*Note*: **DSCP** is the preferred option because the router can gather quickly the needed info from a single byte. However, if ACL or NBAR is used, it should be done as close to the source as possible and then a DSCP value should be added also as follows:
![[Pasted image 20230730201230.png]]

