
# Part 1: Constructing Mixed States

## Activity 1: Phase Distributions and Density Matrices

### Goals

- Construct pure-state projectors using QuTiP.
- Combine projectors into a mixed-state density matrix.
- Visualize individual pure states and the averaged state on the Bloch sphere.
- Investigate how phase uncertainty changes quantum coherence.
- Make physical predictions and verify them using numerical simulations.

### Specific Tasks

1. Complete the **Part 0** environment setup and analytical warm-up.
2. Predict how increasing phase uncertainty will affect the density matrix and Bloch vector.
3. Generate $N=1000$ pure states with phases drawn uniformly from $[-\Delta,\Delta]$.
4. Compare $\Delta=0$, $\pi/2$, and $\pi$ radians.
5. Visualize individual states and their ensemble Bloch vector (Figure 1).
6. Calculate and plot $|\rho_{12}|$ as a function of $\Delta$ and compare with the analytical curve (Figure 2).
7. Verify your numerical results and explain any differences from your predictions.
8. Complete Questions 1–2 and document one meaningful AI interaction in your AI Development Record.

### Physics Background

Begin with a pure equatorial spin state:

$$
|\psi(\phi)\rangle=\frac{1}{\sqrt{2}}
\left(|+z\rangle+e^{i\phi}|-z\rangle\right).
$$

A density matrix for an ensemble of $N$ equally likely preparations is

$$
\rho_N=\frac{1}{N}\sum_{k=1}^{N}
|\psi(\phi_k)\rangle\langle\psi(\phi_k)|.
$$

**Important:** Each $|\psi(\phi_k)\rangle$ is a pure state. The matrix $\rho_N$ generally describes a mixed state.

For a uniform phase range $[-\Delta,\Delta]$,

$$
\lim_{N\to\infty}|(\rho_N)_{12}|
=\frac{1}{2}\left|\frac{\sin\Delta}{\Delta}\right|.
$$

Use radians in code. The value at $\Delta=0$ is defined by its limit, $1/2$.

---

### Prediction: What Happens When Phase Becomes Uncertain?

**Complete this section before running your simulations.** Record your predictions in a Markdown cell in your notebook. Do not use an LLM to generate your predictions.

Consider three ensembles with phase distributions:

- $\Delta=0$: All particles have the same relative phase.
- $\Delta=\pi/2$: Relative phases are distributed over a range of $\pi$ radians.
- $\Delta=\pi$: Relative phases are distributed over the entire circle.

**Prediction A — Density Matrix**

What do you expect to happen to the diagonal and off-diagonal elements of the ensemble density matrix as $\Delta$ increases? Will both change, or only certain elements?

**Prediction B — Bloch Sphere**

Each individual pure state lies on the surface of the Bloch sphere. Where do you predict the **ensemble Bloch vector** will lie for each of the three phase distributions?

Explain your reasoning.

You will revisit these predictions after generating Figures 1 and 2.

---

### Code Stem

Start with the import/basis cell from Part 0, then add:

```python
def make_state(phi):
    """Equatorial pure state with relative phase phi (radians)."""
    return (up + np.exp(1j * phi) * down).unit()


def make_ensemble(N, delta, seed=42):
    """Average N pure-state projectors over uniform phases [-delta, delta]."""
    rng = np.random.default_rng(seed)
    phases = rng.uniform(-delta, delta, N)
    states = [make_state(phi) for phi in phases]
    rho = sum((qt.ket2dm(s) for s in states), start=0) / N
    return rho, states, phases


rho, states, phases = make_ensemble(1000, np.pi/2)

print(rho)
print('Trace:', rho.tr())
print('Hermitian:', rho.isherm)
print('Eigenvalues:', rho.eigenenergies())
```

You may use an LLM to help modify, extend, or debug the code, but you are responsible for verifying that the resulting density matrices are physically valid.

### Figure 1: Bloch Sphere

For each of the three phase spreads, show **some individual pure states** and the **ensemble Bloch vector**.

You may make three panels (one per $\Delta$) or one figure with three clearly labeled views. To prevent clutter, display a representative subset of points instead of all 1000.

An effective way to calculate an ensemble Bloch vector is

```python
def bloch_vector(rho):
    return np.array([qt.expect(qt.sigmax(), rho),
                     qt.expect(qt.sigmay(), rho),
                     qt.expect(qt.sigmaz(), rho)], dtype=float)

print('Average Bloch vector:', bloch_vector(rho))

# QuTiP provides qt.Bloch(); see its add_states and add_vectors methods.
# Ask an LLM for a small plotting helper, or write one yourself.
```

