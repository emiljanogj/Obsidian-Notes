#### TFTP
***
3 Phases:
1. **Connection**: TFTP client sends a request to the server, and the server responds back, initializing the connection.
2. **Data Transfer**: The client and server exchange TFTP messages. One sends data and the other sends acknowledgments.
3. **Connection Termination:** After the last data message has been sent, a final acknowledgment is sent to terminate the connection.
![[Pasted image 20230907225510.png]]


#### IOS File Systems
![[Pasted image 20230907231222.png]]

#### Upgrading Cisco IOS
![[Pasted image 20230907231500.png]]

#### Copying Files(FTP)
```
R1(config)# ip ftp username cisco

R1(config)# ip ftp password cisco
```

*Important Note*: To specify the OS file the device shoud use when it boots next time we use the following command:
```
boot system filepath
```
