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
| --- | --- | ---- |
| $m_e$   | 511               | KeV|
|$\hbar$  | $\frac{1240}{2\pi}| eV nm|
