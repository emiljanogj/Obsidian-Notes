#Linux_PrivilegeEscalation 

![[Pasted image 20240818010122.png]]

**Example of Adding Capability**

```bash
 sudo setcap cap_net_bind_service=+ep /usr/bin/vim.basic
```

**Note**:
- There's 2 options when assigning a capability:
	- `+ep` - Allows effective and permitted privileges for the specified capability to the executable. It doesn't allow it to perform any actions not allowed by the capability
	- `+ei` - Allows effective and inheritable privileges for the specified capability => Child processes spawned by this binary will inherit the capability privileges


**Other Notable Capabilities**

![[Pasted image 20240818105135.png]]


- We can enumerate binaries capabilities using the following command:
```Bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
