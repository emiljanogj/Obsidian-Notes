#Linux_PrivilegeEscalation 

---

- Two options available for NFS:
	- `root_squash` - If the root user is used to access NFS shares, it will be changed to the `nfsnobody` user, which is an unprivileged account.
	- `no_root_squash` - opposite of `root_squash`. Since root user is used to access the NFS shares, this would allow for the creation of malicious scripts with the SUID bit.

