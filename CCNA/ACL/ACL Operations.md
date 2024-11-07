- The `access-list` command only creates the ACL. To apply it, we use the `access-group` command as follows:
```
interface GigabitEthernet 0/1
ip access-group 100 out
ip access-group 101 in
```
- An interface can have at most 1 inbound/outbound ACL
- To check the access group configuration we use the following command:
```
show ip interface {interface-number} | include access list
```

- **Note**: It is important to always first specify the most specific entries at the top of the list since the rules are processed in the order they were defined

- To inject ACE in an existing ACL we use the following commands
```
ip access-list extended 110
15 deny tcp host 10.10.10.11 host 10.10.50.10 eq telnet
```

- If no ACL is applied, all traffic is allowed. If an ACL is applied, all traffic is denied except the one specified.
- To allow all traffic except the one specified we use the following commands:
```
access-list 1 deny 10.10.10.0 0.0.0.255
access-list 1 permit any
```

- Assume we have the following network topology with ACL rules defined as follows:
![[Pasted image 20230426232456.png]]
Above we define an outbound rule for all traffic from R1->R2.  However, this will prevent the traffic from PC1 and PC2 to port `23` of R2, but not the traffic from R1 to R2. To apply this rule to traffic from R1 to R2 also, we need to define it as an inbound rule in R2.