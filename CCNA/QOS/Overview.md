#QoS
***
- On modern networks data, voice and video run over the same shared infrastructure in order to save costs as follows:
![[Pasted image 20230730144738.png]]

- Voice and traditional standard definition video packets must meet these recommended requirements to be an acceptable quality call:
	- Latency <= 150 s
	- Jitter(variation in delay) <= 30 ms
	- Loss <= 1%
- When an interface is congested, it acts as a FIFO queue

Assume we have the following scenario:
![[Pasted image 20230730144948.png]]
Above we see that the voice packets are much smaller than the data packets, so it would be beneficial to have them in front of the data packets

- This is one of the functionalities of a QOS service:
	- Different priorities to different traffic types
	- Limits the rate on an interface
	- It only acts when the link is congested. Otherwise it has no effect.