Assume we have the following network topology:
![[Pasted image 20230417231252.png]]

- We have the following task:
![[Pasted image 20230417231158.png]]
Then we would do the following:
**In Router R1**
```
interface f0/0
ip address 10.10.10.1 255.255.255.0
no shutdown
```

**In Switch S2**
```
interface f0/1
switch mode access
switch access vlan 10
```

On the other hand, in **Switch S1**, we have applied the following config:
```
vlan 10
name Eng
vlan 20
name Sales
interface range f0/1 - 2
switch mode access
switch access vlan 10
interface f0/3
switch mode access
switch access vlan 20
```

```
interface g0/1
switch mode trunk
```

In **Switch S2**
```
int g0/1
switch mode trunk
switch trunk encapsulation dot1q
```

In Switch S2 we configured the interface f0/1 to receive access from VLAN 10. Since interface f0/1 in SW2 is connected to interface f0/0 in R1 => the ip address configured in interface f0/0 of router R1(10.10.10.1) will be the default gateway for the traffic coming from the ENG PCs(subnet: 10.10.10.0 255.255.255.0).

- For the Router on a Stick(assume on router R1 at interface f0/0), we use the following configuration
```
int f0/0
no ip address
no shutdown
int f0/0.10
encapsulation dot1q 10 (+)
ip address 10.10.10.1 255.255.255.0
int f0/0.20
encapsulation dot1q 20 (+)
ip address 10.10.20.1 255.255.255.0
```

On the other hand, we configure switch S2 as follows
```
int f0/1
switchport mode trunk
```

The above configuration ensures that when a packet reaches the Switch S2, the switch will encapsulate it with the VLAN number for the corresponding subnet. It will then forward it to the router R1. Note that in router R1(lines with (+)) we have configured the default gateways for traffic coming from VLAN 10 and VLAN 20 respectively.