# Part 2: Microwave Driving and Rabi Oscillations

## Big Idea

In Part 1, you observed how a quantum superposition accumulates relative phase while the probabilities of measuring its two basis states remain unchanged.

We can do something more powerful: **use microwave radiation to deliberately transfer population between two quantum states**. This controlled evolution produces *Rabi oscillations*.

In this activity, you will simulate microwave-driven dynamics of an NV-center spin, identify the pulse duration required to flip the spin, and investigate what happens when the microwave frequency does not match the spin transition frequency.

**Central question:** How does an oscillating microwave field control the state of an NV center, and why does this control become less effective when the drive is detuned?

---

## Physical Background

### Driving an NV-Center Transition

We continue to use the effective two-level system introduced in Part 0:

```math
|0\rangle\equiv|m_s=0\rangle,\qquad
|1\rangle\equiv|m_s=-1\rangle.
```

A static magnetic field applied along the NV axis changes the energy difference between these states. Let $\omega_{\mathrm{NV}}$ denote the physical angular transition frequency, defined by the magnitude of the energy gap:

```math
|E_1-E_0|=\hbar\omega_{\mathrm{NV}}.
```

A microwave field oscillating at angular frequency $\omega_d$ can drive transitions between the states. The microwave is **on resonance** when $\omega_d=\omega_{\mathrm{NV}}$; otherwise it is **off resonance**.

Students in QLab are developing NV-diamond magnetometers using these transitions. A magnetic field shifts $\omega_{\mathrm{NV}}$, changing the response to microwaves held at a fixed frequency. We will connect the resulting population changes to red fluorescence in Part 3.

### Connection to the Textbook: An Effective Time-Independent Hamiltonian

In Section 3.2.2 of your textbook, you studied a spin in a *static* magnetic field with $x$- and $z$-components. Its Hamiltonian has the form

```math
\hat H=\omega_0\hat S_z+\omega_1\hat S_x
=\frac{\hbar}{2}\left(\omega_0\sigma_z+\omega_1\sigma_x\right).
```

**Our microwave-driven NV center uses an effective Hamiltonian with the same mathematical form:**

```math
\boxed{\hat H_{\mathrm{eff}}=\frac{\hbar}{2}\left(\omega_0\sigma_z+\omega_1\sigma_x\right).}
```

Here the two parameters mean:

- $\omega_0=\omega_{\mathrm{NV}}-\omega_d$: **signed microwave detuning**, playing the role of the textbook's $z$-term coefficient.
- $\omega_1$: **effective microwave coupling strength**, playing the role of the textbook's $x$-term coefficient. On resonance, it is the Rabi angular frequency.

**Important distinction:** The textbook's field is truly static, while the microwave field applied to an NV center oscillates rapidly in the laboratory. The Hamiltonian above is a **time-independent effective model**, valid during a constant-amplitude microwave pulse in a frame rotating with the drive and under the rotating-wave approximation. We will use it without deriving that transformation. In particular, $\omega_0$ in **this part** means *detuning*, **not** the NV transition frequency $\omega_{\mathrm{NV}}$ or the illustrative precession rate used in Part 1.

The Pauli operators here describe our two-state subspace, not the full spin-1 NV ground state. In matrix form, as in the textbook,

```math
\hat H_{\mathrm{eff}}=\frac{\hbar}{2}
\begin{pmatrix}
\omega_0 & \omega_1 \\
\omega_1 & -\omega_0
\end{pmatrix}.
```

We will work in microseconds and radians per microsecond. In QuTiP, supply $H/\hbar$ in angular-frequency units, so the numerical Hamiltonian uses the same coefficients without an explicit factor of $\hbar$.

### Resonant Driving: A Spin Flip

On resonance, $\omega_0=0$, so the Hamiltonian simplifies to

```math
\hat H_{\mathrm{res}}=\frac{\hbar\omega_1}{2}\sigma_x.
```

Starting from $|0\rangle$, the microwave field drives a rotation about the $x$-axis of the Bloch sphere. The population of $|1\rangle$ is

```math
P_1(t)=\sin^2\left(\frac{\omega_1t}{2}\right).
```

After a time

```math
t_\pi=\frac{\pi}{\omega_1},
```

the spin has rotated through an angle $\pi$ and the population has transferred completely from $|0\rangle$ to $|1\rangle$. This is called a **$\pi$-pulse**. A shorter $\pi/2$-pulse prepares an equal superposition.

### Off-Resonant Driving

When $\omega_0\ne0$, the Hamiltonian contains both $\sigma_x$ and $\sigma_z$ terms:

```math
\hat H_{\mathrm{off}}=\frac{\hbar}{2}\left(\omega_0\sigma_z+\omega_1\sigma_x\right).
```

The Bloch vector rotates around a tilted effective axis in the $x$–$z$ plane, just as in the textbook's general-direction field problem. The difference between the two energy eigenvalues defines the generalized oscillation angular frequency

