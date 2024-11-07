Assume we have the following networking topology:

![[Pasted image 20230409151540.png]]

Then, R1 will advertise its routes to R2 as follows:
![[Pasted image 20230409151654.png]]

So, the R2 routing table will now look as follows:
![[Pasted image 20230409151732.png]]

In turn, R2 will advertise the following routes to R3
![[Pasted image 20230409151835.png]]

So, R3 routing table will now look as follows:
![[Pasted image 20230409151904.png]]

As in the static routing case, [[Static Routing#^19c92f]], we can again use route summary to have R2 advertise the following route to R3:
![[Pasted image 20230409153606.png]]

Assume now that one of the links in R1 goes down, i.e., the link 10.0.1.1 goes down. In this case R2 needs to compute a new path if one of its routes goes through it. However, R3 doesn't have a specific route to 10.0.1.0/24 itself, so it doesn't have to update anything. 
