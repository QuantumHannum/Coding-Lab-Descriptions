# Part 1: Spatial Dependencies

## Activity 2
The infinite square well produced an exact equation for the eigenenergies:

```math
E_n=\frac{n^2\pi^2\hbar^2}{2mL^2}
```
As we discovered in the introduction, there is no closed-form equation for the eigenenergies for the finite square well; however, we can still explore some functional dependencies.

---
What you need to turn in for this activity
1. Using the parameters provided in activity 1, produce a plot of the bound state eigenenergies vs. quantum number n.  Fit a curve of the form $E = Ax^B$ using ``scipy.optimize.curve_fit()``.  How close is this fit to the infinite square well prediction?  Produce a second and third similar plots where you double, then triple the value of $V_0$.  What do these other plots indicate about the functional dependence of the eigenenergies, quantum number, and potential height?
2. Using the parameters provided in activity 1, produce a plot of the ground state energy and first excited state energy as you vary the size of the well $2a \in \[1 nm, 10 nm\]$.  Comment on any observations you make from this plot?

## Activity 3
Application of Finite Potential Wells to Quantum Dots.

We can apply the basic concepts of finite potential wells to understand the behavior of quantum dots - which are nanoscale semiconductor particles that confine electrons in a very small region of space. Quantum dots can absorb photons over a wide range of wavelengths, exciting electrons to higher energy states. However, the excited electrons rapidly lose most of that excess energy through interactions with the crystal lattice of the material. These lattice vibrations are called phonons, which are quantized units of vibrational energy in a solid, similar to how photons are quantized units of electromagnetic radiation. Through phonon interactions, electrons quickly relax to the lowest excited state of the system. From there, the electron can transition back to the ground state and emit a photon with energy equal to the difference between the two levels,
```math
E_{photon}=E_2-E_1
```
In activity 2, you should have discovered that the energy gap is determiend by the size of the quantum dot (width of potential well), the emitted photon has a very specific wavelength, giving quantum dots their characteristic color. Smaller dots produce larger energy gaps and emit higher-energy (bluer) light, while larger dots emit lower-energy (redder) light. In this activity, we will use the finite potential well as a simple computational model to explore how quantum confinement determines the emission spectrum of quantum dots.

If you want to learn more about quantum dots, please read these two articles
* \url{What is a Quantum Dot}[https://www.britishcouncil.org/voices-magazine/what-quantum-dot?utm_source=chatgpt.com]
* \url{Quantum Dot Wiki}[https://en.wikipedia.org/wiki/Quantum_dot?utm_source=chatgpt.com]
