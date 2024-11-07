#QoS
***
Queuing is used to manage congestion on routers and switches:
- CBWFQ(Class Based Weighted Fair Queuing) - gives bandwidth guarantees to specified traffic types
- LLQ(Low Latency Queuing) - Same as CBWFQ but uses a priority queue, where the traffic in the priority queue is sent before any other traffic.
- Cisco QoS uses the MQC Modular QoS CLI, which has 3 main sections:
	- Class Maps - define the traffic to take action on
	- Policy Maps - define the action on the selected traffic
	- Service Policies - apply the policy to an interface


#### Example
Assume we have the following network topology:
![[Pasted image 20230730202228.png]]

- We would like to assign `256kbps` for voice calls and `512kbps` for data, and we have to take into consideration that data traffic may sometimes burst above `512kbps`, causing congestion. This behavior can be achieved with the following configuration:
```
class-map VOICE-PAYLOAD
match ip dscp ef
class-map CALL-SIGNALING
match ip dscp cs3
```

```
policy-map WAN-EDGE
class VOICE-PAYLOAD
priority percent 33 - Put the voice packets to the front of the queue and dedicate 1/3 of the bandwidth to voice calls(cannot go higher)
class CALL-SIGNALING
bandwidth percent 5 - dedicate at least 5 percent of the bandwidth to call signalling(dedicated 5% but can go higher)
class class-default(for all the other classes not mentioned above)
fair-queue(a fairer policy than FIFO)
```

```
interface Serial0/0/0
bandwidth 768
service-policy WAN-EDGE
```