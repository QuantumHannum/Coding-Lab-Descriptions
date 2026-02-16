# Part 1: Building Oracles By Hand
---
## Activity 1: Basic Circuits in Qiskit

Before we move on to Grover's Algorithm we need need fluency in circuit construction, simulation, and visualization. This lab builds that technical foundation.
---
### Conceptual Goals
1. Understand how quantum circuits are represented programmatically in Qiskit.
2. Interpret single-qubit gates geometrically (Bloch sphere intuition).
3. Understand entanglement through controlled operations.
4. Connect statevectors to measurement probabilities.
5. Distinguish between:
      * Statevector simulation (ideal amplitudes)
      * Shot-based simulation (measurement statistics)

### Technical Goals
1. Initialize a `QuantumCircuit` with specified numbers of qubits and classical bits
2. Apply basic single-qubit gates
3. Apply controlled gates and multi-controlled gates
4. Add measurements
5. Draw circuits using matplotlib
6. Run circuits using
     * Statevector simulator
     * Shot-based Aer simulator
7. Extract
     * Statevectors
     * Measurement counts
     * Probability distributions
8. Produce histograms of results.

---
