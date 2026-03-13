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
This defines the eigenfunction (wavefunctions) $\phi_n(x)$ and corresponding eigenenergies $E_n$.  Further defining the one-dimensional Hamiltonian.

```math
H(x) = -\frac{\hbar}{2m}\frac{d^2}{dx^2} + V(x)
```
and then combining all of the above equations gives us the governing second-order differential equation

```math
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) + V(x)\phi(x) = E\phi(x)
```
We can then separate the problem into two physical locations: (1) Outside the box, i.e., when $|x|>a$, and (2) Inside the box, when $|x|<a$.

### Outside
```math
\begin{aligned}
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) + V_0\phi(x) &= E\phi(x)\\\\
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x)  &= (E-V_0)\phi(x)\\\\
\frac{d^2}{dx^2}\phi(x) &= \frac{2m(V_0-E)}{\hbar^2}\phi(x)\\\\
\frac{d^2}{dx^2}\phi(x) &= q^2\phi(x)\\\\
\end{aligned}
```
Which gives solutions of the form 
```math
\phi(x) = Ae^{qx}+Be^{-qx}
```
where $q^2=\frac{2m(V_0-E)}{\hbar^2}$, and $A$ and $B$ are normalization constants.

### Inside
```math
\begin{aligned}
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) &= E\phi(x)\\
\frac{d^2}{dx^2}\phi(x) &= -\frac{2mE)}{\hbar^2}\phi(x)\\
\frac{d^2}{dx^2}\phi(x) &= -k^2\phi(x)\\
\end{aligned}
```
Which gives solutions of the form 
```math
\phi(x) = C\sin(kx)+D\cos(kx)
```
where $k^2=\frac{2mE}{\hbar^2}$, and $C$ and $D$ are normalization constants.

Putting these solutions together into one piecewise wavefunction
```math
\phi(x)=
    \begin{cases}
        Ae^{qx}+Be^{-qx} & x < -a\\
        C\sin(kx)+D\cos(kx) & -a \le x \le a\\
        Fe^{qx}+Ge^{-qx} & x > a\\
    \end{cases}
```
We now need to solve for all of the normalization constants and the eigenenergies (inside the $k$ and $q$ terms).  If we consider just the even (symmetric) wavefunction, we can reduce the above piecewise function to:
```math
\phi(x)=
    \begin{cases}
        Ae^{qx} & x < -a\\
        C\sin(kx)& -a \le x \le a\\
        Ge^{-qx} & x > a\\
    \end{cases}
```
Using wavefunction continuity, differentiability, and the boundary condition at $x=a$,

```math
D\cos(ka)=Ae^{-qa}
```
```math
-kD\sin(ka) = -qAe^{-qa}
```
Dividing these two equations eliminates the normalization coefficients and leaves us with an expression involving only $k$ and $k$, both of which depend only on physical parameters and the eigenenergies.  

```math
\sqrt{\frac{2mE}{\hbar^2}}\cdot\tan\left[ \sqrt{\frac{2mE}{\hbar^2}}a\right]=\sqrt{\frac{2m(V_0-E)}{\hbar^2}}
```

However, a quick inspection of this equation shows it is transcendental and thus impossible to solve analytically for the eigenenergies.  This necessitates a numeric appraoch to finding the eigenenergies and eigenstates for the finite well.

---
## Conceptual Goals

