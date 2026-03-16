# Part 0: Introduction — Finite Square Well

## A Particle in a Box

Consider a particle of mass $m$ in a one-dimensional potential well defined by

```math
V(x)=
    \begin{cases}
        V_0 & x < -a\\
        0 & -a \le x \le a\\
        V_0 & x > a\\
    \end{cases}
```

Applying the time-independent Schrödinger equation

```math
H(x)\phi_n(x)=E_n\phi_n(x)
```

defines the eigenfunctions (wavefunctions) $\phi_n(x)$ and corresponding eigenenergies $E_n$. The one-dimensional Hamiltonian is

```math
H(x) = -\frac{\hbar}{2m}\frac{d^2}{dx^2} + V(x)
```

Combining these expressions gives the governing differential equation

```math
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) + V(x)\phi(x) = E\phi(x)
```

To solve this equation we consider two spatial regions:

1. **Outside the well:** $|x| > a$  
2. **Inside the well:** $|x| \le a$

---

## Outside the Well

```math
\begin{aligned}
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) + V_0\phi(x) &= E\phi(x)\\\\
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x)  &= (E-V_0)\phi(x)\\\\
\frac{d^2}{dx^2}\phi(x) &= \frac{2m(V_0-E)}{\hbar^2}\phi(x)\\\\
\frac{d^2}{dx^2}\phi(x) &= q^2\phi(x)\\\\
\end{aligned}
```

The solutions have the form

```math
\phi(x) = Ae^{qx}+Be^{-qx}
```

where

```math
q^2=\frac{2m(V_0-E)}{\hbar^2}
```

and $A$ and $B$ are normalization constants.

---

## Inside the Well

```math
\begin{aligned}
-\frac{\hbar^2}{2m}\frac{d^2}{dx^2}\phi(x) &= E\phi(x)\\
\frac{d^2}{dx^2}\phi(x) &= -\frac{2mE}{\hbar^2}\phi(x)\\
\frac{d^2}{dx^2}\phi(x) &= -k^2\phi(x)\\
\end{aligned}
```

The solutions are

```math
\phi(x) = C\sin(kx)+D\cos(kx)
```

where

```math
k^2=\frac{2mE}{\hbar^2}
```

and $C$ and $D$ are normalization constants.

---

## Piecewise Wavefunction

Combining the solutions gives

```math
\phi(x)=
    \begin{cases}
        Ae^{qx}+Be^{-qx} & x < -a\\
        C\sin(kx)+D\cos(kx) & -a \le x \le a\\
        Fe^{qx}+Ge^{-qx} & x > a\\
    \end{cases}
```

The constants and allowed energies are determined by enforcing **continuity of the wavefunction and its derivative** at the boundaries.

If we consider only **even (symmetric) wavefunctions**, the expression simplifies to

```math
\phi(x)=
    \begin{cases}
        Ae^{qx} & x < -a\\
        D\cos(kx)& -a \le x \le a\\
        Ge^{-qx} & x > a\\
    \end{cases}
```

Applying continuity and differentiability at $x=a$ gives

```math
D\cos(ka)=Ae^{-qa}
```

```math
-kD\sin(ka) = -qAe^{-qa}
```

Dividing these equations removes the normalization constants and produces an expression involving only $k$ and $q$:

```math
\sqrt{\frac{2mE}{\hbar^2}}\cdot\tan\left[ \sqrt{\frac{2mE}{\hbar^2}}a\right]=\sqrt{\frac{2m(V_0-E)}{\hbar^2}}
```

This equation is **transcendental**, meaning it cannot be solved analytically for the eigenenergies. Therefore we must use **numerical methods** to determine the allowed energies and wavefunctions.

---

# Conceptual Goals

By completing this tutorial, students should understand that:

1. Stationary quantum states arise from the eigenvalue problem

```math
\hat{H}\phi_n(x)=E_n\phi_n(x)
```

which produces discrete energies $E_n$ and eigenfunctions $\phi_n(x)$.

2. Bound states occur only when the particle energy is below the potential outside the well. The number of bound states depends on the well depth and width.

3. Quantum confinement produces discrete energy spectra.

4. Energy differences between quantum states determine photon energies

```math
E_{photon}=E_i-E_f
```

5. Wavefunctions represent probability amplitudes. The probability density is

```math
P=|\psi(x)|^2
```

6. Finite wells allow **quantum tunneling**, so bound-state wavefunctions extend beyond the well.

7. Higher-energy states penetrate further into the barriers.

8. In the deep-well limit, the spectrum approaches that of the infinite square well and becomes approximately quadratic in $n$.

9. Many quantum systems require **numerical methods** when analytic solutions are not possible.

---

# Computational / Coding Goals

Students will learn how to:

1. Discretize a spatial domain into a numerical grid  
2. Approximate derivatives using finite differences  
3. Construct a **tridiagonal Hamiltonian matrix**  
4. Use sparse matrices for computational efficiency  
5. Compute eigenvalues and eigenvectors using `scipy.linalg.eigh_tridiagonal()`  
6. Normalize eigenvectors numerically  
7. Interpret eigenvectors as physical wavefunctions  
8. Plot eigenfunctions and energy levels  
9. Plot energy vs. quantum number  
10. Explore how well width and depth affect the spectrum  
11. Compute photon wavelengths from energy transitions  
12. Identify well sizes that produce visible-light emission  

---

# Mathematical Goals

Students will practice:

1. Finite-difference approximations to derivatives  
2. Matrix representations of differential operators  
3. Numerical normalization of wavefunctions  
4. Numerical integration of probability densities  
5. Converting energy differences into photon wavelengths
