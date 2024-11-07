``
#Port_Security
***

- To increase the number of devices allowed to connect to an interface(from default=1), we use the following commands:

```
interface {interface-name}
switchport port-security maximum {max number of allowed devices}
```

- We can also lock down the port to a specific host as follows:
```
interface {interface-name}
switchport port-security
switchport port-security mac-address {MAC address}
switchport port-security maximum {max device number}
```

- If we want to lock a large number of interfaces to the devices they're currently connected to, we use the following commands:
```
interface {interface-name}
switchport port-security
switchport port-security mac-address sticky
# copy the running configuration to the startup config
show port-security address
```


#### Security Violation Actions

3 options when an unauthorised MAC address sends traffic to the port:
- **Shutdown (Default)** - The interface is placed into the error-disabled state and blocks all traffic
- **Protect** - Only traffic from allowed addresses is forwarded
- **Restrict** - Traffic from unauthorised addresses is dropped, logged and the violation counter is increased while the traffic from allowed addresses is forwarded normally.
- To set the port-security we use the following command:
```
SW1(config)# int {interface-number}
Sw1(config-if)# switchport port-security violation protect|restrict
```
- In case the violation is set to shutdown and the violation occurs we should do the following:
	- Physically remove the host with the offending MAC address
	- Manually restart the interface
- Or we can set an option to have the interface restart automatically:
```
SW1(config)# errdisable recovery cause psecure-violation
SW1(config)# errdisable recovery interval 600
```