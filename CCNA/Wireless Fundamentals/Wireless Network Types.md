#Wireless 
***

2 Network Types:
- Ad Hoc Networks - 2 or more wireless stations communicate directly with each other. Independent Basic Service Set(IBSS)
![[Pasted image 20230805114240.png]]
*Note*: Scalability issue, because if we added another laptop as shown above, which is not in the coverage area of the first laptop => it won't manage to be part of the network.

- Infrastructure Mode - Stations communicate via a Wireless Access Point(WAP), which can provide access to a wired network
![[Pasted image 20230805114939.png]]
In the image above we can see that the Wireless AP communicates with the router => it gives the PCs access to that wired network.

		*Note*: Wireless stations work either in Ad-Hoc or Infrastructure mode, but not in both at the same time.  To solve this issue, WiFi Direct was introduced. (1)

- Wifi Direct - Allows devices to be connected to an Access Point and also be part of a peer-to-peer wireless network. However, this doesn't contradict (1) since it doesn't operated in Ad-Hoc IBSS mode, but is rather an extension to the Infrastructure Mode.


**Wireless Bridge**
- It's used to connect areas which are not reachable via cable to the network as shown below:
![[Pasted image 20230805120407.png]]

**Mesh Networks**
- Another option to increase coverage. In this case, one AP radio connects to the clients and the other connects to the backhaul network as shown below:
![[Pasted image 20230805120533.png]]
