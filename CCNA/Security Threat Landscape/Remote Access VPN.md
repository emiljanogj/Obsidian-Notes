- Uses a software(e.g Cisco AnyConnect) to connect to a specific network(usually corporate network) via TLS as follows:
![[Pasted image 20230728094559.png]]

Two methods:
- Split Tunneling: Traffic to the internet goes the normal route, while corporate traffic goes through the VPN:
![[Pasted image 20230728094929.png]]
- Full Tunneling
![[Pasted image 20230728094955.png]]
All traffic goes through the corporate Firewall, which may be inefficient at times.