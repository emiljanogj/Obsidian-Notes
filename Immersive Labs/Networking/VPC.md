- Each region has a default VPC created by AWS
- Unless otherwise specified, this is the VPC in which the instances are launched. In the default VPC, there is one subnet for every AZ in the region.
![[Pasted image 20230611133717.png]]

- Every subnet is associated with a single route table that determines where the network traffic is routed to. Routes are prioritised based on their specificity. For example, given the iptable below:
![[Pasted image 20230611134009.png]]
=> The traffic to `172.31.0.0/16` range are routed locally, and all traffic outside this range is routed via the IGW. 


### Amazon DNS Server(Route 53 Resolver)

- Located at the primary private IPV4 CIDR range provisioned to our VPC + 2
- The DNS name for an EC2 instance is `private-ipv4-address.ec2.internal` 
- We can use the private IP DNS Name hostname for communication between instances in the same VPC.
- We can use also the DNS hostname for communication between instances in different VPCs within the same region, if their IP is in the following private IP address space ranges:
	- `10.0.0.0 - 10.255.255.255`
	- `172.16.0.0 - 172.31.255.255`
	- `192.168.0.0 - 192.168.255.255`
- Example of a simple architecture:
![[Pasted image 20230611144012.png]]

- Public subnets have a route to the internet, usually via an internet gateway(IGW) as follows:
![[Pasted image 20230611144336.png]]

- Private subnets are those with no route to the internet or a route that allows outbound-only internet access as shown below:
![[Pasted image 20230611144700.png]]


### Gateways

- To enable communication between networks with overlapping CIDR ranges, we use the following architecture:
![[Pasted image 20230611173151.png]]

- AWS Site-to-Site VPNs are used to connect a VPC network with one or more on-premise networks
![[Pasted image 20230611173549.png]]

#### Network ACLs

- SGs are applied to EC2 instances while NACLs are applied to subnets. Order of evaluation is as follows:
![[Pasted image 20230611185133.png]]

- **VPC Peering** establishes a private connection between two VPCs with different CIDR range. This can get quite messy as shown below:
![[Pasted image 20230611222557.png]]

 - To avoid the above problem, we create a **Transit Gateway**, and we attach VPCs to it. as follows:
![[Pasted image 20230611222851.png]]


### PrivateLink and Endpoint Services

- When you create an interface or gateway load balancer endpoint, an endpoint network interface (ENI) is created in each associated subnet, through which the traffic is routed to the service. For interface and gateway load balancer endpoints the network interface is connected to the load balancer (that fronts the endpoint service) via **AWS PrivateLink**.
- For gateway endpoints no ENI is created. 

- An example of using endpoint interfaces to access a lambda service is shown below:
![[Pasted image 20230611234312.png]]
In this case, endpoint policies can also be used for extra security.

- **Gateway Load Balancer Endpoints(GLBE)** - are used when we want to route traffic to virtual appliances, such as firewalls. In this case the following sequence happens:
	- IGW route table targets the GLBE for traffic destined for the application subnet
	- The traffic reaches the GWLB, which then distributes it to the virtual appliances
	- The virtual appliances return the traffic to the LB after inspection
	- The GWLB is deployed in a separate subnet, in the same VPC as the application subnet so the load balancer uses the `VPC-local` route in the subnet route table to route the traffic to the application.
	- This architecture is also described below:

![[Pasted image 20230612003032.png]]
