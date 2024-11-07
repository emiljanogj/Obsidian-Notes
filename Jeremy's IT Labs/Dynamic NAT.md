#NAT 
***
- The router dynamically maps inside local addresses to inside global addresses as needed
- An ACL is used to identify which traffic should be translated
	- If the source IP is permitted by the ACL it will be translated
	- If the source IP is denied, it will not be translated
![[Pasted image 20230908115610.png]]


#### Dynamic NAT Configuration

![[Pasted image 20230908142929.png]]

#### PAT Configuration I
```
R1(config)# int g0/1
R1(config-if)# ip nat inside

R1(config-if)# int g0/0
R1(config-if)# ip nat outside

R1(config)# access-list 1 permit 192.168.0.0 0.0.0.255

R1(config)# ip nat pooli POOL1 100.0.0.0 100.0.0.3 prefix-length 24

R1(config)# ip nat nat inside source list 1 pool POOL1 overload
```


#### PAT Configuration II
```
R1(config)# int g0/1
R1(config-if)# ip nat inside

R1(config)# int g0/0
R1(config-if)# ip nat outside

R1(config)# access-list 1 permit 192.168.0.0. 0.0.0.255

R1(config)# ip nat inside source list 1 interface g0/0 overload -> Note the overload keyword
```

**Note(ACL and NAT)**: When using ACL + NAT, permit means translating the IP and deny means not translating the IP.