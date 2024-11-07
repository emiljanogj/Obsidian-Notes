Network topology is split into:
- Access Layers
- ![[Pasted image 20230416003213.png]]
- It is the connection point to end hosts => client access security measures are implemented at this layer
- Distribution Layers
- ![[Pasted image 20230416003604.png]]
- They serve as an aggregation point for the Access Layer + provide scalability
- As shown in the above image, they're usually deployed in pairs to avoid a single point of failure
- Most software policy such as QoS is enabled here
- Core Layers
- ![[Pasted image 20230416004020.png]]
- Note that the Core Layer switches are only located in one building and they connect the switches from different buildings together
- Also deployed in redundant pairs + it is designed for speed