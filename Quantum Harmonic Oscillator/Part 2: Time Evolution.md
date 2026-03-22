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
At $t=0$, an electron in the same quadratic potential from part 1 is known to be in the state:

```math
\Large
\psi(x,0) = \frac{1}{(2\pi\sigma^2)^{1/4}} \cdot e^{\left[-\frac{(x-x_0)^2}{4\sigma^2} \right]} \cdot e^{ik_0x}
```
Using the following values for constants

|Constant|Value|Unit|
|:------:|:---:|:--:|
|$x_0$     |$2.0$|$nm$|
|$\sigma$  |$0.45$|$nm$|
|$k_0$     |$1.8$|$nm^{-1}$|

--- 
What you need to turn in:
1. Decompose $\psi(x,0)$ into its eigenspectrum, then make a plot with both $|\psi(x,0)|^2$ and $|\sum c_n\phi_n(x)|^2$.  You are going to have to determine a reasonable number of eigenfunctions to use - my suggestion is to keep track of the sum of all $c_n$ and use $|c_n|^2\approx 1$ as your convergence criteria.  In the title of your plot, include the number of eigenfunctions you used to approximate the eigenspectrum.
   * Make sure to evaluate both $\psi(x,0)$ and $\phi_n(x)$ at the same gridpoints
   * I would use something like ``scipy.integrate.simpson()`` for your integrator
   * If you store the values of the functions in ``np.array()`` you will not have any issues with the complex numbers involved with this calculation.  Remember python uses ``j`` as the imaginary number, and you have to give it a coefficient.  So $i=1*j$ in your code.
   * You also may want to use ``np.exp()``, ``np.conjugate()``, and ``np.abs()`` in your code.  They all handle complex numbers just fine.
