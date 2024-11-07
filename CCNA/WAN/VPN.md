- Provides a virtual tunnel between private networks across a shared public network such as the internet
- Traffic travelling over the tunnel is encrypted so it can be read by authorized parties only
- Since the private networks use shared infrastructure, VPN connections are cheapear than using a dedicated physical link
- Several type of VPNs:
	- Site to Site - connection terminated on a router or firewall. IPsec used for encryption so no extra software needed(shown in Fig.1)
	- Remote Access - connection bw the router/firewall in the office and a software installed in the user's device => It can be used anywhere. Encryption done usually via SSL(Fig.2)

**Fig. 1**
![[Pasted image 20230519102419.png]]

**Fig.2**
![[Pasted image 20230519102735.png]]

#### Site-to-Site IPsec VPN Configuration Options

- **IPsec Tunnel** - Open standard IPsec tunnel. Does not support multicasting
- **GRE(Generic Routing Encapsulation) over IPsec Tunnel** - Adds support for multicasting
- **IPSEC VTI(Virtual Tunnel Interface)** - Same as GRE but Cisco proprietary.
- **DMVPN(Dynamic Multipoint VPN)** - Simple hub and spoke style config. If we had a main office + a lot of branches and we would like to have a full mesh config => We would need a lot of IPsec tunnels, which is not practical. With this config, we add a tunnel between each branch site and the hub site, but not between each pair of branch sites itself. This makes the configuration much simpler while still providing a full mesh config.
- **FlexVPN** - newer version of **DMVPN** + Cisco proprietary
- **GETVPN(Group Encrypted Transport VPN)** - Scalable centralised policy for VPN over non-public infrastructure. Cisco proprietary.