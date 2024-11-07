3 Protocols:
- LACP
	- The switches on both sides negotiate the port channel creation and maintenance
	- Open standard
	- Interfaces can be defined as Active or Passive. The port channel will come up if at least one of the Switches is set to Active.
	- Configuration is done as follows:
```
interface range f0/23 -24
channel-group 1 mode active
```
Then we define the settings for the port channel as follows:
```
interface port-channel 1
switchport mode trunk
```


- PAgP(Port Aggregation Protocol)
	- Same as LACP but Cisco proprietary
	- Configuration is done the same way but we have 2 states: Desirable or Auto
	- Port channel will come up only if at least one of the sides is set to Desirable
- Static Etherchannel - No negotiation between switches but they must have the same settings defined

To show the EtherChannel summary we use the following command:
```
show etherchannel summary
```

The benefits of EtherChannel are shown below:
![[Pasted image 20230423163317.png]]
Note that before EtherChannel is created, Acc3 blocks interface F0/24 since it may create a loop. After EtherChannel is created we have the following:
![[Pasted image 20230423163414.png]]
Note that Acc3 now sees only 1 interface(PortChannel 1)