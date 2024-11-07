
Ansible can be install with the following command:

```
sudo yum install ansible python-netaddr
```

Important options include the following:
- serial - specifies the number of hosts that are completed before moving to the other host. By default it is set to 0 => all the targeted hosts are run in parallel.
- fork - The number of hosts Ansible talks in parallel for each task. By default it is set to 5 => Combining serial = 0 + fork = 5 => Ansible talks to 5 hosts for each task, then continue to the next 5 etc. After the first task is finished, Ansible continues with 5 hosts for the 2nd task, the next 5 etc.