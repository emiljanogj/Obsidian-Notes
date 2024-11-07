#Routing 

***

Assume we have the following network topology:

![[Pasted image 20230410004429.png]]

Assume that RC belongs to a partner organisation with which we need connection, but we don't want to send network information to => the routing protocol will be enabled on all the interfaces except for f2/0.  In this case, we would like to learn how to reach 10.0.2.0/24 from RA and RB but we don't want to send network information there because of the security implications. That's where *Passive Interfaces* come into play.

**Passive Interfaces** ^3600a6
- Best  practice is to configure loopback interface as passive interfaces
- This implies that the loopback interface will be advertised by the routing protocol, but it will not listen or send hello packets.

**Lab**
- Enabling rip as the routing protocol
 ```
 router rip
 version 2
 no auto-summary
```

- Setting an interface(loopback0 in this example) as a passive interface. This means that the other routers won't send any network info to it, but not vice versa, as the routers still need to discover how to reach this interface.
```
passive-interface loop0
```

