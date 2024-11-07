#Footprinting 

---

- To query the version of the DNS server we can use the following command:
```bash
dig CH TXT version.bind {IP}
```

**Note**: The options indicate the following:
- **CH**  - Specifies the class of the DNS query(i.e Chaosnet). The usual class is **IN**(internet)
- **TXT** - Specifies the type of the DNS record being queried. We're querying the version here and that's why we use the generic **TXT** class
- **version.bind** - a special class inside the **CH** class used to retrieve the version of the DNS server

## Dangerous Options

![[Pasted image 20241019235716.png]]

- If allow transfer is misconfigured we can use the following command:
```bash
dig axfr inlanefreight.htb @{DNS Server IP}
```

which issues a **DNS zone transfer** request. This transfers all DNS records(A, CNAME, MX) from the DNS primary server.

## Subdomain Brute-Force

2 Methods to do a subdomain brute-force:

```bash
for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig ${DOMAIN} @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```
This finds all the subdomains in `{DOMAIN}`

- Another method is to use `dnsenum` as follows:
```bash
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt {DOMAIN}
```

- A **zone transfer** in DNS is the transfer of all the DNS records for a zone from a primary to a secondary server. To check if zone transfer is available we use the following command:
```bash
dig axfr {domain} {DNS-server}
```
We then use the enumeration method above to enumerate all the subdomains of a given domain.

