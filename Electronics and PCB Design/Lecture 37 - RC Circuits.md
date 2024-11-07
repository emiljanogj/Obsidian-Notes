- The charging stage follows this curve:
![[Pasted image 20240129142044.png]]

*Note*: At $t = 5 \times RC$ we have $V(t) \approx V_i$ , so the capacitor is fully charged.

### Discharging Equations
- In case the current is flowing clockwise as shown below, the following equation holds:
![[Pasted image 20240129152630.png]]
$$
I = C \frac{dV}{dt}
$$
- However when the capacitor is discharging, the current flows in the opposite direction as shown below:
![[Pasted image 20240129152731.png]]
=> $I = -C \frac{dV}{dt}$
- Replacing $I = \frac{V}{R}$ we get $\frac{V}{R} = -C\frac{dV}{dt}$ which implies the following:
$$
\frac{dV}{dt} = -\frac{1}{CR}V(t)
$$
- Above is a differential equation whose solution is given by:
$$
V(t) = Ae^{-\frac{t}{\tau}}
$$
- At t = 0, we know that the capacitor is fully charged => it has the same voltage as the source and $A=V(0)=V_0$
- So substituting the above we get the following:
$$
V(t) = V_0e^{-\frac{t}{\tau}}
$$
- Taking the derivative with respect to $t$, we get:
$$
\frac{dV}{dt} = -\frac{1}{\tau}V_0e^{-\frac{t}{\tau}} = -\frac{1}{\tau}V(t) = -\frac{1}{CR}V(t) \implies \tau = CR
$$
where **C** - capacitance and **R** - resistance.

