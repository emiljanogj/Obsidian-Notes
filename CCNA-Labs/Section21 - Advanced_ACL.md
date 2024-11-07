We have the following network topology:

![[Pasted image 20230813232318.png]]

and need to solve the following exercise:

Configure ACLs as follows (hints below):

1) Restrict traffic internally as follows:

- Use access list number 100

- Inside PC 1 on subnet 10.1.2.0/24 can only access HTTP servers 1 and 2 on subnet 10.1.1.0/24 using HTTP and HTTPS (Use only two lines in your ACL to accomplish this).

- No other PCs or servers on subnet 10.1.2.0/24 can access subnet 10.1.1.0/24 (Explicitly add this line. This is normally done to log the traffic with the word log, but PT does not support logging)

- Hosts on subnet 10.1.2.0/24 can access any other network

- Bind access list in the most efficient place on Router1

	*Note*: If we want to include a single rule for the hosts `10.1.100` and `10.1.1.101` we need this mask: `0.0.0.1` (since only the last bit differs)

- The following ACL rules accomplish the above requirements:
```
10 permit tcp host 10.1.2.101 10.1.1.100 0.0.0.1 eq www (27 match(es))
    20 permit tcp host 10.1.2.101 10.1.1.100 0.0.0.1 eq 443
    30 deny tcp 10.1.2.0 0.0.0.255 10.1.1.0 0.0.0.255 (55 match(es))
    40 permit ip 10.1.2.0 0.0.0.255 any (81 match(es))
```



 We are also asked to achieve the following:

2) Restrict traffic externally as follows:

- Use access list number 101

- Any external device can access internal HTTP Servers using HTTP or HTTPS

- No external device can access the user subnet 10.1.2.0/24 (Explicitly add this line. This is normally done to log the traffic with the word log, but PT does not support logging)

- Bind access list in the most efficient place on Router1

This can be achieved via the following ACL:
```
10 permit tcp any 10.1.1.0 0.0.0.255 eq www

20 permit tcp any 10.1.1.0 0.0.0.255 eq 443

30 permit udp host 8.8.8.8 eq domain 10.1.2.0 0.0.0.255

40 deny ip any 10.1.2.0 0.0.0.255
```

