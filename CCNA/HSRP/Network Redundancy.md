- Main goal is to eliminate single points of failure
![[Pasted image 20230419230848.png]]

- On the access layer we don't implement any redundancy because end hosts only have 1 network card + if an access layer switch goes down, only the PCs connected to that switch will lose connectivity. For example, in the above image if `Acc3` fails => only PC1 will lose connectivity and if `Acc4` fails => only PC2 will lose connectivity
- In the network above we define the following rules in R1:
	- static route to SP1: `ip route 0.0.0.0 0.0.0.0 203.0.113.1`
	- backup default route via R2 if link to SP1 fails(note we set the AD=5 which is lower than the previous static route with AD=1): `ip route 0.0.0.0 0.0.0.0 10.10.20.2 5`
	- backup route to `10.10.10.0/24` via R2 if link to CD1 fails: `ip route 10.10.10.0 255.255.255.0 10.10.20.2`
