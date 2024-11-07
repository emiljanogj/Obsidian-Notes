![[Pasted image 20230728001758.png]]

- They use symmetric encryption such as DES, 3DES and AES to send encrypted traffic. 
- They typically terminate on a firewall or router on both sides
- PSK or Certificate can be configured on both sides of the tunnel, but certificates are more scalable


#### IPsec
- A framework of open standards that provides secure encrypted communication over an IP network
- IKE - handles negotiation of protocols and algorithms and generates encryption and authentication keys
- ISAKMP - defines the procedure for authenticating and communicating peer creation. Typically uses IKE for key exchange.
- Authentication Header provides integrity, authentication and protection from replay attacks
- Encapsulating Security Payload(ESP) provides all of the above + confidentiality
	- ESP Tunnel Mode - IP header of the original packet is encrypted
	- Transport mode - encrypts only the payload and ESP trailer => IP header of the original packet is not encrypted. It is used when another tunneling protocol(GRE, L2TP) is used to first encapsulate the IP data packet, and IPsec is used simply to protect the GRE/L2TP tunnel packets.

#### Replay Attack Example

[Session IDs](https://en.wikipedia.org/wiki/Session_ID), also known as session tokens, are one mechanism that can be used to help avoid replay attacks. The way of generating a session ID works as follows.

1. Bob sends a one-time token to Alice, which Alice uses to transform the password and send the result to Bob. For example, she would use the token to compute a hash function of the session token and append it to the password to be used.
2. On his side Bob performs the same computation with the session token.
3. If and only if both Alice’s and Bob’s values match, the login is successful.
4. Now suppose an attacker Eve has captured this value and tries to use it on another session. Bob would send a different session token, and when Eve replies with her captured value it will be different from Bob's computation so he will know it is not Alice.

#### IPsec VPN Implementation

- The VPN devices recognise the interesting traffic, i.e. the traffic to encrypt.
- ISAKMP/IKE Phase 1: The VPN devices negotiate an IKE security policy, authenticate each other and establish a secure channel
- ISAKMP/IKE Phase 2: They then negotiate an IPsec security policy to encrypt the data
- Data Transfer: After encryption, the data is transmitted over the secure channel.


#### IPsec VPN Configuration Example

Assume we have the following network topology:
![[Pasted image 20230728091032.png]]

Then we configure the IPsec VPN tunnel as follows:
- Phase 1
```
R1(config)# crypto isakmp policy 1
R1(config-isakmp)#encr aes
R1(config-isakmp)#hash sha
R1(config-isakmp)#authentication pre-share
R1(config-isakmp)#group 2 -> DH Key Exchange
R1(config-isakmp)#lifetime 86400
R1(config-isakmp)#crypto isakmp key Flackbox address 203.0.113.5 -> shared secret is Flackbox + 203.0.113.5 is the IP on the other side
```

- Define interesting traffic using an ACL rule
```
R1(config)#ip access-list extended FlackboxVPN-ACL
R1(config-ext-nacl)#permit ip 10.10.10.0 0.0.0.255 10.10.20.0 0.0.0.255
```

- Phase 2
```
R1(config-ext-nacl)#crypto ipsec transform-set FlackboxTS esp-aes esp-sha-hmac
R1(config)#crypto map FlackboxCM 10 ipsec-isakmp
R1(config-crypto-map)#set peer 203.0.113.5
R1(config-crypto-map)#set transform-set FlackboxTS -> transform set defined in the 1st point
R1(config-crypto-map)#match address FlacbkoxVPN-ACL -> ACL defined in the previous step 
R1(config-crypto-map)#interface Serial10/1/0
R1(config-if)crypto map FlacbkoxCM -> Assign the crypto map defined in the previous step to the proper interface
```

- Exclude VPN Traffic from NAT ACL(This rule prevents the traffic going through the VPN from being NAT-ed)
```
R1(config)#ip access-list extended FlackboxNAT-ACL
R1(config-ext-nacl)#deny ip 10.10.10.0 0.0.0.255 10.10.20.0 0.0.0.255
R1(config-ext-nacl)#permit ip 10.10.10.0 0.0.0.255 any
```