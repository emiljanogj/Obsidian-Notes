- Load the logs using the Edit tab and filter using the following filters:
```
(http.request or tls.handshake.type eq 1) and !(ssdp)
```

#### Tshark

```
tshark -r [file] -Y [wireshark display filter]
```

Example:

```
tshark -r [file] tcp.port == 80 || udp.port == 80
```


- `bootp` filter can be used in Wireshark to view the DHCP traffic.
- To filter NBNS(DNS) traffic, we use the `nbns` filter.
- We can also use Kerberos traffic to locate hostnames of Windows hosts using the filter `kerberos.CNameString`
- Further info can be found using the filter `http.request and !(ssdp)`
- Good example of Wireshark filter:
```bash
(http.request or ssl.handshake.type == 1 or tcp.flags eq 0x0002 or dns) and !(udp.port eq 1900)
```

#### ngrep

- To search for a string in a pcap file we use the following syntax:
```
ngrep -I ngrep.pcap "POST"
```
- To display empty packets we use the following syntax:
```
ngrep -I ngrep.pcap -e
```
- To filter all POST requests to a given host we use the following syntax:
```
ngrep -I ngrep.pcap "POST" host 'IP_ADDRESS'
```
- To output 