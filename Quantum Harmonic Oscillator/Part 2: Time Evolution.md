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
### What you need to turn in:
Decompose $\psi(x,0)$ i(initial wavefunction) into the harmonic oscillator eigenbasis

```math
   c_n=\int\phi^*_n(x)\psi(x,0)dx
```
Then reconstruct the wavefunction:

```math
\psi_{reconstructed}(x,0)=\sum_n c_n \phi_n(x)
```
and make a plot comparing

```math
|\psi(x,0)|^2 \quad \quad |\psi_{reconstructed}(x,0)|^2
```

You will need to determine a reasonable number of eigenfunctions to include in this expansion.  I suggest using the normalization condition as a convergence check:

```math
\sum_n|c_n|^2 \approx 1
```
Increase the number of eigenfunctions until this condition is satisfied.  You should decide for yourself how close to ``1`` you want this sum to be.  

>In the title of your probability plot, include the number of eigenfunctions you used.

### Tips/Pitfalls
* Make sure to evaluate both $\psi(x,0)$ and $\phi_n(x)$ on the same grid points.
* I recommend using ``scipy.integrate.simpson()`` to compute overlap integrals.
* Store all functions as ``np.array`` objects.  NumPy handles complex numbers automatically, so you should never need any special conversions.
* Python uses ``j`` for the imaginary unit, not ``i``.  You also need to give it a coefficient!! so $i = 1j$ in python.
* The eigenvectors returned by ``sicpy.linalg.eigh_tridiagonal()`` are real-valued ``np.array``.  When you multiply them by a complex wavefunction, NumPy will automatically handle the complex arithmetic.
* You will likely find the following NumPy functions useful; all of them can handle complex numbers:
   * ``np.exp()``
   * ``np.conjugate()``
   * ``np.abs()``
* As an additional check for your plot, you may want to compare the original and reconstructed wavefunctions by plotting the real and imaginary parts separately, or by computing their overlap.

---
### Things to turn in:
1. A plot showing $|\psi(x,0)|^2$ and $|\psi_{reconstructed}|^2 that indicated how many expanstion terms you used.
---

## Activity 3
To evolve a wavefunction forward in time, we just need to add a phase to each term in the expansion:
```math
\psi_{reconstructed}(x,t)=\sum_n c_n \cdot phi_n(x) \cdot e^{-iE_n/\hbar t}
```
Where $E_n$ are the eigenenergies (found using ``scipy.linalg.eigh_tridiagonal()``).  In this part of the coding lab, you will produce an animation of the time evolution of the initial state $\psi(x,t)$.

### Constructing the time steps

To animate the evolution, you will need to choose a sequence of times $t_0, t_1, t_2, ...$ and evaluate the reconstructed wavefunction at each time.

A good way to choose the total evolution time is to use the natural oscillator period
```math
T=\frac{2\pi}{\omega}
```
where $\omega$ is the angular frequency from your potential (part 1).

A good strategy is:
* evolve for several full oscillation periods so the motion is easy to see, for example, let $t_{max}=4T$
* divide that total time into many small time steps so the animation looks smooth

```python
t_vals = np.linspace(0,t_max,num_frames)
```
where the number of frames should be a few hundred.

For each value of ``t`` in ``t_vals``:
- build the time-dependent wavefunction
  ```math
   \psi(x,t)=\sum_n c_n \cdot \phi_n(x) \cdot e^{-iE_n t/\hbar}
  ```
- compute the probability density
  ```math
   P(t)=|\psi(x,t)|^2
  ```
- store that probability density so it can be plotted frame-by-frame in an animation.


### Code snippet to make .mov file
This example assumes you already have:
* ``x`` = spatial grid
* ``energies`` = array of eigenenergies
* ``eigenvectors`` array of eigenfunctions evaluated on the grid ``x``
* ``c_vals`` = overlap coeficients
* ``hbar`` = reduced Planck constant (from part 1)
* ``omega`` = oscillator angular frequency (from part 1)

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import simpson
from matplotlib.animation import FuncAnimation, FFMpegWriter

# ----------------------------------------
# Choose time range
# ----------------------------------------
T = 2 * np.pi / omega
t_max = 4 * T
num_frames = 300
t_vals = np.linspace(0, t_max, num_frames)

num_states_used = len(c_vals)

# ----------------------------------------
# Store wavefunction and probability density
# ----------------------------------------
psi_frames = []
probability_frames = []

for t in t_vals:
    psi_t = np.zeros_like(x, dtype=complex)

    for n in range(num_states_used):
       #************ ADD YOUR CODE HERE ***************
       # This section should find psi(x,t) by adding up the
       # contributions from each c_n * phi_n(x) and store it
       # as a np.array named psi_t
       #***********************************************




    psi_frames.append(psi_t)

    prob_density = np.abs(psi_t)**2
    probability_frames.append(prob_density)

psi_frames = np.array(psi_frames)
probability_frames = np.array(probability_frames)

# ----------------------------------------
# Animation of probability density
# ----------------------------------------
fig, ax = plt.subplots(figsize=(8, 5))
line, = ax.plot(x, probability_frames[0], lw=2)

ax.set_xlabel("x (nm)")
ax.set_ylabel(r"$|\psi(x,t)|^2$")
ax.set_title("Time Evolution of Probability Density")

