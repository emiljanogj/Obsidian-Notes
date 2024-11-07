- To list all the install packages we use the following command:
```
ls -l /bin/ | grep yum | awk '{print $9 " " $10 " " $11}'
```
- To filter installed or available packages based on name we use the following command:
```
dnf list 'package-name*'
```
- To filter installed/available packages based on description, name or summary fields we use the following command:
```
dnf search all 'web server'
```
- To list infos about a package we use the following command:
```
dnf info {package-name}
```
- To list the packages that provide the file `/var/www/html` for example we use the following command:
```
dnf provides /var/www/html
```
- To list all installed and available kernels we use the following command:
```
dnf list kernel

uname -r

uname -a
```
- We can also install groups of packages:
```
dnf group list

dnf group install "Group name"
```
- To print history and reverse a transaction we use the following command:
```
dnf history

dnf undo {transaction-number}
```

**Summary**

![[Pasted image 20240407183206.png]]

- To enable a repo we use the following command:
```
dnf config-manager --enable rhel-9-server-debug-rpms
```

- To add a repo we add the file `{repo-name}.repo` in `/etc/yum.repos.d` with the following format
```
[repo-name]
name={repo-name}
baseurl={url}
enabled=1
gpgcheck={0|1}
gpgkey={file:///{filepath}}
```

- We can also import a GPG key using the following command:
```
rpm --import {GPG_URL}
```


- **Note**: If we run the following command:
```
sudo rpm --install {package}
```
the package will still remain uninstalled in case of unmet dependencies.