# Part 0: Introduction to the Quantum Harmonic Oscillator

## A 1D quantum harmoic oscillator

What defines the 1-D quantum harmonic oscillator is the potential function
```math
v(x) = \frac{1}{2}m\omega x^2
```

This gives the larger Hamiltonian for a particle of mass $m$ as:

```math
\hat(H) = -\frac{\hbar^2}{2m}\frac{d^2}{dx^2} + \frac{1}{2}m\omega x^2
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
and $H_n(\xi) the nth Hermite Polynomial given by:
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


