Two main types:
- IGP(Interior Gateway Protocols)
	- Distance Vector routing protocols
		- Each router sends its directly connected neighbors a list of all its known networks + the distance do those networks
		- It doesn't advertise the network topology. The router only knows its directly connected neighbors and the lists of networks these neighbors are connected to.
		- For the above reasons, it's also known as routing by rumour
	- Link State routing protocols
		- The router describes itself and a list of its interfaces to its directly connected neighbors
		- The information is then passed down unchanged, i.e., its neighbors then pass down that info to their neighbors which pass it down to their neighbors etc.
		- So its router gets an idea of the global topology, i.e., info about every other router and its interfaces => the routers can make better routing decisions because of this.

A list of all the routing protocols in use today is given below:
![[Pasted image 20230409161612.png]]

**Important Note from the Lab**: 
![[Pasted image 20230709232602.png]]
