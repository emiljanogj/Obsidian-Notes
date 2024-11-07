
![[Pasted image 20240528082445.png]]

Two pods:
- **Controller** pod - used to distribute addresses from the IP pool
- **Speaker** pod - deployed with their corresponding bindings and listeners. Each worker node receives a speaker pod which is then responsible for routing traffic

**Note**: It seems that the speaker pod is not available in the master node.