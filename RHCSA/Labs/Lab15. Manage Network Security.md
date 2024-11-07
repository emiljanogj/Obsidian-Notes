- To list all the SELinux related alerts we use the following command
```
sealert -a /var/log/audit/audit.log
```
- The first problem comes from the fact that `http` service is unable to bind to port `1001`. So, we first need to modify the port type with the following command:
```
semanage port -a -t http_port_t -p tcp 1001
```
- Then we need to add the port to the appropriate firewall zone with the following command:
```
firewall-cmd --zone=public --permanent --add-port=1001/tcp
```
