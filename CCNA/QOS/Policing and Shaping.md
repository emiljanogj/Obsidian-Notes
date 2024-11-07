#QoS
***
- Traffic shaping buffers any excess traffic so the overall traffic stays within the desired rate limit
- Traffic policing is stricter that traffic shaping in that it drops the excess traffic completely
- For example, if a link has a bandwidth of 100 Mbps and the customer has only paid for 10 Mbps in their SLA, we need to introduce policing to limit the traffic to 10 Mbps.
- We can also police junk traffic(i.e. peer to peer file sharing) by marking it with DSCP 8(CS1)


#### Shaping Example

```
class-map VOICE
match ip dscp ef

class-map VIDEO
match ip dscp af41

class-map SIGNALLING
match ip dscp cs3

policy-map NESTED
 class VOICE
  priority 1024 -> move to priority queue and guarantee 1 Mbps
 class VIDEO
  priority 3072 -> move to priority queue and guarantee 3 Mbps
 class SIGNALLING
  bandwidth 128 -> at least 128 Kbps for signalling
 class class-default
  fair-queue -> Fair queue for all other traffic, i.e. traffic that takes lower bandwidth should be passed to front of the queue
```

*Note*: We can only apply one policy to the interface. If we want to apply more, we have to nest the policies as follows:
```
policy-map WAN-EDGE
 class class-default
 shape average 10000000
 service-policy NESTED
```

- Apply the policy to the interface as follows:
```
Interface FastEthernet0/0
 service-policy out WAN-EDGE
```
