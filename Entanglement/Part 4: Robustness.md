# Part 4: Robustness and particle loss

## Activity 7
### Goals
- What happens to entanglement if one particle is lost or ignored in the system?

### Specific Tasks
1.  For an N = 5, 6, 7, 8
   - Build $GHZ_N$ and $W_N$
   - Trace out one qubit
   - Compute the purity of the remaining state

So far, all of our analysis has assumed something ideal:

> We always have access to **every qubit** in the system.

In real physical systems, this is rarely true.

- A photon might be lost in an optical fiber.
- An atom might leave a trap.
- A qubit might decohere into its environment.
- A detector might simply fail to register one particle.

From the point of view of the remaining system, **losing a particle is the same as tracing it out**.

Mathematically, this means:

> Particle loss = partial trace over that particle.

Why is this a deep physical question?

When you lose one particle from an entangled system, two very different things *might* happen:

1. The remaining system might still be **strongly entangled**.
2. The remaining system might become **almost completely classical** (separable or nearly so).

Which of these happens depends on **how the entanglement was stored in the original system**.

So this tier asks a very concrete, physically motivated question:

> Is this entanglement **robust** or **fragile** under particle loss?

- **Robust entanglement**:
  - Losing one particle does **not** destroy all the quantum correlations.
  - The remaining system is still noticeably mixed and entangled.
  - This is what you should expect for **distributed, many-body entanglement**.

- **Fragile entanglement**:
  - Losing one particle **collapses the structure** of the correlations.
  - The remaining system becomes close to a classical mixture.
  - This is what you should expect for **global, all-or-nothing entanglement**.
 
The robustness is exactly the question that determines whether a form of entanglement is useful for:
- quantum communication
- quantum networks
- quantum sensing
- or large-scale quantum devices
