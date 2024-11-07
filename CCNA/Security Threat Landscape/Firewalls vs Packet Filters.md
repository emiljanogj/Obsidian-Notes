f- In stateful firewall, the incoming traffic that's coming as a response to an outgoing request is always accepted. For example:
![[Pasted image 20230727233931.png]]

![[Pasted image 20230727234009.png]]

However, if the traffic was not initiated by the host with IP `10.10.10.10` first, it will be blocked as shown below:
![[Pasted image 20230727234109.png]]


#### Packet Filters

- They do not maintain a connection table => they affect traffic only in 1 direction
- ACLs are usually used to block traffic and hence provide an extra layer of security
- All traffic is allowed if no ACL is applied
- We can also add the `established` keyword to an ACL rule as follows:
```
access-list 100 permit tcp any eq 80 10.10.10.0 0.0.0.255 established
```
*Note*: The `established` keyword doesn't make the routers a stateful firewall. It just checks for the 'Ack' flag in the return traffic, which an attacker can set manually.

**Example of Usage of ACL in Conjunction with Firewalls and IPS**

![[Pasted image 20230727235238.png]]

*Note*: DeptA and DeptB can use the switch to forward traffic to one another without any restrictions. To prevent this, we can create an ACL rule in the switch.