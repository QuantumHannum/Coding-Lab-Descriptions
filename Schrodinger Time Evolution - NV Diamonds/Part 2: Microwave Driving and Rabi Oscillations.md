# Part 2: Microwave Driving and Rabi Oscillations

## Big Idea

In Part 1, you observed how a quantum superposition accumulates relative phase while the probabilities of measuring its two basis states remain unchanged.

We can do something more powerful: **use microwave radiation to deliberately transfer population between two quantum states**. This controlled evolution produces *Rabi oscillations*.

In this activity, you will simulate microwave-driven dynamics of an NV-center spin, identify the pulse duration required to flip the spin, and investigate what happens when the microwave frequency does not match the spin transition frequency.

**Central question:** How does an oscillating microwave field control the state of an NV center, and why does this control become less effective when the drive is detuned?

---

## Physical Background

### Microwave Driving: Natural Frequency, Applied Frequency, and Resonance

We continue to model two ground-state spin levels of the NV center:

```math
|0\rangle\equiv|m_s=0\rangle,\qquad |1\rangle\equiv|m_s=-1\rangle.
```

The energy gap defines the NV center's **natural transition angular frequency**:

```math
\omega_0=\frac{|E_1-E_0|}{\hbar}.
```

A transverse microwave magnetic field oscillates at an **applied angular frequency** $\omega$ and can drive transitions between these states. The relevant frequency difference is called **detuning**:

```math
\boxed{\Delta\omega=\omega-\omega_0.}
```

- **On resonance:** $\omega=\omega_0$, so $\Delta\omega=0$. An appropriately timed microwave pulse can completely flip the spin.
- **Near resonance:** The two frequencies are similar, but population transfer is incomplete.
- **Far from resonance:** The mismatch is large relative to the coupling strength; only a small population transfer is possible.

Here $\omega_1$ denotes the **microwave coupling strength** (in rad/$\mu$s), not another microwave frequency. All three quantities are angular frequencies, but they play different physical roles.

### From an Oscillating Field to the Rabi Formula

The actual microwave field varies in time, for example as $B_x(t)=B_1\cos(\omega t)$. The corresponding *laboratory-frame* two-level Hamiltonian has the form

```math
\hat H_{\mathrm{lab}}(t)=\frac{\hbar\omega_0}{2}\sigma_z
+\hbar\omega_1\cos(\omega t)\sigma_x.
```

The constant $\omega_1$ is defined here so that the **resonant Rabi angular frequency** is $\omega_1$ under the rotating-wave approximation. The microwave field itself is genuinely time-dependent, even when its amplitude is constant during a pulse.

For a drive close to resonance and weak compared with the microwave carrier frequency, the probability of a transition from $|0\rangle$ to $|1\rangle$ after a pulse of duration $t$ is approximately

```math
\boxed{P_{0\rightarrow1}(t)=
\frac{\omega_1^2}{(\omega-\omega_0)^2+\omega_1^2}
\sin^2\!\left(
\frac{\sqrt{(\omega-\omega_0)^2+\omega_1^2}}{2}t
\right).}
```

This is the **driven Rabi formula**. Notice that the mismatch between the natural frequency and the applied frequency appears in both the maximum population transfer and the oscillation rate:

```math
P_{0\rightarrow1}^{\max}=
\frac{\omega_1^2}{(\Delta\omega)^2+\omega_1^2},
\qquad
\Omega=\sqrt{(\Delta\omega)^2+\omega_1^2}.
```

At resonance, $\Delta\omega=0$: the maximum probability is 1, and the spin oscillates at angular frequency $\omega_1$. When $|\Delta\omega|$ increases, the maximum transfer decreases while $\Omega$ increases.

### Why We Can Use a Time-Independent Hamiltonian

We have not yet derived the mathematics of the rotating frame. We will do that in a later lesson. For now, accept the following **effective model** of the *slow spin dynamics relative to the microwave drive*:

```math
\boxed{\hat H_{\mathrm{eff}}=
-\Delta\omega\,\hat S_z+\omega_1\hat S_x
=\frac{\hbar}{2}\bigl(-\Delta\omega\,\sigma_z+\omega_1\sigma_x\bigr).}
```

This is a **time-independent approximation during a constant-amplitude microwave pulse**, not the literal laboratory-frame Hamiltonian. A rotating-frame transformation, followed by the rotating-wave approximation, produces it when the drive is near resonance and not too strong. You will derive that connection later; in this lab, you will study the consequences using the time-independent techniques from class.

In the textbook's static-field problem, the Hamiltonian also contains $S_z$ and $S_x$ terms. The mathematics of the rotation is identical, but the *meaning of the terms* is different here:

- The effective $z$ coefficient is $-\Delta\omega=\omega_0-\omega$, representing **frequency mismatch**, not an extra applied static field.
- The effective $x$ coefficient is $\omega_1$, representing microwave coupling.

