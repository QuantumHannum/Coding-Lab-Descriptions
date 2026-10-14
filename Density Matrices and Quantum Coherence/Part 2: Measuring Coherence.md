# Part 2: Measuring Coherence

## Activity 2: Purity and the density-matrix Born rule

### Goals

- Calculate purity from a density matrix.
- Predict spin measurements in the $x$, $y$, and $z$ bases with the Born rule.
- Connect the location of the average Bloch vector to measurable spin statistics.

### Specific Tasks

1. **Reuse** the ensemble density matrices constructed in Part 1.
2. For each phase spread $\Delta$, calculate $\operatorname{Tr}(\rho^2)$.
3. Use projection operators to calculate $P(+x)$, $P(+y)$, and $P(+z)$.
4. Create **Figure 3**, with purity on the upper plot and measurement probabilities on the lower plot, both versus $\Delta$.
5. Check the limiting cases analytically.

### Physics Background

A pure state satisfies $\rho^2=\rho$ and has

```math
\mathcal P=\operatorname{Tr}(\rho^2)=1.
```

For a mixed qubit, $1/2\le\mathcal P<1$. The density-matrix Born rule for a projection $P_{+j}$ is

```math
P(+j)=\operatorname{Tr}(\rho P_{+j}).
```

For our phase ensemble centered at zero,

```math
\gamma(\Delta)=\frac{\sin\Delta}{\Delta},\qquad
\mathcal P(\Delta)=\frac12(1+\gamma^2),
```

```math
P(+x)=\frac12(1+\gamma),\qquad P(+y)=P(+z)=\frac12.
```

These equations are **predictions to test**, not code to substitute in place of the simulation.

### Code Stem

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

# TODO: reuse the delta sweep from Part 1.
# For each rho, save all four results and plot them.
# Add analytical predictions to check your numerical curves.
```

### Figure 3: One figure, two panels

- **Top panel:** Numerical $\mathcal P$ and analytical $\tfrac12(1+\gamma^2)$ versus $\Delta$.
- **Bottom panel:** Numerical $P(+x)$, $P(+y)$, and $P(+z)$ versus $\Delta$ (overlay theory where useful).

Use radians or degrees consistently and label the axis accordingly. Your plots should demonstrate both the fall in purity and the fact that not all measurement bases are equally sensitive to the changing phase distribution.

### Optional short experiment

Modify the phase range to be centered on $\phi_0=\pi/2$ instead of zero. Do the roles of $x$- and $y$-measurement probabilities change? Can the density matrix's coherence magnitude remain the same while its complex phase changes?

---

## Questions to Consider

**3.** How does purity change as the average Bloch vector shrinks? Why does the purity of a completely phase-randomized **equatorial** ensemble approach $1/2$ instead of zero?

**4.** Why does $P(+x)$ depend on the phase spread while $P(+z)$ does not? Explain using the off-diagonal and diagonal elements of $\rho$, not only the graphs.

## What You Must Record

- **Figure 3:** The two-panel purity and measurement-probability plot.
- A table reporting purity and $P(+x)$, $P(+y)$, $P(+z)$ at $\Delta=0$, $\pi/2$, and $\pi$.
- Your answers to Questions 3–4.

Continue in [Part 3](Part%203%3A%20Quantum%20Interference.md).
