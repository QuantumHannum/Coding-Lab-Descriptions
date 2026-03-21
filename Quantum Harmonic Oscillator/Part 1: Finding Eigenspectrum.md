# Part 1: Finding the Eigenspectrum of Quantum Harmonic Oscillator

## Activity 1: Use finite difference methods (again)

Using the following constants, follow the same process you used for the Finite Square Well coding lab and approximate the values of the eigenenergies and eigenfunctions.

We use an electron-scale harmonic trap, with energies measured in eV, positions in nm, and time in fs. The chosen values will give realistic nanoscale confinement, produce wavefunctions spread over about 1 nm, and yield oscillation periods on the order of 
$10^2$ $fs$, which is ideal for numerical plots and animations.

|       Constant       | Value             | Units           |
|:--------------------:|-------------------|-----------------|
| $\hbar$              | $0.6582$          | $eV \cdot fs$   |
| $\frac{\hbar^2}{2m}$ | $0.0381$          | $eV \cdot nm^2$ |
| $\hbar \omega$       | $0.050$           | $eV$            |
| $m\omega^2$          | $0.0328$          | $eV/nm^2$       |
| $x$                  | $-10\le x \le 10$ | $nm$            |
| $N$(Num. Grid Points)| $1500$            |                 |

___
Things to turn in:
1. Executable copy of your code in either .py or .ipynb
2. Plots (one per axis) of the first 5 probability distributions $|\phi_n(x)|^2$
3. A Plot showing the 20th eigenfunction probability and a plot of the **classical** harmonic oscillator probability given in the equation below.
5. A table for the first 15 quantum states showing the following values:
|Quantum Number | Calculated Eigenenergies | Exact Eigenenergies | Percent Error |
|---------------|--------------------------|---------------------|---------------|
| 1             |                          |                     |               |
| 2             |                          |                     |               |
| 3             |                          |                     |               |

Classical Harmonic Oscillator Probability Density (normalized)
```math
P(x) = \frac{1}{\pi\sqrt{A^2-x^2}}
```
with
```math
A=\sqrt{\frac{2E}{k}}
```
where k is the spring constant for the system.
