- Using a separate interface on the router for each VLAN is not efficient since we will run out of interfaces at some points. *Router on a Stick* is a solution to prevent that.

![[Pasted image 20230417131235.png]]

Note in the above picture, in the interface f0/1, we create 2 sub-interfaces f0/1.10 and f0/1.20

The above topology works as follows:

- Say the `ENG PC1` wants to send out traffic. It sees that its default gateway is configured as `10.10.10.1`. It then sends an ARP request to find the MAC address of the router. 
- We have configured the interface `f0/1` as trunk in SW1 => The switch will add a Dot1q header with wthe correct VLAN number, so the router knows where to send the response back.

We configure the router R1 as follows:
```
FastEthernet 0/1
no ip address
no shutdown
interface FastEthernet 0/1.10
encapsulation dot1q 10
ip address 10.10.10.1 255.255.255.0
interface FastEthernet 0/1.20
encapsulation dot1q 20
ip address 10.10.20.1 255.255.255.0
ip route 0.0.0.0 0.0.0.0 203.0.113.2 (+)
```
(+) is the route to access the internet

The Switch needs to be configured as follows:
```
int f0/1
switchport mode trunk
switchport trunk encapsulation dot1Q
```