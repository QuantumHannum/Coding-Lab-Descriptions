
# Part 2: Measuring Coherence

## Activity 2: Purity and the Density-Matrix Born Rule

### Goals

- Calculate purity from a density matrix.
- Predict spin measurements in the $x$, $y$, and $z$ bases using the Born rule.
- Connect the location of the average Bloch vector to measurable spin statistics.
- Make physical predictions and verify them using numerical simulations.

### Specific Tasks

1. **Reuse** the ensemble density matrices constructed in Part 1.
2. Predict how purity and spin-measurement probabilities will change as the phase spread increases.
3. For each phase spread $\Delta$, calculate $\mathrm{Tr}(\rho^2)$.
4. Use projection operators to calculate $P(+x)$, $P(+y)$, and $P(+z)$.
5. Create **Figure 3**, with purity on the upper plot and measurement probabilities on the lower plot, both versus $\Delta$.
6. Verify numerical results against analytical predictions and known physical limits.
7. Complete Questions 3–4 and document one meaningful AI interaction in your AI Development Record.

### Physics Background

The **purity** of a density matrix is defined as

$$
\mathcal{P} = \mathrm{Tr}(\rho^2).
$$

A pure state satisfies $\rho^2=\rho$ and has purity $\mathcal{P}=1$.

For a mixed spin-1/2 state,

$$
\frac{1}{2}\leq\mathcal{P}<1.
$$

The density-matrix Born rule gives the probability of obtaining measurement outcome $+j$:

$$
P(+j)=\mathrm{Tr}(\rho P_{+j}),\qquad j=x,y,z.
$$

For the equatorial phase ensemble centered at zero, define the coherence factor

$$
\gamma(\Delta)=\frac{\sin\Delta}{\Delta}.
$$

The analytical predictions are

$$
\mathcal{P}(\Delta)=\frac{1}{2}(1+\gamma^2)
$$

and

$$
P(+x)=\frac{1}{2}(1+\gamma),
\qquad
P(+y)=P(+z)=\frac{1}{2}.
$$

At $\Delta=0$, use the limiting value $\gamma=1$.

These equations are **predictions to test**, not code to substitute in place of the numerical simulation.

---

### Prediction: Can Measurements Reveal Lost Coherence?

**Complete this section before running your Part 2 calculations.** Record your predictions in a Markdown cell in your notebook. Do not use an LLM to generate your predictions.

In Part 1, you constructed ensembles of pure states with increasing uncertainty in their relative phases. You observed how the ensemble Bloch vector and off-diagonal density-matrix elements changed.

Now consider what those changes might mean for measurable quantities.

**Prediction A — Purity**

As the phase spread increases from $\Delta=0$ to $\Delta=\pi$, do you expect the purity $\mathcal{P}$ to increase, decrease, or remain constant?

What values do you expect for the fully coherent and completely phase-randomized limits?

Explain your prediction using the physical meaning of a pure versus mixed state.

**Prediction B — Stern–Gerlach Measurements**

Imagine directing the ensembles through separate Stern–Gerlach analyzers oriented along $x$, $y$, and $z$.

Which measurement probabilities do you predict will change as the phase spread increases?

Which will remain constant?

Explain your reasoning using the density matrices and Bloch vectors from Part 1.

You will revisit these predictions after generating Figure 3.

---

### Code Stem

Reuse the import statements, basis states, and ensemble-generation functions from Part 1.

Add the following projection operators and analysis function:

```python
plus_x = (up + down).unit()
plus_y = (up + 1j * down).unit()

P_x = qt.ket2dm(plus_x)
P_y = qt.ket2dm(plus_y)
P_z = qt.ket2dm(up)


def analyze_density_matrix(rho):
    purity = (rho * rho).tr().real
    p_x = (rho * P_x).tr().real
    p_y = (rho * P_y).tr().real
    p_z = (rho * P_z).tr().real
    return purity, p_x, p_y, p_z


print(analyze_density_matrix(rho))

# TODO: Reuse the delta sweep from Part 1.
# For each simulated density matrix, calculate
# purity, P(+x), P(+y), and P(+z).
#
# Save the numerical results for plotting.
# Add analytical predictions for comparison.
```

You may use an LLM to help extend the code, organize your calculations, or construct the plots. However, your results must come from the **simulated density matrices**, not simply from evaluating the analytical equations.

### Figure 3: Purity and Measurement Probabilities

Create **one figure containing two panels**.

- **Top panel:** Numerical purity $\mathcal{P}$ versus phase spread $\Delta$, with the analytical prediction $\frac{1}{2}(1+\gamma^2)$ overlaid.
- **Bottom panel:** Numerical $P(+x)$, $P(+y)$, and $P(+z)$ versus phase spread $\Delta$, with analytical predictions overlaid where useful.

