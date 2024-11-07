 Assume we have the following network topology:
![[Pasted image 20230422005657.png]]

- Switches send **Bridge Protocol Data Units(BPDU)** to detect other switches and potential loops. The switch will not forward any traffic until it's certain that it is loop free.
- BPDU contains the switch's bridge id which uniquely identifies the switch on the LAN. Bridge ID contains the switch's unique MAC address and its administrator defined priority.
- The Bridge ID is then used to select the **Root Bridge**, where the switch with the lowest priority is preferred, or in case of a tie, the bridge with the lowest MAC address.

**Spanning Tree Example**

![[Pasted image 20230422104904.png]]

- In the example above, since we have not set the priority manually, all the switches will have the same priority(32768) => CD1 is selected as the **Root Bridge**, since it has the lowest MAC address.
- A spanning tree does not do load balancing. In case of equal cost paths towards the Root Bridge, it will select the neighbor switch with the lowest Bridge ID.
- After selecting the Root Bridge and the Root Port we then proceed to select the Designated Ports.
- Root Ports point towards the root Bridge while Designated Ports point away from it => All the ports on the Root Bridge are always the Designated Ports
- The Designated ports are the most direct paths to and from the Root Bridge => they will always transition to a forwarding state.
- CD1 is selected as the Root Bridge. Then, we assign the root ports to each Switch. The Root Port is defined as the switch's exit interface on the lowest cost path to the Root Bridge. For example, in the above topology, the root ports are denoted with yellow circles as follows:
![[Pasted image 20230716183032.png]]
- The ports opposite the Root Ports are called Designated Ports and are denoted with purple circles below. After selecting the Root Ports and the Designated Ports, we end up with the following:
![[Pasted image 20230422111531.png]]
- The Root Ports and Designated Ports are the most direct paths to and from the Root Bridge so they will transition to a forwarding state.

- On the remaining interfaces we do the following:
	- Pick the switch with the lowest cost path to the Root Bridge(use the Bridge ID in case of a tiebreaker)
	- The port on this switch will be a Designated Port
	- The port on the opposite end will be a Blocking Port

- The blocking port continues to send BPDU which will be used in case of a failover, but blocks all other traffic.

The only interfaces that don't have a Root and Designated Port assigned are the F0/21 in Acc3 and F0/24 in Acc4. Without the STP protocol, they would end up forming a loop. In this case the switch with the lowest cost path to Root Bridge will have a Designated Port and the other switch will have a Blocked Port. Ties are determined by the lowest Bridge ID. 

![[Pasted image 20230422112208.png]]

- Blocked Ports are denoted with red and Designated Ports with purple. Blocked Ports only allow BPDU traffic.

#### Spanning Tree Protocol
1. Determine the Root Bridge
2. All ports in the Root Bridge are Designated Ports
3. Determine the Root Ports on the other switches

After removing the Blocked Ports in the initial topology, we get the following:
![[Pasted image 20230422112817.png]]

We now see that after PC1 sends an ARP request, it will be flooded over the Spanning Tree, but no loops are formed this time.

*Note*: In the above diagram, we see that the routers still form loops. However, the L3 protocols(TTL, HSRP) prevent traffic looping at this layer, so STP are only needed for L2 switches.