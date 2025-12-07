# Schrodinger Time Evolution with Gaussian Pulse

## Introduction
In this lab you will extend the work we have done in class on time-independent and time-dependent Hamiltonians by numerically exploring how quantum states evolve under realistic control pulses. We have already seen that a driven two-level system exhibits Rabi oscillations, and that by choosing the correct pulse duration and amplitude, one can implement fundamental quantum-computing gates such as the X, Y, and arbitrary-axis rotations. However, real laboratory pulses are never perfectly shaped—small fluctuations in amplitude, width, or timing can distort the intended rotation and push the final state away from its ideal Bloch-sphere trajectory. In this investigation you will use QuTiP to simulate both ideal and jittered control sequences, quantify the resulting errors, and connect these deviations to the crucial practical requirement for precise pulse engineering in quantum information processing.

## Learning Goals
- Use QuTip to simulate Schrodinger Time Evolution based on a time-dependent Hamiltonian.
- Explore the evolution of a spin state after multiple Gaussian pulses applied in random axial directions (x, y, z).
- Quantify the change in measurement error due to realistic (jittered) pulses with the Trace Distance

## Theortical Background
In many quantum control experiments, a Gaussian-shaped pulse is used to gently turn on and off the interaction that drives a qubit.  A guassian pulse has a smooth, bell-shaped form and can be written as:

```math
\omega(t) = \omega_oe^{-\frac{(t-t_0)^2}{2\sigma^2}}
```

$t_0$ is the time at which the pulse is centered, and $\sigma$ controls how wide the pulse is. $\omega_0$ is the mximum strength of the pulse that **could** be derived from an an applied magnetic field $B_{max}$ acting on a electron with mass $m$, charge $q$, and gryomagnetic ratio $g$:

```math
\omega_0=g\frac{q}{2m}B_{max}
```
When this pulse drives a two-level system(spin state, quibt), it acts like a time-dependent rotation about one of hte Bloch-sphere axes.  For example, if the pulse is appled along the $z$-axis, the Hamiltonian has the form:

```math
\hat{H}(t) = \frac{\hbar}{2}\omega(t)\sigma_z
```
We can see the action of this Hamiltonian on a spin state initially in the $|\psi_0\rangle= \frac{1}{\sqrt{2}}\left[|+z\rangle+|-z\rangle \right]$ in the video below.  Note, the inset where you can see the gaussian pulse evolution and the corrisponding (red) magnetic field vector in the $z$ direction. It is important to see the changing rate of rotation of the state (given by the blue dots) shown in the Bloch sphere.

https://github.com/user-attachments/assets/d570b238-749e-4aa0-a7e3-dd5bbeb99a05

If we change the direction of the applied field to be in the $x$, or $y$, the corrisponding Hamiltonians only change sliglty be swapping out the Pauli-$z$ with the Pauli-$x$ or Pauli-$y$ operators.

The time evolution of this state is governed by the Schrodinger equation:

```math
i\hbar\frac{d}{dt}|\psi(t)\rangle = \hat{H}(t)|\psi(t)\rangle
```
We can also view this evolution as the application of a unitary operator on the initial state $|\psi_0\rangle$

```math
|\psi(t)\rangle = \hat{U}(t)|\psi_0\rangle
```
Where $U(t)$ is given by:

```math
\hat{U}(t)=exp\left[ -\frac{i}{2}\sigma_k\int_0^T\omega(t) dt\right]
```

The quantity inside the integral (via unit analysis) determines the total rotation angle in time $T$

```math
\theta = \int_0^T\omega(t)dt
```
This makes the percise control of the pulse shape extremley important: even small changes in the maximum height, width, or timing of the Guassina pulse can chagne the value of $\theta$ and lead to accumulating errors in state (or qubit) rotations.
