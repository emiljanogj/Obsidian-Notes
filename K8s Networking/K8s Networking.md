- When we deploy a pod, the following things happen:
![[Pasted image 20230514163236.png]]
1. The pod gets its own network namespace
2. An IP address is assigned
3. Any containers in the pod share the same networking namespace so they can see each other in localhost

#### How do Pods communicate with one another
- To enable communication between 2 pods, we create a bridge connection between each pod's namespace and the root namespace as follows:

![[Pasted image 20230514163921.png]]

When Pod A wants to communicate to Pod B, the following happens:
1. It sends a packet to its default interface `eth0`
2. The packet is then forwarded to the `cni0` interface, which is connected to `eth0` via `veth0`.
3. `cni0` then sends an `ARP` packet to find the `MAC address` of the destination as follows:
![[Pasted image 20230514164715.png]]

4. It then gets a response from `Pod B`, which is stored in the bridge `ARP cache` as follows:
![[Pasted image 20230514164910.png]]

5. The `MAC -> IP` mapping is stored in the `ARP cache`, and then bridge forwards the packet to `veth1` which in turn forwards it to `eth0` in `Pod B`. 

#### Communication Between Different Nodes

Communication between different pods occurs in exactly the same way as the communication bw the pods, but an extra node is required as follows:
![[Pasted image 20230514165439.png]]


#### CNI Plugin with Overlay Networking
Assume we have installed a CNI plugin, i.e. Flannel. Flannel then installs a new interface between the root's `eth0` and `cni0` as follows:

![[Pasted image 20230514171217.png]]


**Setup Flannel with VXLAN**

https://stackoverflow.com/questions/66449289/is-there-any-way-to-bind-k3s-flannel-to-another-interface

**Very good tutorial:** https://addozhang.medium.com/learning-kubernetes-vxlan-network-with-flannel-2d6a58c95300

**Note**: Route the traffic to the other node via the **flannel interface**, not the **cni0** interface. 

**Installation**:
``` Bash
export INSTALL_K3S_VERSION=v1.23.8+k3s2  
curl -sfL https://get.k3s.io | sh -s - --disable traefik --flannel-backend=none --write-kubeconfig-mode 644 --write-kubeconfig ~/.kube/config
```
