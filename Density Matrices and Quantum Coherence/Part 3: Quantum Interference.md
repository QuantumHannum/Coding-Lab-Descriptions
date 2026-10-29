

# Part 3: Quantum Interference

## Activity 3: Phase Uncertainty in a Stern–Gerlach Interferometer

### Goals

- Connect off-diagonal coherence to observable interference fringes.
- Predict the effect of uncontrolled phase shifts between two Stern–Gerlach paths.
- Relate interference-fringe visibility to the off-diagonal elements of the density matrix.
- Distinguish **phase averaging** from **which-path measurement** while recognizing that both can produce the same reduced spin density matrix.
- Make physical predictions and verify them using numerical simulations.

### Specific Tasks

1. Model a spin prepared as $|+z\rangle$ and split coherently into $|+x\rangle$ and $|-x\rangle$ paths.
2. Predict how the final measurement probabilities will depend on the relative phase and phase uncertainty.
3. Include a controllable relative phase $\delta$ between paths before **ideal coherent recombination**.
4. Reuse the Part 1 phase-averaging model to include an additional random phase $\epsilon\in[-\Delta,\Delta]$ on each preparation.
5. Calculate the final $P(+z)$ as a function of $\delta$ for $\Delta=0$, $\pi/2$, and $\pi$.
6. Create **Figure 4** containing the three interference fringes and calculate their visibility.
7. Verify fringe visibility against the analytical prediction and the density-matrix coherence calculated in Part 1.
8. Complete Questions 5–6 and document one meaningful AI interaction in your AI Development Record.

### Physics Background

The first $x$-oriented Stern–Gerlach magnet resolves two components:

$$
|+z\rangle=\frac{1}{\sqrt{2}}\big(|+x\rangle+|-x\rangle\big).
$$

### Ideal Coherent Recombination

Assume the two paths are separated and then coherently recombined into the **same spatial mode** (with no residual path information). A controlled relative phase $\delta$ produces

$$
|\psi(\delta)\rangle=\frac{1}{\sqrt{2}}
\big(|+x\rangle+e^{i\delta}|-x\rangle\big).
$$

The following diagram illustrates the idealized Stern–Gerlach interferometer. The two paths are recombined coherently, without recording which-path information.

![Coherent Stern–Gerlach recombination](images/coherent_stern_gerlach_recombination.png)

*Figure A: Coherent Stern–Gerlach recombination. When the relative phase is zero, the initial $|+z\rangle$ state is recovered, and all particles exit through the $+z$ output of the final analyzer.*

The probability of a final $+z$ outcome is

$$
P(+z)=\frac{1}{2}(1+\cos\delta).
$$

Notice that the output probabilities depend on the **relative phase** between the two paths. When $\delta=0$, all particles emerge from the $+z$ output. When $\delta=\pi$, all particles emerge from the $-z$ output.

### Random Phase and Reduced Visibility

Now let the actual phase be $\delta+\epsilon$, where the uncontrolled phase $\epsilon$ varies uniformly across preparations in $[-\Delta,\Delta]$.

On averaging,

$$
P(+z)=\frac{1}{2}
\left[1+\frac{\sin\Delta}{\Delta}\cos\delta\right].
$$

For this **balanced, ideal** interferometer, the fringe visibility is

$$
V=\frac{P_{\max}-P_{\min}}{P_{\max}+P_{\min}}
=\left|\frac{\sin\Delta}{\Delta}\right|.
$$

At $\Delta=0$, use the limiting value $V=1$.

The diagram below shows an interferometer in which one path acquires an uncontrolled relative phase. Unlike the ideal coherent case, the observed interference pattern has reduced visibility.

![Partial coherence and random phase](images/partial_coherence_stern_gerlach_interferometer.png)

*Figure B: Partial coherence caused by random phase shifts. A fluctuating relative phase reduces the visibility of the interference fringes. Complete phase randomization removes the interference pattern.*

