#QoS 
***
**IP Phones**
- They have an internal 3-port switch
	- 1 port is the uplink to the external switch
	- 1 port is the downlink to the PC
	- 1 port connects internally to the phone itself
*Note*: This allows the PC and the phone to share a single switch port. Traffic from the PC passes through the IP phone to the switch
![[Pasted image 20230908172920.png]]

- In this case, we should send the data traffic untagged and tag the voice traffic as follows:
```
SW1(config)# interface g0/0
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config-if)# switchport voice vlan 11
```
*Note*: In this case PC1 will send traffic untagged and the switch will tell the phone to tag its traffic in VLAN 11


#### POE
- Allows Power Sourcing Equipment(PSE) to provide power to Powered Devices(PD) over an Ethernet cable
- Typically PSE is a switch and the PDs are IP phones, WAP etc
![[Pasted image 20230908174815.png]]

- When a device is connected to a PoE-enabled port, the PSE(switch) sends low power signals, monitors the response and determines how much power the PD needs
- **Power policing** can be configured to prevent a PD from taking too much power
	- **power inline police action err-disable** - the interface will be put into *error-disabled* state and can be re-enabled with `shutdown` followed by `no shutdown`
	- **power inline police action log** - the interface is not shut down but it just sends an error message to Syslog

#### QoS

**QoS** is used to manage the following characteristics of network traffic:
1) Bandwidth
- The overall capacity of the link, measured in bits per second (Kbps, Mbps, bps, etc)
- OoS tools allow you to reserve a certain amount of a link's bandwidth for specific kinds of traffic. For example: 20% voice traffic, 30% for specific kinds of data traffic, leaving 50% for all other traffic.
2) Delay
- The amount of time it takes traffic to go from source to destination = one-way delay
- The amount of time it takes traffic to go from source to destination and return = two-way delay
3) Jitter
- The variation in one-way delay between packets sent by the same application
- IP phones have a 'jitter buffer' to provide a fixed delay to audio packets.
4) Loss
- The % of packets sent that do not reach their destination
- Can be caused by faulty cables.
- Can also be caused when a device's packet queues get full and the device starts discarding packets.

#### QoS Queuing 
- When the queue is full => the packet cannot be put there so it's dropped. This is called **tail drop**
- **Tail drop**  can lead to **TCP global synchronization**
- **TCP Sliding Window**
	- A host increases the rate at which the packet is sent
	- When a packet is dropped, the rate will be reduced
	- The packet will be sent with the reduced rate
- When **tail drop** occurs, all TCP hosts will slow down the rate at which they send packets
- After that, they will decrease the rate again => congestion
- After a certain delay, all TCP routers will increase their transmission rate again and the problem will occur again. The delay will expire at the same time in all the routers and that's the reason why it's called the **TCP global synchronization**. 

![[Pasted image 20230909021103.png]]

- A solution to prevent the above issue is **Random Early Detection**. When the amount of traffic in the queue reaches a certain threshold, random packets are dropped
- Only the TCP flows that were affected by the drop will decrease their rate, thus avoiding **TCP global synchronization**
- QoS improves **RED** to **Weighted RED(WRED)**

**Quiz**
![[Pasted image 20230909021549.png]]

**QoS Classification**
- There are 4 main methods that can be used for classification:
	- **ACL**
	- **NBAR** - performs a deep packet inspection L3-L7 to identify the kind of traffic
	- **PCP** - A field of the 802.1Q tag
	- **DSCP** - can be used to identify high/low priority traffic

**PCP**
- 3 bits => 8 possible values

![[Pasted image 20230909111440.png]]

- It can be used over:
	- trunk links
	- access links with a voice VLAN

**L3 Classification**

- IPv4 header has the following field(**DSCP**) that can be used to classify the traffic
![[Pasted image 20230909111944.png]]
- **DSCP** is 6 bits long => 64 possible values
- **DSCP** contains the following standard markings:
	- **Default Forwarding(DF)** - best effort traffic
	- **Expedited Forwarding(EF)** - low loss/latency/jitter traffic
	- **Assured Forwarding(AF)** - 12 standard values
	- **Class Selection(CS)** - 8 values. Ensures backwards compatibility with **IPP(older DSCP version)**

**DF**
![[Pasted image 20230909113438.png]]

**EF**
![[Pasted image 20230909113454.png]]

**AF**
![[Pasted image 20230909113640.png]]
- Higher drop precedence => more likely to drop packet during congestion 
- Notation -> **AFXY** where X is the decimal for the class and Y is for the drop precedence

![[Pasted image 20230909114053.png]]

![[Pasted image 20230909121617.png]]
*Note*: X - priority, Y - drop precedence

- DSCP number = 8X + 2Y 

- RFC offers the following recommendations for **DSCP** priority values:
	- Voice traffic - EF=46
	- Interactive video - AF4x
	- Streaming video - AF3x
	- High priority data - AF2x
	- Best Effort - DF

#### Queuing/Congestion management
- Multiple queues are used by the QoS
- Prioritization allows the scheduler to give certain queues more priority than others
- A common scheduling method is *weighted round-robin*
- **CBWFQ(Class-Based Weighted Fair Queuing)** - uses a the *weighted round-robin* while guaranteeing each queue a certain percentage of the interface's bandwidth
- *Round-robin* is not optimal for voice/video traffic since this traffic is still delayed and jittered even though it is guaranteed a min amount of bandwidth
- **LLQ(Low Latency Queuing)** - Designated one or more queues as priority queues => the scheduler will always take the next packet from that queue until it's empty. However this may mean that other queues are starved
- To solve the **LLQ** starvation issue we use *Policing* to control the amount of traffic allowed in the strict policy queue so that it can't take all of the link's bandwidth

#### Shaping and Policing
- *Shaping* - buffers traffic in queue if the traffic rate goes over the configured rate
- *Policing* - drops traffic if the rate goes over the configured rate.
	- *Note*: We can configure a burst rate to allow burst traffic for a short period of time
	- It also has the option of re-marking the traffic instead of dropping it


#### QoS Lab

- In the lab we use the following 2 commands:
```
R1(config)# class-map {class-name}
R1(config-cmap)# match protocol {protocol name} # e.g match protocol ICMP
```
- We then define the following policy:
```
R1(config)# policy-map {policy-name}
R1(config-pmap-c)# priority percent {percentage}  (1)
R1(config-pmap-c)# bandwidth percent {percentage} (2)
```
*Note*: The difference between command (1) and command (2) is that (1) places the traffic in a strict priority queue => it guarantees a 10% bandwidth in the interface where the policy was applied, while (2) means that this traffic will use at most 10% of the interface's bandwidth

- Another useful command to set the `DSCP` value is:
```
R1(config-pmap-c)# set ip dscp {afXY|dscpX}
```
where X/Y are variables that determine the priority class and the drop precedence

- To apply the policy we then use the following command:
```
R1(config)# int g0/0/0
R1(config-if)# service-policy output {policy-name}
```