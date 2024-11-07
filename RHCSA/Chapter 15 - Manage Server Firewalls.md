
### Predefined Zones
![[Pasted image 20240204204834.png]]
*Note*: We map the traffic from a given IP address or an interface to a certain zone, and each zone is associated with different **SELinux** policy modules

### Predefined Services
![[Pasted image 20240204205901.png]]

- To get the predefined service configuration we use the following command:
```
firewall-cmd --get-services
```

- Examples of commands with `firewall-cmd`:
```
[root@host ~]# firewall-cmd --set-default-zone=dmz  
[root@host ~]# firewall-cmd --permanent --zone=internal \ --add-source=192.168.0.0/24  
[root@host ~]# firewall-cmd --permanent --zone=internal --add-service=mysql [root@host ~]# firewall-cmd --reload
```
*Note*: The above commands set the default zone to `dmz`, maps the traffic incoming from `192.168.0.0/24` to the `internal` zone and opens all the ports for the `mysql` service in the internal zone. Since the changes are permanent(because of the `--permanent`) option we need to reload `firewalld`.


### Control SELinux Port Labeling
- To list the SELinux port label we use the following command:
```
semanage port -l | grep {filter}
```
- To filter based on the port number:
```
semanage port -l | grep -w {port-number}
```

- To add a label to a port we use the following command:
```
semanage port -a -t {type} -p {protocol} {port-number}
```
*Note*: To modify/delete a label we use the option `-m` and `-d` respectively instead of `-a`.

- To view changes to the default policy we use the following command:
```
semanage port -l -C
```

- To check for SELinux-related alerts in audit logs we use the following command:
```
sealert -a /var/log/audit/audit.log
```

- To list the open ports in the default zone we use the following command:
```
firewall-cmd --zone={default-zone} --list-all
```
- The default zone can be retrieved with the command:
```
firewall-cmd --get-default
```

**Important Notes**":
- `firewall-cmd --add-source` adds traffic from a given source to a certain zone
- `firewall-cmd --zone={zone-name} --add-service={service}` allows traffic to a predefined service in a particular zone.