Equivalently,

$$
V=2|\rho_{12}|,
$$

**provided** $\rho$ is expressed in the intermediate $x$-path basis. A $z$-basis coherence element is not automatically the relevant one for a path interferometer.

---

### Prediction: What Happens to Quantum Interference?

**Complete this section before running your Part 3 simulation.** Record your predictions in a Markdown cell in your notebook. Do not use an LLM to generate your predictions.

Consider the interferometer illustrated in Figure A. The incoming beam is prepared in $|+z\rangle$, split into two $x$-oriented paths, and coherently recombined before entering the final $z$-analyzer.

**Prediction A — Controlled Relative Phase**

Suppose you can adjust the relative phase $\delta$ between the two paths.

Predict the probability of obtaining $+z$ at the final analyzer for:

- $\delta=0$
- $\delta=\pi/2$
- $\delta=\pi$

How should the output probability change as $\delta$ varies continuously from $0$ to $2\pi$?

Explain your prediction using the interference between the two probability amplitudes.

**Prediction B — Random Phase Uncertainty**

Now suppose the phase fluctuates randomly between preparations.

Predict what will happen to the interference pattern as the phase spread increases through:

- $\Delta=0$
- $\Delta=\pi/2$
- $\Delta=\pi$

Will the average probability of obtaining $+z$ change? Will the maximum and minimum probabilities change?

What do you predict will happen to the **visibility** of the interference fringes?

Explain your reasoning using what you learned about off-diagonal density-matrix elements in Parts 1 and 2.

You will revisit these predictions after generating Figure 4.

---

### Code Stem

Reuse the imports and basis states defined in Parts 0–2.

```python
plus_x = (up + down).unit()
minus_x = (up - down).unit()
P_out = qt.ket2dm(up)          # final +z detector


def path_state(phase):
    return (plus_x + np.exp(1j * phase) * minus_x).unit()


def output_probability(delta, Delta, N=1000, seed=42):
    rng = np.random.default_rng(seed)
    random_shifts = rng.uniform(-Delta, Delta, N)

    rho_out = sum(
        (qt.ket2dm(path_state(delta + eps)) for eps in random_shifts),
        start=0
    ) / N

    return (rho_out * P_out).tr().real


# Check one output:
print('Fully coherent:', output_probability(0, 0))

# TODO: Sweep controlled delta from 0 to 2*pi.
# Calculate P(+z) for Delta = 0, pi/2, pi.
# Plot the three simulated interference curves.
# Compare with the analytical fringe formula.
```

**Computational suggestion:** The double loop over phases and many $\delta$ values is intentionally transparent, but may be slow in shared notebook systems.

You can sample random offsets **once**, calculate their mean complex phase factor, and reuse that mean for every $\delta$—or have an LLM help vectorize the simulation.

Explain any optimization you make. **Do not simply replace simulation results with the analytical curve.** The numerical model should independently calculate the ensemble density matrix and measurement probabilities.

### Figure 4: Interference Fringe Visibility

Plot the simulated $P(+z)$ versus controlled phase $\delta$, for all three phase-spread values:

- $\Delta=0$
- $\Delta=\pi/2$
- $\Delta=\pi$

Include the analytical predictions on the same axes, using different line styles or markers to distinguish simulations from theory.

Your figure should have:

- A clearly labeled horizontal axis showing $\delta$ in radians or degrees.
- A vertical axis showing $P(+z)$.
- A legend identifying the three phase distributions.
- A caption explaining the physical significance of the changing interference visibility.

Use a sufficiently fine phase grid to estimate the maximum and minimum probabilities.

Calculate fringe visibility using

$$
V=\frac{P_{\max}-P_{\min}}{P_{\max}+P_{\min}}.
$$

Report the calculated visibility for each phase spread.

Compare your simulated interference patterns with the behavior illustrated in Figures A and B.

### Verification: Testing the Interference Model

After generating Figure 4, verify your results using the following checks.

