- Routers are usually configured with a static IP address. However, when we want to connect to the Internet but we don't have any router that needs to be publicly exposed to the internet but the internal hosts need to have a public IP to connect to the internet => we configure a router as DHCP client as following:
```
interface f0/0
ip address dhcp
no shutdown
```