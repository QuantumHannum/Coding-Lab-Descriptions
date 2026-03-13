# Part 0: Introduction Finite Square Well

## A Particle in a box

If we place a particle of mass $m$ into a potential well defined by:
```math
V(x)=
    \begin{cases}
        V_0 & x < -a\\
        0 & -a \le x \le a\\
        V_0 & x > a\\
    \end{cases}
```
and then apply the time-independent Schrodinger Equation
```math
H(x)\phi_n(x)=E_n\phi_n(x)
```
This defines the eigenfunction (wavefunctions) $\phi_n(x)$ and corresponding eigenenergies $E_n$.  Futher defining the one-dimensional Hamiltonian

```math
H(x) = -\frac{\hbar}{2m}\frac{d^2}{dx^2} + V(x)
```
and then combining 
