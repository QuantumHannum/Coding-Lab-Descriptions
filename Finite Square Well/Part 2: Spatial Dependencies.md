# Part 2: Spatial Dependencies

## Activity 2
The infinite square well produced an exact equation for the eigenenergies:

```math
E_n=\frac{n^2\pi^2\hbar^2}{2mL^2}
```
As we discovered in the introduction, there is no closed-form equation for the eigenenergies for the finite square well; however, we can still explore some functional dependencies.

---
What you need to turn in for this activity
1. Using the parameters provided in activity 1, produce a plot of the bound state eigenenergies vs. quantum number n.  Fit a curve of the form $E = Ax^B$ using ``scipy.optimize.curve_fit()``.  How close is this fit to the infinite square well prediction?  Produce a second and third similar plots where you double, then triple the value of $V_0$.  What do these other plots indicate about the functional dependence of the eigenenergies, quantum number, and potential height?
2. Using the parameters provided in activity 1, produce a plot of the ground state energy and first excited state energy as you vary the size of the well $2a \in \[1 nm, 10 nm\]$.  Once again, add a curve of the form $E = Ax^B$ for these two curves. Comment on any observations you make from this plot?

## Activity 3
Application of Finite Potential Wells to Quantum Dots.

The concepts of finite potential wells help us understand the behavior of quantum dots, which are nanoscale semiconductor particles that confine charge carriers in a very small region of space. Because of this confinement, the allowed energies of particles in a quantum dot become discrete, similar to the energy levels of a particle in a potential well.

Quantum dots can absorb photons over a wide range of wavelengths, which promotes an electron from the valence band to the conduction band, leaving behind a positively charged hole. The excited electron and hole initially may occupy higher energy states, but they rapidly lose excess energy through interactions with vibrations of the crystal lattice. These vibrations are called phonons, which are quantized units of vibrational energy in a solid, analogous to photons being quantized units of electromagnetic radiation.

Through phonon interactions, the electron and hole quickly relax to their lowest confined energy states before emitting light. When the electron recombines with the hole, a photon is emitted. In a simple model, the emitted photon energy is approximated as the sum of the semiconductor band-gap energy and the confinement energies of the electron and hole:
```math
E_{photon} \approx E_g + E_{e,1} + E_{h,1}
```
Here $E_g$ is the bulk band-gap energy of the semiconductor, while $E_{e,1}$ and $E_{h,1}$ are the ground-state confinement energies of the electron and hole. Because these confinement energies depend on the size of the quantum dot, the emitted photon wavelength also depends on the dot size. Smaller dots produce larger confinement energies and emit higher-energy (bluer) light, while larger dots emit lower-energy (redder) light.  The wavelength of the emitted photon is directly calculated by:
```math
\lambda = \frac{hc}{E_{photon}}
```
To learn more about this process and some uses for Quantum Dots, watch this short video [Color by Size](https://www.youtube.com/watch?v=dhRIHF1DENI)

Here are some realistic parameters for CdSe quantum dots
| Parameter | Symbol| Approx. Value | Units|
|-----------|:-----:| :-----------:|:-----|
| Bulk band gap| $E_y$| 1.74| eV|
|Free-electron mass| $m_e$ | 511 | $keV/c^2$|
|Electron effective mass| $m_e^*$|$0.13m_e$||
|Hole effective mass|$m_h^*$|$0.45m_e$||
|Electron well depth| $V_{0,e}$|4.0|eV|
|Hole well depth|$V_{0,h}$|4.0|eV|
|Reduced Plank constant|$\hbar$|$\frac{1250}{2\pi}$| eV nm|
|Plank constant $\times$ speed of light|$hc$| 1240|eV nm|

If we then set the potential well for the electron to be:
```math
V_e(x) =
  \begin{cases}
    0 & |x| \le L/2\\
    V_{0,e} &|x| \ge L/2
  \end{cases}
```
and the potential well for the hole to be:
```math
V_h(x) =
  \begin{cases}
    0 & |x| \le L/2\\
    V_{0,h} &|x| \ge L/2
  \end{cases}
```

---
Here is what you should turn in
1. Make a scatter plot of emitted photon wavelength $\lambda$ vs. quantum dot width $L$ for $L \in \[1nm, 10nm\]$ with $\Delta L = 1mn$.  All of your wavelengths should be in the visible range of 400nm - 700nm.
2. Answer these questions
   * Between which quantum dot sizes does the emitted wavelength change the most rapidly? What does this suggest about how sensitive quantum dot color is to small changes in size?
   * In this model, both the electron and the hole contribute to the photon energy through their confinement energies. Which particle contributes more strongly to the energy shift: the electron or the hole? Explain why using their effective masses.
   * Suppose you want to design a quantum dot that emits green light $\(\lambda=550nm\)$. Use your model to estimate the quantum dot width required to produce this wavelength.
