 #Wireless 
***

**IEE 802.11 Standards**

![[Pasted image 20230805230455.png]]

- Ranges from 2.4 - 2.4835 GHz and is divided into 22 MHz ranges called channels.
![[Pasted image 20230805230810.png]]
*Note*: In the above image we see that there's a lot of overlapping. If we were to pick 3 APs in the same service area we would need to pick them in channel 1, channel 6 and channel 11.

**2.4 GHz Spectrum**
- Each AP operates in 1 channel, i.e. 22 MHz range => APs with overlapping service areas should use non-overlapping channels.

**5 GHz Spectrum**
- `2.4 GHz` channels are `22 MHz` wide while `5 GHz` channels are `20 MHz` wide => `5 GHz` channels have less overlap than `2.4 GHz` channels.
- Neighbouring channels should still be separated by at least 1 channel.
- Channels can be bonded(40, 80 or 160 MHz) to increase the  data rate by 2, 4 or 8 times as shown below:
![[Pasted image 20230805233451.png]]

**2.4 vs 5 GHz**
- `2.4 GHz` has greater range and better propagation through obstacles, but is more crowded
- `5 GHz` has higher throughput than `2.4 GHz` but our client stations may only be compatible with `2.4 GHz`
- Site surveys should be conducted to reduce interference