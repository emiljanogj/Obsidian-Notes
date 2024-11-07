- **Container Management Tools**:
	- **podman** - manages containers and container images
	- **skopeo** - inspects, copies, signs and deletes images
	- **buildah** - builds images

- To be able to use `podman` we first need to install the proper library with the following command:
```
dnf install container-tools
```
- The config file is stored in `/etc/containers/registries.conf`. Since it's recommended to not start container in root mode we can create another config file in `$HOME/.config/containers/registries.conf` and it will overwrite the previous file.
- If we don't specify the fully qualified domain name, the list of registries in the **unqualified-search-registries** will be used

### Podman Commands

![[Pasted image 20240211105720.png]]

- To inspect an image we use the following command
```
skopeo inspect docker:{registry-fqdn}
```
- Assume we have the following Containerfile:
```
[user@host python36-app]$ cat Containerfile 
FROM registry.access.redhat.com/ubi8/ubi:latest RUN dnf install -y python36  
CMD ["/bin/bash", "-c", "sleep infinity"]
```
- Then to build the image locally we use the command:
```
podman build -t {name:version} {Dockerfile-directory}
```
- We can then inspect the local image using the command:
```
podman inspect localhost/{name:version}
```
- Then we can create the container to run latter using the command:
```
podman create --name {container-name} {image-id}
```
*Note*: We retrieve the image ID from the previous inspect command
- We can the start the container using the command:
```
podman start {container-name}
```

- To run a command inside a container we use the following command:
```
podman exec {container-name} {command}
```
- If the command contains special characters(like > below) we may need to encapsulate it as follows:
```
podman exec {container-name} sh -c '{command}'
```
- To copy a file from the host machine to the container we use the following command:
```
podman cp {filename} {container-name}:{filename}
```

### Manage Container Storage and Network Resources
- In a **rootful** container, the UIDs and GIDs in the container and the host match while in a rootless container they differ. To check the mapping we use the following commands:
```
[user@host ~]$ podman unshare cat /proc/self/uid_map 

		 0 1000 1

		  1 100000 65536

[user@host ~]$ podman unshare cat /proc/self/gid_map

         0       1000          1
         1     100000      65536
```
*Note*: The above means that the root user in the container(UID and GID 0) maps to UID=1000 and GID=1000 in the host machine

**Shared Storage**
**Goal**: Mount a directory(in this case MySQL) to a container(`db01`) with the correct permissions.

1. As a first step, we find the UID and GID of the `mysql` user in the container:
```
	docker exec -it db01 grep mysql /etc/passwd
mysql:x:27:27:MySQL Server:/var/lib/mysql:/sbin/nologin
```
So we see that the `mysql` user has UID=27 and GID=27.
2. We then create a directory we want to share in the host machine and give it the correct ownership with the `unshare` command as follows:
```
mkdir /home/user/db_data
podman unshare chown 27:27 /home/user/db_data
```
3. We then mount the `/home/user/db_data` directory in the host machine to the `/var/lib/mysql` folder in the docker container
```
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql \
registry.lab.example.com/rhel8/mariadb-105
```

*Note*: The above command will fail because of the incorrect SELinux context in the `/home/user/db_data`

**SELinux**
- The directory we mount must have the `container_file_t` SELinux context type in order for the previous command to work. We set it using the `Z` option as shown below:
```
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
registry.lab.example.com/rhel8/mariadb-105
```
- To map all traffic from a port in the containerhost to a port in the container we use the following command:
```
podman run -d --name db01 \
-e MYSQL_USER=student \
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \
-e MYSQL_ROOT_PASSWORD=redhat \
-v /home/user/db_data:/var/lib/mysql:Z \
-p 13306:3306 \
registry.lab.example.com/rhel8/mariadb-105
```
- After the above command we need to allow traffic in the specified port in the container host so we use the following command:
```
firewall-cmd --add-port=13306/tcp --permanent
firewall-cmd --reload
```

**DNS Configuration in a Container**
- Podman supports 2 backend for containers, **Netavark** or **CNI**. To find the backend we use the following command:
```
podman info --form {{.Host.NetworkBackend}}
```
- We can set the backend in `/usr/share/containers/containers.conf`
- To create a DNS-enabled network we use the following command:
```
podman network create --gateway 10.87.0.1 --subnet 10.87.0.0/16 db_net 
```
- To inspect the network we use the command
```
podman network inspect db_net
```
- We can then create 2 containers(`db01` and `container01` in this case) in the same subnet as follows:
```
[user@host ~]$ podman run -d --name db01 \ -e MYSQL_USER=student \  
-e MYSQL_PASSWORD=student \
-e MYSQL_DATABASE=dev_data \  
-e MYSQL_ROOT_PASSWORD=redhat \  
-v /home/user/db_data:/var/lib/mysql:Z \  
-p 13306:3306 \  
--network db_net \ registry.lab.example.com/rhel8/mariadb-105 [user@host ~]$ podman run -d --name client01 \ --network db_net \ registry.lab.example.com/ubi8/ubi:latest \ sleep infinity
```
- To create a subnet we use the following command:
```
podman network create {network-name}
```
- To connect a network to a running container we use the following command:
```
podman connect {network-name} {container-name}
```

### Manage Containers as System Services

- We first generate the unit file for the service using the following command:
```
podman generate systemd --name {container-name} --files /home/{user}/{container-name}.service
```
- We then move the unit file to `~/.config/systemd/user/` as follows:
```
mkdir -p ~/.config/systemd/user/
mv /home/{user}/{container-name}.service ~/.config/systemd/user/
```
- We can then treat the container as a service with the following commands:
```
systemctl --user start {container-name}.service
systemctl --user status {container-name}.service
```
*Note*: When the container is managed by `systemd`, it will restart automatically upon failure => we shouldn't use the `podman` commands to start/stop the container.

**System vs User Services**
![[Pasted image 20240212231919.png]]

*Note*: To configure a `systemd` user service to persist after user logout we use the following command:
```
loginctl enable-linger
```
- To then check if `Linger` is set we use the following command:
```
loginctl show --user {username}
```


- The `~/.config/containers/registries.conf` is defined as follows:
```
unqualified-search-registries = ['lab.example.com']

[[registry]]
location = 'lab.example.com'
insecure = true
blocked = false
```
