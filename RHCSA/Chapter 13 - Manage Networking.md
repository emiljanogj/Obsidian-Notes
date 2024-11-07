6,mcat- To show the statistics for an interface we use the following command:
```
ip -s link show {interface-name}
```
- When pinging the link-local all nodes multicast group we need to specify the exit interface as follows:
```
	ping6 ff02::1%{interface-name}
```
- To display socket statistics we use the following command:
```
ss -ta
```
*Note*: It contains the following options:
	- **n**  - show numbers instead of names for interfaces and ports
	- **t** - show only TCP ports
	- **u** - show only UDP ports
	- **l** - show only listening ports
	- **a** - show all sockets
	- **p** - show the process that uses the sockets
	- **-A inet** - displays active connections for the inet connection family

- To display the IP address and netmask for all the interfaces we use the command:
```
ip -br addr
```

### Configure Networking from the Command Line
- Network settings are managed by the **NetworkManager** daemon
- Service configuration files are stored in `/etc/NetworkManager/system-connections` directory
- To create/edit connection files from the command line we use the `nmcli` command as follows:
```
nmcli dev status
```
- To display a list of all connections we use the command:
```
nmcli con show
```
*Note*: To show only the active connections we add the `--active` option to the above command

- To add a connection we use the following command:
```
nmcli con add con-name {connection-name} type ethernet ifname {interface-name}
```
- To add a connection and specify the IP address and the gateway we use the following command:
```
nmcli con add con-name {connect-name} type ethernet ifname {interface-name} ipv4.method manual ip4.address {IPv4/subnet-mask} ipv4.gateway {gateway}
```
*Note*: We can do the same with an IPv6 address using the `ipv6` class instead of `ipv4`

### Manage Network Connections
- To show all connections we use the command
```
nmcli con show
```
- To bring a connection up we use the command
```
nmcli con up {connection-name}
```
- To bring a connection down we use the command:
```
nmcli dev disconnect {if-name}
```
*Note*: The difference when bringing the connection up(using the connection name) and taking it down(using the interface's name). We can also use `nmcli con down`, but since most interfaces have the `autoconnect` parameter set, the previous command will have no effect and that's why we use `nmcli dev disconnect`

- To list the settings for a connection we use the command `nmcli con show {connection-name}`
- To modify an existing connection we use the following command:
```
nmcli con mod {conn-name} ipv4.address {IP/subnet-mask} ipv4.gateway {Gateway-IP} connection.autoconnect {yes|no}
```
*Note*: the `ipv4.method` can be set to `{manual | dhcp}`
- To add DNS to existing connection we use the following command:
```
nmcli con mod {con-name} +ipv4.dns {DNS-IP}
```
- To persistently apply the new settings, i.e. modify the config files in `/etc/NetworkManager/system-connections/` we have to apply the following command
```
nmcli con reload
```
- To load only a specific profile we use the command:
```
nmcli con reload {con-name}
```
- To delete a connection:
```
nmcli con del {con-name}
```
- To show the **NetworkManager** permissions of the current user we use the following command:
```
nmcli gen permissions
```

### Common NetworkManager Settings
![[Pasted image 20240131234139.png]]

![[Pasted image 20240131234223.png]]

![[Pasted image 20240131234831.png]]

- 3 directories that can be used to store network configuration files:
	- `/etc/NetworkManager/system-connections` - stores persistent profiles. When the user creates/edits a profile with `nmcli` the config file is stored here
	- `/run/NetworkManager/system-connections` - stores temporary profiles
	- `/usr/lib/NetworkManager/system-connections` - stores predefined immutable profiles. When such profiles are edited with the NetworkManager API, the config file is stored either in the permanent or the temporary folder

- Whenever a config file is modified we need to reload connections using the following command:
```
nmcli con reload
```

*Note*: `dig` retrieves info from the DNS server and `getent hosts` retrieve info from several sources including `/etc/hosts` file.

- `hostname {hostname}` temporarily changes the hostname while `hostnamectl hostname {hostname}` makes the change persistently, i.e. it modifies `/etc/hostname`


### Setting the IP and Gateway

- To set the IP we use the following command:
```
ip addr add {IP}/{SUBNET-MASK} dev {if-name}
```
- To set the default gateway we use the following command:
```
ip route add {gw-IP} dev {if-name}
```