```math
\Omega=\sqrt{\omega_0^2+\omega_1^2}.
```

The textbook's Rabi formula (Eq. 3.63) becomes, for our initial $|0\rangle$ state,

```math
P_{0\rightarrow1}(t)=
\frac{\omega_1^2}{\omega_0^2+\omega_1^2}
\sin^2\!\left(\frac{\sqrt{\omega_0^2+\omega_1^2}}{2}t\right).
```

The maximum population transfer is

```math
P_{0\rightarrow1}^{\max}=\frac{\omega_1^2}{\omega_0^2+\omega_1^2}.
```

Thus **increasing the magnitude of detuning increases the generalized oscillation frequency while decreasing the maximum probability of a complete spin flip**.

**Diagram notation:** The reference image uses the common labels $\Delta$ and $\Omega_R$. These correspond to our textbook-style $\omega_0$ and $\omega_1$, respectively. Its $\Omega_{\mathrm{eff}}$ is the same as $\Omega$ here.

<p align="center">
  <img src="Images/nv_rabi_resonant_vs_detuned_bloch.png"
       alt="Comparison of resonant and off-resonant effective rotation axes on the Bloch sphere"
       width="450">
</p>

*Reference diagram (with alternate notation): On resonance, the effective rotation axis lies along x. With nonzero detuning, it tilts toward z. Students will calculate the corresponding spin trajectories in Figure 3.*

---

## Computational Investigation A: Resonant Microwave Driving

### Step 1: Predict the Evolution

Start with the initial state

```math
|\psi(0)\rangle=|0\rangle
```

and the resonant Hamiltonian

```math
\hat H_{\mathrm{res}}=\frac{\hbar\omega_1}{2}\sigma_x.
```

**Record your predictions before running the simulation:**

1. Where does the spin begin on the Bloch sphere, and about which axis will it rotate?
2. What should happen to $P_0(t)$ and $P_1(t)$?
3. What duration should produce a complete spin flip? What happens to that duration if the microwave strength doubles?

### Step 2: Simulate Rabi Oscillations

Use the instructor-provided QuTiP functions and the parameters

```math
\omega_1=2\pi\ \mathrm{rad/\mu s},\qquad
\omega_0=0,
```

with a simulation interval of

```math
0\le t\le2\ \mathrm{\mu s}.
```

This interval contains two complete population-oscillation periods.

Your code should:

1. Prepare the initial state $|0\rangle$.
2. Construct the resonant Hamiltonian using the supplied Pauli operator.
3. Evolve the state with the supplied QuTiP evolution function.
4. Calculate the Bloch-vector components and the two spin-state populations.
5. Determine the first time at which $P_1$ reaches its maximum.
6. Generate Figure 2.

**AI coding task:** Ask an LLM to help you implement the physical model using the existing notebook functions and variable names. Specify the Hamiltonian, initial state, parameter values, and expected plots. Do not ask it to replace the notebook or rewrite the instructor-supplied utilities.

### Step 3: Generate Figure 2

**Figure 2 — Resonant Rabi Oscillations**

Create one numbered figure with two panels:

- **Panel A: Bloch-sphere trajectory.** Show the rotation of the spin beginning at $|0\rangle$, mark the initial state, and indicate the direction of evolution. Make the north and south poles identifiable.
- **Panel B: State populations.** Plot $P_0(t)$ and $P_1(t)$ versus time, label the axes and units, and mark the first $\pi$-pulse time.

Include a caption describing how rotation on the Bloch sphere corresponds to population transfer.

### Step 4: Change the Microwave Strength

Repeat the simulation with

```math
\omega_1=4\pi\ \mathrm{rad/\mu s},\qquad \omega_0=0.
```

Keep the initial state and total time interval unchanged.

Report the expected and observed first $\pi$-pulse durations for both coupling strengths. Explain the change in the population-oscillation period.

An additional comparison plot may help, but it is **not another required numbered figure**.

---

## Computational Investigation B: Off-Resonant Microwave Driving

### Step 5: Predict the Effect of Detuning

Return to the original microwave coupling

```math
\omega_1=2\pi\ \mathrm{rad/\mu s}
```

and consider three driving conditions:

| Case | Detuning $\omega_0$ | Interpretation |
|---|---:|---|
| A | $0$ | Resonant |
| B | $\omega_1$ | Moderately detuned |
| C | $2\omega_1$ | More strongly detuned |

**Before running the simulations, predict:**

1. Which case should allow complete population transfer to $|1\rangle$?
2. As detuning increases, will the maximum $P_1$ increase or decrease?
3. How should the effective rotation axis change on the Bloch sphere?

### Step 6: Simulate Detuned Driving

Use the full effective Hamiltonian:

```math
\hat H=\frac{\hbar}{2}\left(\omega_1\sigma_x+\omega_0\sigma_z\right).
```

For each of the three detunings, evolve the same initial state $|0\rangle$ over $0$ to $2\ \mathrm{\mu s}$.

