- Allows us to add/edit/delete VLANs on switches configured as VTP servers and have other switches configured as VTP clients synchronize their VLAN database with them. This is quite convenient if we manage a large set of switches
- There are 3 VTP modes:
	- VTP Server - can add/edit/delete VLANs. We can have multiple VTP servers and they will synchronize their VLAN database from another server with a higher revision number
	- VTP Client - cannot add/edit/delete VLANs. Can only synchronize their VLAN database from a server with the highest revision number
	- VTP Transparent - Does not synch its VLAN database with any other switches but can pass on VTP info to VTP clients

### LAB

- Setting the VTP domain name
```
vtp domain {domain-name}
```
- Setting the VTP mode
```
vtp mode {client|server|transparent}
```
- Checking VTP status
```
show vtp status
```

**Important Note**: VTP only adds/edits/deletes VLANs automatically. Configuration of these VLANs still needs to be done manually
