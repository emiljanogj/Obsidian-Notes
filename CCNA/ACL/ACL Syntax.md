- We can choose to apply a certain ACL rule to only tcp/udp traffic as follows

```
access-list 100 deny tcp 10.10.10.0 0.0.0.255 10.10.50.0 0.0.0.255 eq 80
```

- Or we can choose to apply it to all traffic as follows

```
access-list 100 deny ip 10.10.10.0 0.0.0.255 10.10.50.0 0.0.0.255
```

- The 2 commands below do the same thing

```
access-list 100 permit tcp 10.10.10.10 0.0.0.0
access-list 100 permit tcp host 10.10.10.10
```

- And same goes for the 2 commands below

```
access-list 100 permit tcp 0.0.0.0 255.255.255.255
access-list 100 permit tcp any
```

- Instead of a port number, we can also specify a protocol as follows:
```
permit tcp host 10.10.30.10 host 10.10.20.1 eq {telnet|www|...}
```

telnet - port 2233 and www - port 80 etc.

- We can also specify the `log` keyword to log to the console or to an external monitoring service.
