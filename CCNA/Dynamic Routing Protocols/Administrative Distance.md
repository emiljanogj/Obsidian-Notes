If path to a destination is computed using different protocols, the protocol with the lowest administrative distance is used. The default administrative distance values are as follows:
![[Pasted image 20230409222811.png]]

**Floating Static Routes**
Assume we have the following network topology:
![[Pasted image 20230409223510.png]]
If we are currently using OSPF as a routing algorithm, and we would like to add a static route as a backup, we do the following:

```
ip route 10.0.1.0 255.255.255.0 10.1.3.2 115
```

We add the 115 at the end, which is larger than the AD of OSPF, so that it isn't used if OSPF is supported.

Same concept can be used if we want to add a backup static route as follows:
``` 
ip route 10.0.1.0 255.255.255.0 10.1.1.2
ip route 10.0.1.0 255.255.255.0 10.1.3.2 5
```

*Note*: We have to be careful with this, because if the route from R4 -> R3 fails, then R4 will detect that, since it's its gateway and will adjust for that. However if the link R3 -> R2 or R2 -> R1 fails, then R4 won't actually detect it, and therefore it won't redirect the traffic through the 2nd path, so the backup path in this case fails.