In matrix form,

```math
\hat H_{\mathrm{eff}}=\frac{\hbar}{2}
\begin{pmatrix}
-\Delta\omega & \omega_1\\
\omega_1 & \Delta\omega
\end{pmatrix}.
```

We use time in $\mu$s and angular frequencies in rad/$\mu$s. In QuTiP, enter $\hat H/\hbar$ directly (without a separate numerical factor of $\hbar$). The effective Bloch trajectories produced in this lab are **rotating-frame trajectories**, not the full gigahertz-speed laboratory motion.

### Resonant Driving: A Spin Flip

When the applied frequency matches the natural frequency, $\omega=\omega_0$ and $\Delta\omega=0$, so the effective Hamiltonian simplifies to

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

When $\Delta\omega\ne0$, the Hamiltonian contains both $\sigma_x$ and $\sigma_z$ terms:

```math
\hat H_{\mathrm{off}}=\frac{\hbar}{2}\left(-\Delta\omega\,\sigma_z+\omega_1\sigma_x\right).
```

The Bloch vector rotates around a tilted effective axis in the $x$–$z$ plane, just as in the textbook's general-direction field problem. The difference between the two energy eigenvalues defines the generalized oscillation angular frequency

```math
\Omega=\sqrt{(\Delta\omega)^2+\omega_1^2}.
```

The driven Rabi formula given earlier, evaluated for our initial $|0\rangle$ state, can be written in terms of detuning as

```math
P_{0\rightarrow1}(t)=
\frac{\omega_1^2}{(\Delta\omega)^2+\omega_1^2}
\sin^2\!\left(\frac{\sqrt{(\Delta\omega)^2+\omega_1^2}}{2}t\right).
```

The maximum population transfer is

```math
P_{0\rightarrow1}^{\max}=\frac{\omega_1^2}{(\Delta\omega)^2+\omega_1^2}.
```

Thus **increasing the magnitude of detuning increases the generalized oscillation frequency while decreasing the maximum probability of a complete spin flip**.

<p align="center">
  <img src="Images/nv_rabi_resonant_vs_detuned_bloch.png"
       alt="Comparison of resonant and off-resonant effective rotation axes on the Bloch sphere"
       width="450">
