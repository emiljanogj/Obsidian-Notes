### PortGuard and BPDUGuard

![[Pasted image 20230719232924.png]]

*Note*: In the above topology we can disable the Spanning Tree in interface G0/1 of R1, interface G0/1 of R2 since they are connected to a router, and we know that a router doesn't forward traffic => no possibility of a loop being formed. Same for F0/1 of Acc3 and F0/1 of Acc4 since they are connected to end hosts. This is done as follows
```
int {interface-number}
spanning-tree portfast
```

We also need to add an option to disable this port when it detects a BPDU as follows:
```
int {interface-number}
spanning-tree bpduguard enable
```

