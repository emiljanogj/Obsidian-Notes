- To set SELinux context(i.e. `httpd_sys_content_t`) for all the files in a certain directory(i.e. `lab-content`) we use the following command:
```
semanage fcontext -a -t httpd_sys_content_t '/lab-content(/.*)?'
```
- To then restore the context for all the files in the directory we use the following command
```
restorecon -R /lab-content/
```

- To search for all the SELinux related alerts in `/var/log/audit/audit.log` we use the following command:
```
sealert -a /var/log/audit/audit.log
```
