# Part 1: Constructing Mixed States

## Activity 1: Phase distributions and density matrices

### Goals

- Construct pure-state projectors using QuTiP.
- Combine projectors into a mixed-state density matrix.
- Visualize individual pure states and the averaged state on the Bloch sphere.
- Test how phase uncertainty changes coherence.

### Specific Tasks

1. Complete the **Part 0** environment setup and analytical warm-up.
2. Generate $N=1000$ pure states with phases drawn uniformly from $[-\Delta,\Delta]$.
3. Compare $\Delta=0$, $\pi/2$, and $\pi$ radians.
4. Visualize the individual states and ensemble Bloch vector (Figure 1).
5. Calculate and plot $|\rho_{12}|$ as a function of $\Delta$ and compare with the analytical curve (Figure 2).

### Physics Background

Begin with a pure equatorial spin state:

```math
|\psi(\phi)\rangle=\frac1{\sqrt2}\left(|+z\rangle+e^{i\phi}|-z\rangle\right).
```

A density matrix for an ensemble of $N$ equally likely preparations is

```math
\rho_N=\frac1N\sum_{k=1}^N|\psi(\phi_k)\rangle\langle\psi(\phi_k)|.
```

**Important:** Each $|\psi(\phi_k)\rangle$ is a pure state. The matrix $\rho_N$ generally describes a mixed state. For a uniform phase range $[-\Delta,\Delta]$,

```math
\lim_{N\to\infty}|(\rho_N)_{12}|=\frac12\left|\frac{\sin\Delta}{\Delta}\right|.
```

Use radians in code. The value at $\Delta=0$ is the limit $1/2$.

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

### Figure 1: Bloch sphere

For each of the three phase spreads, show **some individual pure states** and the **ensemble Bloch vector**. You may make three panels (one per $\Delta$) or one figure with three clearly labeled views. To prevent clutter, display a representative subset of points instead of all 1000. An effective way to calculate an ensemble Bloch vector is

```python
def bloch_vector(rho):
    return np.array([qt.expect(qt.sigmax(), rho),
                     qt.expect(qt.sigmay(), rho),
                     qt.expect(qt.sigmaz(), rho)], dtype=float)

print('Average Bloch vector:', bloch_vector(rho))

# QuTiP provides qt.Bloch(); see its add_states and add_vectors methods.
# Ask an LLM for a small plotting helper, or write one yourself.
```

### Figure 2: Coherence versus phase spread

Sweep $\Delta$ from $0$ to $\pi$. Plot $|\rho_{12}|$ from the **simulated ensemble** and overlay the analytical prediction.

```python
deltas = np.linspace(0, np.pi, 31)

# TODO: for each delta, make an ensemble and record
# abs(rho.full()[0, 1]).

analytic_coherence = 0.5 * np.abs(np.sinc(deltas / np.pi))
# np.sinc(x) = sin(pi*x)/(pi*x), including the limit at x=0.
```

Use a common seed or more than one random realization if you want to distinguish sampling noise from the physical trend. **Do not** force the simulation to equal the analytic prediction; small finite-sample differences are expected.

---

## Questions to Consider

**1.** Every state in the ensemble gives $P(+z)=P(-z)=1/2$. Why do the ensemble density matrices change as $\Delta$ increases? Explain which matrix elements change and which remain constant.

**2.** Why does the off-diagonal element approach zero for phases spread across the entire circle, even though every constituent state is pure? Use the idea of averaging complex numbers in your explanation. Does a mixed state *always* have zero off-diagonal entries?

## What You Must Record

- **Figure 1:** Bloch representations for the three specified phase spreads, including the average vector.
- **Figure 2:** Simulated $|\rho_{12}|$ versus $\Delta$, with the theoretical curve.
- The numerical density matrices at $\Delta=0$, $\pi/2$, and $\pi$.
- Your answers to Questions 1–2.

Continue using the same notebook in [Part 2](Part%202%3A%20Measuring%20Coherence.md).
