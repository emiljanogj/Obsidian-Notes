- Starting from RHEL 9, SELinux can be disabled only by using the `selinux=0` kernel parameter at boot. It cannot be disabled anymore by setting `SELINUX=disabled` in `/etc/selinux/config`.
- If `SELINUX=disabled` is set in `/etc/selinux/config`, no actions are loaded => everything is disallowed by default
- An SELinux policy has the following format:
![[Pasted image 20240107113502.png]]
- To show SELinux info we use the `-Z` option as follows:
```
[root@host ~]# ls -Z /var/www 
system_u:object_r:httpd_sys_script_exec_t:s0 cgi-bin system_u:object_r:httpd_sys_content_t:s0 html
```

- To change the context of a file/directory we use the following command:
```
chcon -t {context} {file/directory}
```
- To restore the context we use the command
```
restorecon -v {file/directory}
```
- A database of file labelling policies can be found in `/etc/selinux/targeted/contexts/files` directory. An example of such policy is shown below:
```
/var/www/cgi-bin(/.*)?  all files  system_u:object_r:httpd_sys_script_exec_t:s0
```
*Note*: The policy specifies the context(`system_u:object_r:httpd_sys_script_exec_t:s0`) for the `/var/www/cgi-bin` dire ctory and all the files under it.

- To add an SELinux file context policy for a directory we use the following command:
```
semanage fcontext -a -t httpd_sys_content_t '/{directory-name}(/.*)?'
```

- To apply the context to the directory we use the following command:
```
restorecon -Rv /{directory-name}
```

- To view local customizations to the default policy, we use the following command:
```
semanage fcontext -l -C
```

### SELinux Booleans
- Used to control applications' behavior. To get a policy boolean we use the following command:
```
getsebool {boolean}
```
- And to set it we use 
```
setsebool -P {boolean} {on | off}
```
*Note*: -P is needed to make the change persistent.

- To list all boolean we use the command:
```
semanage boolean -l
```
- To list only the ones that have a different value from their boot time value we use the command:
```
semanage boolean -l -C
```
- To search audit logs for a message we use the following command:
```
[root@host ~]# ausearch -m AVC -ts recent
```
- The following command generates an action plan to fix SELinux issues:
```
sealert -l {UUID}
```
