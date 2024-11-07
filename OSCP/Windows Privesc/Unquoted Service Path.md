For unquoted paths the OS assumes the file location(i.e. `C:\Program Files\Vulnerable Service1\Service 1.exe`) using the following rules:

1. `C:\Program.exe`
2. `C:\Program Files\Vulnerable.exe`
3. `C:\Program Files\Vulnerable Service1\Service.exe`
4. `C:\Program Files\Vulnerable Service1\Service 1.exe`

- To find services with unquoted binary paths we use the following command:
```
wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """
```

