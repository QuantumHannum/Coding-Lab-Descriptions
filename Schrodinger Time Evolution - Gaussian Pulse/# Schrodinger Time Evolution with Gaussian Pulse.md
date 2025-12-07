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
We can see the action of this Hamiltonian on a spin state initially in the $|\psi_0\rangle= \frac{1}{\sqrt{2}}\left[|+z\rangle+|-z\rangle \right]$ in the .gif below.  Note the inset where you can see the gaussian pulse evolution, and its effect on the rotation rate of the spin state shown in the Bloch sphere.
