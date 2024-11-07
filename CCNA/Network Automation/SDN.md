#Automation
***
- Router and Switch Planes
	- **Data Plane** - Traffic which is forwarded through the device
	- **Control Plane** - Makes decisions about how to forward traffic. Includes routing protocol or spanning tree updates destined or originating on the device itself
	- **Management Plane** - The device is configured and monitored in the management plane(i.e. CLI through Telnet or SSH, via SNMP or via an API)

**Traditional Method vs SDN**
- Traditional - Devices are responsible for their own control and data planes
- SDN - decouples data and control planes, where the devices are still responsible for forwarding traffic, but the control planes move to a centralized SDN controller

**Pure SDN vs Hybrid SDN**
- Pure SDN - The control plane runs purely on an SDN controller and the data plane runs purely on the network devices
- Hybrid SDN - The majority of the control plane intelligence is still provided by an SDN controller but the network devices retain some control plane intelligence in addition to the data plane operations -> This is the most common SDN implementation.
- DNA Center is a widely used SDN controller.


#### SDN Architecture

![[Pasted image 20230731232855.png]]

