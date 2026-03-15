# Part 1: Find Eigenfunctions and Eigenenergies

## Activity 1
For this activity, use the finite difference approximation for the kinetic energy component of the Hamiltonian:
```math
\frac{-\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x_i) \approx -t\phi(x_{i-1})+2t\phi(x_i)-t\phi(x_{i+1})
```
where t is given by:
```math
t=\frac{\hbar^2}{2m\Delta x^2}
```
to determine the Eigenenergies and Eigenfunctions of an electron confined in a finite potential well given by:
```math
V(x)=
    \begin{cases}
        V_0 & x < -a\\
        0 & -a \le x \le a\\
        V_0 & x > a\\
    \end{cases}
```
With the following physical parameters and constants.  Notice that all units have been converted to values that will not cause numeric instability from being too small or too large.

| Parameter | Value| Units |
| :---: | :---: | ---- |
| $m_e$   | 511               | KeV|
| $\hbar$ | $\frac{1240}{2\pi}$| eV nm|
| $2a$    |1.5                |nm|
| $V_0$   |5                  |eV|

---
What you need to turn in for this activity:
1. Plots of all probability functions given by $P=|\phi_n(x)|^2$ for **ALL** bound states for this particular finite well.  Remember two things:
   * It is important to discretize the position space of the problem $x\in{x_1,x_2,...x_N}$ much wider than the width of the well to account for the exponential wings of the wavefunctions. Make sure to fix $\phi(x_1)=\phi(x_N)=0$.
   * Use ``scipy.linalg.eigh_tridiagonal()`` but remember the eigenvectors output by this function are not normalized.  I recommend either a simple Riemann sum or ``scipy.integrate.simpsons()`` to calculate normalization factors.  Also, remember we only want the bound states!
2. A table showing the following information for **ALL** bound energy states:
    | Quantum Number  (n) | Eigenenergie (eV) | Probability of Electron inside Well|
    | :---: | :---: | ---- |
    | 1  | | |
    | 2  | | |
    | 3  | | |
   * Increase the number of grid points until the first few (n=1,2,3) bound-state energies change by less than 0.001eV between successive simulations. Once again, make sure to use a spatial domain large enough that the wavefunction is essentially zero at the boundaries.
3. Your code in executable form (.py or .ipynb)
