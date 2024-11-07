#SNMP
***
- Open standard for network management
- SNMP Manager(the SNMP server/NMS) can collect info from the SNMP agent, which is the SNMP software which runs on managed devices such as routers and switches.
- The SNMP Manager can pull the information from the device('Get') or the device can push it to the server('Trap')
- Data variables on SNMP managed systems are organized in a Management Information Base(MIB), which is shard by both the SNMP Manager and Agent.

![[Pasted image 20230730122817.png]]

**SNMP Versions**
- v1 - plain text authentication and no support for bulk retrieval
- v2 - uses plain text Community strings and support bulk retrieval
- v3 - supports strong authentication and encryption  

**Community Strings**
- Matching community strings need to be set on both sides for the manager and agent to communicate
- The read-only(ro) community is used by the manager to read info
- The read-write(rw) community is used by the manager to set information

```
R1(config)#snmp-server community Flackbox1 ro
R1(config)#snmp-server community Flackbox2 rw

R1(config)#snmp-server host 10.0.0.100 Flackbox1
R1(config)#snmp-server enable traps config
```
*Note*: The last 2 lines enable SNMP traps => whenever a configuration change happens in R1, a trap will be sent to the NMS system at `10.0.0.100` using the `ro` Community string.

**Security Concerns**
- Most devices use a default ro Community string of 'public' and a default rw Community string of 'private'
- From the above point, we see that it's best practice to disable SNMP on devices where it's not used
- Otherwise, always use SNMPv3 if supported


#### SNMPv3 Configuration

- Support both authentication and encryption
- Security model works with both users and groups
- A matching user account needs to be set on both the NMS server and the network device
- Supports 3 security levels:
	- noAuthnoPriv
	- AuthnoPriv
	- AuthPriv
- SNMPv3 Configuration Groups:
	- Access - can be used to reference an access list which limits the device to communicating with the IP address of the NMS server only
	- Contexts - used on Switches to specify which VLANs are accessible via SNMP
- SNMPv3 Configuration Views:
	- Can be used to specify what info is accessible to the NMS server
	- If no read view is specified => all MIB objects are accessible to read
	- If no write view is specified => no MIB objects are accessible to write
- Setting the authentication and encryption passwords is done as follows:
![[Pasted image 20230730125429.png]]
