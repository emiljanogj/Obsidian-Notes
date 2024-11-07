	#Port_Security  
***
- To re-enable an interface we use the following command:
```
SW1(config)# errdisable recovery cause psecure-violation
```

- To change the time it takes for an `errdisable` interface to go up we use the following command
```
SW1(config)#  errdisable recovery interval 180
```


#### Violation Modes

There are three different violation modes that determine what the switch will do if an
unauthorized frame enters an interface configured with port security.
- **Shutdown**
	- Effectively shuts down the port by placing it in an err-disabled state.
	- Generates a Syslog and/or SNMP message when the interface is disabled.
	- The violation counter is set to 1 when the interface is disabled.
- **Restrict**
	- The switch discards traffic from unauthorized MAC addresses.
	- The interface is NOT disabled.
	- Generates a Syslog and/or SNMP message each time an unauthorized MAC is detected.
	- The violation counter is incremented by 1 for each unauthorized frame.
- **Protect**
	- The switch discards traffic from unauthorized MAC addresses.
	- The interface is NOT disabled.
	- It does NOT generate Syslog/SNMP messages for unauthorized traffic.
	- It does NOT increment the violation counter.

#### Secure MAC Address Aging

- Can be configured using the following command:
```
switchport port-security aging time {minutes}
```

- 2 types of aging:
	- **Absolute** - After the MAC address is learned the aging timer starts and the MAC is removed after the timer expires
	- **Inactivity** - After the secure MAC address is learned the aging timer starts but is reset every time a frame is received from that MAC address
	- Can be configured using the following command:
```
switchport port-security aging type {absolute | inactivity}
```

**Sticky MAC address**

- They will never age out + will be added to the running config
```
SW1(config-if)# switchport port-security mac-address stricky
```

- To show all secure MAC addresses on the switch we use the following command:
```
show mac address-table secure
```

- To show a summary of errdisable recovery information we use the following command:
```
SW1# show errdisable recovery
```

#### DHCP Option 82

- Known as 'DHCP relay agent information option'. Is usually added by DHCP relay agents as they forward DHCP messages to the DHCP server
- When DHCP snooping is enabled in Cisco switches, the Option 82 will be added even if the switch is not a relay agent. This may result in dropped packets as Cisco switches will drop DHCP messages with Option 82 received on untrusted ports

![[Pasted image 20230913122731.png]]
*Note*: In the network topology above, SW2 will discard DHCP messages received by SW1 since by default they will have the Option 82 enabled.
- To prevent this we disable Option 82 on switches using the following command:
```
SW1(config)# no ip dhcp snooping information option
```