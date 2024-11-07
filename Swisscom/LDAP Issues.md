- https://jira.swisscom.com/browse/TCIOPS-12649
- https://jira.swisscom.com/browse/TCIOPS-12820
- https://jira.swisscom.com/browse/TCIOPS-6814

- Setting the LDAP password
```
ldappasswd -H ldapi:/// -ZxWD "cn=iiq,dc=telcloud" -S "uid=avi-bind-olt,ou=SA,dc=olt,dc=telcloud"
```§