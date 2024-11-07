#SELinux

- **SELinux** implements **Mandatory Access Control(MAC)** => Every process and system resource has a special security context called an *SELinux context*.
- **SELinux contexts** have several fields: user, role, type and security level where the **SELinux** type information is the most important for a policy.
- **Mandatory vs Discretionary**:
	- **Discretionary** - User permissions. They are called discretionary since a user can authenticate as another user and change their permissions at will
	- **Mandatory** - **SELinux** permissions. A process cannot change its security context with it being explicitly allowed
- To get the `security.selinux` extended attribute for a particular file we use the following command:
```
root# getfattr -m security.selinux -d /var/log/audit/audit.log
```
or we use the equivalent command:
```
root# ls -lZ /var/log/audit/audit.log
```

### SELinux Modes: Permissive vs Enforcing
In **permissive** mode, **SELinux** is basically turned off but it still logs what it would have denied if it were turned on.

- To change the context of a file we use the following command:
```
chcon -v-t {context} {filename}
```
*Note*: This is not persistent and the original context is restored with the command `restorecon -v {filename}`