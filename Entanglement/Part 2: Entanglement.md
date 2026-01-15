# Part 2: Entanglement


## Activity 3
### Goals
- A subsystem can be mixed even when the global state is perfectly pure

### Specific Tasks
1.  Build a Bell state and compute:
   - Purity and entropy of the global state
   - Purity and entropy of a subsystem
2.  Explore the entropy of a 3-qubit GHZ (Greenburg - Horne - Zeillinger)

So far, you have only worked with **product states**, where each qubit has its own independent state. In those cases, both the global system and every subsystem were always **pure**.

Now we will study the opposite situation: **entangled states**.

The Bell state: The simplest entangled system are the **Bell States**

```math
|\Phi^+\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)
```

is a pure state of the two-qubit system**, but each qubit by itself is in a mixed state.

This is the first example of a deep and counterintuitive fact:

> A subsystem can look completely random even when the full system is perfectly known.

This happens not because of ignorance, but because the qubit is entangled with another qubit.

Another interesting and useful entangled state is the GHZ state, which shares entanglement among many particles

We now generalize this idea to three or more particles.

The three-qubit GHZ (Greenberger–Horne–Zeilinger) state is:

```math
|\mathrm{GHZ}_3\rangle = \frac{1}{\sqrt{2}}(|000\rangle + |111\rangle)
```

More generally, for N qubits:

```math
|\mathrm{GHZ}_N\rangle = \frac{1}{\sqrt{2}}(|00\cdots0\rangle + |11\cdots1\rangle)
```

This is a state where:

- All particles are perfectly correlated
- The system is in a superposition of two macroscopically distinct configurations
- The entanglement is global: it involves the entire system at once

GHZ states are important because:

- They represent genuinely multipartite entanglement** (not just many Bell pairs)
- They are used in:
  - tests of quantum nonlocality beyond Bell’s inequalities
  - quantum information protocols
  - quantum metrology and sensing
- They are extremely fragile: losing even one particle destroys their entanglement

Most importantly for this lab, GHZ states are the cleanest example of a situation where:

> The whole system is perfectly pure, but every small piece of it looks mixed.