**Verification — Figure 1**

After creating the figure:

1. Confirm that the individual pure-state Bloch vectors have magnitude 1, within numerical precision.
2. Calculate the magnitude of the ensemble Bloch vector for each $\Delta$.
3. Compare the position and length of each ensemble Bloch vector with your Prediction B.

In your notebook, briefly explain whether the simulation supported your prediction. If your prediction was incorrect, explain what the simulation revealed.

### Figure 2: Coherence Versus Phase Spread

Sweep $\Delta$ from $0$ to $\pi$.

Plot $|\rho_{12}|$ from the **simulated ensemble** and overlay the analytical prediction.

```python
deltas = np.linspace(0, np.pi, 31)

# TODO: for each delta, make an ensemble and record
# abs(rho.full()[0, 1]).

analytic_coherence = 0.5 * np.abs(np.sinc(deltas / np.pi))

# np.sinc(x) = sin(pi*x)/(pi*x), including the limit at x=0.
```

Use a common seed or more than one random realization if you want to distinguish sampling noise from the physical trend.

**Do not** force the simulation to equal the analytical prediction; small finite-sample differences are expected.

**Verification — Figure 2**

After generating your graph:

1. Confirm that the diagonal elements of each density matrix remain approximately $1/2$.
2. Compare the simulated $|\rho_{12}|$ with the analytical prediction at $\Delta=0$, $\pi/2$, and $\pi$.
3. Check that your numerical density matrices are Hermitian, have trace 1, and have nonnegative eigenvalues within numerical tolerance.
4. Identify any differences between your numerical results and the theoretical curve.

For at least one nontrivial value of $\Delta$, report the simulated and analytical values of $|\rho_{12}|$ and their absolute difference.

Briefly explain why the numerical results might not agree exactly with the analytical prediction.

**Return to Prediction A:** Did the diagonal and off-diagonal elements behave as you expected? Explain any differences between your prediction and the simulation.

---

## Questions to Consider

Answer these questions independently in Markdown cells. As explained in Part 0, you may use an LLM to help develop the simulation, but **you may not use an LLM to generate your physics analysis answers**.

**1.** Every state in the ensemble gives $P(+z)=P(-z)=1/2$. Why do the ensemble density matrices change as $\Delta$ increases? Explain which matrix elements change and which remain constant.

**2.** Why does the off-diagonal element approach zero for phases spread across the entire circle, even though every constituent state is pure? Use the idea of averaging complex numbers in your explanation. Does a mixed state *always* have zero off-diagonal entries?

---

## AI Development Record — Activity 1

As described in Part 0, document **one meaningful AI interaction** associated with this activity.

You may choose an interaction involving the construction of the ensemble, Bloch-sphere visualization, calculation of coherence, debugging, or verification of results.

In a Markdown cell under your notebook's **AI Development Record** section, include:

- **Objective:** What scientific or computational task were you trying to accomplish?
- **Prompt:** Copy the actual prompt you submitted to the LLM.
- **AI Contribution:** What code, explanation, or suggestion did the LLM provide?
- **Verification or Revision:** How did you check, test, correct, or modify its output?

If you did not use an LLM, describe how you developed and verified your code independently.

You do not need to document every prompt or submit an entire AI conversation. Select one interaction that demonstrates how you used computational assistance while retaining responsibility for the scientific investigation.

---

## What You Must Record

Include the following in your Jupyter notebook:

- **Initial Predictions:** Your predictions about changes to the density matrix and ensemble Bloch vector.
- **Figure 1:** Bloch representations for the three specified phase spreads, including the average vector.
- **Figure 2:** Simulated $|\rho_{12}|$ versus $\Delta$, with the theoretical curve.
- **Numerical Results:** Density matrices at $\Delta=0$, $\pi/2$, and $\pi$.
- **Verification:** Results of your numerical checks, including one quantitative comparison between simulation and theory.
- **Prediction Revisited:** A brief comparison of your original predictions with the simulation results.
- **Physics Analysis:** Your independently written answers to Questions 1–2.
- **AI Development Record:** One documented AI interaction, or a description of your independent development and verification process.

Continue using the same notebook in [Part 2: Measuring Coherence](Part%202%3A%20Measuring%20Coherence.md).

