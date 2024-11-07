#Capacitor 
![[Pasted image 20231108222555.png]]

- From Gauss's law, each plate must contain an equal and opposite charge given by
$$
Q = \epsilon AE = \epsilon A \frac{V}{d}
$$
*Note*: In the equation above, $E$ is the electric field given by the voltage applied to the plates divided by the distance between them. Both plates have equal but opposite sign charges
- $\epsilon$ is a constant depending on the material between the plates. Free space has a permittivity equal to $\epsilon_0=8.85 \times 10^{-12} C^2/N * m^2$
- The constant term $\frac{\epsilon A}{d}$ is known as the **Capacitance(C)** where A is the area of the plates
- *dielectric constant* of a material is given by $k=\frac{\epsilon}{\epsilon_0}$
- For an n-plate capacitor the **Capacitance(C)** is computed as follows:
$$
C=\frac{k \epsilon_0 A}{d}(n-1)
$$

### Maxwell's Displacement Current
- Basically Maxwell suggested the theory of an electric flux that permeated the dieletctric between the two plates, approximated by $\phi_E = EA$.
- The displacement was caused by a stress within the ether with the "stress" being electric and magnetic field
- He computed the current as follows:
$$
I_d = \frac{dQ}{dt}=\frac{d}{dt}(\epsilon_0 A E)=\epsilon_0 \frac{d\phi_E}{dt}
$$

### Charge-Based Model
- The current flowing through a capacitor is obtained as follows:
$$
I_C = \frac{dQ}{dt}=\frac{d(CV_C)}{dt} = C\frac{dV}{dt} \ \ (1)
$$
This implies the following:
$$
V = \frac{1}{C}\int I_C dt
$$
The above equations are true in an ideal capacitor as shown below:
![[Pasted image 20231109000631.png]]

**Energy in a Capacitor**
Using the definition of power
$$
P=dE/dt
$$
and $P=VI$, we get the following
$$
E=\int Pdt = \int VI dt 
$$
Substituting $I$ from Eq 1, we get the following
$$
E = \int V C \frac{dV}{dt} dt = \int VCdV = \frac{1}{2}CV^2
$$


### RC Circuit
![[Pasted image 20231109102601.png]]

**Charging Phase**
- From Kirchhoff's loop rule(sum of all the electric potential differences around a loop is 0), we know that the following equality holds:
$$
\epsilon - V_R - V_C = 0 \implies \epsilon - IR - \frac{q}{C} = 0 \implies \epsilon - R\frac{dq}{dt} - \frac{q}{C}
$$
From the above we get the following
$$
\frac{dq}{dt} = \frac{\epsilon C - q}{RC} \implies \int_{0}^{q} \frac{dq}{\epsilon C - q} = \frac{1}{RC} \int_{0}^{t} dt
$$
Let $u=\epsilon C - q$, then $du = -dq$. Then the following equation becomes
$$
-\int_{\epsilon C}^{\epsilon C - q} \frac{du}{u} = \frac{1}{RC}\int_0^t dt
$$
We further have the following:
$$
\int_{\epsilon C}^{\epsilon C - q}\frac{du}{u} = ln(\epsilon C - q) - ln(\epsilon C) = ln(\frac{\epsilon C - q}{\epsilon C})
$$

From the above, we get the following
$$
ln(\frac{\epsilon C - q}{\epsilon C}) = - \frac{1}{RC}t \implies \frac{\epsilon C - q}{\epsilon C} = e^{-\frac{t}{RC}}
$$
Simplifying results in the following:
$$
q(t) = C\epsilon (1-e^{-\frac{t}{RC}}) = Q(1-e^{-\frac{t}{\tau}})
$$
where $\tau = RC$ is the **time constant**.

Using the equation $I(t) = \frac{dq}{dt}$ we can also derive the following:
$$
I(t)=I_0e^{-\frac{t}{RC}}
$$
![[Pasted image 20231109150945.png]]
*Note*: From the above image we see that during the **Charging Phase**, as the charge in the capacitor increases with time, the voltage difference also increases. Meanwhile, the current in the resistor decreases and so does the voltage difference.

**Discharging a Capacitor**
- The charge of a capacitor over time is given by the following formula:
$$
q(t) = Qe^{-\frac{t}{\tau}}
$$
The current is given by $I = \frac{dq}{dt}$ which gives us the following
$$
I(t) = -\frac{Q}{RC}e^{-\frac{t}{\tau}}
$$
*Note*: The negative sign implies that the current flows in the opposite direction to its direction while the capacitor was charging


### Examples
![[Pasted image 20231109142409.png]]

From Kirchhoff's loop law we have the following:
$$
V_C(t) = \epsilon - R\frac{dq(t)}{dt} \implies V_C(t) = \epsilon - R \times I(t) = \epsilon - R \times I_0e^{-\frac{t}{\tau}} = \epsilon - R \times \frac{\epsilon}{R}e^{-\frac{t}{\tau}} = \epsilon(1-e^{-\frac{t}{\tau}})
$$
From the above we obtain
$$
t = -\tau ln(1 - \frac{V_C(t)}{\epsilon})
$$


### RC Circuit Charging Phase

![[Pasted image 20231109173400.png]]

![[Pasted image 20231109173428.png]]


### Discharging Capacitor
![[Pasted image 20231109173857.png]]

![[Pasted image 20231109173919.png]]


### Capacitors in Parallel
- When capacitors are placed in parallel, their capacitances add
$$
C_{tot} = C_1 + C_2 + \dots + C_n
$$
and the current is given by the following equation
$$
I = C_1 \frac{dV}{dt} + C_2\frac{dV}{dt} + \dots + C_n\frac{dV}{dt}
$$
Capacitors connected in parallel can be thought of as a single capacitor since the voltage is the same across all of them:
![[Pasted image 20231109175023.png]]
*Note*: The largest volume that can be applied to the capacitors in parallel is limited by the capacitor with the lowest voltage rating

### Capacitors in Series
- They act similarly to resistors in parallel
$$
\frac{1}{C_{tot}} = \frac{1}{C_1} + \frac{1}{C_1} + \dots + \frac{1}{C_n}
$$
and voltage is given by the following equation
$$
V = (\frac{1}{C_1} + \frac{1}{C_2} + \dots + \frac{1}{C_N}) \int Idt
$$
The voltage across a single capacitor(i.e. C1) is given by $\frac{C_{total}}{C_1}V_{IN}$