Use radians or degrees consistently and label the horizontal axis accordingly.

Include descriptive titles, axis labels, and a legend identifying the numerical and analytical results.

Your plots should demonstrate both the decrease in purity and the fact that not all measurement bases are equally sensitive to the changing phase distribution.

### Verification: Testing the Numerical Results

After generating Figure 3, verify your results using the following checks.

**1. Verify the limiting cases.**

For $\Delta=0$ and $\Delta=\pi$, compare your simulated purity and measurement probabilities with the analytical predictions.

In particular, check whether the results agree with the expected behavior of a pure $|+x\rangle$ state and a maximally mixed spin state.

**2. Verify an intermediate case.**

At $\Delta=\pi/2$, calculate the theoretical coherence factor:

$$
\gamma=\frac{\sin(\pi/2)}{\pi/2}=\frac{2}{\pi}.
$$

Use this value to calculate the expected purity and $P(+x)$ analytically.

Compare these predictions with the simulated values. Report the absolute difference for each quantity.

**3. Check the Born-rule probabilities.**

For each density matrix, verify that the calculated probabilities lie between 0 and 1.

For at least one ensemble, also calculate $P(-x)$ and verify that

$$
P(+x)+P(-x)=1.
$$

**4. Connect purity to Bloch-vector length.**

Using the Bloch-vector function from Part 1, select one of your simulated density matrices and check the relationship

$$
\mathcal{P}=\frac{1}{2}(1+|\mathbf{r}|^2).
$$

Compare the purity calculated directly from the density matrix with the value calculated from the Bloch-vector magnitude.

**Return to Your Predictions**

Compare Figure 3 and the verification results with Predictions A and B.

Briefly explain:

- Which predictions were supported by the simulation?
- Did any results differ from what you initially expected?
- How do the off-diagonal density-matrix elements help explain the measurement probabilities?

Record your explanation in a Markdown cell.

---

### Optional Short Experiment: Rotating the Average Phase

In the original model, the phase distribution is centered at $\phi_0=0$.

Modify the phase-generation function so that the distribution is instead centered at

$$
\phi_0=\frac{\pi}{2}.
$$

Keep the phase spread $\Delta$ unchanged.

Investigate the following:

- Do the roles of $P(+x)$ and $P(+y)$ change?
- Does the purity change?
- Can the magnitude of the off-diagonal coherence remain constant while its complex phase changes?

This experiment is optional and does not require an additional submitted figure.

---

## Questions to Consider

Answer these questions independently in Markdown cells. As explained in Part 0, you may use an LLM to develop the computational simulation, but **you may not use an LLM to generate your physics analysis answers**.

**3.** How does purity change as the average Bloch vector shrinks? Why does the purity of a completely phase-randomized **equatorial** ensemble approach $1/2$ instead of zero?

**4.** Why does $P(+x)$ depend on the phase spread while $P(+z)$ does not? Explain using the off-diagonal and diagonal elements of $\rho$, not only the graphs.

---

## AI Development Record — Activity 2

As described in Part 0, document **one meaningful AI interaction** associated with this activity.

You may choose an interaction involving calculating purity, implementing the density-matrix Born rule, generating Figure 3, debugging, or verifying the simulation against analytical predictions.

In a Markdown cell under your notebook's **AI Development Record** section, include:

- **Objective:** What scientific or computational task were you trying to accomplish?
- **Prompt:** Copy the actual prompt you submitted to the LLM.
- **AI Contribution:** What code, explanation, or suggestion did the LLM provide?
- **Verification or Revision:** How did you check, test, correct, or modify its output?

If you did not use an LLM, describe how you developed and verified your code independently.

You do not need to document every prompt or submit an entire AI conversation. Select one interaction demonstrating how you used computational assistance while retaining responsibility for the scientific investigation.

---

## What You Must Record

Include the following in your Jupyter notebook:

- **Initial Predictions:** Your predictions about purity and measurement probabilities.
- **Figure 3:** The two-panel purity and measurement-probability plot.
- **Numerical Results:** A table reporting purity and $P(+x)$, $P(+y)$, and $P(+z)$ at $\Delta=0$, $\pi/2$, and $\pi$.
- **Verification:** Analytical comparisons for the limiting cases and $\Delta=\pi/2$, including numerical differences.
- **Bloch-Vector Check:** One numerical verification of the relationship between purity and Bloch-vector magnitude.
- **Prediction Revisited:** A brief comparison of your original predictions with the simulation results.
- **Physics Analysis:** Your independently written answers to Questions 3–4.
- **AI Development Record:** One documented AI interaction, or a description of your independent development and verification process.

Continue using the same notebook in [Part 3: Quantum Interference](Part%203%3A%20Quantum%20Interference.md).
e.md).
