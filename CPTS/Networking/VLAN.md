#Networking

---

![[Pasted image 20240919225849.png]]

`Tag protocol identifier` (`TPID`) is a 16-bit field always set to `0x8100` to identify the `Ethernet` frame as an `802.1Q`-tagged frame. `Tag Control Information` (`TCI`) is a 16-bit field containing `Priority code point` (`PCP`), `Drop eligible indicator` (`DEI`) (previously known as `Canonical format indicator` (`CFI`)), and `VLAN identifier` (`VID`). The main field concerning `VLANs` is `VID`, occupying the low-order 12-bits of `TCI`. Since it is 12 bits, it allows 2^12 - 2 = 4096 (remember, `0` and `4095` are reserved) `VLAN` IDs. Therefore, an `802.1Q`-tagged frame can contain information for 4094 `VLANs`; the practice of inserting multiple `802.1Q` tags within a single packet is known as `Double Tagging`, introduced by [802.1ad](https://standards.ieee.org/ieee/802.1Q/10323/). `VLAN tagging` is the process of inserting `VLAN` information into an `802.1Q` `Ethernet header`, while `VLAN untagging` is the process of removing the `VLAN` information from an `802.1Q`-tagged `Ethernet` frame and forwarding the packet to the destined ports.

- To assing a VLAN to a subinterface of an interface(i.e `eth0`) we use the following command:
```bash
sudo ip link add link eth0 name eth0.20 type vlan id 20
```
- To then assign an IP to the interface and enable it we use the following commands:
```bash
emi2024@htb[/htb]$ sudo ip addr add 192.168.1.1/24 dev eth0.20
emi2024@htb[/htb]$ sudo ip link set up eth0.20
```



### Assigning VLAN ID using Powershell

- First we need to get the network interfaces in PS:
```powershell-session
PS C:\> Get-NetAdapter | Format-Table -AutoSize

Name                                           InterfaceDescription                                                          ifIndex Status             MacAddress              LinkSpeed
----                                           --------------------                                                          ------- ------             ----------              ---------
VirtualBox Host-Only Network  VirtualBox Host-Only Ethernet Adapter                                        20 Up                    0A-00-27-10-42-15       1 Gbps
Ethernet 2                                 ASIX AX88772B USB2.0 to Fast Ethernet Adapter                            55 Up                    90-EB-78-14-21-7F    100 Mbps
Bluetooth Network Connection  Bluetooth Device (Personal Area Network)                                   18 Disconnected   38-41-25-E8-DE-2D        3 Mbps
Wi-Fi                                         Intel(R) Wireless-AC 9560 160MHz                                                12 Disconnected   8E-36-6A-7A-BA-6A 866.7 Mbps
```

- To get the `VLAN ID` for a given interface we use the following command:
```powershell-session
PS C:\> Get-NetAdapterAdvancedProperty -DisplayName "vlan id"

Name                      DisplayName                    DisplayValue                   RegistryKeyword RegistryValue
----                      -----------                    ------------                   --------------- -------------
Ethernet 2                VLAN ID                        10                                     VLAN_ID               {10}
```

- To assing VLAN ID we use the following command:
```powershell-session
PS C:\> Set-NetAdapter -Name "Ethernet 2" -VlanID 10
```

