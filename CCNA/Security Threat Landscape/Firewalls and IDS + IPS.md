![[Pasted image 20230727232719.png]]

From the above image, we can see the difference between IPS and IDS:
- IPS blocks traffic => Since it stands in the way of traffic it can become a bottleneck
- IDS doesn't stand in the way of traffic => it doesn't block it but at the same time it cannot become a bottleneck


**Firewalls vs IPS**:
- Firewalls block or permit traffic based on rules
- IPS blocks traffic based on signature. It scans packet signatures up to layer 7 of the OSI stack, looking for traffic which matches known attacks.
- IPS and Firewalls are usually deployed in conjunction with one another
- Modern firewalls however are very advanced and can also act as IPS or endpoints of VPN tunnels.

![[Pasted image 20230727233540.png]]

In the example above, we use both firewalls and IPS:
- Firewalls are used to filter outside traffic
- IPS is used to filter traffic to critical internal servers in the demilitarised zone(DMZ) 