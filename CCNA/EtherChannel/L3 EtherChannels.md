We configure L3 EtherChannels as follows:
```
interface range GigabitEthernet 1/0/1 - 2
no switchport (+)
channel-group 1 mode | active | auto | desirable | on | passive
```
*Note*: 
- active/passive - LACP
- auto/desirable - PagP
- on/passive - Static

```
interface port-channel 1
ip address 192.168.0.1 255.255.255.252 (-)
no shutdown
```

*Note*: (+) and (-) are the only difference between configuring and L2 and L3 switch.

A simple solution to avoid this complex architecture(Spanning Trees + EtherChannels) is to replace L2 switches with L3 switches as follows:
![[Pasted image 20230423173901.png]]

*Note*: The DG will now move from Distribution Layer to Access Layer.


