In the lab we have the following topology:

![[Pasted image 20230721175223.png]]

Then, we configure the EtherChannel as follows:

*Switch1*: 

```
interface range g1/0/1 - 2
no switchport
channel-group 1 mode active -> for LACP
exit
interface port-channel 1
ip address 192.168.0.1 255.255.255.252
no shut
```

On *Switch2* we define the same channel-group as follows:

```
interface range g1/0/1 - 2
no switchport
channel-group 2 mode active -> LACP
interface port-channel 2
ip address 192.168.0.2 255.255.255.252
no shut
```

We do the same thing for the pairs (SW1, SW2) and (SW2,SW3). We then need to configure the switches to advertise the IP subnets defined in their EtherChannel in Area 0. We do the following in all the 3 switches:
```
config t
ip routing
router ospf 1
network 192.168.0.0 255.255.255.252
```

At the end we see the following(example below in SW3):
![[Pasted image 20230721180004.png]]

We see above that the subnets 192.168.0.4/30 and 192.168.0.8/30 are directly connected from the EtherChannel 2 bw SW1(192.168.0.5 -> belongs to subnet 192.168.0.4/30) and SW3(192.168.0.6) and EtherChannel3 bw SW2(192.168.0.9 -> belongs to subnet 192.168.0.8/30).
On the other hand the subnet 192.168.0.0/30(SW1 is connected via the interface with IP 192.168.0.1) is reachable via SW2at IP 192.168.0.9(EtherChannel 3) or SW1 at IP 192.168.0.5(EtherChannel 2), since both SW1 and SW2 have direct interfaces in 192.168.0.0/30 at 192.168.0.1 and 192.168.0.2 respectively.q

