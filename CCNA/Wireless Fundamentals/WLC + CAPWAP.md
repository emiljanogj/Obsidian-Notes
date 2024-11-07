	#Wireless 
***

- Configuring a large amount of APs one by one is unmanageable. To avoid this, we can use a **Wireless LAN Controller(WLC)** as follows:
![[Pasted image 20230805125951.png]]

- According to above, Access Points can then be classified as:
	- **Standalone Access Points** - Autonomous Access Points
	- **Lightweight Access Points** - Access Points with a WLC


#### Lightweight Access Points

- **LAPs** support **Zero Touch Provisioning** => APs can discover the WLC automatically using these options
	- DHCP - option 43 gives the IP address of the WLC
	- DNS - `cisco-capwap-controller` resolves the IP address of the WLC
	- Local subnet broadcast
- Lightweight APs download their configuration from the WLC(includes what WLANs they should support and their settings)



#### WLC

- WLC also monitors the wireless quality and controls the channels and the power of the APs
- It can also detect rogue APs
- Wireless stations can roam across Wireless APs supporting the same WLAN. The infrastructure can be configured to make the roaming seamless, i.e. same SSID with same security options:
![[Pasted image 20230805132152.png]]

#### CAPWAP
- `CAPWAP` is a protocol that enables a WLC to manage a collection of Wireless Access Points
- Communications are encrypted inside a DTLS CAPWAP tunnel
- It uses ports `5246` and `5247`
- When `WLC` is used, the work is moved from the APs to the WLC, hence the term Lightweight APs
- However, real-time traffic is still handled by the WLC => `Split MAC`

**Split MAC - AP Operations**
- Client handshake when connecting
- Beacons
- Performance monitoring - The performance and coverage area will be measured by the `AP` and this info will be send to the `WLC` which will then decide on what action to take.
- Encryption and Decryption
- Clients in power save

**Split MAC - WLC Operations**
- Authentication
- Roaming control
- 802.11 - 802.3 communiction
- RF management
- Security management
- QoS management

**Traffic Flow with Autonomous AP**
![[Pasted image 20230805133606.png]]

**Traffic Flow with CAPWAP**
![[Pasted image 20230805133917.png]]
*Note*: If the AP wants to communicate with the Switch, it first communicates with the `WLC` on the right, which then communicates with the Switch. Management traffic between the `AP` and `WLC` also goes through the CAPWAP Channel. **Etherchannel** [[EtherChannel Load Balancing]] is also used if there are multiple links between the `WLC` and the Switch(**Link Aggregation**)

**FlexConnect**
- It is used to transfer traffic from a small branch(with no `WLC`) to a bigger branch(with `WLC`) as follows:
![[Pasted image 20230805134334.png]]

