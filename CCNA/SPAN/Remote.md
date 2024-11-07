#SPAN 
***

Assume we have the following network topology:

![[Pasted image 20230821162329.png]]

And we have the following requirements:

Configure REMOTE SPAN as follows:

1) All traffic sent and received by PC1 in VLAN 10 (Switch 1) should be copied to PC6 (Switch 2).

- Use SPAN VLAN 99

Verification:

1) Use Simulation mode to verify that ICMP packets sent and recevied from PC1 to P3 are copied to PC6.


#### Configuration
```
Switch1(config)# int vlan 99
Switch1(config-vlan)# remote-vlan
Switch1(config)# monitor session 10 source interface f0/1 both
Switch1(config)# monitor session 10 destination remote vlan 99 reflector-port f0/24


Switch2(config)# int vlan 99
Switch2(config-vlan)# remote-vlan
Switch2(config)# monitor session 11 source remote vlan 99
Switch2(config)# monitor session 11 destination interface f0/2
```