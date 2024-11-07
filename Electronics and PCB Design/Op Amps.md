- **Non-Inverting OpAmp**
![[Pasted image 20240115233335.png]]

- **Inverting OpAmp**
![[Pasted image 20240115233153.png]]

- **Feedback OpAmp(Negative or Positive)**
![[Pasted image 20240116000653.png]]


### Example(Summing Op-Amp Analysis)
![[Pasted image 20240116184950.png]]

**Analysis**:
We know that $i = i_a + i_b$. We also know that $i_a = \frac{V_a}{R_a}$ and $i_b=\frac{V}{R_b} \implies i = \frac{V_a}{R_a} + \frac{V_b}{R_b}$. Because of the virtual ground, we have $V_{in}^{-} = 0 \implies V_{in}^{+}=0$. We also know that $i = \frac{0-V_o}{R_f} = \frac{-V_o}{R_f}$ . All of these apply the follows:
$$
-\frac{V_o}{R_f} = \frac{V_a}{R_a} + \frac{V_b}{R_b} \implies V_o = -(\frac{R_f}{R_a}V_a + \frac{R_f}{R_b}V_b)
$$


### Example(Active Low Pass Filter)

- It's called active because it uses an active component(Op Amp)
 - An **active low pass filter with amplification** is designed as follows:

*Note*: In the above circuit the gain is $1 + \frac{R2}{R1}$.
**Proof**:
![[Pasted image 20240116230606.png]]
The highlighted point is equal to $V_{in}^{-} = V_{in}^{+} = V_{in}$ since we know that the difference should be 0. However $R_2$ and $R_1$ are simply voltage dividers so we know that $V_{in} = \frac{R_1}{R_1+R_2}V_o \implies V_o = (1+\frac{R2}{R1})V_{in}$.

- The magnitude of Gain in decibels is given by the following formula:
$$
A(DB) = 20 log_{10}(A_V)
$$
- The relation between the gain in frequency and the gain in voltage is given by the following formula:
$$
A_V = \frac{V_{out}}{V_{in}} = \frac{A_F}{\sqrt{1+{(\frac{f}{f_c})}^2}}
$$
where:
	-  $A_F$ - The pass band gain of the filter(1 + $\frac{R_2}{R_1}$)
	- f - the frequency of the input signal
	- $f_c$ - the cutoff frequency
- The following formula is also useful:
$$
f_c = \frac{1}{2\pi RC}
$$


### Simple Overdrive Pedal with OpAmp

![[Pasted image 20240116235501.png]]

### Example: Overdrive Pedal with OpAmp

![[Pasted image 20240117000843.png]]

- Note the OpAmp with gain equal to $\frac{500 k \ohm}{1 k\ohm} = 500$
- The components that are circled are called soft-clipping diodes(**TODO**: Learn more about soft-clipping diodes)
- 