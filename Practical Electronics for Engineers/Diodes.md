#Diode #PNP

**Forward Biased**
![[Pasted image 20231117101006.png]]

**Reverse Biased**
![[Pasted image 20231117101026.png]]


**Flyback Diode**
- When the switch in a circuit is opened, the current will be dissipated according to the following graph:
![[Pasted image 20231117153318.png]]
where $\tau = \frac{L}{R}$ where L is the inductance in Henry and R is the resistance present in the circuit

- Assume we have the following circuit:
![[Pasted image 20231117153601.png]]
where the switch is closed(shown in the left).

- When we open the switch, the inductor will reverse its polarity so we will get the following loop between the diode and the inductor:
![[Pasted image 20231117155033.png]]


### Rectification with Diodes
- **Rectification** - Process of converting AC to DC
![[Pasted image 20231117232552.png]]
*Note*: The above image shows a half-wave rectifier, and as we can see it's quite inefficient because it discards the voltage for half the cycle.

- To address the issue above with the **half-wave rectifier** we use the **full-wave rectifier** as shown below:
![[Pasted image 20231117233254.png]]

**Positive Half-Cycle**
![[Pasted image 20231117235437.png]]
*Note*: During the positive cycle, only the upper half is transmitting current, since the lower half diode is reverse-biased => it blocks the current 

**Negative Half-Cycle**
![[Pasted image 20231117235646.png]]
*Note*: During the negative half-cycle only the lower-half diode allows current to pass


### Full-Wave Bridge Rectifiers
![[Pasted image 20231118000053.png]]

**Positive Half-Cycle**
![[Pasted image 20231118000504.png]]

**Negative Half-Cycle**
![[Pasted image 20231118000515.png]]

*Note*: The disadvantage compared with the previous design is that the current now goes through 2 diodes => voltages decreases by $2*0.7V = 1.4V$ for Si-based diodes.

### Peak Detector Diode
![[Pasted image 20231118121539.png]]
