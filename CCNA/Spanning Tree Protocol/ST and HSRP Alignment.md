
Assume we have the following network topology:

![[Pasted image 20230422140104.png]]

where CD1 is the Root Bridge.

- Assume we have also configured HSRP for routers R1 and R2. In this case HRSP should be configure to match the Spanning Tree Path => If CD1 is selected as the ST Root Bridge, R1 should be selected as the active router. Otherwise, if R2 was selected as an active router, we would introduce and extra hop(CD1 -> CD2).
- To ensure the CD1->R1 and CD2->R2 pairings + backup availability in case one of the pairs fail, we use the following configurations:

**On R1(using the Router on a Stick approach)**

```
interface g0/1.10
encap dot1 vlan 10
ip address 10.10.10.2 255.255.255.0
no shutdown
standby 1 ip 10.10.10.1
standby 1 priority 110
standby 1 pre-empt

int g0/1.20
ecap dot1 vlan 20
ip address 10.10.10.2 255.255.255.0
no shutdown
standby 1 ip 10.10.10.1
standby 1 priority 90
standby 1 pre-empt
```


**On R2**

```
int g0/1.10
encap dot1 vlan 10
ip address 10.10.10.3 255.255.255.0
no shutdwon
standby 1 ip 10.10.10.1
standby 1 priority 90
standby 1 pre-empt

int g0/1.20
encap dot1 vlan 20
ip address 10.10.10.3 255.255.255.0
no shutdown
standby 1 ip 10.10.10.1
standby 1 priority 110
standby 1 pre-empt
```

**On CD1**

```
spanning-tree vlan 10 root primary
spanning-tree vlan 20 root secondary
```

**On CD2**

```
spanning-tree vlan 20 root primary
spanning-tree vlan 10 root secondary
```

