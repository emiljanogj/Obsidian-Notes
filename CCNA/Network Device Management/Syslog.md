- Syslog message format is as follows:
```
seq no: time stamp: %facility-severity-MNEMONIC:description
```

- Severity levels range from 0-7 as follows:
![[Pasted image 20230730121009.png]]

- Syslog messages can be logged to various locations:
	- Console line - events will be shown in the CLI when you are logged in over a console connection. All events are logged by default
	- VTY Terminal Lines - events will be shown in the CLI when logged in over a Telnet or SSH session
	- Logging buffer - events saved in RAM memory and can be seen using the `show logging` function
	- External syslog servers


#### Syslog Configuration
```
R1(config)#no logging console -> disables logging to the console line

R1(config)#logging monitor 6 -> events with severity level >=6(informational) will be logged to the VTY lines

R1(config)#logging buffered debugging -> events with severity level 7 and higher will be logged to the buffer
```

**External Server Logging**
- We can also log to an external Syslog server to centralise event reporting
```
R1(config)#logging 10.0.0.100
R1(config)#logging trap debugging
```
*Note*: In the 2nd line we specify that the server will receive trap notifications from the devices being monitored.

**Notable Things**
- When we enable debugging with the command `debug {command}`, this will print the debugging info both to the console line and to the buffer and may overwhelm the device.
- We can enable debug output to the VTY lines using the command `R1#terminal monitor`.