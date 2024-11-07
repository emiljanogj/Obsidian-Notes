#### Leased Line

![[Pasted image 20230927171729.png]]


#### MPLS

- Stands for **Multi Protocol Label Switching**
- The label switching allows VPNs to be created over the MPLS infrastructure through the use of labels
- Important terms:
	- CE router - Customer Edge router
	- PE router - Provider Edge router
	- P router - Provider core router
- When the PE routers receive frames from the CE routers they add a label to the frame. These labels(not the destination IP) are then used to make forwarding decisions
- The CE routers do not use MPLS. It is only used by the PE/P routers  

![[Pasted image 20230927173819.png]]

- When using a *Layer 3 MPLS VPN*, the CE and PE routers peer using OSPF
![[Pasted image 20230927174016.png]]

- When using a **Layer 2 MPLS VPN**, the CE and PE routers do not form peering, and the service provider network is entirely transparent to the CE routers
![[Pasted image 20230927174221.png]]
*Note*: If a routing protocol is used above, the CE routers will peer directly with each other => PE routers act like a big switch
![[Pasted image 20230927174321.png]]

#### Site-to-Site VPNs(IPsec)

1) The sending device combines the original packet and session key (encryption key) and runs them through an encryption formula.
2) The sending device encapsulates the encrypted packet with a VPN header and a new IP header.
3) The sending device sends the new packet to the device on the other side of the tunnel.
4) The receiving device decrypts the data to get the original packet, and then forwards the original packet to its destination.

*Note*: In a 'site-to-site' VPN, a tunnel is formed only between two tunnel endpoints (for example, the two routers connected to the Internet).
All other devices in each site don't need to create a VPN for themselves. They can send unencrypted data to their site's router, which will encrypt it and forward it in the tunnel as described above.

#### Limitations of IPsec

- Doesn't support broadcast/multicast traffic, only unicast => routing protocols like OSPF cannot be used over the tunnels(This is solved with *GRE over IPsec*)
- Configuring a full mesh of tunnels bw many sites is a labor intensive task.


#### GRE over IPsec

- **GRE**(Generic Routing Encapsulation) creates tunnels like IPsec but it does not encrypt the original packet so it's not secure but it can encapsulate a wide variety of L3 protocols + broadcast/multicast traffic
- To get the flexibility of GRE with the security of IPSEC, **GRE over IPsec** can be used

![[Pasted image 20230927180218.png]]


#### DMVPN

- Cisco-developed solution that allows routers to dynamically create a full mesh of IPsec tunnels without having to manually configure every tunnel

![[Pasted image 20230927183330.png]]


![[Pasted image 20230927183343.png]]


#### Site-to-Site vs Remote-Access VPN

- Site-to-Site VPNs typically use IPsec.
- Remote-Access VPNs typically use TLS.


- Site-to-Site VPNs provide service to many devices within the sites they are connecting.
- Remote-Access VPNs provide service to the one end device the VPN client software is
installed on.
 

- Site-to-Site VPNs are typically used to permanently connect two sites over the Internet.
- Remote-Access VPNs are typically used to provide on-demand access for end devices that
want to securely access company resources while connected to a network which is not secure.
