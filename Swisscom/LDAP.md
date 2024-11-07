- Remove a user from LDAP
```
ldapdelete -n -v -D "cn=iiq,dc=telcloud" -H ldap:// -W "uid=${tdcrogui},ou=People,dc=telcloud"
```

- Search for all the users that belong to a given group in LDAP:
```
ldapsearch -x -LLL -b "ou=People,dc=telcloud" "(memberOf=cn=telcloud_ecm_tc6000_Prod_zhb_usr,ou=People,dc=telcloud)"
```
