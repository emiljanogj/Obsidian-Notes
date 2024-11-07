#Packet_Life
***
1. Host A wants to send a packet to the FQDN www.flackbox.com , but it doesn't know its IP address => Host A will hold the packet and send a DNS request to its DNS server at `10.10.100.10`.
2. Host A compares its IP address and subnet mask to the destination address of the DNS server and sees that they are on different subnets => Host A will send a broadcast ARP request for its default gateway at 10.10.10.1
3. ![[Pasted image 20230706230853.png]]
4. The ARP request will be received by Switch 1, and Switch 1 will add an entry in its MAC address table mapping Host A's MAC address 1111.2222.3333 to Port 1
![[Pasted image 20230706231210.png]]

5. The ARP request will hit router A's interface `10.10.10.1`. Router A will process the request and see that it's for itself and send a unicast ARP reply to host A. Router A will add an entry for host A mapping IP `10.10.10.10` to the MAC address `1111.2222.33333` in its ARP cache
![[Pasted image 20230706232310.png]]

6. Switch 1 will add an entry in its MAC address table mapping Router A's MAC address `444.5555.6666` to port 2.
![[Pasted image 20230706232504.png]]

Host A will then add an entry for Router A mapping `10.10.10.1` to MAC address `4444.5555.6666`
7. Host A will then send a DNS request to its DNS servers
![[Pasted image 20230706233757.png]]

*Note*: The destination MAC is the MAC of its default gateway, while the destination IP is the IP of the DNS server. 

8. Router A will now receive the packet and see that its destination IP is `10.10.100.10`. Router A has an interface in the subnet `10.10.100.0/24`. It doesn't know the MAC address of `10.10.100.10` so it will hold the DNS request packet and send an ARP request via the `10.10.100.1` interface.
![[Pasted image 20230706234650.png]]

9. The ARP request will be received by Switch 3 which will broadcast it in all its ports except the ports it came in from. Switch 3 will also add an entry in its ARP cache mapping the IP `10.10.100.1` to `8888.9999.AAAA`. 
10. The ARP request will be hit the DNS server's interface at `10.10.100.10` which will send a unicast reply to Router A. the DNS server will add an entry for Router A mapping IP address `10.10.100.1` to the MAC address `8888.9999.AAAA`.
![[Pasted image 20230706235104.png]]

11. Switch 3 will add an entry in its MAC address table mapping the DNS server's MAC address `3333.4444.5555` to port 3.
12. Switch 3 will send the ARP reply via the Port 1
![[Pasted image 20230706235559.png]]

13. Router A will also add an entry to its ARP cache mapping the IP address `10.10.100.10` to the MAC address `3333.4444.5555`

**Important Note**: The source and destination MAC addresses of a packet are updated hop by hop while the source and destination IP remain unchanged. 

14. Now that the ARP request is finished, Router A will proceed to sending the DNS request it was holding
![[Pasted image 20230707000321.png]]

15. Switch 3 now has the IP -> MAC mapping for the DNS server so it will send the DNS request only via Port 3.![[Pasted image 20230707000439.png]]
16.  The DNS server will look for the record in its database. After finding it, it know that it needs to send the response to `10.10.10.10`(Source IP). Since this IP is in another subnet, it knows that it needs to send it via Router A. 
![[Pasted image 20230707000927.png]]

17. Router A will receive the DNS reply and see that it's destined for `10.10.10.10`, and Router A has an interface at `10.10.10.0/24`.  Router A also has the MAC address for IP `10.10.10.0`
![[Pasted image 20230707001542.png]]

18. Switch 1 will receive the DNS reply and send it out only via Port 1.
![[Pasted image 20230707001830.png]]


### HTTP Request
***
Assume now we send the following GET request:
![[Pasted image 20230707081915.png]]

1. Host A sees that the destination IP is in another subnet so it forwards the request to its default gateway at `10.10.10.1`. Switch 1 already has Router A in its MAC address table so it will send the packet via Port 2. 
2. Router A will receive the packet and see that it has no interface in the subnet `10.10.12.0/24` => This route need to be added either manually or it needs to be discovered by the routing protocols. In this case, assume the network engineer has added a static route to `10.10.11.2`. Router A has an interface in the `10.10.11.0`. It doesn't know the MAC address of `10.10.11.2` so it will send an ARP request for it. 
![[Pasted image 20230707084819.png]]

3. Router B will get the ARP request and see that it's for itself, so it will send a unicast reply to Router A. It will also add an entry in its ARP cache mapping `10.10.11.1` to `5555.6666.7777`
![[Pasted image 20230707085224.png]]

4. Router A will now forward the request to Router B. Router B has an interface in the `10.10.12.0/24` subnet but it doesn't know the MAC address of `10.10.12.1` so it will hold the packet and send an ARP request.
![[Pasted image 20230707085400.png]]

5. The ARP request will be received by Switch 2 which will forward it to all its ports beside the port it came in. Switch 2 will also add an entry in its MAC address table mapping `10.10.12.1` to `7777.8888.9999`. 
![[Pasted image 20230707085730.png]]

6. Request will now hit the server. The server sees that it's for itself so it will send a unicast reply to Router B, and update its ARP cache to map `10.10.12.1` to `7777.8888.9999`. 
![[Pasted image 20230707085903.png]]

7. Switch 2 will add an entry mapping `10.10.12.10` to `2222.3333.4444` and since it updated its MAC address table when receiving the request from Router B, it knows that it needs to send the reply via Port 1.
8. Router B will add an entry in its ARP cache mapping `10.10.12.10` to `2222.3333.4444`., and will then send the GET request it was holding.
![[Pasted image 20230707090936.png]]
9. Switch 2 will then send the GET request via Port 2, and the request will finally reach the server. 
10. When the server receives the packet it will decode it as follows:
![[Pasted image 20230707091226.png]]
11. All subsequent requests will go through immediately without the need for ARP or DNS requests.