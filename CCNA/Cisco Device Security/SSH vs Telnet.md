#Cisco-Security 
***
- SSH traffic is encrypted while Telnet traffic is not => SSH is the more secure option
- To be able to use SSH, we need to generate a TLS certificate as follows:
```
R1(config)#ip domain-name flackbox.com
R1(config)#crypto key generate rsa
```
- To disable Telnet on VTY lines, we use the following commands:
```
R1(config)#username {username} secret {password}
R1(config)#line vty 0 15
R1(config-line)#transport input ssh -> Note that telnet is not added
R1(config-line)#login local -> login with local usernames
R1(config-line)#exit
R1(config)#ip ssh version 2 -> limit SSH to v2
```
*Note*: Note that the password is generated on the router. We then ssh to `R1` from the client as follows:
```
ssh -l {username} 10.0.0.1
Password: 
```
