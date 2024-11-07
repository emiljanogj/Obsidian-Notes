#Cisco-Security 
***
Password security can be configured through the use of static, locally defined passwords at 3 different levels:
- Console line - accessing user exec mode when connecting via a console line
- VTY line - accessing user exec mode when connecting remotely via Telnet or SSH
- Privileged Exec Mode - entering the 'enable' command


*Note*: Only one admin can connect over a console line at a time so the line number is always 0 as follows:
```
R1(config)#line console 0
R1(config-line)#password {password}
R1(config-line)#login
```

- IOS devices do not accept Telnet connections by default => an IP address and VTY line access must be configured
- L2 Switches are not IP aware. However, they do support a single IP address for management + a default gateway to allow connectivity to other subnets. The switch's management IP is configured as follows:
```
Switch(config)# interface vlan 1
Switch(config-ig)# ip address 192.168.0.10 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if) exit
Switch(config)# ip default-gateway 192.168.0.1
```

- For Telnet access, VTY lines should be allocated as discussed previously. However, there's a limited number of such lines => If all configured lines are used by the admins, additional admins will not be able to login:
```
R1(config)#line vty 0 15 -> range from VTY line0 - VTY line14
R1(config-line)# password {pass}
R1(config-line)# login
```

- Both the console and VTY lines will be timed out after 10' of inactivity. To configure it we use the following:
```
R1(config)#line vty 0 15 -> range VTY line0 - VTY line15
R1(config-line)#exec-timeout 5 30 -> 5' and 30"
```

- We can limit access to VTY lines using Access Lists as follows:
```
R1(config)# access-list 1 permit host 10.0.0.10
R1(config)# line vty 0 15
R1(config-line)# password {password}
R1(config-line)# access-class 1 in
```
*Note*: The above config allows Telnet/SSH from host `10.0.0.10` only.