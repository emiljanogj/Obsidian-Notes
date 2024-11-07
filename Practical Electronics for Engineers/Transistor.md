- Two types of transistors:

**NPN**
![[Pasted image 20231124174304.png]]
- The ratio $\frac{I_B}{I_C}$ is called the DC current gain and is denoted by $h_{FE}$
- When small current $I_B$ flows from the **Base** to the **Emitter**, $h_{FE} \times I_B$ flows from the **Collector** to the **Emitter** $\implies$ the total current we get on the emitter's end is $I_B + I_C = I_B + h_{FE} \times I_B = I_B \times (h_{FE} + 1)$

**PNP**
![[Pasted image 20231124174848.png]]


- To reduce the noise of the base and provide more stable operation, the transistors are usually used with two resistances as follows:
![[Pasted image 20231124175804.png]]


### MOSFET(Metal-Oxide-Semiconductor Field-Effect Transistors)

- Two types of MOSFETs

**N-channel MOSFET**
![[Pasted image 20231124181554.png]]

**P-channel MOSFET**
![[Pasted image 20231124181709.png]]



### Zener Diodes

- They are able to conduct in reverse-bias mode but they do not conduct until the applied voltage reaches or exceeds a certain voltage
- When that happens they try to limit the voltage dropped across it to the Zener voltage point
![[Pasted image 20231130221700.png]]
*Note*: In the above circuit the Zener diode is reverse-biased. As long as the supply voltage remains above 100V, the voltage dropped will be ~100V.

- The circuit must be designed such that the diode's power dissipation rating is not exceeded keeping **Joule's Law** in mind, i.e. $P=IE$.

### Mathematical Analysis of Zener Diode Regulating Circuit

![[Pasted image 20231130222634.png]]

![[Pasted image 20231130222758.png]]
=> A Zener diode with a power rating of $0.5W$ and a resistor with $1.5W$ of dissipation would be appropriate. 

![[Pasted image 20231130224309.png]]




### MOSFET
![[Pasted image 20231130233920.png]]

- **MOSFETs** have a larger gate lead input impedances($> 10^{14} \ohm$) as compared to the **JFET** impedances($>10^9 \ohm$) 

**MOSFET Construction**
![[Pasted image 20231201001622.png]]


**Several Regions**

- **Ohmic Region** - MOSFET behaves like a normal resistor, i.e. $I_D$ increases almost linearly w.r.t $V_{GS}$
- **Active Region** - Mostly influenced by $V_{GS}$ and hardly influenced by $V_{DS}$
- **Cutoff Voltage** - Often referred to as the pinch-off voltage $V_p$. Is the $V_{GS}$ that  causes the **MOSFET** to block almost all drain-source current flow
- **Breakdown Voltage** - The drain-source voltage that causes current to "break through" MOSFET's resistive channel. Denoted by $BV_{DS}$
- **Drain Current for Zero Bias** - Represents the drain current when $V_{GS} = 0$. Denoted by $I_{DSS}$
- **Transconductance** - Denotes the rate of change in the drain current with the change in gate-source voltage when the drain-source voltage is kept constant. Denoted by $g_m$

![[Pasted image 20231201003848.png]]
