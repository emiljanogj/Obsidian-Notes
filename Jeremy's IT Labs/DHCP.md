#DHCP
***
![[Pasted image 20230906000451.png]]

#### DHCP Relay
 ![[Pasted image 20230906001044.png]]
*Note*: In the above configuration the first message is the message `PC1 -> R1`(broadcast messages) and the 2nd message is the message `R1 -> DHCP Server`(unicast message). `R1` serves as the relay agent for DHCP broadcast messages.

#### DHCP Configuration

```
R1(config)# ip dhcp excluded-address {range-of-excluded addresses}
R1(config)# ip dhcp pool {pool-name}
R1(dhcp-config)# network 192.168.1.0 /24
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# domain-name {domain_name}
R1(dhcp-config)# default-router {default-gateway}
R1(dhcp-config)# lease {lease time in days hours minutes} OR
R1(dhcp-config)# lease infinite
R1# show ip dhcp binding
```

#### DHCP Relay Agent Configuration
```
R1(config)# interface g0/1
R1(config-if)# ip helper-address {DHCP-server-IP-address}
```

#### DHCP Client Configuration
```
R2(config)# interface g0/1
R2(config-if)# ip address dhcp
```