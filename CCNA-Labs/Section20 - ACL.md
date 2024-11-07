- ACL defined for the following exercise:


Configure ACLs as follows:

1) Restrict traffic internally using Router1 as follows:

- Use access list number 100

- Inside PC1 can only access the HTTP server 1 using HTTP on subnet 10.1.1.0/24

- Inside PC2 can only access the HTTP server 2 using HTTPS on subnet 10.1.1.0/24

- No other PCs or servers on subnet 10.1.2.0/24 can access subnet 10.1.1.0/24 (Explicitly add this line. This is normally done to log the traffic with the word log, but PT does not support logging)

- Hosts on subnet 10.1.2.0/24 can access any other network

- Bind access list in the most efficient place on Router1

  

2) Verification:

- Verify that Inside PC1 can access the internal HTTP server 1 using HTTP, but not ping HTTP server 2

- Verify that Inside PC2 can access the internal HTTP server 2 using HTTPS, but not ping HTTP server 1

- Verify that both inside PC1 and PC2 can browse to cisco.com and facebook.com


![[Pasted image 20230813223837.png]]

is as follows:

![[Pasted image 20230813223902.png]]
