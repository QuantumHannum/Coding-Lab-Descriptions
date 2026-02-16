# Part 2: Building Oracles By Hand

## Activity 2: A basic 3-qubit search
In the previous section, you learned how to construct and simulate basic quantum circuits using single-qubit gates and controlled operations. In this next stage, we transition from building circuits to engineering logic inside circuits. Grover’s algorithm does not simply apply gates — it encodes a decision problem into a phase oracle that marks specific computational basis states by flipping their phase. To do this, we must combine multi-controlled gates, anti-controls, and conditional phase operations to construct circuits that implement Boolean logic directly in Hilbert space. This section connects circuit mechanics (CNOT, CZ, CCX, multi-controlled gates) to the mathematical structure of Grover’s oracle:
```math
|x\rangle \rightarrow (-1)^{f(x)}|x\rangle
```
### Conceptual Goals
1. Understand the difference between bit-flip logic (X-type gates) and phase-flip logic (Z-type gates).
2. Explain why Grover’s oracle uses phase marking rather than bit flipping.
3. Interpret multi-controlled gates as quantum implementations of Boolean AND conditions.
4. Understand how anti-controls allow marking of arbitrary computational basis states.
5. Recognize that phase changes do not alter measurement probabilities directly but affect interference.

### Technical/Coding Goals
1.  Build a phase oracle that marks a specific 3-qubit state (e.g. $|101\rangle$
2.  Build and simulate a complete Grover algorithm including phase oracle and diffuser
3.  Run full Grover algorithm on:
     * Statevector mode
     * Shot-based measurment on both noise-less and noisy backends
---

Grover's algorithm has is composed of three steps. 
1. Initialize all qubits into a superposition
2. Apply Phase Oracle to tag state that is being searched for
3. Apply Diffuser to amplify the measurement amplitude of the state being searched for

This can be seen in the basic circut diagram below:
<img width="850" height="364" alt="GroverCircuit" src="https://github.com/user-attachments/assets/7d0c587f-924f-47be-9e5d-c843acd5c9f6" />


