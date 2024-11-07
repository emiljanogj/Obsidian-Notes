#EtherChannel 
***

- Assume we have the following network topology
![[Pasted image 20230830142758.png]]
We not that although there are several links between `ASW1` and `DSW1` only 1 is used, because otherwise a loop would be formed and we would end up with broadcast storms. In this case using `Spanning Tree` protocol has resulted in wasted bandwidth. To resolve this issue, `Etherchannel` is used, where the 4 separate interfaces above are considered as a single interface. 

### Etherchannel

- An `Etherchannel` interface can load balance using the following:
	- Source/Destination MAC address(1 or both)
	- Source/Destination IP address(1 or both)
- To configure the loadbalancing method we use the following command:
```
ASW1(config)# port-channel load-balance src-dst-mac
```

- There are three methods of EtherChannel configuration on Cisco switches:
	- PAgP (Port Aggregation Protocol)
		- Cisco proprietary protocol
		- Dynamically negotiates the creation/maintenance of the EtherChannel.
	- LACP (Link Aggregation Control Protocol)
		- Industry standard protocol (IEEE 802.3ad)
		- Dynamically negotiates the creation/maintenance of the EtherChannel.
	- Static EtherChannel
		- A protocol isn't used to determine if an EtherChannel should be formed.
		- Interfaces are statically configured to form an EtherChannel.

*Note*: `EtherChannel` can be used for up to 8 interfaces(LACP allows 16 but 8 are in standby mode in case one of the interfaces in standby mode fails).

#### EtherChannel Modes

As in `DTP`, `EtherChannel` can also be in several modes:
- active - enable `LACP` unconditionally
- auto - enable `PAgP` if a `PAgP` device detected
- desirable - enable `PAgP` unconditionally
- on - enable `Etherchannel` only. It works only with `on` mode. `on` + `desirable` or `on` + `active` doesn't work
- passive - enable `LACP` only if a `LACP` device is detected

- The channel group number is used to identify the `EtherChannel` in the local switch. However channel-group 1 on switch 1 can form an `EtherChannel` with channel-group 2 on switch 2.
- To configure an `EtherChannel` we use the following command:
```
SW1(config)# interface port-channel {etherchannel-number}
```

- Physical interfaces that are encapsulated by an `EtherChannel` must have the same properties:
	- same duplex(full/half)
	- same speed
	- same switchport mode(access/trunk)
	- same allowed/native VLANs
*Note*: If one of the interfaces doesn't match the properties of the other interfaces it is automatically excluded from the `EtherChannel`


### EtherChannel Commands

![[Pasted image 20230830153031.png]]
