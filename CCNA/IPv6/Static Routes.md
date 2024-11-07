- Works very similarly to the IPv4 static routing, but each router has a separate routing table for IPv4 and IPv6.
- The main difference is that IPv4 is enable by default on a Cisco IOS router while the IPv6 routing is disable by default and need to be enable using the following command:
```
ipv6 unicast-routing
```

 - To display the IPv6 routes we use the following command:
```
show ipv6 route
```

- IPv6 static routes are added as follows:
![[Pasted image 20230726082727.png]]

- Summary(/48 -> 255.255.0.0) and default routes(::/0 -> 0.0.0.0 0.0.0.0) are added as follows:
![[Pasted image 20230726083041.png]]

