
- Allow current to flow in 1 direction
![[Pasted image 20231006172737.png]]

![[Pasted image 20231006172828.png]]


### Conductor - SemiConductor - Insulator

![[Pasted image 20231006173021.png]]

- In the above picture, the electrons in the **Valence shell** have the highest energy
- If an electron in the **Valence shell** can easily reach the **Conduction band** => the material is a good conductor(that is the case for copper for example where the **Conduction band** overlaps with the **Valence shell**)
- The opposite is true for an **Insulator**(rubber for example), where the **Conduction band** is very far from the **Valence shell**
![[Pasted image 20231006173348.png]]

- A conductor has between 1-3 electrons in the **Valence shell**. A **semiconductor**(silicon for example) has 4 electrons(1 too many to be a conductor)
![[Pasted image 20231006173634.png]]

- However, if we provide some energy, one of its electrons may make the jump to the **Conduction band** and silicon will become an conductor


**Silicon**

- A silicon has the following atomic structure
![[Pasted image 20231006174157.png]]
- Each atom has 4 electrons in its **Valence shell** but it wants 8 => The silicon atoms form covalence bonds:
![[Pasted image 20231006174225.png]]

**N-Type vs P-Type Semiconductors**
- Assume now that a silicon is injected with P(Phosphorous), it will take the position of some of the Si atoms. However, P has 5 electrons in its valence
![[Pasted image 20231006174840.png]]
=> When the covalence bonds between Si and P are formed there will be some free electrons that will move around

- On the other hand, if we add Aluminium(Al), it will form covalence bonds with Si. However, Al has 3 electrons in its valence => There will be a hole when Al forms a covalence bond with Si as shown below:
![[Pasted image 20231006175233.png]]

- If we join an N-type material with a P-type material we get an N-P junction(Diode). This also leads to the creation of a **Depletion Region** as shown below:
![[Pasted image 20231006175411.png]]

- In this depletion region an **Electric Field** will also be formed with a potential difference of `0.7V`
- This electric field will prevent more electrons from flowing. However we can connect a voltage source with `+` connected to the **Anode** and `-` connected to the **Cathode** as follows:
![[Pasted image 20231006175737.png]]
=> This will enable the current to flow(Called **Forward Bias** in this case)
*Note*: The voltage source must be strong than the barrier(potential difference of `0.7V`)

- The opposite happens if we connect the voltage source to the opposite side => The barrier will expand and the material will act as an insulator
![[Pasted image 20231006180058.png]]

- Note that the diode can act as an insulator up to a certain voltage difference. If the difference exceeds a certain value, current will flow and the diode will die
![[Pasted image 20231006180420.png]]


=> The main function of a Diode is to control the direction of the current flow. That's why it is used to convert AC to DC

#### AC to DC Conversion through a Diode
![[Pasted image 20231006180850.png]]

- We can also use a **Full Wave Rectifier** to allow the negative current to pass also but invert it to positive + smooth out the current using a capacitor
![[Pasted image 20231006181248.png]]

![[Pasted image 20231006182648.png]]

### DC Converter Schematics

![[Pasted image 20231007133940.png]]

We have the following elements:

- First a transformer that converts the 120V AC to 12V AC
- The 12V AC will then be converted to a 12V DC using a **Full Bridge Rectifier**
- We also use a `2200 microF` capacitor to store energy for latter use(when the converter is unplugged) + to smooth out the `12V` DC
