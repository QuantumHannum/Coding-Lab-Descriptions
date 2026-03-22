# Part 2: Time Evolution of the Quantum Harmonic Oscillator

## Activity 2
Any general wavefunction can be decomposed into its eigenspectrum - a linear combination of eigenfunctions $\phi_n(x)$ of the defining Hamiltonian weighted with coefficients:

```math
\psi(x) = \sum_{n=0}^{\infty} c_n\phi_n(x)
```
where the overlap integral calculates the coefficients $c_n$:

```math
c_n=\int_{-\infty}^{\infty} \phi^{*}_n(x) \psi(x) dx
```
When performing numerical simulations, this integral is evaluated only over the simulation's spatial mesh.  

---
at $t=0$ an electron in the same quadratic potential from part 1 is known to be in the state:

```math
\psi(x,0) = \frac{1}{(2\pi\sigma^2)^{1/4}e^{\left[-\frac{(x-x_0)^2}{4\sigma^2} \right]}

```
