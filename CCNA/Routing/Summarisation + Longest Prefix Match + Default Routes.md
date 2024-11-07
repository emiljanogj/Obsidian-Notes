#Routing

- When there are overlapping routes, the longest prefix will be selected. For example, in the case below, the 2nd route(/32) will be picked over the 1st one(/16).
![[Pasted image 20230708185044.png]]

- When multiple equal length routes are added for the same destination, the router will add all of them and load balance between routes.
![[Pasted image 20230708185200.png]]

- Default route(`0.0.0.0`) when no matches found
![[Pasted image 20230708185235.png]]


### Lab

**Note**: By default, the router's interfaces are turned off so we need to turn them on. On the other hand, the switches' interfaces are turned on by default. An example is shown below:
![[Pasted image 20230708231937.png]]


**Note**: `tracert` verifies only 1-way traffic, while `ping` verifies 2-way traffic.