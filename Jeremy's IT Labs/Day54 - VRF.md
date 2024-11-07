- It allows a router to build multiple routing tables
	- Interfaces and routes are configured to be in a specific VRF
	- Router interfaces, SVIs & routed ports on multilayer switches can be configured in a VRF
- Traffic in a VRF cannot be forwarded out of an interface in another VRF
	- An exception can be made with the *VRF Leaking*
- Allows one device to carry traffic from multiple customers
	- Each customer traffic is isolated from the other
	- Customer's IP addresses can overlap without issues
![[Pasted image 20230925215940.png]]


#### VRF Configuration
Assume we have the following scenario:
![[Pasted image 20230925220100.png]]
*Note*: Both customers use the same network range `192.168.1.0/30`. 

- Without the use of a **VRF** two interfaces in the same router cannot be in the same subnet
- To enable **VRF** we use the following commands:
```
ip vrf {VRF-name}
R1(config-vrf)# interface {interface-name}
R1(config-if)# ip vrf forwarding {VRF-name} -> the IP address the interface had before is removed when the VRF is configured => re-assign the address
R1(config-if)# ip address {IP} {SUBNET-MASK}
show ip route vrf {VRF-NAME}
ping vrf {VRF-NAME} {IP}
```

