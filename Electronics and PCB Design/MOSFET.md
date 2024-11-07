![[Pasted image 20231015011137.png]]
*Note*: The `p` and `n` symbols are used to indicate n-doped and p-doped materials.

- As is usual in NPN materials, we have a depletion region which is shown below in purple:
![[Pasted image 20231015011839.png]]


![[Pasted image 20231015013956.png]]
*Note*: The region in red is the newly formed **Depletion Region**

- It has 3 main components
	- **Drain**
	- **Gate**
	- **Source**
![[Pasted image 20231022095747.png]]
- As opposed to **BJTs** which work by applying current to the base, a **MOSFET** works by applying voltage to the gate, i.e. when voltage is applied between the **Gate** and the **Source**, the current is allowed to flow between the **Drain** and the **Source** as follows:
![[Pasted image 20231022095915.png]]

*Note*: The higher the voltage between the **Gate** and the **Source**, the lower the resistance between the **Drain** and the **Source**. This resistance is denoted by $R_{DS}ON$ and can be found on the transistor's datasheet.

=> We must follow these steps when working with **MOSFETS**:
1. From the data sheet, extract the **Gate Threshold** which is denoted as $V_{GS}$ or $V_{TH}$
2. Extract also the resistance from the **Drain** to the **Source**, denoted as $R_{DS}ON$
3. To compute the dissipated power we use the formula:
$$
P_D = \frac{max(T_J)-T_A}{R_{\theta JA}}
$$ 
### MOSFET Architecture

- At the basis we have a **P-Type Substrate** where we create 2 heavily-doped **N-Type** regions as follows:
![[Pasted image 20231022103603.png]]
*Note*: The 2 `N-Type` Regions are the **Source** and the **Drain** that we discussed in the previous section.

- On top of the **Substrate** we add an **Oxide** which acts as an insulator. On top of the **Oxide**, a metal is added, and this structure(Oxide + Metal) forms the **Gate**.
- As the voltage between the **Gate** and the **Source**($V_{GS}$) is increased, the positive voltage at the gate pushes the holes between the **Source** and the **Drain** aways so a depletion region is formed there as shown below:
![[Pasted image 20231022104219.png]]

- At the same time, an inversion layer of electrons is formed at the source as shown below:
![[Pasted image 20231022104549.png]]
*Note*: This field will only reach the **Drain** if the voltage supplied to the gate surpasses the threshold voltage $V_{TH}$
- When a voltage greater the $V_{TH}$ is applied, the current starts to flow from the drain to the source as follows:
![[Pasted image 20231022110704.png]]


![[Pasted image 20231022125508.png]]
*Note*: In the ohmic region, the voltage supplied to the gate $V_{GS}$ is not large enough =>  no current is passing from the source to the drain, so the transistor acts like a **voltage-controlled** resistor, and the drain current is proportional to the gate-source voltage and is computed with the following formula:
$$
I_D = \frac{V_{GS} - V_{TH}}{R}
$$
In the **saturation region**, the MOSFET operates as a voltage-controlled current source and is commonly used as a digital switch, i.e. the transistor is fully turned on and operates as a closed switch allowing a maximum current


### MOSFET Characteristic Curves
There are 3 regions in the characteristic curve:
- **Ohmic region** - In this region the transistor acts as a standard resistor, where $I_D$ increases linearly with $V_{DS}$
- **Non-linear region** - In this region the increase of $I_D$ with respect to $V_{DS}$ is non-linear
- **Active/saturation region** - In this region, the transistor is saturated with majority charge carriers => $I_D$ depends only on $V_{GS}$ and is independent of $V_{DS}$
![[Pasted image 20231022134619.png]]
