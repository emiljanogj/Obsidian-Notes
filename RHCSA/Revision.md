
- Check chage man pages
- Check semanage for directory + restorecon -Rfvv


- To change the SELinux context(i.e. `httpd_sys_content_t`) of a whole folder(i.e `/a`) we use the following command:
```
semanage fcontext -a -t httpd_sys_content_t /a(/.*)?

restorecon -Rv /a
```



- The format of a podman config file is as follows:
```
unqualified-search-registries = ['lab.example.com']

[[registry]]
localtion={url}
blocked=false
insecure=true
```

- To add a repo in RedHat we add the following file in `/etc/yum.repos.d/{repo-name}.repo`
```
[repo]
baseurl={url}
enable=1
gpgcheck=0
gpgkey={file:///{path_to_file}}
```