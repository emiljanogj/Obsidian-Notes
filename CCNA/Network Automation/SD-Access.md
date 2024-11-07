- Traditional Way to control access and traffic flows within a network is with fixed VLANs, IP addresses and ACLs. The configuration can get complex and is not flexible, i.e. users are expected to always connect to the same physical port where they are assigned an access VLAN and IP subnet
- With `SD-Access`, traffic flow security is based on user identity, not physical location and IP address. It uses 2 components:
	- **Identity Service Engine(ISE)** - used to authenticate the users
	- **Security policy** - permitted and denied communication bw groups defined in the DNA center


**Underlay and Overlay Network**
- Underlay network - It's the underlying physical network
- Overlay network - The logical topology used to connect devices


**Underlay Network**
- When SD-Access is deployed into existing('brownfield') network, any config can be used for the underlying physical network. Link bw devices can be L2 or L3 and any routing protocol can be used
- DNA Center can be used to automatically provision the underlay network in new sites. In this case L3 links are used and IS-IS is used as the routing protocol.

**Overlay Network**
- LISP used for the Control Plane
- VXLAN is used for the Data Plane
- Cisco TrustSec CTS is used for the policy
- Each technology has been optimized for SD-Access

![[Pasted image 20230801005431.png]]
*Note*: The Switch at `10.10.10.1` always asks the Control Plane Node for info since all the network information is stored in the SDN(Control Plane Node).

![[Pasted image 20230801005501.png]]
*Note*: When PC1 learns of the IP address of `192.168.1.2`, it creates a VXLAN tunnel to reach PC2.

![[Pasted image 20230801010239.png]]
*Note*: When PC2 mvoes from `10.10.10.2` to `10.10.10.3`, it will send a notification to the Control Plane Node as follows =>The VXLAN Tunnel will also change as follows:
![[Pasted image 20230801010326.png]]


**Policy Plane - Cisco TrustSec CTS**
- Users are authenticated by the **Identity Service Engine(ISE)**
- Security policy configured on DNA Center
- Users are allocated a Scalable Group Tag(SGT)
- Cisco TrustSec secures traffic flows based on the security policy and SGTs
- Trustsec needs support end-to-end while SD-Access uses overlay tunnels so can work with other devices