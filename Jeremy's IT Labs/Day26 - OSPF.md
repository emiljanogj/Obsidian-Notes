### OSPF Areas

- Assume we have the following network topology:
![[Pasted image 20230831224159.png]]
*Note*: All areas are connected to the area 0, which is the `Backbone` area.

- An area is a set of routers and links that share the same LSDB.
- The backbone area (area 0) is an area that all other areas must connect to.
- Routers with all interfaces in the same area are called internal routers.
- Routers with interfaces in multiple areas are called area border routers (ABs).
- Routers connected to the backbone area (area 0) are called backbone routers.
- An intra-area route is a route to a destination inside the same OSPF area.
- An interarea route is a route to a destination in a different OSPF area.
- OSPF areas should be contiguous