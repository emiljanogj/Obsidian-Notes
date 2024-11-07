![[Pasted image 20230420120206.png]]

- Very similar to FHRP(active-standby)
- Routers send hello messages to each other over their HSRP interfaces and if the standby router stops receiving hello from the active router, it will transition to be the active router.
- The router with the highest IP address will be prefered as the active one
- We can pick which one is active by settting the priority
- If pre-emption is not enabled(default), the lower priority router will remain active when the failed router comes back to life. This is preferred since the higher priority router may be constantly flapping.

#### HSRP Configuration

```
interface g0/1
ip address 10.10.10.2 255.255.255.0
no shutdown
standby 1 ip 10.10.10.1
standby 1 priority 110
standby 1 preempt
standby version 2
```

#### Loadbalancing HSRP

To loadbalance we simply define 2 standby groups, and in the first group, we give the higher priority to the 1st router and in the 2nd group we give the highest priority to the 2nd router.

```
interface g0/1
ip address 10.10.10.2 255.255.255.0
no shutdown

standby 1 ip 10.10.10.1
standby 1 priority 110
standby 1 preempt

standby 2 ip 10.10.10.254
standby 2 priority 90
```

- To print the HSRP status we use the following command:
```
show standby
```