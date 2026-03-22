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
Increase the number of eigenfunctions until this condition is satisfied.  **In the title of your probability plot, include the number of eigenfunctions you used.**

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
from matplotlib.animation import FuncAnimation, FFMpegWriter

# ----------------------------------------
# Choose time range
# ----------------------------------------
T = 2 * np.pi / omega        # one oscillation period
t_max = 3 * T                # evolve for 3 periods
num_frames = 300
t_vals = np.linspace(0, t_max, num_frames)

# ----------------------------------------
# Precompute probability density at each time
# ----------------------------------------
probability_frames = []

num_states_used = len(c_vals)

for t in t_vals:
    psi_t = np.zeros_like(x, dtype=complex)

    for n in range(num_states_used):
        # *********** PUT YOUR CODE HERE ************ 



    prob_density = np.abs(psi_t)**2
    probability_frames.append(prob_density)

probability_frames = np.array(probability_frames)

# ----------------------------------------
# Set up figure
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

# Optional: show the potential in the background, rescaled
# V_plot = V / np.max(V) * np.max(probability_frames) * 0.8
# ax.plot(x, V_plot, "--", alpha=0.6, label="Scaled potential")
# ax.legend()

# ----------------------------------------
# Animation update function
# ----------------------------------------
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

# ----------------------------------------
# Save as .mov
# ----------------------------------------
writer = FFMpegWriter(fps=20)
anim.save("sho_time_evolution.mov", writer=writer)

plt.show()
```
