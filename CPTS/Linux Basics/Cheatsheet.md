![[Linux_Fundamentals_Module_Cheat_Sheet.pdf]]


- To find the config file that is >25K and smaller than 28K, created after 03.03.2020 we use the following command
```bash
find / -name *.conf -type f -size +25k -size -28k -newermt 2020-03-03 -exec ls -la {} \; 2>/dev/null
```

- To list all the fully-installed packages(packages installed and configured correctly with all the required dependencies) we use the following command:
```bash
dpkg --list | grep '^ii' | wc -l
```

- To list all the users along with their shells we use the following command:
```bash
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'
```

- In the above command we can also use `sed` to replace a specific string:
```bash
emi2024@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'
```

- To list all the background processes we use the following command:
```bash
jobs
```
- To switch a job from background to foreground we use the command `fg {job-number}`

- To start a web server using npm we follow these steps:
```bash
npm install http-server

http-server -p {port-number}
```

- To start a server using `php` we use the following command:
```bash
php -S 127.0.0.1:8080
```


### TCP Wrappers

- Allow access to system based on the source IP
- Allow rules are defined in `/etc/hosts.allow`:
```bash
emi2024@htb[/htb]$ cat /etc/hosts.allow

# Allow access to SSH from the local network
sshd : 10.129.14.0/24

# Allow access to FTP from a specific host
ftpd : 10.129.14.10

# Allow access to Telnet from any host in the inlanefreight.local domain
telnetd : .inlanefreight.local
```

- Deny rules are defined in `/etc/hosts.deny`:
```bash
emi2024@htb[/htb]$ cat /etc/hosts.deny

# Deny access to all services from any host in the inlanefreight.com domain
ALL : .inlanefreight.com

# Deny access to SSH from a specific host
sshd : 10.129.22.22

# Deny access to FTP from hosts with IP addresses in the range of 10.129.22.0 to 10.129.22.255
ftpd : 10.129.22.0/24
```

