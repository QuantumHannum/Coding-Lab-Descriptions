<<<<<<< Updated upstream
# Part 0: Introduction to the Quantum Harmonic Oscillator

## A 1D quantum harmoic oscillator

What defines the 1-D quantum harmonic oscillator is the potential function
```math
V(x) = \frac{1}{2}m\omega^2 x^2
```

This gives the larger Hamiltonian for a particle of mass $m$ as:

```math
\hat{H} = -\frac{\hbar^2}{2m}\frac{d^2}{dx^2} + \frac{1}{2}m\omega^2 x^2
```

It can be shown by employing raising and lowering operators (factorization method) that this Hamiltonian has exact energy eigenvalues and eigenfunctions of:

```math
E_n= \left( n+\frac{1}{2} \right) \hbar \omega \quad \quad n = 0, 1, 2, ...
```
```math
\phi_n(x)=\frac{1}{\sqrt{2^nn!}}\left(\frac{m\omega}{\pi \hbar} \right)^{1/4}H_n(\xi)e^{-\xi^2/2}
```
with
```math
\xi = \sqrt{\frac{m\omega}{\hbar}}x
```
and $H_n(\xi)$ the nth Hermite Polynomial given by:
```math
\begin{align}
H_0(\xi) &=1\\
H_1(\xi) &= 2\xi\\
H_2(\xi) &= 4\xi^2-2\\
H_3(\xi) &= 8\xi^3-12\xi
\end{align}
```
all of which can be generated using the Rodrigues formula

```math
H_n(x) = (-1)^ne^{x^2}\frac{d^n}{dx^n}\left(e^{-x^2}   \right)
```

Despite the exact solutions of the quantum harmonic oscillator, there is still value in exploring its behavior computationally using the finite-difference tri-diagonal Hamiltonian matrix we have previously developed. 

---
## Conceptual Goals
1. Explain why the quantum harmonic oscillator has linearly spaced energy levels and how this differs from other systems (e.g., infinite square well).
2. Interpret eigenvalues as allowed energy measurements and eigenfunctions as stationary states.
3. Describe why stationary states have time-independent probability densities, even though the wavefunction evolves.
4. Explain why superpositions lead to time-dependent behavior due to relative phase evolution.
5. Connect the shape of quantum probability densities to classical motion (correspondence principle).
6. Explain why higher-energy eigenstates better approximate the classical probability distribution.
7. Interpret an eigenspectrum $|c_n|^2$ as a distribution of energy measurement probabilities.
8. Predict qualitative motion (e.g., oscillation, spreading, interference) from the composition of eigenstates.
9. Explain how expectation values like $\langle \hat{x} \rangle (t)$ and $\langle \hat{p} \rangle (t)$ can behave like classical oscillatory motion, even though the underlying probability density may be more complex.

## Computational/Coding Goals
Students will learn how to:
1. Construct a finite-difference Hamiltonian matrix for the harmonic oscillator and use it to generate approximations for the eigenenergies and eigenfunctions.
2. Compute expansion coefficients $c_n$ (overlapp) using numerical integration to decompose a general wavefunction into its eigenspectrum.
3. Generate an animation (.gif or .mov) of the time-dependent probability density.
4. Computation of expectation values of position and momentum as functions of time and compare them to classical oscillatory behavior.


=======
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
By completing this tutorial, students should understand that:
1. Stationary quantum states arise from an eigenvalue problem:
    ```math
    \hat{H}\phi_n(x)=E_n\phi_n(x)
   ```
   which produced discrete energy eigenvalues $E_n$ and associated eigenfunctions $\phi_n(x)$.
2. Bound states occur only for energies below the potential outside the well. The number of bound states depends on the depth and width of the well.
3. Quantum confinement produces discrete energy spectra. When a particle is confined to a small region of space, only certain energies are allowed.
4. Energy gaps between quantum states determine photon emission energies.
When a particle transitions between states, a photon with energy
```math
E_{photon}=E_i-E_f
```
5. Wavefunctions represent probability amplitudes. The probability density of finding a particle at position $x$ is:
    ```math
    P=|\psi(x)|^2
    ```
6. Finite potential wells allow tunneling. Bound-state wavefunctions extend beyond the well into the classically forbidden region.
7. Higher-energy states penetrate further into the barriers. As energy approaches the top of the well, the probability outside the well increases.
11. The finite well approaches the infinite well in the deep-well limit. The energy spectrum becomes approximately quadratic in the quantum number.
12. Quantum systems can be solved numerically when analytic solutions are difficult or impossible.

## Computational/Coding Goals
Students will learn how to:
1. Discretize a continuous spatial domain into a numerical grid.
2. Approximate differential operators using finite differences.
3. Construct a tridiagonal Hamiltonian matrix representing the Schrödinger equation.
4. Use sparse matrix representations for computational efficiency.
5. Use a numerical eigensolver ``scipy.linalg.eigh_tridiagonal`` to compute eigenvalues and eigenvectors.
6. Normalize eigenvectors numerically.
7. Interpret eigenvectors as physical wavefunctions.
8. Plot eigenfunctions and energy levels.
9. Plot the energy spectrum as a function of quantum number.
10. Investigate how well width and well depth affect the energy spectrum.
11. Compute the photon wavelength emitted by a quantum transition.
12. Determine what well widths produce visible-light emission.


## Mathematical Goals
By completing this tutorial, students will practice:
1. Finite difference approximations to derivatives
2. Matrix representations of differential operators, producing a tridiagonal Hamiltonian.
3. Normalization of wavefunctions via numeric integration
4. Numerical integration of probability densities
5. Converting energy differences into photon wavelengths
>>>>>>> Stashed changes
