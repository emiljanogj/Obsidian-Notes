#Password-recovery
***

#### Recovering a password in a Router

- Trigger a reboot, and press CTRL-C while rebooting to enter the `ronmon` mode
- Change the configuration register mode as follows:
```
confreg 0x2142
```
This mode will skip the `startup-config` while rebooting. 
- Then we have to copy the `startup-config` to the `running-config` to still have the changes:
```
copy startup-config running-config
```
- Set the new password
```
R1(config)# enable password {pass}
R1(config)# service password-encryption -> to encrypt the psasword
```

#### Recovering a password in a Switch

- If no on/off button available in the physical view of the switch, we have to power cycle the devices.
- While rebooting, press the Mode button in the physical view of the switch several times to enter the `ronmon` mode. Then we can erase the `startup config` from the Config tab, copy the running config to the startup config and reset the password similarly to the previous step. 