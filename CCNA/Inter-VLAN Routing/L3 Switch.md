 ![[Pasted image 20230417161827.png]]

- The above image shows how a L3 Switch works. We have configured the switch interfaces connected to the end hosts in access mode and the L3 switch knows that the traffic to a particular VLAN needs to be sent to the corresponding *Switched Virtual Interfaces(SVI)*. 
- We configure the switch as follows
```
ip routing
interface vlan 10
ip address 10.10.10.1 255.255.255.0
interface vlan 20
ip address 10.10.20.1 255.255.255.0
```
- In the above picture, we could also configure the switch to forward internet traffic to the router as follows:
**Switch Config(SW1)**
```
interface FastEthernet 0/1
no switchport
ip address 10.10.100.1 255.255.255.0
ip route 0.0.0.0 0.0.0.0 10.10.100.2
```

**Switch Router(R1)**
```
interface FastEthernet 0/1
ip address 10.10.100.2 255.255.255.0
interface FastEthernet 0/2
ip address 203.0.113.1 255.255.255.0
ip route 0.0.0.0 0.0.0.0 203.0.113.2
ip route 10.10.0.0 255.255.0.0 10.10.100.1
```