- It takes up to 50s to change a port to forwarding state when it becomes active. However we can speed up this process by noticing that a loop cannot be formed in ports where a single device is plugged in. For example, in the  image below, when we connect the PC to the switch we can change the switch port immediately to active and disable ST on that port.

![[Pasted image 20230422142728.png]]

- Assume the PC in the image above is connected to the interface `f0/1`. To transition that interface to an active state, as soon as the PC is connected(since no risk of a loop exists) we use the following commands:

```
int f0/10
spanning-tree portfast
```

- If we've enabled `portfast`, we run the risk of introducing a loop when a new device is added as follows:

![[Pasted image 20230422143243.png]]

To prevent this we add a `BPDU Guard`, which will shut down the port when a `BPDU` packet is received in that port. To enable the `BPDU Guard`, we use the following command:

```
interface f0/10
spanning-tree portfast
spanning-tree bpduguard enable
```

or to have it enabled by default:

```
spanning-tree portfast bpduguard default
```

- When a `BPDU` packet is detected on a `Portfast` interface, the interface will shut down. The manual solution is then to fix the issue and `shut` + `no shut` the interface. We can also enable an automatic solution as follows:
```
SW2(config)# errdisable recovery cause bpduguard
SW2(config)# errdisable recover interval 30 -> checks every 30s
```

- Assume we introduce a new switch to our network and that switch has not been reset to default. Assume also that this switch has a higher priority than our current Root Bridge => This old switch which resides in a non-central part of our network will become the Root Bridge.
- To prevent the above scenario, we enable **Root Guard**. When tthe other switches receives a **BPDU** from this old switch, they will transition to root-inconsistent and the ports connected to the old switch will be blocked. To enable guard root we use the following command:

```
interface fa0.2
spanning-tree guard root
```



![[Pasted image 20230718222224.png]]

*Note*: The switch at the left is the root switch, and assume we want to add the Switch at the bottom. In order to prevent this switch from becoming the Root Bridge, we enable the Root Guard in the switches connected to it. 