The following are the steps we need to follow to configure the DHCP server:

- Restrict the DHCP server from handing reserved IP addresses(we sepcify the lower and upper bound of a scope)
```
ip dhcp excluded-address 10.10.10.1 10.10.10.10
```
- Creating the dhcp pool
```
ip dhcp pool {dhcp-pool-name}
```
- Specify the network
```
network 10.10.10.0 255.255.255.0
```
- Specify the default GW
```
default-router 10.10.10.1
```
- Specify the DNS server
```
dns-server 10.10.20.10
```
- To see the IP address assigned by the DHCP server we use the following command
```
show ip dhcp binding
```
