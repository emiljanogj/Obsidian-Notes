#Windows_Privesc 

---


![[Windows_Fundamentals_Module_Cheat_Sheet.pdf]]

- To grant a user full permission to a folder(i.e. `c:\users`) we use the following command:
```bash
icacls c:\users /grant {user}:f
```
- To list permissions of a folder(`c:\users`) we use the following command:
```bash
icacls c:\users
```
