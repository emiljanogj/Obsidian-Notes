- Standard ACL Range: 1-99, 1300 - 1999. They filter traffic only based on the source IP address => Easier to process. Usually placed near the destination.
- Extended ACL Range: 100-199, 2000-2699. They filter traffic based on the source and destination IP address, protocol types, port numbers etc.
- ACL Example 1:
```
access-list 1 deny 10.10.10.10 0.0.0.0
access-list 1 permit 10.10.10.10 0.0.0.255
```
The above command is allowing traffic from 10.10.10.0/24 subnet and denying traffic only for 10.10.10.10.
*Note*: `0.0.0.0` is the default mask so we could have replaced the first command with:
```
access-list 1 deny 10.10.10.10
```

- ACL Example 2:
```
access-list 100 deny tcp 10.10.10.10 0.0.0.0 gt 49151 10.10.50.10 0.0.0.0 eq 23
```
The above rule denies traffic coming from a port greater than `49151` from `10.10.10.10` to port `23` of `10.10.50.10`.
*Note*: The number `100` in the command above identifies the ACL rule. If I define another rule with the same number, it will overwrite the existing rule.

- Named ACLs begin with `ip access-list {standard | extended}`
- Example of named ACL:
```
ip access-list standard {acl-name}
deny 10.10.10.10. 0.0.0.0
permit 10.10.10.0 0.0.0.255
```


### Standard, Extended and Named ACLs

- In standard ACL, the default wildcard mask is /32, i.e 0.0.0.0. On the other hand, the extended ACL has no default wildcard mask.