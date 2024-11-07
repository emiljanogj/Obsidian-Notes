- *Note*: To enable `VTP` we need the interface to be in trunking mode:
```
SW(config)# int g1/0/1
SW(config-if)# switchport trunk encapsulation dot1q
SW(config-if)# switchport mode turnk
SW# show int trunk
```