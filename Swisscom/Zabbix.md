If we want to monitor the CPU usage of a network device using SNMP and Zabbix we follow these steps:
1. We add the network device to Zabbix as a host, specifying its IP address or hostname
2. Configure the SNMP agent on the network device and sepcify the SNMP version, community string, OID(Object Identifier) for the CPU usage metric
3. In Zabbix we create an SNMP item for the CPU usage metric, specifying the SNMP OID that corresponds to the CPU usage metric
4. Create a trigger that specifies the threshold for the CU usage metric such as "If the CPU usage is above 90% for more than 5 min, trigger an alert"
5. We can then configure Zabbix to send an alert notification via email or SMS


#### Configuring the SNMP Agent

In the network device, we follow these steps:

1. Install the **SNMP** daemon and related packages with the following command:

```
sudo apt-get install snmpd snmp
```

2. Edit the **SNMP** configuration file in `/etc/snmp/snmpd.conf` as follows:

```
rocommunity public
syslocation Unknown
syscontact Root <root@localhost>

# Define the "ssCpuUser" MIB object
extend .1.3.6.1.4.1.2021.11.9.0 ssCpuUser /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print 100-$NF}'

# Define the "ssCpuSystem" MIB object
extend .1.3.6.1.4.1.2021.11.10.0 ssCpuSystem /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print $NF}'

# Define the "ssCpuIdle" MIB object
extend .1.3.6.1.4.1.2021.11.11.0 ssCpuIdle /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print $NF}'

# Define the "ssCpuRawUser" MIB object
extend .1.3.6.1.4.1.2021.11.50.0 ssCpuRawUser /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print $2}'

# Define the "ssCpuRawSystem" MIB object
extend .1.3.6.1.4.1.2021.11.52.0 ssCpuRawSystem /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print $4}'

# Define the "ssCpuRawIdle" MIB object
extend .1.3.6.1.4.1.2021.11.53.0 ssCpuRawIdle /bin/sh /usr/bin/mpstat 1 1 | awk 'NR==4{print $12}'

```

For the CPU usage metric specifically, we define the following configuration:

```
rocommunity public
syslocation "Your location"
syscontact "Your contact info"
extend .1.3.6.1.4.1.9.9.109.1.1.1.1.3.7 cpu_usage /bin/bash -c "top -b -n1 | grep 'Cpu(s)' | awk '{print $2 + $4}'"
```

3. Save the `snmpd.conf` file and restart the **SNMP** daemon as follows:

```
sudo systemctl restart snmpd
```

4. Create an SNMP item for the CPU usage item in Zabbix by navigating to the host and clicking on "Items". Then, we click "Create Item" and enter the following details
	- Type: SNMPv2 agent
	- Key: cpu_usage
	- SNMP OID: 1.3.6.1.4.1.9.9.109.1.1.1.1.3.7
	- Type of information: Numeric(float)
	- Update interval: 60s

5. We can further proceed to create a trigger by clicking on "Triggers" => "Create trigger" with the following details:
	- Name: High CPU usage on {HOST.NAME}
	- Expression: {<host-name>:cpu.usage.avg(5m)} > 90
		  Severity: High


- Call AVI API from Zabbix:
Searc for AVI host -> search for runtime -> Open AVI cluster runtime -> Run test


### SNMP

Great link: https://www.zabbix.com/documentation/5.0/en/manual/discovery/low_level_discovery/snmp_oids



