# Part 2: Microwave Driving and Rabi Oscillations

## Big Idea

In Part 1, you observed how a quantum superposition accumulates relative phase while the probabilities of measuring its two basis states remain unchanged.

We can do something more powerful: **use microwave radiation to deliberately transfer population between two quantum states**. This controlled evolution is called *Rabi oscillation*.

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

A static magnetic field applied along the NV axis changes the energy difference between these states. The transition has an angular frequency $\omega_0$ determined by that energy difference:

```math
E_1-E_0=\hbar\omega_0.
```

A microwave magnetic field oscillating at angular frequency $\omega_d$ can drive transitions between the two states. We define the **detuning** as

```math
\Delta=\omega_0-\omega_d.
```

When $\Delta=0$, the microwave field is **on resonance**. When $\Delta\ne0$, the microwave field is **off resonance**.

In the QLab, students developing NV-diamond magnetometers must understand this microwave control. Because a magnetic field changes the NV transition frequency, a microwave drive at a fixed frequency can become resonant or detuned as the field changes. The resulting changes in spin populations can ultimately be detected through red fluorescence, which we will investigate in Part 3.

### The Rotating-Frame Hamiltonian

To describe microwave-driven evolution, we use a reference frame rotating with the applied microwave field. In this frame, and within the rotating-wave approximation, our two-level Hamiltonian is

```math
\hat H=\frac{\hbar}{2}\left(\Omega_R\sigma_x+\Delta\sigma_z\right),
```

where:

- $\Omega_R$ is the **resonant Rabi angular frequency**, controlled by the microwave field strength.
- $\Delta$ is the detuning between the NV transition and the microwave drive.
- $\sigma_x$ and $\sigma_z$ are the Pauli operators for our effective two-level system.

We will work in units of microseconds and radians per microsecond. QuTiP uses $\hbar=1$ when we enter the Hamiltonian in angular-frequency units, so the computational Hamiltonian is $H/\hbar$.

### Resonant Driving: A Spin Flip

On resonance, $\Delta=0$, so the Hamiltonian simplifies to

```math
\hat H_{\mathrm{res}}=\frac{\hbar\Omega_R}{2}\sigma_x.
```

Starting from $|0\rangle$, the microwave field drives a rotation about the $x$-axis of the Bloch sphere. The population of $|1\rangle$ is

```math
P_1(t)=\sin^2\left(\frac{\Omega_Rt}{2}\right).
```

After a time

```math
t_\pi=\frac{\pi}{\Omega_R},
```

the spin has rotated through an angle $\pi$ and the population has transferred completely from $|0\rangle$ to $|1\rangle$. This is called a **$\pi$-pulse**. A shorter $\pi/2$-pulse prepares an equal superposition.

### Off-Resonant Driving

When $\Delta\ne0$, the Hamiltonian contains both $\sigma_x$ and $\sigma_z$ terms:

```math
\hat H_{\mathrm{off}}=\frac{\hbar}{2}\left(\Omega_R\sigma_x+\Delta\sigma_z\right).
```

The Bloch vector now rotates about a tilted effective axis in the $x$–$z$ plane, rather than the $x$-axis alone. The oscillations occur at the **generalized Rabi angular frequency**

```math
\Omega_{\mathrm{eff}}=\sqrt{\Omega_R^2+\Delta^2},
```

but the maximum population transfer is reduced:

```math
P_{1,\max}=\frac{\Omega_R^2}{\Omega_R^2+\Delta^2}.
```

The distinction is important: **detuning can make the population oscillate faster while preventing a complete spin flip**.

<p align="center">
  <img src="Images/nv_rabi_resonant_vs_detuned_bloch.png"
       alt="Comparison of resonant and off-resonant effective rotation axes on the Bloch sphere"
       width="450">
</p>

*Reference diagram: On resonance, the effective rotation axis lies along x. With nonzero detuning, it tilts toward z. Students will calculate the corresponding spin trajectories in Figure 3.*

---

## Computational Investigation A: Resonant Microwave Driving

### Step 1: Predict the Evolution

Start with the initial state

```math
|\psi(0)\rangle=|0\rangle
```

and the resonant Hamiltonian

```math
\hat H_{\mathrm{res}}=\frac{\hbar\Omega_R}{2}\sigma_x.
```

**Record your predictions before running the simulation:**

1. Where does the spin begin on the Bloch sphere, and about which axis will it rotate?
2. What should happen to $P_0(t)$ and $P_1(t)$?
3. What duration should produce a complete spin flip? What happens to that duration if the microwave strength doubles?

### Step 2: Simulate Rabi Oscillations

Use the instructor-provided QuTiP functions and the parameters

```math
\Omega_R=2\pi\ \mathrm{rad/\mu s},\qquad
\Delta=0,
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
\Omega_R=4\pi\ \mathrm{rad/\mu s},\qquad \Delta=0.
```

Keep the initial state and total time interval unchanged.

Report the expected and observed first $\pi$-pulse durations for both microwave strengths. Explain the change in the population-oscillation period.

An additional comparison plot may help, but it is **not another required numbered figure**.

---

## Computational Investigation B: Off-Resonant Microwave Driving

### Step 5: Predict the Effect of Detuning

Return to the original microwave strength

```math
\Omega_R=2\pi\ \mathrm{rad/\mu s}
```

and consider three driving conditions:

| Case | Detuning $\Delta$ | Interpretation |
|---|---:|---|
| A | $0$ | Resonant |
| B | $\Omega_R$ | Moderately detuned |
| C | $2\Omega_R$ | More strongly detuned |

**Before running the simulations, predict:**

1. Which case should allow complete population transfer to $|1\rangle$?
2. As detuning increases, will the maximum $P_1$ increase or decrease?
3. How should the effective rotation axis change on the Bloch sphere?

### Step 6: Simulate Detuned Driving

Use the full rotating-frame Hamiltonian:

```math
\hat H=\frac{\hbar}{2}\left(\Omega_R\sigma_x+\Delta\sigma_z\right).
```

For each of the three detunings, evolve the same initial state $|0\rangle$ over $0$ to $2\ \mathrm{\mu s}$.

Use the supplied evolution and plotting functions to:

1. Generate Bloch-sphere trajectories for the resonant and detuned cases.
2. Calculate $P_0(t)$ and $P_1(t)$.
3. Determine the maximum of $P_1(t)$ for each detuning.
4. Compare the numerical maxima with the analytical expression for $P_{1,\max}$.

**AI coding task:** Ask an LLM to extend your existing resonant simulation so that only the detuning changes. Retain the same definitions and plotting functions. Request a comparison rather than three unrelated programs.

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

For $\Omega_R=2\pi\ \mathrm{rad/\mu s}$, the predicted first spin flip occurs at

```math
t_\pi=0.5\ \mathrm{\mu s}.
```

Verify that $P_1(t_\pi)$ is approximately 1. When the Rabi frequency is doubled, verify that the first $\pi$-pulse duration is approximately halved.

### 3. Detuning and Maximum Population Transfer

Check the simulated maximum population against

```math
P_{1,\max}=\frac{\Omega_R^2}{\Omega_R^2+\Delta^2}.
```

For the three detunings, the predicted maximum probabilities are 1, 0.5, and 0.2, respectively. Small numerical differences due to time sampling are acceptable.

### 4. Generalized Rabi Frequency

Verify that the oscillations change frequency as predicted by

```math
\Omega_{\mathrm{eff}}=\sqrt{\Omega_R^2+\Delta^2}.
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
