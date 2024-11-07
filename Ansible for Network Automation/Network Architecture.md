
We're splitting the architectural development in 5 stages:

- Start-up: cloud-based, one server and a cloud services router
![[Pasted image 20230428010949.png]]

- Expansion Phase: Moving into an office(spine01 router is now at the office)
![[Pasted image 20230428011252.png]]

`spine01` router is at the office and `leaf01` is positioned where one expects a switch, while `cs01` represents a device that is still hosted with a cloud service provider.

- **Consolidation Phase**: Two locations, a data center and the original router and server hosted in the cloud.
![[Pasted image 20230428011625.png]]

Note now the introduction of a true spine-leaf architecture where every switch in the leaf layer is connected to a switch in the spline layer.

- **Break-up Phase**: Split in 3 different organizations that communicate routing information with one another

![[Pasted image 20230428012222.png]]


- **Adjustment Phase**: Making the **Break-up Phase** more robust

![[Pasted image 20230428012406.png]]