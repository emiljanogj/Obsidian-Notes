.#NTP
***
- To manually configure the software clock on a device we use the following command:
```
clock set 14:30:00 27 Dec 2020
```

*Note*: SW and HW clock are separate and should also be configured separately. To configure the HW clock we use the same command using the `calendar` command.

- To sync the calendar to the clock's time we use the following command:
```
clock update calendar
```

- To sync the clock to the calendar's time we use the following command:
```
clock read-calendar
```

- To set the timezone we use the command:
```
R1(config)# clock timezone {timezone} {offset}
R1(config)# do show clock
```

- To set the clock to the Daylight Saving Time we use the following command:
```
clock summer-time EDT recurring 2 Sunday Mach 02:00 1 Sunday November 02:00
```
*Note*: We specify the start and the end time of the daylight saving time 

- NTP uses UDP port 123 to communicate
- NTP clocks function in a hierarchical structure

![[Pasted image 20230905090737.png]]

- Anything below level 15 is considered unreliable
- Devices can also peer with other devices at the same stratum to provide more accurate time. This is called symmetric active mode
- 3 modes
	- server mode
	- client mode
	- symmetric active mode
- **Primary servers** - get the time directly from the reference clocks
- **Secondary servers** - get their time from other NTP servers => they operate in both client and server mode

#### NTP Server Configuration

```
R1(config)# ntp server 216.230.35.0 prefer
R1(config)# ntp server 216.239.35.8
```

- To update the HW clock, we use the following command:
```
R1(config)# ntp update-calender
```
- Assume we have the following network topology:
![[Pasted image 20230905092718.png]]
We want to use R1 as the NTP server. To do that we first define a loopback interface in R1 as follows:
```
R1(config)# interface loopback0
R1(config-if)# ip address 10.1.1.1 255.255.255.255
R1(config-if)# exit
R1(config)# ntp source loopback0
```
In the above topology we could have used `10.0.0.1` as the NTP source also but if that link goes down, then R2 won't received any more NTP updates from R1. We then configure R2 as follows:
```
R2(config)# ntp server 10.1.1.1
```

#### NTP Authentication
- Assume we have 3 routers, where R1 is the NTP server and R2, R3 are peers that use R1 as the NTP server. The configuration in this case is as follws:
```
R1(config)# ntp authenticate
R1(config)# ntp authentication-key 1 md5 {pass}
R1(config)# ntp trusted-key 1

R2(config)# ntp authenticate
R2(config)# ntp authentication-key 1 md5 {pass}
R2(config)# ntp trusted-key 1
R2(config)# ntp server 10.0.12.1 key 1
R2(config)# ntp peer 10.0.23.2 key q

R3(config)# ntp authenticate
R3(config)# ntp authentication-key 1 md5 {pass}
R3(config)# ntp trusted-key 1
R3(config)# ntp server 10.0.12.1 key 1
R3(config)# ntp peer 10.0.23.2 key 1

R1# show ntp associations
R1# show ntp status
```

- To make the hw clock match the sw clock time we use the following command:
```
clock update-calendar
```

- To configure the router to update the HW clock using NTP we use the following:
```
ntp update-calendar
```

- Show a list of configured NTP server
```
show ntp associations
```

- Configure the source of NTP messages
```
ntp source {interface}
```

- Configure the device to be an NTP server
```
ntp master {stratum}
```
