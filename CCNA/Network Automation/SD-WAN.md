- Automated setup of WAN connectivity between sites
- Monitoring and failover is automated and traffic flow control is application aware


#### SD-WAN Architecture

![[Pasted image 20230801011445.png]]


#### Data Plane - vEdge Routers

- Physical or virtual routers
- They form an IPsec encrypted data plane between each other
- A site usually has 2 routes for redundancy

#### Control Plane - vSmart Controllers

- Run as virtual machines
- They distribute policy and forwarding information to the vEdge routers using TLS tunnels
- Each vEdge router connects to 2 vSmart Controllers for redundancy

#### Management Plane - vManage NMS

- Provides the management plane GUI
- Enables centralized configuration, which simplifies changes
- Runs as VM and provides real-time alerting
- Multiple vManage NMS used for clustering


#### Orchestration - vBond Orchestrator

- Authenticates all vSmart controllers, vManage NMS and vEdge routers that join the SD-WAN network
- When a vEdge router joins a networks, it first connects to the vBond Orchestrator which has a public IP and is deployed in the DMZ + runs as a VM
- After connecting to the vBond Orchestrator, this new vEdge router then discovers other vEdge routers
- Multiple vBond orchestrators can be deployed with round robin DNS

#### Zero Touch Provisioning Service(ZTP)

- Cloud based shared service hosted by Cisco
- When a vEdge router is first booted, it directs it to the vBond Orchestrator to orchestrate joining it to the network

#### Building the Data Plane

- The vSmart Controller directs the vEdge routers to build a full mesh of IPsec VPN tunnels bw themselves
- It propagates policy and routing information to the vEdge routers using the Overlay Management Protocol

![[Pasted image 20230801013114.png]]


#### Traffic Forwarding Options

If multiple tunnels are available, there are several options
- Active/Active
	- Equal traffic over all tunnels
- Weighted Active/Active
	- Decides based on a weight where to send most of the traffic
- Application pinning Active/Standby
	- Send a specific type of application over a certain tunnel
- Application Aware Routing
	- BFD monitors the latency, jitter and loss across a tunnel
	- SD-WAN ensures the application is sent over a link which meets its SLA requirements by monitoring the QoS metrics over time, but will fall back to another link if no suitable link is available