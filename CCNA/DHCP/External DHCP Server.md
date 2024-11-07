Assume we're using the following network topology:

![[Pasted image 20230419214058.png]]

- We're now using an external DHCP + DNS server at 10.10.20.10. Assume PC2 is powered on and requests an IP. It sends the DHCP request(broadcast traffic) to R1. By default routers do not forward broadcast traffic, so R1 simply drops it => the DHCP request will never reach the DHCP server. To prevent this from happening we use the following commands:
```
interface f0/1
ip helper-address 10.10.20.10
```
