- To find the MAC address of a switch we use the following command
```
show spanning-tree vlan {VLAN-number}
```

We also use the above command to find the **Bridge Port** of a switch(under the port field).

- Ports are labelled as follows
![[Pasted image 20230422170955.png]]

*Note*: CD1 and CD2 both have equal cost distances from the Root Bridge(Acc3) via FE => DP/BP(ALTN) will be determined by the MAC address. CD2 MAC = 0090.0C16.7A9B  and 
CD1 MAC = 0090.0CA0.3902 => CD2_MAC < CD1_MAC => CD2 = DP and CD1 = ALTN

- After removing the blocked ports, the network topology becomes as follows:
![[Pasted image 20230422171303.png]]



### Lab 1

- First we check the vlan info in Acc3 and Acc4 switches, and we get the following info:
```
show vlan brief
```

![[Pasted image 20230718233400.png]]

- Then we check the Root Bridge with the following command:
```
show spanning-tree vlan 10
```

![[Pasted image 20230718233911.png]]

=> Acc3 is the Root bridge which of course is not optimal => All the ports in Acc3 will be marked as Designated Ports.  The above command gives us also the port info. Putting together all the info, we get the following:

![[Pasted image 20230718234534.png]]

- After removing the Blocking(ALTN) ports we get the following topology:

![[Pasted image 20230718234621.png]]

From the above topology, we can see that to get to 203.0.113.0/24, traffic from PC2 will follow the path `Acc4 -> CD2 -> Acc3 -> CD1 -> R1`, which is not the shortest path. We can also verify this as follows:
- Login to PC2 and clear the ARP cache:
```
arp -d
```

- Login to Acc4 and enter the command
```
show mac-address-table
```

![[Pasted image 20230719000330.png]]
We see that it goes through F0/24 to CD2 => We now login to CD2 and do:
```
show mac-address-table
```

![[Pasted image 20230719000456.png]]

We see that it goes through F0/21 to Acc4 => We now login to Acc3 and do the same:
![[Pasted image 20230719000557.png]]

We see that it goes through F0/24 to CD1 => We now login to CD1 and do the same ....
