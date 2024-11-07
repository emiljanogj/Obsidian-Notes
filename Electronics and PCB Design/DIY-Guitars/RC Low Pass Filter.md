![[Pasted image 20240118223541.png]]

- R and C above are connected in series and they serve as voltage dividers, so we know that:

$$
V_{out} = \frac{X_C}{X_C + R} \times V_{in}
$$
where 
$$
X_C = \frac{1}{2 \pi fC}
$$
We notice above that at high frequencies $X_C = 0 \implies$ this circuit acts as a low-pass filter.

![[Pasted image 20240118224239.png]]

### Deriving the Expression for the Cutoff Frequency

First, we know that:
$$
V_{out} = \frac{X_C}{X_C +R} \times V_{in}
$$
taking the magnitude we get:
$$
\frac{|V_{out}|}{|V_{in}|} = \frac{|X_C|}{\sqrt{{|X_C|}^2 + {|R|}^2}}
$$
At the cutoff frequency we know that $V_{out} = \frac{1}{\sqrt{2}}V_{in} \implies$ 

$$
\frac{1}{\sqrt{2}} = \frac{\frac{1}{2\pi fC}}{\sqrt{R^2 + {(\frac{1}{2 \pi fC})}^2}} \implies
$$
From $\omega = 2\pi f$ we get the following:
$$
\frac{1}{\sqrt{2}} = \frac{\frac{1}{\omega C}}{\sqrt{R^2 + {(\frac{1}{\omega C})}^2}} \implies
$$
If we simplify further we get: $\omega = \frac{1}{RC}$

### Phase Response

- The phase response of a low-pass filter is given as below:
![[Pasted image 20240118231038.png]]

![[Pasted image 20240118231058.png]]


### 2nd Order Low Pass Filter

![[Pasted image 20240118231726.png]]

- The cutoff frequency is given by the following formula:
$$
f_C = \frac{1}{2\pi \sqrt{R_1 C_1 R_2 C_2}}
$$
- In this case, at the cutoff frequency we have
$$
V_{out} = {\frac{1}{\sqrt{2}}}^2 \times V_{in}
$$