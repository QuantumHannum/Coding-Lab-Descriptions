# Part 4: Robustness and particle loss

## Activity 7
### Goals
- What happens to entanglement if one particle is lost or ignored in the system?

### Specific Tasks
1.  For $N = \{5, 6, 7, 8\}$
   - Build $GHZ_N$ and $W_N$
   - Trace out one qubit
   - Perform calculations to determine the entanglement properties of the $(N-1)$ qubit subsytem.

So far, all of our analysis has assumed something ideal:

> We always have access to **every qubit** in the system.

In real physical systems, this is rarely true.

- A photon might be lost in an optical fiber.
- An atom might leave a trap.
- A qubit might decohere into its environment.
- A detector might fail to register one particle.

From the point of view of the remaining system, **losing a particle is the same as tracing it out**.

Why is this a deep physical question?

When you lose one particle from an entangled system, two very different things might happen:

1. The remaining system might still be **strongly entangled**.
2. The remaining system might become **almost completely unentangled** (separable or nearly so).

Which of these happens depends on how the entanglement was stored in the original system.  So this tier asks a very concrete, physically motivated question:

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

To generate the reduced density matrix after removing one qubit, write **a very short** function:

```python
def remove_one_qubit(rho,N):

   return 
```
This function should take in the full system density matrix $\rho$ and the number of qubits $N$, and then return the reduced density matrix with the $|q_0\rangle$ qubit removed.

In a previous section, we generated the **entropy** heatmap for the full $GHZ_N$ and $W_N$ states. Use the `remove_one_qubit` function, then plot heatmaps of just the **values** in the reduced density matrix for both $GHZ_N$ and $W_N$ states.  These are two different heatmaps!

**Entropy and purity** tell you *how mixed* the state is using a single number. The **density matrix heatmap** shows you *where* the coherence and structure are located inside the state.

Together, these give a much more complete picture of:

> How fragile or robust the quantum state really is under particle loss.

---
Making use of the above code, here are some questions to consider:
1. For $N = \{5, 6, 7, 8\}$, remove one qubit, then using other functions described in this lab, explore the purity and entanglement of the reduced systems for both $GHZ_N$ and $W_N$ states.
2. How would you describe the changes in the internal structures of $GHZ_N$ and $W_N$ states after removing a qubit from each, based on your heatmaps?
3. Which family of states is more robust under particle loss?

---

## What to Submit for this lab

### Required Plots
1.  Entropy Heatmaps from Activity 5
2.  Plots of Average Entropy vs. k for GHZ and W states from Activity 6
3.  Reduced density matrix heatmaps for GHZ and W states from Activity 7

### Answer these Required Questions
1.  Throughout this lab, you measured entanglement using numbers (entropy, purity), and made visualizations (entropy heatmaps and density-matrix heatmaps).  Explain the difference between:
   - The amount of entanglement in a system
   - The structure of entanglement in a system
2.  Give a concrete example from your results where two states look "similar" according to one measure (purity vs. entropy), but look very different in a heatmap or bipartition plot?
3. Both $GHZ_N$ and $W_N$ states are highly symmetric under permutations of the qubits.  How does this symmetry show up in the Entropy heatmaps and the Average Entropy vs. k curves?  Why is symmetry useful to consider in interpreting the Average Entropy vs. k curves?
4. Using the density-matrix heatmaps:
   - Explain the difference between **Global coherence** (coherence stored in the full N-quibt system) and **Local or Internal coherence** (coherence that survives inside subsystems).
   - Which type of coherence do $GHZ_N$ and $W_N$ states primarily rely on?
   - How does this explain the difference in robustness under particle loss?
