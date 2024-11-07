 #Wireless 
***
#### Authentication Methods

The original 802.11 standard included two options for authentication:
- **Open Authentication**
	- The client sends an authentication request, and the AP accepts it. No questions asked!
	- This is clearly not a secure authentication method.
	- After the client is authenticated and associated with the AP, it's possible to require the user to authenticate via other methods before access to the network is granted (ie. Starbuck's WiFi).
- **WEP (Wired Equivalent Privacy)**
	- WEP is used to provide both authentication and encryption of wireless traffic.
	- For encryption, WEP uses the RC4 algorithm.
	- WEP is a 'shared-key' protocol, requiring the sender and receiver to have the same key.
	- WEP keys can be 40 bits or 104 bits in length.
	- The above keys are combined with a 24-bit 'IV' (Initialization Vector) to bring the total length to 64 bits or 128 bits.
	- WEP encryption is not secure and can easily be cracked.

![[Pasted image 20230920092136.png]]

- **Extensible Authentication Protocol(EAP)**
	- Provides port-based network access control
	- 3 main entities in `802.1X`:
		- **Supplicant** - the devices that wants to connect to the network
		- **Authenticator** - the device that provides access to the network
		- **Authentication Server** - the device that receives client credentials and permits/denies access
![[Pasted image 20230920094438.png]]


**LEAP(Lightweight EAP)**
- Improvement over WEP
- Clients must provide a username and password to authenticate
- Both client and server send a challenge to each-other
- Dynamic WEP keys are used => WEP keys are changed frequently
![[Pasted image 20230920102720.png]]

**EAP-FAST(EAP Flexible Authentication via Secure Tunneling)**
- Consists of 3 phases:
	1. PAC(Protected Access Credential) - generated from the server and passed to the client
	2. A secure TLS tunnel is established between the client and the server
	3. The client and server communicate further to authenticate/authorize client
![[Pasted image 20230920105405.png]]



**Protected EAP**
- Instead of a PAC the server has a digital certificate and the client authenticates using the certificate
- After establishing a secure TLS tunnel with the server's certificate, the client still needs to authenticate within the secure channel, using MS-CHAP for example
![[Pasted image 20230920105624.png]]

**EAP-TLS**
- Requires a certificate on the **AS** and on every client => it's the most secure authentication method but also the most computationally expensive
- The client and server are both authenticated when the encrypted TLS tunnel is established, but this tunnel is used to exchange encryption key information
![[Pasted image 20230920105905.png]]

#### Encryption and Integrity Methods

- **Temporal Key Integrity Protocol(TKIP)**
	- Wireless HW was built to work with WEP, which is insecure => **TKIP** was created to fix some of the issues with WEP
	- A **MIC** was added to ensure the integrity of the messages
	- A **timestamp** was added to prevent replay attacks
	- A **TKIP sequence number** was added to keep track of the number of frames sent from a source MAC address. This is also used to prevent replay attacks
- **CCMP (Counter/CBC-MAC Protocol)**
	- CCMP was developed after TKIP and is more secure.
	- It is used in WPA2.
	- To use CCMP, it must be supported by the device's hardware. Old hardware built only to use WEP/TKIP cannot use CCMP.
	- CCMP consists of two different algorithms to provide encryption and MIC.
		1) AES (Advanced Encryption Standard) counter mode encryption
			- AES is the most secure encryption protocol currently available. It is widely used all over the world.
			- There are multiple modes of operation for AES. CCMP uses 'counter mode'.
		2) CBC-MAC (Cipher Block Chaining Message Authentication Code) is used as a MIC to ensure the integrity of messages.

- **GCMP(Galois/Counter Mode Protocol)**
	- More secure and efficient than CCMP
	- Allows higher data throughput than CCMP
	- It is used in WPA3
	- Consists of 2 algorithms
		- **AES counter mode encryption**
		- **GMAC(Galois Message Authentication Code)** is used as a MIC to ensure the integrity of the messages


#### Wifi Protected Access

- The **WPA** certification was developed after WEP was proven to be vulnerable and includes the following protocols:
	- **TKIP (based on WEP)** provides encryption/MIC.
	- 802.1X authentication (Enterprise mode) or PSK (Personal mode)
- **WPA2** was released in 2004 and includes the following protocols:
	- CCMP provides encryption/MIC.
	- 802.1X authentication (Enterprise mode) or PSK (Personal mode)
- **WPA3** was released in 2018 and includes the following protocols:
	- GCMP provides encryption/MIC.
	- 802.1X authentication (Enterprise mode) or PSK (Personal mode)
	- **WPA3** also provides several additional security features, for example:
		- PM (Protected Management Frames), protecting 802.11 management frames from eavesdropping/forging.
		- SAE (Simultaneous Authentication of Equals) protects the four-way handshake when using personal mode authentication.
		- Forward secrecy prevents data from being decrypted after it has been transmitted over the air. So, an attacker can't capture wireless frames and then try to decrypt them later.