ax.set_xlim(x.min(), x.max())
ax.set_ylim(0, 1.1 * np.max(probability_frames))

time_text = ax.text(
    0.02, 0.95, "", transform=ax.transAxes,
    verticalalignment="top"
)

def update(frame):
    line.set_ydata(probability_frames[frame])
    time_text.set_text(f"t = {t_vals[frame]:.2f} fs")
    return line, time_text

anim = FuncAnimation(
    fig,
    update,
    frames=num_frames,
    interval=40,
    blit=True
)

writer = FFMpegWriter(fps=20)
anim.save("sho_time_evolution.mov", writer=writer)

plt.show()
```

If saving ``.mov`` files doesn't work on your computer, it usually means FFmpeg isn't installed.  In that case, you can save your animation as a ``.GIF`` file using ``PillowWriter``.

```python
from matplotlib.animation import PillowWriter
anim.save("sho_time_evolution.gif", writer=PillowWriter(fps=20))
```

---
### Things to turn in
1. Your .mov (or .gif) file showing the time evolution of the initial state $|\psi(x,t)|^2$
----

## Activity 4

In addition to animating the time evolution of the system, it's useful to calculate the expectation value of position $\langle \hat{x} \rangle (t)$.  This can be done in two ways:

```math
\langle \hat{x} \rangle (t) = \int \psi^*(x,t) \cdot x \cdot \psi(x,t) dt
```
and because x commutes with $\psi(x,t)$

```math
\langle \hat{x} \rangle (t) = \int  x \cdot |\psi(x,t)|^2 dt
```

For a wave packet in the harmonic oscillator, you should find that $\langle \hat{x} \rangle (t)$ oscillates approximately sinusodially, similar to the position of a classical mass on a spring.  This correspondence is referred to as 

> **Ehrenfest's Theorem**, which states that expectation values of quantum operators follow classical equations of motion.

### Calculating Expectation Value of Position as a function of time
Go back to your last code and add another list to track the expectation value near the other lists:
```python
psi_frames = []
probability_frames = []
x_expectation_vals = []
```
Then, inside the main for loop, after you have found the ``probability_density``, calculate the expectation value of position:
```python
psi_frames.append(psi_t)
prob_density = np.abs(psi_t)**2
probability_frames.append(prob_density)

#******** PUT YOUR CODE HERE ************
# calculate x_exp
# append x_exp into x_expectation_vals
#****************************************
```
Finally, convert ``x_expectation_vals`` into a ``np.array``.

---
What to turn in
1.  Using the code below, make a plot of $\langle \hat{x} \rangle (t)$ and then fit a cosine curve of the form:

```math
\langle \hat{x} \rangle (t) = A \cos(\omega t + \phi) + C
```

```python
from scipy.optimize import curve_fit

# ----------------------------------------
# Define fitting function
# ----------------------------------------
def sinusoid(t, A, omega, phi, C):
    return A * np.cos(omega * t + phi) + C

# ----------------------------------------
# Initial guesses
# ----------------------------------------
A_guess = (np.max(x_expectation_vals) - np.min(x_expectation_vals)) / 2
C_guess = np.mean(x_expectation_vals)
omega_guess = omega   # use known oscillator frequency as a starting guess
phi_guess = 0

initial_guess = [A_guess, omega_guess, phi_guess, C_guess]

# ----------------------------------------
# Perform fit
# ----------------------------------------
params, covariance = curve_fit(
    sinusoid,
    t_vals,
    x_expectation_vals,
    p0=initial_guess
)

A_fit, omega_fit, phi_fit, C_fit = params

# ----------------------------------------
# Print results
# ----------------------------------------
print(f"Fitted amplitude A = {A_fit:.4f} nm")
print(f"Fitted angular frequency omega = {omega_fit:.4f} fs^-1")
print(f"Fitted phase phi = {phi_fit:.4f} rad")
print(f"Fitted offset C = {C_fit:.4f} nm")

print("\nExpected omega =", omega)
print("Percent error =", abs((omega_fit - omega)/omega) * 100, "%")

# ----------------------------------------
# Plot data and fit
# ----------------------------------------
t_fine = np.linspace(t_vals.min(), t_vals.max(), 1000)
fit_curve = sinusoid(t_fine, A_fit, omega_fit, phi_fit, C_fit)

plt.figure(figsize=(8,5))
plt.plot(t_vals, x_expectation_vals, 'o', label="Data")
plt.plot(t_fine, fit_curve, '-', label="Fit")
plt.xlabel("time (fs)")
plt.ylabel(r"$\langle x \rangle (t)$ (nm)")
plt.title("Sinusoidal Fit to Expectation Value")
plt.legend()
plt.grid(True)
plt.show()
```

Then answer the following questions
1.  Why does a single eigenstate not change its probability density in time, while a superposition does?

2. In your animation, what role do the relative phases between different eigenstates play in producing motion?

3. You fit your data to a sinusoidal function and extracted a frequency.  How did your fitted frequency compare to the expected $\omega$?

4. In this lab, the expectation value $\langle \hat{x} \rangle (t)$ behaved like a classical oscillator.
   * Does this mean the particle itself is moving like a classical particle?
   * What is actually oscillating in the quantum system?
