`![[Pasted image 20231014200710.png]]

- Assume a 1 $\micro$F capacitor at 1 kHZ provides a resistance of $160 \Omega$ => A capacitor 10 $\micro$F at 1 kHz will provide a resistance of 16 $\Omega$. Same will happen if we increase the frequency => It will allow high frequencies and block low frequencies:
![[Pasted image 20231014203130.png]]
*Note*: The two lines above are used to represent a **Capacitor**


### Low Pass Filter
![[Pasted image 20231014203405.png]]
- In the above image we see that the **Input** is connected directly to the **Output** => Low frequencies are sent directly out while high frequencies are sent to the **Ground**, i.e. they are disposed of.


- The graph below demonstrates the concept of cutoff frequency for **HPF**
![[Pasted image 20231014211606.png]]

The formula to calculate the cutoff frequency is as follows
$$
f_{cutoff} = \frac{1}{2 \pi RC}
$$

**Example**: Assume we want to design a **HPF** with a frequency of 30 kHZ and we want to use a 10k$\Omega$ resistor. Then we compute the capacitance of our capacitor using the above formula and it computes to $C=564pF$. 

![[Pasted image 20231014212231.png]]


![[Pasted image 20231014212257.png]]

