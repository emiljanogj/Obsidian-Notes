.- CDP is Cisco proprietary
- LLDP is industry standard protocol(**IEEE 802.1AB**)


#### CDP
- Enabled on Cisco devices
- CDP messages are periodically sent to multicast MAC address `0100.0CCC.CCCC`
- CDP messages are sent once every 60s
- CDP holdtime is 180s. If a message isn't received from a neighbor for 180s, the neighbor is removed from the CDP neighbor table
- Works on Layer2 so no need for IP
- Enabled by default
- To display the CDP neighbors we use the following command:
```
R1# show cdp neighbors
```
The output we get is as follows:
![[Pasted image 20230903232304.png]]
*Note*: Local interface is the interface ID on this device, while Port ID is the interface ID on the neighbors

- To enable/disable CDP globally we use the following command:
```
R1(config)# [no] cdp run
```

- To enable/disable CDP on specific interfaces:
```
R1(config-if)# [no] cdp enable
```

- To configure the CDP time:
```
R1(config)# cdp timer seconds
```

- To configure the CDP holdtime:
```
R1(config)# cdp holdtime seconds
```

- To enable/disable CDPv2:
```
R1(config)# [no] cdp advertise-v2
```

#### LLDP

- A device can run CDP and LLDP at the same time.
- LLDP messages are periodically sent to multicast MAC address `0180.C200.000E`.
- When a device receives an LLDP message, it processes and discards the message. It does
no forward it to other devices.
- By default, LLDP messages are sent once every• Adevice can run CDP and LLDP at the same time.
- By default, the LLDP holdtime is 120 seconds.
- LLDP has an additional timer called the 'reinitialization delay'. If LLDP is enabled
(globally or on an interface), this timer will delay the actual initialization of LLDP. **2 seconds**
by default.
- Only supported on physical interfaces + can discover up to 1 server per port


**LLDP Commands**

- Enable LLDP globally
```
R1(config)# lldp run
```

- Enable LLDP on specific interfaces(tx):
```
R1(config-if)# lldp transmit
```

- Enable LLDP on specific interfaces(rx)
```
R1(config-id)# lldp receive
```

- Configure the LLDP timer
```
R1(config-if)# lldp timer seconds
```

- Configure the LLDP holdtime
```
R1(config)# lldp holdtime seconds
```

- Configure the LLDP reinit timer
```
R1(config)# lldp reinit seconds
```