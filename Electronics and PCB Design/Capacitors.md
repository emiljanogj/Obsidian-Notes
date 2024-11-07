- Stores electric charge
- It doesn't store as much energy as a battery but it charges and releases much faster
- If we connect a capacitor to a battery, the capacitor will store electrons in one plate until the capacitor has the same voltage as the battery. When this happens, no more electrons will flow
![[Pasted image 20231006234146.png]]

![[Pasted image 20231006234451.png]]



#### How does a capacitor work
1.  ![[Pasted image 20231007005149.png]]

2. ![[Pasted image 20231007005230.png]]
3. ![[Pasted image 20231007005315.png]]
4. ![[Pasted image 20231007005358.png]]
5. ![[Pasted image 20231007005429.png]]
6. ![[Pasted image 20231007005538.png]]
7. ![[Pasted image 20231007005552.png]]
8. Assume we now add molecules in between the two plates. The following happens:
![[Pasted image 20231007010202.png]]
*Note*: It has the same effect as increasing the area of the plates, i.e. increasing the **capacitance** of the capacitor. Same can be achieved by moving the plates closer together also.


### Basic Calculations for Capacitors in Series and Parallel

- Computing the charge of a capacitor
```
Charge      =     Capacitance x Voltage
(Coulombs).       (Farads)      (Volts)
```

- Energy stored in a capacitor is computed as follows:
```
Energy = 0.5 x Capacity x Voltage^2
(Joules)       (Farads)   (Volts)   
```

- 2 Ways to connect capacitors, in series or in parallel as follows:
![[Pasted image 20231007104437.png]]


*Note*: Because of the insulator, the current doesn't flow through a capacitor
![[Pasted image 20231007105235.png]]
What happens is the electrons accumulate in one of the plates and push away the electrons on the other plate as follows:
![[Pasted image 20231007105320.png]]

- When capacitors are connected in parallel, we simply add up the capacitance
- When they are connected in series we use the following formula:
![[Pasted image 20231007105528.png]]
*Note*: The resulting capacitance will be smaller than the smallest individual capacitor since we are basically increasing the thickness of the resulting insulator:
![[Pasted image 20231007105723.png]]

- In this case the individual charge of each capacitor will be equal to the total charge => smallest capacitor is the limiting factor. However the individual voltage is different since it depends on the capacity, and we use the following formula:
![[Pasted image 20231007110048.png]]

- A capacitor is not charged linearly wrt time. It follows the following curve:
![[Pasted image 20231007110718.png]]


- We see that it takes 5 time constants to get a capacitor to full charge. To compute how many seconds a time constant is we use the following formula:
```
Time constant = Resistance x Capacity
                (Ohms)       (Farads)
```

- Discharging follows this curve:
![[Pasted image 20231007110932.png]]
