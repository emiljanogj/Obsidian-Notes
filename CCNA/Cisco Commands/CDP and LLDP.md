- **CDP** - Cisco proprietary L2 protocol which is used to share info with other directly connected Cisco devices
```
Switch(config)# cdp run
Switch(config)# no cdp run -> disables CDP globally
Switch(config-if)# no cdp enable -> disables CDP on interface level
Switch# show cdp
Switch# show cdp neighbors
Switch# show cdp neighbors detail
```

- **LLDP** is also an L2 protocol
	- supported only on physical interfaces => it can detect up to 1 device per port
	- it can discover Linux servers also
```
Switch(config)# lldp run
Switch(config)# no lldp run
Switch(config-if)# no lldp transmit
Switch(config-if)# no lldp receive
Switch# show lldp
Switch# show lldp neighbors
Switch# show lldp neighbors detail
```

