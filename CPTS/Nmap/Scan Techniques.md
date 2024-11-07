#nmap

```bash
emi2024@htb[/htb]$ nmap --help

<SNIP>
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
<SNIP>
```


```bash
emi2024@htb[/htb]$ sudo nmap -sS localhost

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-11 22:50 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000010s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
5432/tcp open  postgresql
5901/tcp open  vnc-1

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

- To provide a file(**hosts.lst**) as input to nmap we use the following command:
```bash
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

![[Network_Enumeration_With_Nmap_Module_Cheat_Sheet.pdf]]

### Connect Scan
- The Nmap [TCP Connect Scan](https://nmap.org/book/scan-methods-connect-scan.html) (`-sT`) uses the TCP three-way handshake to determine if a specific port on a target host is open or closed. The scan sends an `SYN` packet to the target port and waits for a response. It is considered open if the target port responds with an `SYN-ACK` packet and closed if it responds with an `RST` packet.
- It's used when a user does not have `root` privileges to send `SYN` packets
- However, as opposed to the `SYN` scan it doesn't leave unfinished connections or packets on the target host, which makes it less likely to be detected by **IPS** or **IDS** systems


### Command Explanation

- Assume we have the following command:
```bash
sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn
```

- The options mean the following:
	- **-n**: No DNS resolution => scanning is sped up by not attempting to resolve the IP address to a hostname
	- **-Pn**: Nmap assumes the host is online => it doesn't use traditional methods(ICMP, TCP SYN) to check it
	- **--disable-arp-ping**: Disables ARP, which is used to check if the host is on local network


## Nmap Scripting Categories

![[Pasted image 20241015163942.png]]


- To send **Nmap** requests from a specific interface we use the following command:
```bash
emi2024@htb[/htb]$ sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```

- We can also specify a source port using the following command:
```bash
ncat -nv --source-port 53 10.129.2.28 50000
```


## FW Lab Hard

1. First step is to do an nmap scan using `53(DNS)` as source port as follows:
```bash
sudo nmap -sC -sV -Pn --source-port 53 10.129.34.225 -p-
```

2. Then connect to the discovered port(i.e. `50000`) using **ncat** and **--source-port 53** as follows
```bash
sudo ncat -nv --source-port 53 10.129.34.225 50000
```

