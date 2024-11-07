- WAN connectivity can be provided over an MPLS infrastructure, usually operated by a service provider
- Traffic from multiple customers can travel over the provider's shared MPLS network, so this acts like a VPN service.
- Different levels of SLA, uptime, and traffic delay are available at different price points
![[Pasted image 20230727131559.png]]

- CE routers peer at L3 with the PE routers. However, the CE routers treat the PE routers as usual CE routers. In this case, the customers may be in different subnets, and they discover each other in 2 ways:
	- static IP addresses
	![[Pasted image 20230727131946.png]]
	- routing protocols
![[Pasted image 20230727132027.png]]

#### L2 MPLS VPN

- The entire provider network is transparent to the CE and CEs do not pair with PEs
- The clients are in the same subnet:
![[Pasted image 20230727132312.png]]
- L2 MPLS terminology:
	- VPLS(Virtual Private LAN Service) - Multipoint L2 VPN
	- VPWS(Virtual Pseudowire service) - L2 point-to-point VPN
