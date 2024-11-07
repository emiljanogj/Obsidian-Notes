- It is usually used for internal hosts which need to connect to the Internet but do not accept incoming connections. If the hosts were to accept incoming Internet connections, they would need to be configured with Static Nat([[CCNA/NAT/Static NAT]]).
- It addresses the limitations of the Static NAT, where the outbound public connections remain idle if the pool of public IP addresses is exhausted.

Assume we have again the following scenario:

![[Pasted image 20230501170250.png]]

In this case assume that we can initiate a requests from **INT-S1** for which we will get a response. However, **EXT-S1** cannot initiate a request to **INT-S1** => Whenever **INT-S1** initiates a request, it can just pick a dynamic **IP** from the pool of available public IPs. 

To configure a dynamic NAT, we use the following commands:
- Configure the inside and outside NAT interfaces:
```
int f0/0
ip nat outside


int f1/0
ip nat inside
```
- Set up the IP pool with the following command:
```
ip nat pool Flackbox 203.0.113.4 203.0.113.14 netmask 255.255.255.240
```

- Create an access list to allow traffic to the internal IPs we want to translate
```
access-list 1 permit 10.0.2.0 0.0.0.255
```

- Link the IP pool with the access list as follows:
```
ip nat inside source list 1 pool Flackbox
```

- Print the NAT translation with the following command:
```
show ip nat translation
```

- To remove all dynamic translations we use the following command
```
clear ip nat translation *
```


