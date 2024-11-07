- All PCs connected to a hub reside in the same collision domain
![[Pasted image 20230826130924.png]]

#### CSMA/CD

- To avoid packet collision for devices that reside in the same collision domain **CSMA/CD**(Carrier Sense Multiple Access with Collision detection) is used
- Before sending frames, devices listen to the collision domain until they detect that other devices are not sending any packets
- If a collision occurs, the device(Hub in the above case) sends a jamming signal to inform all the connected devices that a collision occurred
- Each device waits for a random period of time before resending the packets


#### Speed and Duplex

![[Pasted image 20230826131851.png]]

If the speed is 10 or 100 Mbps, the switch will use half duplex. However, if autonegotiation is on, the switch will detect whether the connected devices use half/full duplex and default to that.