</p>


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
\Delta\omega=0,
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
\omega_1=4\pi\ \mathrm{rad/\mu s},\qquad \Delta\omega=0.
```

Keep the initial state and total time interval unchanged.

Report the expected and observed first $\pi$-pulse durations for both coupling strengths. Explain the change in the population-oscillation period.

An additional comparison plot may help, but it is **not another required numbered figure**.

---

## Computational Investigation B: Approaching Resonance

### Step 5: Predict the Effect of Detuning

Return to $\omega_1=2\pi\ \mathrm{rad/\mu s}$ and hold the coupling strength fixed. Approach resonance by decreasing the **magnitude** of the microwave detuning. For consistency with our earlier Bloch-axis diagrams, choose the following negative values of $\Delta\omega$ (the applied drive begins below the natural transition frequency):

| Case | $\Delta\omega/\omega_1$ | Microwave frequency | Interpretation |
|---|---:|---|---|
| A | $-4$ | $\omega=\omega_0-4\omega_1$ | Strong detuning |
| B | $-2$ | $\omega=\omega_0-2\omega_1$ | Moderate detuning |
| C | $-1$ | $\omega=\omega_0-\omega_1$ | Smaller detuning |
| D | $0$ | $\omega=\omega_0$ | On resonance |

For these values, the effective $z$ coefficient $-\Delta\omega$ is positive and decreases in the ratio $4:2:1:0$. The effective $x$ coefficient stays fixed at $\omega_1$.

**Before simulating, predict:** (1) which trajectory will remain closest to the north pole, (2) which will reach the south pole, (3) how the rotation axis tilts as the microwave frequency approaches $\omega_0$, and (4) whether the population oscillations speed up or slow down.

### Step 6: Simulate the Detuning Sequence

Use the same effective Hamiltonian throughout:

```math
\hat H_{\mathrm{eff}}=\frac{\hbar}{2}
\left(-\Delta\omega\,\sigma_z+\omega_1\sigma_x\right).
```

For each of the four detunings, initialize $|0\rangle$ and simulate $0\le t\le2\ \mu\mathrm s$. Use sufficiently dense sampling for the most strongly detuned case.

Use the **same** `nv_hamiltonian(detuning, omega1)` and `simulate_spin()` helpers for every simulation; do not define a new solver. The `detuning` argument is always $\Delta\omega=\omega-\omega_0$, and `nv_hamiltonian` internally supplies the minus sign on $\sigma_z$.

**AI coding task:** Extend your resonant calculation into a loop over the four detuning values, collecting Bloch trajectories, populations, and maxima without duplicating the numerical code.

### Step 7: Generate Figure 3

**Figure 3 — Approaching Resonance: Bloch-Sphere Trajectories and Spin-Flip Probabilities**

Create one numbered figure with:

- **Panel A:** Four separate Bloch spheres, in order $\Delta\omega/\omega_1=-4,-2,-1,0$. Label each with its detuning. Mark $|0\rangle$, show time direction, and keep viewing angles consistent. Show the two effective Hamiltonian components: $-\Delta\omega$ along $z$ and $\omega_1$ along $x$, using relative arrow lengths $4:2:1:0$ for $z$ and the same length for $x$.
- **Panel B:** Four $P_1(t)$ curves on shared axes, with time in $\mu$s and a legend.

The vector arrows represent **effective rotation terms**, not literal applied laboratory magnetic fields. Include a caption relating detuning, rotation-axis direction, population-transfer amplitude, and oscillation frequency.

### Step 8: Report and Interpret the Detuning Results

Print a compact comparison table with columns:

| $\Delta\omega/\omega_1$ | Maximum $P_1$ (simulation) | Maximum $P_1$ (theory) | Generalized $\Omega$ (rad/$\mu$s) |
|---|---|---|---|
| $-4$ | *Your result* | *Calculate* | *Calculate* |
| $-2$ | *Your result* | *Calculate* | *Calculate* |
| $-1$ | *Your result* | *Calculate* | *Calculate* |
| $0$ | *Your result* | *Calculate* | *Calculate* |

Use

```math
P_{1,\max}=\frac{\omega_1^2}{(\Delta\omega)^2+\omega_1^2},
\qquad \Omega=\sqrt{(\Delta\omega)^2+\omega_1^2}.
```

Explain why moving closer to resonance causes larger population oscillations but a smaller generalized oscillation frequency.

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

Verify that $P_1(t_\pi)$ is approximately 1. When $\omega_1$ is doubled, verify that the first $\pi$-pulse duration is approximately halved.

### 3. Four Detuning Values and Maximum Population Transfer

Check the simulated maximum population against

```math
P_{1,\max}=\frac{\omega_1^2}{\omega_1^2+(\Delta\omega)^2}.
```

For $\Delta\omega=-4\omega_1,\ -2\omega_1,\ -\omega_1,\ 0$, the predicted maximum probabilities are, respectively,

```math
\frac{1}{17}\approx0.0588,\qquad
\frac15=0.20,\qquad
\frac12=0.50,\qquad
1.
```

Small numerical differences due to time sampling are acceptable. The time interval must be long enough, and sampling sufficiently fine, to capture the peaks.

### 4. Generalized Rabi Frequency

Compare the oscillation frequencies with

```math
\Omega=\sqrt{\omega_1^2+(\Delta\omega)^2}.
```

Confirm that the oscillations become **slower** as the magnitude of detuning decreases toward zero while microwave coupling remains fixed, even as population transfer becomes more complete.

Record your numerical results and briefly identify any discrepancies.

---

## Physics Analysis

Answer these two questions **independently** in the designated Markdown cells of your notebook. Do not use an LLM to generate the answers.

**Question 3 — Resonant Spin Control**

In Part 1, the spin accumulated relative phase without changing its $|0\rangle$ and $|1\rangle$ populations. In this activity, resonant microwave driving caused those populations to oscillate. Explain the difference in terms of the Hamiltonian and the axis of rotation on the Bloch sphere. Why can a resonant $\pi$-pulse completely flip the spin?

**Question 4 — Approaching Resonance and Magnetometry**

Use your **four trajectories and population curves** to explain how decreasing detuning toward resonance changes the effective rotation axis, the maximum population transfer, and the oscillation frequency. Then consider an NV center exposed to a changing magnetic field while the microwave frequency is held fixed. Explain how the resulting change in detuning could provide information about the magnetic field.

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
- [ ] Simulated all four detuning values in order from strong detuning toward resonance.
- [ ] Generated **Figure 3** with four separate Bloch-sphere trajectories and four population curves.
- [ ] Checked normalization, the four predicted maximum transfer values, and the generalized oscillation frequencies.
- [ ] Answered both physics-analysis questions independently.
- [ ] Completed the Part 2 AI Development Record entry.

**Next: Part 3 — Optical Readout and NV Magnetometry.** You have examined spin evolution at four discrete microwave detunings. Next, you will sweep the microwave frequency through a continuous range around resonance while keeping the pulse duration fixed. Instead of displaying only spin populations, you will convert the simulated final populations into predicted red fluorescence. This will produce a simplified pulsed-ODMR spectrum and show how a magnetic-field-induced resonance shift can be measured.

**Important distinction for Part 3:** The maximum spin-flip probability calculated above is not necessarily the probability at a particular fixed pulse duration. For the frequency sweep, you will use the full time-dependent population $P_1(\tau)$ evaluated at the *same* microwave pulse duration for every detuning.
