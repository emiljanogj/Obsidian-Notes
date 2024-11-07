#Backup 
***

- Checking the IP on Fuelhost
```
[root@htcl-fuel-ceezbl040(fuelhost)]:~# ip -4 a s bond2.116 | grep inet | awk '{print $2}'
10.123.223.10/24
```

- Checking the IP on Bacula
```
taagjem2@vtcl-lbacula-zbl-101:~$ ip -4 a s enp2s0 | grep inet | awk '{print $2}'
10.123.220.37/27
```

- Add route on bacula server via hieradata(in `vtcl-lbacula-zbl-101`) and run Puppet:
```
https://scm.swisscom.com/tci/cfmg-int/hieradata/vtcl-puppet-zbl-101_sharedtcs_net/-/commit/11f38c13983b2f192b4d24d6c26fc2e6051b97f1
```

- Checking routing on Fuelhost
```
[root@htcl-fuel-ceezbl040(fuelhost)]:~# ip route get 10.123.220.10
10.123.220.37 via 10.123.223.1 dev bond2.116 src 10.123.223.10 uid 0
    cache
```

- Checking routing on Bacula
```
taagjem2@vtcl-lbacula-zbl-101:~$ ip route get 10.123.223.10
10.123.223.10 via 10.123.220.33 dev enp2s0 src 10.123.220.37 uid 7032455
    cache
```

- Add iptables rule manually. Needs to be done by Pace.
```
iptables -I TCI-SEC 01 -s 10.123.220.37/32 -p tcp -m tcp --dport 9102 -m conntrack --ctstate NEW -m comment --comment "Allow tcp from Bacula" -j ACCEPT
```

- Verify connection from bacula to fuelhost:
```
taagjem2@vtcl-lbacula-zbl-101:~$ echo quit | telnet 10.123.223.10 9102
Trying 10.123.223.10...
Connected to 10.123.223.10.
```

- Verify connection from fuelhost to bacula:
```
[taagjem2@htcl-fuel-ceezbl040(fuelhost)]:~$ echo quit | telnet 10.123.220.37 9103
Trying 10.123.220.37...
Connected to 10.123.220.37.
```

- Disable schedule on old bacula server:
```
[root@htcl-bkp-bil-srv]:/etc/bacula/jobs# grep -i schedule htcl-fuel-ceezbl040-vfuel.conf
  # Schedule = "Saturday Full 07:30"
```

- Reload config:
```
[root@htcl-bkp-bil-srv]:/etc/bacula/jobs# echo reload | bconsole
```

- Check that there is no schedule for the job anymore:
```
[root@htcl-bkp-bil-srv]:/etc/bacula/jobs# jobname='htcl-fuel-ceezbl040-vfuel' ; echo "status schedule days=10 job=${jobname}" | bconsole  | grep 'No Scheduled Jobs'
No Scheduled Jobs.
```

- Create MW
![[Pasted image 20230619085604.png]]

![[Pasted image 20230619085613.png]]

![[Pasted image 20230619085621.png]]

- Make sure the bacula services are still running fine:
```
[root@vtcl-lbacula-zbl-101]:~# systemctl is-active  bacula-sd
active
[root@vtcl-lbacula-zbl-101]:~# systemctl is-active  bacula-director
active
```

- Get the correct client password on the bacula server
```
[root@vtcl-lbacula-zbl-101]:~# cat /etc/bacula/clients/client_htcl-fuel-ceezbl040.conf
....
```

- Set the password in bacula-fd in the client"
```
[root@htcl-fuel-ceezbl040(fuelhost)]:~# vim /etc/bacula/bacula-fd.conf
...
Password = ...
...
```

- Get the correct director name
```
[root@vtcl-lbacula-zbl-101]:~# grep "Name.*-dir" /etc/bacula/bacula-dir.conf
  Name = vtcl-lbacula-zbl-101.sharedtcs.net-dir
```

- Change the bacula server in bacula-fd on the client:
```
[root@htcl-fuel-ceezbl040(fuelhost)]:~# grep "Name.*-dir" /etc/bacula/bacula-fd.conf
  Name = vtcl-lbacula-zbl-101.sharedtcs.net-dir
```

- Restart bacula service
```
systemctl restart bacula-fd.service
```

- Verify client connection on the bacula server:
```
[root@vtcl-lbacula-zbl-101]:~# echo 'status client=htcl-fuel-ceezbl040-fd' | bconsole  | grep Version
1000 OK: 103 vtcl-lbacula-zbl-101.sharedtcs.net-dir Version: 9.0.6 (20 November 2017)
htcl-fuel-ceezbl040-fd Version: 9.0.6 (20 November 2017) x86_64-pc-linux-gnu ubuntu 18.04
```

- Run the job manually. Before running test backup: Make sure there is no old, unneeded data in the backup folder (for cee 9: /var/lib/qemu/vfuel-backup/ and /var/lib/qemu/cic-data-backup).
Run test backup:
```
[root@vtcl-lbacula-zbl-101]:~# echo "run job=htcl-fuel-ceezbl040 yes" | bconsole
Job queued. JobId=1124
```

- List joblog to verify the job runs ok:
```
[root@vtcl-lbacula-zbl-101]:~# echo list joblog jobid=1124 | bconsole | grep -E "\|\s*B\s*\||Bytes Written|(Client|FileSet):|Elapsed|Software Comp"

```

- List files to verify job is ok:
```
[root@vtcl-lbacula-zbl-101]:~# echo list files jobid=1124 | bconsole | grep -E "/var/lib/qemu/|fuel_master.xml"
```

- Remove old config:
```
[root@htcl-bkp-bil-srv]:/etc/bacula/jobs# rm /etc/bacula/jobs/htcl-fuel-ceezbl040-vfuel.conf
[root@htcl-bkp-bil-srv]:/etc/bacula/clients# rm /etc/bacula/clients/htcl-fuel-ceezbl040.conf

[root@htcl-bkp-bil-srv]:/etc/bacula/clients# echo 'reload' | bconsole
[root@htcl-bkp-bil-srv]:/etc/bacula/clients# systemctl restart bacula-director.service
[root@htcl-bkp-bil-srv]:/etc/bacula/clients# systemctl is-active bacula-director.service
```