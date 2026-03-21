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
\phi_n(x)=\frac{1}{\sqrt{2^nn!}}\left(\frac{m\xi}{\pi \hbar} \right)^{1/4}H_n(\xi)e^{-\xi^2/2}
```
with
```math
\xi = \sqrt{\frac{m\omega}{\hbar}}x
```
and $H_n(\xi) the nth Hermite Polynomial given by:
```math
\begin{align}
H_0(\xi) $=1\\
H_1(\xi) $= 2\xi\\
H_2(\xi) $= 4\xi^2-2\\
H_3(\xi) $= 8\xi^3-12\xi
\end{align}
```
all of which can be generated using the Rodrigues formula

```math
H_n(x) = (-1)^ne^{x^2}\frac{d^n}{dx^n}\left(e^{-x^2}   \right)
```
---
## Conceptual Goals


## Computational/Coding Goals
Students will learn how to:



## Mathematical Goals
By completing this tutorial, students will practice:

