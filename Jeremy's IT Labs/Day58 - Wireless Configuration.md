#Wireless 
***

- We have the following network topology:
![[Pasted image 20230920120518.png]]

#### WLC Ports/Interfaces

- **Management interface:** Used for management traffic such as Telnet, SSH, HTTP, HIPS, RADIUS authentication, NTP, Syslog, etc. CAPWAP tunnels are also formed to/from the WLC's management interface.
- **Redundancy management interface**: When two WLCs are connected by their redundancy ports, one WLC is 'active' and the other is 'standby'. This interface can be used to connect to and manage the 'standby' WLC.
- **Virtual interface**: This interface is used when communicating with wireless clients to relay DHCP requests, perform client web authentication, etc. Also provides the same IP addresses that the APs use to communicate with the WLC when they roam
- **Service port interface**: If the service port is used, this interface is bound to it and used for out-of-band management. It's a physical port that can be used in case the WLC goes down
- **Dynamic interface**: These are the interfaces used to map a WLAN to a VLAN. For example, traffic from the 'Internal' WLAN will be sent to the wired network from the WLC's 'Internal' dynamic interface. Used to segment traffic in **WLC**, similar to **VLAN**.
- **AP-manager** - controls all L3 communications between a **WLC** and a Lightweight **AP** => it contains the IP addressed that the APs used to communicate with the **WLC**
- 



#### QoS Settings

![[Pasted image 20230920133013.png]]
