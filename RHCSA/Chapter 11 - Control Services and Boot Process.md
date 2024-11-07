- The systemd daemon is the first process that starts in RH, i.e. PID=1
- It manages different type of objects:
	- *Service units* - have the **.service** extension
	- *Socket units* - Have the **.socket** extension and are used for inter-process communication. When a clients connects to the socket, **systemd** starts a daemon and passes the connection to it. Can also be used to delay the start of a service or to start less frequent services on demand
	- *Path units* - Have the **.path** extension and are used to delay service activation until a specific filesystem change occurs
- To list all the current services we use the following command:
```
systemctl list-units --type=service
```
*Note*: This lists only active services. To filter based on other criteria we use the `--state` option. To list all the services we use the following command:
```
systemctl list-units --type=service --all
```
- To list the unit files we use the following command:
```
systemctl list-unit-files --type=service
```

- When we restart a service, it starts again with a new PID, but when we reload it, it only reloads the configuration file.
- To list dependencies of a service we use the command:
```
systemctl list-dependencies {service-name}
```
- To prevent a service from starting we use the `mask` command, which creates a link in the config file to `/dev/null`:
```
systemctl mask {service-name}
```
- To enable the service to start at boot time we use the `enable` command. It creates a symlink in `/etc/systemd` as follows:
```
[root@root ~]# systemctl enable sshd.service  
Created symlink /etc/systemd/system/multi-user.target.wants/sshd.service → /usr/ lib/systemd/system/sshd.service.
```
*Note*: To enable the service to start at boot time and to start it now also, we use the `--now` option with the above command.


### Select the Boot Target

![[Pasted image 20240122151157.png]]

- First when the machine starts it runs a Power-ON(POST) test to check the hardware
- Then it checks for a bootable device, which is either configured in UEFI firmware or it looks for a **Master Boot Record(MBR)** on all disks
- Then, the bootloader takes control. The bootloader is GRUB2. The bootloader loads its configuration from `/boot/grub2/grub.cfg` when using BIOS or from `/boot/efi/EFI/redhat/grub.cfg`
- The bootloader then loads the kernel and **initramfs** from disk and places them in memory. **initramfs** contains all the kernel modules for the hardware that is needed during boot time, while in RHEL9, it contains a bootable root fs with a running kernel and **systemd** instance.
- Then `dracut` and `lsinitrd` are run to check **initramfs**. The first command loads its configuration from `/etc/dracut.conf.d`
- The kernel then initializes all the hardware for which it can find a module in **initramfs**. It also runs `/sbin/init` which is simply a symbolic link to `systemd`. This is the first process that is run with PID=1.
- `systemd` executes all the units required for the `initrd.target`, which includes mounting the root filesystem to `/sysroot`. It's configured in `/etc/fstab`
- `systemd` also switches from the root filesystem in **initramfs** to the root system in `/sysroot` and re-executes itself using the `systemd` copy in the disk
- `systemd` then looks for the default target specified from the kernel command line or configured in the system, and starts/stops units to reach that state. Configured with `/etc/systemd/system/default.target` or `/etc/systemd/system`

### Systemd Targets

![[Pasted image 20240122154838.png]]

- To list dependencies for a given target we use the command:
```
systemctl list-dependencies graphical.target | grep target
```
- To list all targets we use the command:
```
systemctl list-units -type=target --all
```
- To switch to another target we use the command:
```
systemctl isolate {target-name}
```
*Note*: We can only isolate a target if `AllowIsolate` is set to `yes` when we do `systemctl cat {target}`
- To set the default target we use the command:
```
systemctl set-default {target}

systemctl get-default
```

- To change it at boot time we append the kernel command with `systemd.unit={target}`


### Resetting the root password
- To reset the password we follow these steps:
	- Interrupt bootloader
	- Go to the kernel entry and press e
	- At the end of the `rescue` command we add the `rd.break` which will cause the system to break just before it hands control from **initramfs** to the actual system
- At the last step the root filesystem is mount as readonly on `/sysroot` so we have to: 
	- remount it as read write with the following command(The remount option allows remounting as rw without unmounting first):
		- `mount -o remount,rw /sysroot`
	- Enter a `chroot` jail where `/sysroot` will be treated as the root of the filesystem tree with the following command:
		- `chroot /sysroot`
	- Change the root password
		- `passwd root`
	- Ensure that all unlabeled files are relabeled during boot. SELinux assigns a label or re-labels files based on the SELinux policies 
		- `touch /.autorelabel`
- If we enable the `debug-shell.service` a **TTY9** shell is spawned during the boot sequence which can be used by admins to debug the OS while it's booting. This can be appended to the kernel command during the boot process.

**Difference between emergency.target and rescue.target**
- **emergency.target** keeps the root file system mounted as ro in `/sysroot`
- **rescue.target** waits for more of the system to be initialized such as logging or the file systems
*Note*: In both cases no changes can be made to `/etc/fstab` until the root filesystem is remounted in read-write
### Changing root password
- Interrupt the bootloader adding `rd.break` at the end of linux line
- Then we do `mount -o remount,rw /sysroot`
- Enter a chroot jail using the command `chroot /sysroot`
- Set the new password with `passwd root`
- 

- When a system has problems with mounting a filesystem we use the following steps:
![[Pasted image 20240123230550.png]]
**Note**: When entering the `emergency.target` mode we need to remount the root filesystem as rw using the following command:

```
mount -o remount,rw /
```
