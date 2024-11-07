#Spanning-Tree 
***
### RST Port States

![[Pasted image 20230829225444.png]]

### Port Roles
- The **root port** role remains unchanged in RSTP
	- The port that is closes to the root bridge becomes the root port for the switch
	- The root bridge is the only switch that doesn't have a root port
- The **designated port** role remains unchanged in RSTP
	- The port on a segment that sends the best BPDU is that segment's designated port
- The **non-designated port** is split in 2 separate roles
	- alternate port role
	- backup port role

*Note*: Backup ports occur when two interfaces are connected to the same collision domain via a hub as follows:
![[Pasted image 20230829230936.png]]
The `BPDU` that SW2 sends via it's designated port is received in the backup port after it's forwarded from the `Hub`.

#### Example

![[Pasted image 20230829231541.png]]


**Important Difference between STP and RSTP**

- In `STP` only the `Root Bridge` sent `BPDU`s while the other switches forwarded the `BPDU`s from the designated ports
- in `RSTP` all the switches send their own `BPDU`s every 2s


### RSTP Link Types

- **Edge**: a port that is connected to forwarding, without negotiation => they are exactly the same as a `Portfast`
- **Point-to-point**: a direct connection between two switches
- **Shared**: a connection to a hub => Must operate in half-duplex mode



#### Quiz

![[Pasted image 20230829233926.png]]
*Note*: The only mistake is in the Switch in the bottom right. We cannot have a link `D-D` on both sides and `G0/1` in `SW2` is the designated port since it is closer to the `Bridge Root`(SW1).

*Note*: In `RSTP` the port cost now starts from `2 000 000` for `10 Mbps` and it decreases by a factor of 10 for every multiple of 10s increase in speed.

## Lab

Assume we have the following network topology:
![[Pasted image 20230830134558.png]]

*Note*: We said before that we have one designated port per collision domain. For `SW1` both `F0/2` and `F0/3` are connected to the hub => They belong to the same collision domain, and that's why 1 is a designated port and the other is a backup port.

**Full Port States**
![[Pasted image 20230830135951.png]]
*Note*: In the above image, `F0/1` should be alternating and `F0/2` should be root, since they both have the same root cost, but `F0/1` connects `SW4` to `SW3` which has a lower `Bridge ID` than `SW2`.