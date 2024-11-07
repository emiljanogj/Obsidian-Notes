- The server first obtains a certificate from a CA out of band(OOB):

![[Pasted image 20230727235740.png]]

- Our browser should trust this CA and must have a copy of their public keys, i.e.:
![[Pasted image 20230727235914.png]]

- When we try to access a website, the server sends us their certificate, which we decrypt with their public key, that we have stored in our browser as described in the previous step:
![[Pasted image 20230728000038.png]]

- In the decrypted certificate, we can also check the hostname, so we know that we are communicating with the correct host. However, this is not enough. To authenticate the server, we also do the following step:
![[Pasted image 20230728000331.png]]

- After the server sends us the encrypted text, we decrypt it with the public key we retrieved from the certificate, and if it matches the text we sent => the host is who it says it is:
![[Pasted image 20230728000458.png]]

- This asymmetric encryption is quite slow though, so we establish a pre-shared key to use with symmetric encryption as follows:
![[Pasted image 20230728001611.png]]