**1. Verify the fully coherent limit.**

For $\Delta=0$, evaluate the simulated probability at:

$$
\delta=0,\qquad \frac{\pi}{2},\qquad \pi.
$$

Compare your results with the analytical predictions.

Confirm that the interference pattern has maximum visibility.

**2. Verify the fully phase-randomized limit.**

For $\Delta=\pi$, examine the output probability as $\delta$ changes.

Does the simulation approach

$$
P(+z)=\frac{1}{2}
$$

for all values of $\delta$?

Explain why finite numerical sampling might produce small fluctuations around this value.

**3. Verify an intermediate case.**

For $\Delta=\pi/2$, calculate the theoretical fringe visibility:

$$
V_{\mathrm{theory}}
=\frac{\sin(\pi/2)}{\pi/2}
=\frac{2}{\pi}.
$$

Compare this value with the visibility calculated from your simulated interference curve.

Report the numerical visibility, theoretical visibility, and absolute difference.

**4. Connect visibility to density-matrix coherence.**

Return to your Part 1 simulation and examine the off-diagonal coherence of the ensemble density matrix at $\Delta=\pi/2$.

Compare the magnitude of this element with the fringe visibility from Part 3.

Verify the relationship

$$
V=2|\rho_{12}|.
$$

Remember that the density matrix must be expressed in the basis corresponding to the two interfering paths.

For the equatorial ensemble in Part 1, the matrix is expressed in the $z$ basis. In Part 3, the interfering paths correspond to the $x$ basis. The numerical coherence magnitudes agree for these mathematically equivalent phase-ensemble models, but the matrix elements refer to different physical bases.

**Return to Your Predictions**

Compare Figure 4 and your verification results with Predictions A and B.

Briefly explain:

- Which predictions were supported by the simulation?
- How did the fringe visibility change as phase uncertainty increased?
- How do the off-diagonal density-matrix elements explain the observed interference?

Record your explanation in a Markdown cell.

---

### Connecting to welcher Weg (Conceptual Extension — No New Code Required)

In the preceding simulation, coherence was reduced by introducing random relative phases. However, interference can also disappear when information about the particle's path becomes physically available.

Consider placing a detector in each arm of the Stern–Gerlach interferometer, as shown below.

![Which-path information recorded](images/which_path_information_diagram.png)

*Figure C: Which-path measurement. Detectors record which intermediate path the particle followed. Even if the paths are subsequently recombined, the retained detector record makes the alternatives distinguishable and eliminates interference in the reduced spin state.*

A path detector may become entangled with the particle:

$$
|\Psi\rangle=\frac{1}{\sqrt{2}}\left(
|+x\rangle|D_+\rangle+
|-x\rangle|D_-\rangle
\right).
$$

If the detector is ignored, the reduced spin matrix in the $x$ basis is

$$
\rho_{\mathrm{spin}}=
\frac{1}{2}
\begin{pmatrix}
1 & \langle D_-|D_+\rangle\\
\langle D_+|D_-\rangle & 1
\end{pmatrix}.
$$

Orthogonal detector states ($\langle D_-|D_+\rangle=0$) give complete which-path distinguishability and remove the reduced spin coherence.

This mechanism differs from averaging classical random phases, even though the resulting spin density matrix can be identical.

A path detector does **not** need to be read by a person to establish physical distinguishability.

**Experimental qualification:** Merely letting the split paths converge on a detector is not enough to restore interference. The apparatus must erase residual spatial/path distinguishability and coherently recombine the modes. This exercise models that ideal limit.

---

## Questions to Consider

Answer these questions independently in Markdown cells. As explained in Part 0, you may use an LLM to develop the computational simulation, but **you may not use an LLM to generate your physics analysis answers**.

**5.** Explain why the interference pattern loses visibility as the random-phase range increases even though the probabilities of entering the two intermediate paths remain $1/2$. Connect your explanation to **off-diagonal density-matrix terms in the $x$ basis**.

