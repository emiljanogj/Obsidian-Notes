Assume we have the following network topology:

![[Pasted image 20230424235639.png]]

Note that PC1 and PC2 are in subnet `10.10.10.0/24` while the DHCP Server is in subnet `10.10.20.0/24` at `10.10.20.10`. When the PCs send a DHCP request, the packet will be sent as a broadcast traffic, and we know that routers don't forward the broadcast traffic. To enable routers to forward the broadcast traffic to the DHCP server we use the following commands:
```
R1(config)# interface f0/1
R1(config-if)# ip helper-address 10.10.20.10
```

Assume now we connect a new device to `SW1` which is configured as a DHCP Server as shown below:
![[Pasted image 20230425001210.png]]
In this case, PC1 and PC2 will receive their IPs from the Rogue DHCP Server. However, its DG and DNS are not correctly configured, so these PCs will be isolated from the rest of the network.

To avoid this issue, we enable **DHCP Snooping**, where we define a port where we expect to receive the **DHCP Server** responses as follows:

```
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 10
SW1(config)# int f0/1
SW1(config-if)# ip dhcp snooping trust
```

With the above code, we configure **SW1** to always expect the DHCP Server responses at interface F0/1 => it will ignore the DHCP responses received at F0/4.