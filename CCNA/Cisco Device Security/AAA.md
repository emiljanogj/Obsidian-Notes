#Cisco-Security 
***

- Stands for Authentication, Authorization and Accounting
- Necessary because configuring line-level security or local usernames on each device is not a scalable solution, since a password change needs to be applied to all the devices. This can be avoided by using an AAA server
- 2 protocols used for AAA services:
	- RADIUS - used for end user level services
	- TACACS+ - used for admin access on Cisco devices as it provides more granular authorization capabilities
- Cisco's AAA server is the Identity Service Engine(ISE)
- Instead of using a special username/password for AAA server, we can also integrate the AAA server with an LDAP solution by using an Active Directory Domain Controller as follows:
1. ![[Pasted image 20230728153231.png]]
2.
![[Pasted image 20230728153311.png]]
3.
![[Pasted image 20230728153337.png]]
4.
![[Pasted image 20230728153354.png]]
5.
![[Pasted image 20230728153412.png]]
6.
![[Pasted image 20230728153439.png]]
7.
![[Pasted image 20230728153459.png]]

### Radius Configuration

**Old Radius Configuration**

```
R1(config)#username {username} secret {secret} -> configure a local user in case connectivity to the AAA server is lost

R1(config)#aaa new-model
R1(config)#radius-server host 10.10.10.10 key Flackbox1
R1(config)#radius-server host 10.10.10.11 key Flackbox2 
# Above we define 2 AAA servers for redundancy. Flackbox1 and Flackbox2 are the preshared keys used for encrypting communication between the two devices. This key needs to be configured on the RADIUS server as well.
```
*Note*: The following step is optional and creates a RADIUS server:
```
R1(config)#aaa group server radius FB-RG
R1(config-sg-radius)#server 10.10.10.10
R1(config-sg-radius)#server 10.10.10.11
```

Then we continue with the normal configuration:
```
R1(config)#aaa authentication login default group radius local -> Use all RADIUS servers and local usernames as a backup

R1(config)#aaa authentication login default group FB-RG local -> Use the previously defined group only(FB-RG) and local usernames as backups
```


**New Radius Configuration**

```
R1(config)#aaa new-model
R1(config)#radius server Server1
R1(config-server-radius)#address ipv4 10.10.10.10
R1(config-server-radius)#key {PSK1}

R1(config)#radius server Server2
R1(config-server-radius)#address ipv4 10.10.10.11
R1(config-server-radius)#key {PSK2}

R1(config-server-radius)aaa group server radius FB-RG
R1(config-sg-radius)#server name Server1
R1(config-sg-radius)#server name Server2

R2(config)#aaa authentication login default group FB-RG local
```

**Old TACACS+ Configuration**

```
R1(config)#username {username} secret {secret}

R1(config)#aaa new-model
R1(config)#tacacs-server host 10.10.10.10 key {PSK1}
R1(config)#tacacs-server host 10.10.10.11 key {PSK2}

R1(config)#aaa group server tacacs+ FB-TG
R1(config-sg-tacacs+)#server 10.10.10.10
R1(config-sg-tacacs+)#server 10.10.10.11

R1(config)#aaa authentication login default group FB-TG local
```

**New TACACS+ Configuration**

```
R1(config)#aaa new-model
R1(config)#tacacs server Server1
R1(config-server-tacacs)#address ipv4 10.10.10.10
R1(config-server-tacacs)#key {PSK1}

R1(config)#tacacs server Server2
R1(config-server-tacacs)#address ipv4 10.10.10.11
R1(config-server-tacacs)#key {PSK2}

R1(config-radius-server)#aaa group server tacacs+ FB-TG
R1(config-sg-tacacs+)#server name Server1
R1(config-sg-tacacs+)server name Server2

R1(config-sg-tacacs+)#aaa authentication login default group FB-TG local
```