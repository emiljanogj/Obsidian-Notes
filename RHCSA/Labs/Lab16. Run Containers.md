- The config file with the container registry should be stored in:
```
~/.config/containers
``` 
- Assume we want to map a director `/dir` from the containerhost to `/var/lib/dir` in the container, and we would like the `/var/lib/dir` to have `uid` and `gid` set to 30. Then we use the following command:
```
podman chown 30:30 /dir
```
*Note*: This ensures that the permission in containerhost will be adjusted accordingly, since we know that for rootless containers, the `uid` and `gid` are different to the ones in containerhost.

- To run a container as a service we use the following commands:
```
1. mkdir ~/.config/systemd/user && cd ~/.config/systemd/user


2. podman generate systemd --files --new


3. systemctl --user daemon-reload


4. systemctl --user enable --now container-{container-name}.service


5. loginctl enable-linger -> ensures user sessions persist even after logout
```