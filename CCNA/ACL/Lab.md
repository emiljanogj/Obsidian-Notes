**Note**: We can only have 1 ACL per interface per direction.

- An example of ACL that allows telnet traffic traffic only from a specific IP, blocks it from all the other PCs but allows all the other kinds of traffic.
```
permit tcp host 10.0.1.10 host 10.0.0.2 eq telnet
deny tcp any host 10.0.0.2 eq telnet
permit ip any any (+)
```

**Note**:  If we have a permit statement => all the other traffic is denied, hence the last statement above.

- To apply an ACL in an interface, we use the following command:
```
ip access-group {ACL-number} in/out
```
*Note*: To remove it we use the same statement with the `no` keyword in front.

To print the number of matches for an ACL we use the following command:

```
show access-lists 110
```
