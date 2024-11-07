#Transistor
- Consists of 3 main parts:
![[Pasted image 20231009175219.png]]

- It may block the current from flowing in a circuit. However, if we power the middle part with a voltage source, it will then allow the current to flow:
![[Pasted image 20231009175357.png]]

- The amplification of the current from the base to the amplifier is called gain:
![[Pasted image 20231009175656.png]]


### NPN and PNP Transistors

**NPN Transistor**
- The  current from the **Control circuit** and **Main circuit** combine with one another as follows:
![[Pasted image 20231009180009.png]]
 *Note*: Above we see that the current is flowing through both wires to the transistor
![[Pasted image 20231009181249.png]]
- Above we see that if we disconnect the main circuit, the control LED will still turn on as the current is flowing through the control circuit to the transistor.


**PNP Transistor**
- The emitter is now connected to the positive pole of the battery
![[Pasted image 20231009181746.png]]

![[Pasted image 20231009181848.png]]


### How does a transistor work?

- The **NPN** Transistor structure
![[Pasted image 20231009182614.png]]
*Note*: The P-type and N-type semiconductors were discussed in [[Lecture 16 - Diodes#Conductor - SemiConductor - Insulator]]

The **PNP Transistor** structure
![[Pasted image 20231009182812.png]]

![[Pasted image 20231009183348.png]]



### Transistors - Udemy Course

![[Pasted image 20231013124535.png]]



![[Pasted image 20231013132156.png]]
*Note*: The 2 N-regions should be asymmetrically doped, i.e. **Emitter** is dopped heavily and the **Collector** is dopped lightly. The red current is the electron current, while the blue one is the traditional current.


#### Transistor in Active Mode

- In this mode, the collector's current is a function of the base's current
 ![[Pasted image 20231013133401.png]]

### Transistor as a Switch

![[Pasted image 20231014110937.png]]

- To calculate the base current needed for a transistor to enter saturation mode(to allow the free flow of the current) we use the following formula:
![[Pasted image 20231014111126.png]]
*Note*: The value of h_FE(min) can be found in the datasheet of the transistor

- We first measure the output voltage using a voltmeter
- Then we compute the base current using the above formula.
- Then we compute the base resistor using the below formulas:
$$
V = V_{bat} - 0.7
$$
*Note*: Since a transistor is simply a combination of NP junctions, the 0.7V is just the voltage drop over the depletion region.
- Then we compute the base resistance using the usual formula:
$$
R_b = V/I_b$$

### How does a diode work
![[Pasted image 20231017222907.png]]

- The region on the left is the **Emitter**, the one in the center is the **Base** and the one on the right is the **Collector**. 
- The **Emitter** and **Collector** are both negatively dopped but the **Emitter** is dopped much more heavily than the **Collector**
- When we connect the first battery(in the bottom), nothing will happen since one of the **Diodes** is **Reverse-biased**(the one on the right in the above image) => no electricity will flow
- When we connect the 2nd Power Source on top, some of the electrons on the left will escape on the right and push other electrons towards the positive charge of the battery
- From the above picture, we see that a small Base current will result in a large Collector current. In the picture above we show the electron current, but the traditional current flows in the opposite direction as shown below:
![[Pasted image 20231017224024.png]]

- We can improve the above circuit by introducing another transistor as shown below
![[Pasted image 20231017224125.png]]

![[Pasted image 20231018133106.png]]
- The ratio $\frac{I_c}{I_b}$ is called the gain($\beta$) where $I_b$ is the current from the base to the emitter and $I_c$ is the current from the collector to the emitter. 
- A simple transistor configuration is given in the diagram below:
![[Pasted image 20231018141849.png]]

### BJT Transistor as a Switch
Assume we have a circuit with the following components:
- Emitter: Connected to the ground (0V reference).
- Collector: Connected to the load (e.g., a lamp).
- Base: Connected to the control signal (e.g., push button, microcontroller).
- Load: Connected between the collector and the supply voltage.
![[Pasted image 20231018144657.png]]

- In the above circuit, if the following conditions hold:
	- No base current, i.e. $I_B = 0$
	- Base-Emiter voltage is < 0.7V(common polarity of the **Depletion Region** )
	- No collector current, i.e. $I_C = 0$
	- $V_{CE} = V_{CC}$ where $V_{CC}$ is the voltage of the battery connected to the Emiter and Collector

=> The transistor acts as an open switch:
![[Pasted image 20231018150025.png]]
### Modes of Operation
- Transistor operates in 3 modes:
	- **Active Mode** - Acts like a normal resistor and amplifies the base current. The amplified current in the collector is proportional to the base current. The conditions for a transistor to act in this mode are:
		- $V_{BE} > 0$
		- Moderate(depends on the transistor and can be found in the datasheet) amount of base current($I_B$)
		- $V_{CE}$ is neither at the min nor at the max(compared to the input voltage), which allows the transistor to swiftly respond to input changes.
	- **Saturation Mode** - Acts as a closed switch and allows max current to flow. Several conditions for a transistor to act in this mode:
		- $V_{BE} > 0$
		- High base current($I_B$)
		- $V_{CE}$ is minimized(related to point above)
	- **Cut-off Mode** - Acts as an open switch and blocks current flow(except some small leakage). Several conditions for a transistor to act in this mode:
		- $V_{BE} \leq 0$
		- $V_{CE}$ is maximized


### MOSFET Transistors

- Works similarly to a **BJT** transistor with the main difference being that in the **BJT** transistor the current from the base to the emitter decides how much current can flow from the collector to the flow while in the **MOSFET** transistor the voltage between the gate and the source decides how much current can flow from the drain to the source
- A **MOSFET** transistor is designed as follows:
![[Pasted image 20231018134156.png]]
- Below is an example circuit for turning on a **MOSFET**
![[Pasted image 20231018134231.png]]

![[Pasted image 20231018141219.png]]


### Bipolar Transistors(from Practical Electronics for Engineers)
![[Pasted image 20231025221138.png]]
*Note*: In the above image we see that the flow of electrons is from the **Base(P-type)** to the **Emitter(N-type)** where they are grounded. However for positive voltages > 0.6V some electrons escape the thin base layer and flow toward the **Collector(N-type)**.


### Transistor's Characteristic Curve
![[Pasted image 20231025221640.png]]
**Terminology:**

- **Saturation region** - Maximum current flow => the transistor acts as a closed switch from collector to emitter. Note that in this region the relation between $I_C$  and $I_B$ is almost linear
- **Cutoff region** - Transistor acts as an open switch, i.e. only a small amount of current is allowed to flow
- **Active mode/region** - The current flows but there's a non-linear relation between $I_C$ and $I_B$. Note that there's a large jump in $I_C$ for a small increase in $I_B$ for the same $V_{CE}$

*Important Note*: In **NPN** transistor, ground is in the **Emitter** => **Base** and **Collector** are both positively charged. In **PNP** transistor, ground is connected to the **Collector** => **Base** and **Emitter** are both positively charged. This is demonstrated in the picture below:
![[Pasted image 20231025223806.png]]
*Note*: There's a voltage drop during the current flow(the conventional current flows from `+` to `-`) which corresponds to the polarity of the **depletion region**(usually `0.6V` for a sillicon transistor).
So we have the following important equations for a transistor:
$$
I_C = \beta I_B
$$
$$
I_E = I_C + I_B \implies I_E=(\beta + 1)I_B
$$
$$
V_{BE} = V_B - V_E = 0.6V \; (npn)
$$
$$
V_BE = V_B - V_E = -0.6V \; (pnp)
$$

### Examples
![[Pasted image 20231025225519.png]]

### Interesting Water Analogies
**NPN**
![[Pasted image 20231025225926.png]]

**PNP**
![[Pasted image 20231025230741.png]]

### Transistor Switch
![[Pasted image 20231025232214.png]]
When the switch is turned off, $V_{CC}$ passed through $R_2$ to the ground. This is the reason why $R_2$ has usually large resistance. When the switch is turned on => transistor will be switched on since there will be current in the **Base** => $V_{CC}$ will flow to the light bulb and into the ground. The amount of current  



### BJ Transistor Biasing

![[Pasted image 20240121155104.png]]

- Assume we have the above AC signal. As we know the transistor acts as an amplifier only when it is connected to the DC supply. This is called **DC Biasing**.

### Fixed Bias Configuration
![[Pasted image 20240121160104.png]]

Since we are interested in the DC analysis of the circuit, and we know that a capacitor filters out the DC current => We can replace the capacitors with open switches to get the following circuit:

![[Pasted image 20240121160326.png]]
*Note*: $V_{BE}$ is the voltage of the depletion region, so most of the time it's $0.7V$.

We know the following equality holds:
$$
V_{CC} - V_{RC} - V_{CE} = 0 \implies V_{CE} = V_{CC} - I_C \times R_C
$$
So we get:
$$
I_C = \frac{V_{CC} - V_{CE}}{R_C} \implies I_C(max) = \frac{V_{CC}}{R_C}
$$
Furthermore, when $I_C = 0$, we have $V_{CE}=V_{CC}$. So we have the following graph:
![[Pasted image 20240121161135.png]]

- We can go to another load line by changing the $R_C$ as shown below:
![[Pasted image 20240121161619.png]]
- Or by changing $V_{CC}$
![[Pasted image 20240121161704.png]]

![[Pasted image 20240121162335.png]]
### Analysis
The analysis is as follows:
- We pick the input current, say in this case we pick $I_B=30 \mu A$
- Assume also the transistor has a gain factor of $\beta = 100$
- We also pick $R_C$ and $V_{CC}$ so assume in this case that $R_C = 1.5 k\ohm$ and $V_{CC}=10V$.
- We compute $I_C = \beta \times I_B = 3 mA$
- We than compute $V_{CE} = V_{CC} - I_C \times R_C = 10 - 4.5 = 5.5$ and we see that it's operating in active mode as shown below:
![[Pasted image 20240121162810.png]]


*Note*: If we have an AC input and the transistor is operating near cut-off point => the output will be distorted as shown below:
![[Pasted image 20240121163001.png]]



### Voltage-Divider Bias i.e. using $V_{CC}$ as the single source

![[Pasted image 20240121222344.png]]

In the above circuit we have the following:

- $V_B = \frac{R_2}{R_1 + R_2} V_{CC}$
- $V_E = V_B - V_{BE}$
- $I_C = I_E = \frac{V_E}{R_E}$
- $V_C = V_{CC} - I_C \times R_C$
- $V_{CE} = V_C - V_E$
- $V_BE= V$