**6.** Compare a phase-randomized ensemble with a system entangled with a which-path detector. How can these different mechanisms produce identical reduced spin density matrices? If the matrices are identical, could any measurement on the **spin alone** distinguish the mechanisms?

---

## AI Development Record — Activity 3

As described in Part 0, document **one meaningful AI interaction** associated with this activity.

You may choose an interaction involving the construction of the interferometer simulation, phase averaging, interference-fringe plotting, numerical optimization, or verification of fringe visibility.

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

- **Initial Predictions:** Your predictions about controlled phase shifts and the effect of phase uncertainty on interference.
- **Figure 4:** Three labeled simulated interference fringes with analytical comparisons.
- **Numerical Results:** Calculated fringe visibility for $\Delta=0$, $\pi/2$, and $\pi$.
- **Verification:** Analytical checks at selected phase values, including one quantitative comparison of simulated and theoretical visibility.
- **Coherence Comparison:** A numerical verification of the relationship between fringe visibility and off-diagonal density-matrix coherence.
- **Prediction Revisited:** A brief comparison of your original predictions with the simulation results.
- **Physics Analysis:** Your independently written answers to Questions 5–6.
- **AI Development Record:** One documented AI interaction, or a description of your independent development and verification process.

---

## Final Submission Requirements (All Parts)

Submit **one executable Jupyter notebook (`.ipynb`)**, completed using Google Colab, OnDemand, or VS Code.

Organize the notebook using clearly labeled sections and Markdown cells for predictions, explanations, analysis, and AI documentation.

Your notebook must include:

1. **Analytical Warm-Up:** Your calculation from Part 0 and its verification using QuTiP.

2. **Four Required Figures:**
   - Figure 1: Bloch-sphere representations of the phase ensembles.
   - Figure 2: Off-diagonal coherence versus phase spread.
   - Figure 3: Purity and measurement probabilities versus phase spread.
   - Figure 4: Stern–Gerlach interference fringes.

3. **Predictions and Verification:** Your initial predictions for all three activities, comparisons with the simulated results, and quantitative verification against analytical calculations or known physical limits.

4. **Six Numbered Physics Explanations:** Independently written answers to Questions 1–6.

5. **Numerical Results:** Density matrices and tables requested in Parts 1–2, and interference-fringe visibility values from Part 3.

6. **AI Development Record:** Three documented AI interactions, one associated with each activity. Each entry must include the objective, actual prompt, AI contribution, and verification or revision. If you did not use an LLM for an activity, describe your independent development and verification process instead.

7. **Final AI Reflection:** Approximately 100 words discussing how AI affected your computational investigation, one limitation or mistake you investigated, and one physics insight you developed through independently interpreting the simulations.

8. **Reproducible Code:** All code necessary to regenerate the numerical results and plots. The notebook must execute from top to bottom in a fresh environment after installing its required packages.

### Evaluation

Your submission will be evaluated using the following weights:

| Component | Weight |
|---|---:|
| Physics predictions and interpretation | 40% |
| Accurate, labeled visualizations | 30% |
| Correct, validated calculations | 20% |
| Clarity, reproducibility, and AI Development Record | 10% |

Code elegance, prompt sophistication, and the amount of code written without assistance are **not** grading objectives.

### Final Validation Checklist

Before submitting, verify that:

- Every density matrix has trace approximately 1.
- Every density matrix is Hermitian.
- Density-matrix eigenvalues are nonnegative within numerical tolerance.
- Measurement probabilities lie between 0 and 1.
- Numerical results have been compared with the analytical predictions.
- All four figures are labeled and included in the notebook.
- All six physics analysis questions are answered independently.
- The three AI Development Record entries and Final AI Reflection are included.

**Submit only the completed Jupyter notebook.** Place your AI Development Record and Final AI Reflection in Markdown cells at the end of the notebook. No separate AI transcript or additional file is required.

