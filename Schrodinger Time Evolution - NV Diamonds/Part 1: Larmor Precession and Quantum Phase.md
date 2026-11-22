# Part 1: Larmor Precession and Quantum Phase

## Big Idea

A quantum state can evolve in time even when the probabilities of measuring its energy eigenstates remain constant.

In this activity, you will investigate **Larmor precession** of an NV-center electron spin in a static magnetic field. You will use QuTiP to simulate Schrödinger time evolution, visualize the motion of the spin state on the Bloch sphere, and examine how its measurement probabilities change with time.

The central question is:

**How can a quantum state change in time when the probabilities of measuring its spin along the $z$-axis remain constant?**

---

## Physical Background

### The NV Center in a Magnetic Field

Recall that we are modeling two states of the NV center's electronic ground state:

```math
|0\rangle\equiv|m_s=0\rangle,
\qquad
|1\rangle\equiv|m_s=-1\rangle.
```

These states form our computational basis.

An NV center has an intrinsic energy splitting between its ground-state spin levels. Even without an applied magnetic field, the $m_s=0$ level is separated from the $m_s=\pm 1$ levels. Applying a magnetic field along the NV axis changes the energy difference between the spin states through the **Zeeman effect**.

The magnetic field shifts the $m_s=+1$ and $m_s=-1$ levels in opposite directions. In this lab, we focus on the $m_s=0\leftrightarrow m_s=-1$ transition, treating it as an effective two-level system.


<p align="center">
  <img src="Images/nv_center_zeeman_splitting_diagram.png"
       alt="NV-center Zeeman splitting energy diagram"
       width="450">
</p>

*Reference diagram: NV-center zero-field splitting and Zeeman shifts. The highlighted gap corresponds to the $m_s=0\leftrightarrow m_s=-1$ transition modeled in this lab. This is an instructor-provided reference image, not one of the required student-generated figures.*

If the magnetic field changes, the transition frequency between these two states also changes.

We will describe their relative phase evolution using the effective Hamiltonian

```math
\hat H=\frac{\hbar\omega}{2}\sigma_z,
```

where $\omega$ is the angular frequency of relative phase evolution. The Pauli-$z$ operator is

```math
\sigma_z=\begin{pmatrix}
1 & 0 \\
0 & -1
\end{pmatrix}.
```

For this activity, we use a **rotating reference frame** in which $\omega$ represents the signed precession rate relative to a chosen reference frequency. This allows us to simulate the relevant phase evolution without numerically resolving the NV center's approximately $2.87\,\mathrm{GHz}$ zero-field splitting.

### Relative Phase and Larmor Precession

Suppose the NV center is initially prepared in the equal superposition

```math
|\psi(0)\rangle=\frac{|0\rangle+|1\rangle}{\sqrt{2}}.
```

Under the Hamiltonian above, the state evolves as

```math
|\psi(t)\rangle=\frac{1}{\sqrt{2}}\left(e^{-i\omega t/2}|0\rangle+e^{i\omega t/2}|1\rangle\right).
```

Removing an overall global phase gives the physically equivalent state

```math
|\psi(t)\rangle\sim\frac{1}{\sqrt{2}}\left(|0\rangle+e^{i\omega t}|1\rangle\right).
```

The **relative phase** between the two basis states changes continuously with time. On the Bloch sphere, this produces a rotation around the $z$-axis.

The populations of the two states, however, remain constant:

```math
P_0(t)=|\langle 0|\psi(t)\rangle|^2,
\qquad
P_1(t)=|\langle 1|\psi(t)\rangle|^2.
```

Your task is to verify this behavior computationally and investigate what happens when the precession rate changes.

---

## Computational Investigation

Use the provided student notebook for this activity. It contains QuTiP imports, basis-state definitions, Pauli operators, time-evolution functions, and plotting tools.

You may use an LLM to generate or modify the Python code needed to carry out the investigation. **Your responsibility is to specify the physical model, predict its behavior, verify the numerical results, and explain the physics.**

### Step 1: Predict the Spin Evolution

Before running any simulations, consider the initial state

```math
|\psi(0)\rangle=\frac{|0\rangle+|1\rangle}{\sqrt{2}}
```

and Hamiltonian

```math
\hat H=\frac{\hbar\omega}{2}\sigma_z.
```

Record your predictions in the notebook:

1. Where will the initial state appear on the Bloch sphere?
2. What path will the Bloch vector follow as time increases?
3. Will $P_0(t)$ and $P_1(t)$ change during the evolution?
4. What will happen to the motion if $\omega$ is doubled?

**Do not run the simulation until you have recorded your predictions.**

### Step 2: Simulate Larmor Precession

Use QuTiP to simulate the time evolution of the initial superposition. For the first simulation, use

```math
\omega=2\pi\ \mathrm{rad/\mu s}
```

and simulate from $t=0$ to $t=2\,\mu\mathrm{s}$. This corresponds to two complete precession periods.

Your code should:

