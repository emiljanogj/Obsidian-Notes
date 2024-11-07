- **Syslog Message Format**
![[Pasted image 20230906225946.png]]

- **Syslog Severity Level**
![[Pasted image 20230906230158.png]]

#### Syslog Logging Locations

- **Console line**: Syslog messages will be displayed in the CLI when connected to the device via the console port. By default, all messages(level 0 - 7) are displayed - *default*
- **VTY Lines**: Syslog messaged will be displayed when connected to the device via telnet/ssh
- **Buffer**: Syslog messages(level 0 - 7) are saved to RAM - *default*
- **External server**: We can configure to send syslog message to an external server(UDP port 514)

#### Syslog Configuration

![[Pasted image 20230906231729.png]]

*Note*: We should use the `logging synchronous` command to prevent the following scenario:
![[Pasted image 20230906231916.png]]

- To print the timestamp of a log event we use the following command:
```
R1(config)# service timestamps log datetime
```
