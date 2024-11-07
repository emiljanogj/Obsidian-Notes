#Ohm

**AC Voltage-Current Relationship**
- They can be in-phase or out-of-phase

![[Pasted image 20230930125606.png]]

*Note*: When calculating the power in a steady state AC circuit element, we have to take into consideration the phase shift between **I** and **V** using the following formula(In this case we compute max instantaneous power but same holds for average):
```
P_max = V_p x I_p x cos(\theta)
```

- Power dissipation is computed with the following formula:
```
P = V x I = V x V/R = V^2/R
```
*Note*: To lower power dissipation we have to lower the voltage down.


### Circuit Analysis

![[Pasted image 20230930133047.png]]


#### Circuit 2

![[Pasted image 20230930185818.png]]
*Note*: With `100 Ohm` resistors we get a current of `50mA` which is way too high for LEDs. That\s why we increase it to `330 Ohms`.