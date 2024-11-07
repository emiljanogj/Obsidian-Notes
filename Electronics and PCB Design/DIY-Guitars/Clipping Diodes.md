### Soft vs Hard Clipping

![[Pasted image 20240121231435.png]]
*Note*: The capacitor in the circuit above is used for the following:

1. **Frequency-Dependent Clipping**: It can create a frequency-dependent clipping threshold. This means that the capacitor will affect the level at which the diodes begin to conduct and clip the signal based on the frequency. Higher frequencies have less impedance through the capacitor, leading to earlier clipping of high frequencies than low frequencies, which can give a more dynamic response to the distortion. That comes from the following equation:
$$
X_C = \frac{1}{2\pi fC}
$$
    where `X_C` is the resistance of the conductor.
2. **Soften Clipping**: The capacitor can also soften the onset of clipping, leading to a more gradual and less harsh transition into distortion. This is sometimes described as "soft clipping" and can make the distortion sound smoother and more musical.
    
3. **Filter High-Frequency Noise**: When diodes clip, they can generate high-frequency harmonics that may be perceived as harsh or unpleasant. The capacitor can help filter out some of this high-frequency content, resulting in a warmer tone.

### Symmetrical vs Asymmetrical Clipping

![[Pasted image 20240121232025.png]]

