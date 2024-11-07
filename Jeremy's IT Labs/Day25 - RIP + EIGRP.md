#Routing 
***
### RIP

**RIPV1:**
- only advertises classful addresses (Class A, Class B, Class C)
- doesn't support VLSM, CIDR
- doesn't include subnet mask information in advertisements (Response messages
	- 10.1.1.0/24 will be become 10.0.0.0 (Class A address, so assumed to be /8)
	- 172.16.192.0/18 will become 172.16.0.0 (Class B address, so assumed to be /16)
	- 192.168.1.4/30 will become 192.168.1.0 (Class C address, so assumed to be /24)
*Note*: To disable the auto summary discussed in the last point, we run the `no auto-summary` command in the route.

**RIPV2:**
- supports VLSM, CIDR
- includes subnet mask information in advertisements
- messages are multicast to 224.0.0.9

*Note(Difference between multicast and broadcast)*: 
	- **Broadcast** messages are delivered to all devices on the local network
	- **Multicast** messages are delivered only to devices that have joined that specific multicast group
- If there are no `RIP` neighbors connected to an interfaces, we can configure it as a passive interface
```
R1(config-router)# passive-interface {interface}
```

- We can also add a default route to a router as follows:
```
R1(config)#ip route 0.0.0.0 0.0.0.0 203.0.113.2
```
We can propagate this info to the other routers using the following command:
```
R1(config-router)#default-information originate
```

### EIGRP
- It uses a wildcard mask instead of a subnet mask. For example, to enable `EIGRP` for a `255.255.255.240` network we use the following command:
```
R1(config-router)# network 172.16.1.0 0.0.0.15
```

**Router ID**
- It is set as follows depending on the order of priority
	- Manual configuration
	- Highest IP address on a loopback interface
	- Highest IP address on a physical interface


#### Wildcard Masks

![[Pasted image 20230831194947.png]]

We can see from the above image that it works as follows:
- Only the `0` bits need to match(in the above case only the 1st bits)

*Note*: `EIGRP` messages are multicast in `224.0.0.10` while `RIP` messages are multicast `224.0.0.9`


#### EIGRP Metric

- By default, EIGRP uses bandwidth and delay to calculate metric.
```
(dK1 * bandwidth + (K2 * bandwidth) / (256 - load) + K3 * delay! * [K5 / (reliability + K4)]) * 256
```
The default 'K' values are K1 = 1, K2 = 0, K3 = 1, K4 = 0, K5 = 0