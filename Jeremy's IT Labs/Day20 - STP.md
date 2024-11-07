Assume we have the following network topology:

![[Pasted image 20230828143416.png]]

The switch with the lowest **Bridge ID** will become the root bridge. Since by default, all switches have a bridge id = 32768 => the bridge with the lowest MAC address will become the **Root Bridge**. In the above topology, that is SW1 => All ports attached to SW1 will become **Designated Ports**, i.e. they are in a forwarding state

![[Pasted image 20230828170618.png]]

From the above diagram, we can now see that the default **Bridge Priority** for VLAN 1 is not 32768 but 32769:
![[Pasted image 20230828170930.png]]
=> The bridge priority can take 16 different values(1st 4 bits) and can only be increased in increments of 4096. 


#### Spanning Tree Protocol

1. One switch is selected as the root bridge based on the root bridge id(MAC address in case of ties). All ports on the root bridge are designated ports
2. Each remaining switch will select on of its interfaces to be its root port. Port across the root port are designated ports. 3 criteria can be used to select the root port ordered by their importance
	1. Lowest root cost
	2. Lowest neighbour bridge id
	3. Lowest neighbor port id
3. Each remaining collision domain will select one interface to be a `designated port`. The other port will be a `non-designated port`, based on the following criteria ordered by importance
	1. Interface on switch with the lowest root cost
	2. Interface on switch with the lowest bridge id 
	3. Interface with the lowest port id


### Example

![[Pasted image 20230828180930.png]]

*Note*: Interface cost is calculated according to the following table:
![[Pasted image 20230828181515.png]]

Spanning Tree Port States

- Blocking State
![[Pasted image 20230828182105.png]]

- Listening State
![[Pasted image 20230828182212.png]]

- Learning State
![[Pasted image 20230828182338.png]]

- Forwarding State
![[Pasted image 20230828182430.png]]

#### Spanning Tree Timers

![[Pasted image 20230828182620.png]]

*Note*: The root bridge sends **BPDU**, and the other switches only forward these **BPDU**s out of their designated ports(no BPDUs forwarded out of the root/blocking ports).
From the above picture, we see that it takes 20sec(Max Age) + 15 sec(listening state) + 15sec(learning state) = 50sec for a blocking port to transition to a forwarding state


#### Portfast
***
- Disable Spanning Tree Protocol to accelerate the time it takes for an interface to go up
- This is why it's a good idea to enable portfast only on access ports, which are connected to end hosts since trunk ports are usually connected to other switches.
- To enable portfast by default we use the following command
```
SW1(config)# spanning-tree portfast default
```
*Note*: This enables portfast on all access ports(not trunk ports).

**BPDU Guard**
***
- When portfast is enabled on an interface, we may still run the risk of a loop being formed in that interface if a switch is latter connected there. To prevent this, we use a feature called **BPDU Guard** as follows:
```
SW1(config)#interface g0/2
SW1(config-if)#spanning-tree bpduguard enable
```
- We can also enable **BPDU Guard** by default on all portfast-enabled interfaces as follows:
```
SW1(config)# spanning-tree portfast bpduguard default
```

- To set the **Bridge Priority**(in increments of 4096) we use the following command:
```
spanning-tree vlan 1 priority {priority-number}
```
*Note*: For VLAN 1, `priority-number` is 24576 for the `root bridge` and `28672` for the `secondary root bridge`.


#### Revision from Flashcards
- 3 fields of the STP **Bridge ID** are:
	- **Bridge Priority** - 4 bits
	- Extended System ID(VLAN ID) - 12 bits
	- MAC Address - 48 bits


## Lab

We have the following network topology:
![[Pasted image 20230829182339.png]]

With the initial configuration:
```
SW1#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.4301.4B81
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0060.2F90.D14A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/3            Root FWD 19        128.3    P2p

VLAN0002
  Spanning tree enabled protocol ieee
  Root ID    Priority    32770
             Address     0001.4301.4B81
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     0060.2F90.D14A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/3            Root FWD 19        128.3    P2p
****

SW2#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.4301.4B81
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.4301.4B81
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/3            Desg FWD 19        128.3    P2p

VLAN0002
  Spanning tree enabled protocol ieee
  Root ID    Priority    32770
             Address     0001.4301.4B81
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     0001.4301.4B81
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/3            Desg FWD 19        128.3    P2p
****

SW3#sh spanning-tree
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.4301.4B81
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0040.0B50.AA56
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Root FWD 19        128.2    P2p
Fa0/3            Desg FWD 19        128.3    P2p

VLAN0002
  Spanning tree enabled protocol ieee
  Root ID    Priority    32770
             Address     0001.4301.4B81
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     0040.0B50.AA56
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/2            Root FWD 19        128.2    P2p
****

SW4#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.4301.4B81
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0090.0C03.2D70
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/2            Altn BLK 19        128.2    P2p

VLAN0002
  Spanning tree enabled protocol ieee
  Root ID    Priority    32770
             Address     0001.4301.4B81
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     0090.0C03.2D70
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/2            Altn BLK 19        128.2    P2p
Fa0/3            Desg FWD 19        128.3    P2p


SW2 is the root bridge for both VLAN1 and VLAN2
```

*NOTE*: In the initial config `SW2` is the `Root Bridge` for both `VLAN1` and `VLAN2`. In the initial config, all the switches have the same `Bridge Priority` => `SW2` is chosen as the `Root Bridge` because it has the smallest MAC address.

In order to load-balance the spanning tree, we make `SW1` the `Root Bridge` of the `Spanning Tree` for `VLAN1` and `SW2` the `Root Bridge` of the `Spanning Tree` for `VLAN2` using the following commands:
```
SW1(config)# spanning-tree vlan 1 root primary
SW1(config)# spanning-tree vlan 2 root secondary

SW2(config)# spanning-tree vlan 2 root primary
SW2(config)# spanning-tree vlan 1 root secondary
```