Use the supplied evolution and plotting functions to:

1. Generate Bloch-sphere trajectories for the resonant and detuned cases.
2. Calculate $P_0(t)$ and $P_1(t)$.
3. Determine the maximum of $P_1(t)$ for each detuning.
4. Compare the numerical maxima with the analytical expression for $P_{1,\max}$.

**AI coding task:** Ask an LLM to extend your existing resonant simulation so that only $\omega_0$ (detuning) changes. Retain the same definitions and plotting functions. Request a comparison rather than three unrelated programs.

### Step 7: Generate Figure 3

**Figure 3 — Resonant and Off-Resonant Microwave Driving**

Create one numbered figure containing:

- **Panel A: Bloch-sphere comparison.** Show the resonant trajectory and at least one detuned trajectory, using consistent axis conventions and distinct labels. Separate spheres are acceptable if they make the trajectories easier to see.
- **Panel B: Population comparison.** Plot $P_1(t)$ for all three detuning values on common axes. Include a legend and units.

Include a caption explaining the effect of detuning on the rotation axis, oscillation frequency, and maximum population transfer.

In your notebook, report the maximum $P_1$ for all three detuning conditions, along with the analytical predictions.

---

## Numerical Verification

Before accepting Figures 2 and 3, verify the following:

### 1. State Normalization and Probabilities

For every simulation, confirm that

```math
\langle\psi(t)|\psi(t)\rangle\approx1,
\qquad P_0(t)+P_1(t)\approx1.
```

All probabilities must lie between 0 and 1.

### 2. Resonant $\pi$-Pulse

For $\omega_1=2\pi\ \mathrm{rad/\mu s}$, the predicted first spin flip occurs at

```math
t_\pi=0.5\ \mathrm{\mu s}.
```

Verify that $P_1(t_\pi)$ is approximately 1. When the microwave coupling $\omega_1$ is doubled, verify that the first $\pi$-pulse duration is approximately halved.

### 3. Detuning and Maximum Population Transfer

Check the simulated maximum population against

```math
P_{1,\max}=\frac{\omega_1^2}{\omega_1^2+\omega_0^2}.
```

For $\omega_0=0,\omega_1,2\omega_1$, the predicted maximum probabilities are 1, 0.5, and 0.2, respectively. Small numerical differences due to time sampling are acceptable.

### 4. Generalized Rabi Frequency

Verify that the oscillations change frequency as predicted by

```math
\Omega=\sqrt{\omega_1^2+\omega_0^2}.
```

Record a concise table of your measured or calculated values and identify any discrepancies.

---

## Physics Analysis

Answer these two questions **independently** in the designated Markdown cells of your notebook. Do not use an LLM to generate the answers.

**Question 3 — Resonant Spin Control**

In Part 1, the spin accumulated relative phase without changing its $|0\rangle$ and $|1\rangle$ populations. In this activity, resonant microwave driving caused those populations to oscillate. Explain the difference in terms of the Hamiltonian and the axis of rotation on the Bloch sphere. Why can a resonant $\pi$-pulse completely flip the spin?

**Question 4 — Detuning and Magnetometry**

Your off-resonant simulation shows a reduced maximum population transfer. Explain why the tilted rotation axis prevents the Bloch vector from reaching the south pole. Then consider an NV center exposed to a changing magnetic field while the microwave frequency is held fixed: explain how a shift in the spin transition frequency would change the microwave-driven response. How could that provide information about the magnetic field?

Your answers should use evidence from Figures 2 and 3, not simply repeat the formulas.

---

## AI Development Record — Part 2

Document **one meaningful AI interaction** from this part in the designated AI Development Record section of your notebook. Include:

1. **Objective:** What did you want the code to accomplish?
2. **Prompt:** Your actual prompt to the LLM.
3. **AI Contribution:** What did it generate or suggest?
4. **Verification:** How did you compare its output with physical predictions or analytical limits?
5. **Revision (if applicable):** What did you improve after testing?

You do not need to submit your full AI conversation. If you worked without an LLM, explain how you developed and checked your code independently.

---

## Part 2 Completion Checklist

Before moving to Part 3, confirm that you have:

- [ ] Recorded predictions before the resonant and off-resonant simulations.
- [ ] Completed the resonant and doubled-strength Rabi simulations.
- [ ] Generated **Figure 2** with a Bloch-sphere trajectory and two population curves.
- [ ] Identified and verified the first $\pi$-pulse duration.
- [ ] Simulated all three detuning values.
- [ ] Generated **Figure 3** with the required Bloch-sphere and population comparisons.
- [ ] Checked normalization, population transfer, and detuning-dependent limits.
- [ ] Answered both physics-analysis questions independently.
- [ ] Completed the Part 2 AI Development Record entry.

**Next: Part 3 — Optical Readout and NV Magnetometry.** You will use the populations calculated here to predict the red fluorescence observed after microwave manipulation of an NV center.
