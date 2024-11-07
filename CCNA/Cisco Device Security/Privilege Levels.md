#Cisco-Security

#### Usernames
***
- We can provide more granular security by configuring individual usernames and passwords for different admins as follows:
```
R1(config)# username admin1 secret {secret1}
R1(config)# username admin2 secret {secret2}
R1(config)# line console 0
R1(config-line)# login local -> use the local usernames defined in the 1st two steps
R1(config)# line vty 0 15
R1(config-lone)# login local
```


#### Privilege Levels
***
- There are 16 privilege levels of admin access(0-15) available on a Cisco router or switch and username/passwords can be attached to each level individually.
- To assign a privilege level to a username, we use the following command:
```
username admin1 privilege {0-15} secrect {pass}
```
- To configure a command to a certain privilege level, we use the following command:
```
privilege exec level 5 show running-config
```
The above command means that only a user with a privilege level >=5 can execute the command `show running-config`.
- To enable secrets for a certain privilege level, we use the following command:
```
enable secret secret1 -> sets password for privilege level 15
enable secret level 5 secret2 -> sets password for privilege level 5
```

*Note*: The `enable password {pass}` command doesn't encrypt the password, so you can still see the password with `show run | include enable password`. To encrypt the password in the running-config, we use the following command:
```
service password-encryption
```