1. Construct the Hamiltonian using the supplied Pauli operators.
2. Prepare the initial superposition.
3. Use the provided time-evolution function to calculate the state over the specified interval.
4. Calculate the Bloch-vector components as functions of time.
5. Calculate $P_0(t)$ and $P_1(t)$.
6. Generate the required Bloch-sphere trajectory and population plots.

**AI coding task:** Ask your LLM to implement these calculations using the existing functions and variables in your student notebook. Do not ask it to rewrite the notebook or create a separate program. Use enough time points to produce a smooth trajectory.

### Step 3: Generate Figure 1

**Figure 1 — Larmor Precession and Spin-State Populations**

Create **one numbered figure containing two panels**.

**Panel A: Bloch-Sphere Trajectory**

Show the evolution of the Bloch vector over two precession periods. Include:

- The Bloch sphere with labeled $x$, $y$, and $z$ axes.
- The trajectory of the spin state.
- A clearly marked initial state.
- An indication of the direction of time evolution.

**Panel B: Spin-State Populations**

Plot both $P_0(t)$ and $P_1(t)$ over the same time interval. Include:

- Time in microseconds on the horizontal axis.
- Probability on the vertical axis.
- Distinguishable curves or labels for $P_0$ and $P_1$.
- A legend and descriptive title.

Include a short caption explaining the relationship between the Bloch-sphere trajectory and the population measurements.

### Step 4: Investigate the Effect of Precession Frequency

Repeat the simulation with

```math
\omega=4\pi\ \mathrm{rad/\mu s}.
```

Keep the initial state and total simulation time unchanged.

Compare the two simulations. In your notebook, report:

- The precession period for each value of $\omega$.
- The number of complete revolutions during the two-microsecond interval.
- Whether the populations change when the precession frequency is doubled.

You do not need to create another numbered figure. You may display an additional comparison plot if it helps you investigate the difference.

---

## Numerical Verification

Before accepting your simulation, verify the following results.

### 1. State Normalization

For every simulated time,

```math
\langle\psi(t)|\psi(t)\rangle=1.
```

### 2. Population Conservation

Verify that

```math
P_0(t)+P_1(t)=1
```

throughout the simulation. Also check that neither population changes during free precession, within numerical precision.

### 3. Precession Period

The expected precession period is

```math
T=\frac{2\pi}{|\omega|}.
```

Compare the period observed in your Bloch-sphere simulation with this analytical prediction.

### 4. Bloch-Vector Behavior

Calculate the expectation values

```math
\langle\sigma_x\rangle,\qquad
\langle\sigma_y\rangle,\qquad
\langle\sigma_z\rangle.
```

Verify that the Bloch vector remains on the equator and that its length remains approximately 1.

Record the numerical results of your verification in the notebook. If your results do not agree with the predictions, investigate the discrepancy before continuing.

---

## Physics Analysis

Answer the following questions **independently** in the designated Markdown cells of your notebook.

**Question 1 — Time Evolution Without Population Transfer**

Your Bloch sphere shows the quantum state evolving continuously in time, but your population graph shows constant values of $P_0$ and $P_1$.

Explain how these two observations are consistent. In your answer, distinguish between the relative phase of a quantum superposition and the probabilities of measuring its basis states.

**Question 2 — Magnetic Fields and Precession**

Increasing the precession frequency changes how rapidly the Bloch vector moves around the $z$-axis.

Explain why changing the energy difference between the two basis states changes the rate of relative phase accumulation. How does this connect to the effect of a magnetic field on the NV-center spin?

Your answers should refer to the physical behavior observed in your simulations, not simply restate equations.

**Do not use an LLM to generate your physics-analysis answers.**

---

## AI Development Record — Part 1

Document one meaningful AI interaction from this activity in the designated section of your student notebook. Include:

1. **Objective:** What were you trying to accomplish?
2. **Prompt:** Copy the actual prompt you submitted.
3. **AI Contribution:** What did the LLM generate or suggest?
4. **Verification:** How did you check that its result was physically and numerically correct?
5. **Revision (if applicable):** What did you change or improve?

If you did not use an LLM, describe your independent development and verification process. You do not need to include your complete conversation history.

---

## Part 1 Completion Checklist

Before moving to Part 2, confirm that you have:

- [ ] Completed the prediction questions before running the simulations.
- [ ] Simulated Larmor precession for both values of $\omega$.
- [ ] Generated Figure 1 with the required Bloch-sphere trajectory and population plot.
- [ ] Calculated the Bloch-vector components and verified population conservation.
- [ ] Compared the simulated precession periods with analytical predictions.
- [ ] Answered both physics-analysis questions independently.
- [ ] Completed your Part 1 AI Development Record entry.

**You are now ready for Part 2: Microwave Driving and Rabi Oscillations.**

In Part 1, the Hamiltonian changed the relative phase of the quantum state without changing its populations. In Part 2, you will introduce microwave driving and investigate how an applied field can transfer population between the two states.
