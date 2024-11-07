	
 We can define port security with the following commands:

```
int {interface-name}
switchport port-security
```

- In this case Port Security only allows one MAC address to transmit on the port
- The current MAC address can be disconnected and replaced because the port is not locked down to a single MAC address. If a shared device is connected and multiple hosts try to transmit the port will shut down.
- 3 options when unauthorised MAC address sends traffic to the port:
	- Shutdown - the interface is placed into the error-disabled state and all traffic is blocked
	- Protect - Traffic from unauthorised MAC addresses is droped
	- Restrict - Traffic from unauthorised MAC address is dropped, logged and violation counter is incremented
- To set the volation action we use the following commands:

```
int {interface-name}
switchport port-security violation {shutdown |  protect | restrict}
```
- To enable auto-recovery after shutdown, we define the following configuration:
```
errdisable recovery cause psecure-violation
errdisable recovery interval 600 (+)
```

*Note*: The recovery interval in (+) is set in seconds.
- *psecure-violation* - port-security violation