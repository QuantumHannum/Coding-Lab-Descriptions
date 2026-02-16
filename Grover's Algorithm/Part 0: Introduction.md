# Part 0: Introduction to Grovers Algorithm and Unstructured Search

Grover’s algorithm is a quantum search algorithm that provides a quadratic speedup for searching an unstructured database. Classically, searching among 
$𝑁$ possible items requires $O(N)$ queries in the worst case because there is no structure to exploit. Grover’s algorithm reduces this to $O(\sqrt{N})$ queries by using quantum superposition, interference, and amplitude amplification inside an oracle. The algorithm works by (1) preparing a uniform superposition over all candidate states, (2) applying a phase-flip oracle that marks the correct solution(s), and (3) applying a diffusion operator that amplifies the amplitude of the marked state. Repeating this process approximately $\frac{\pi}{4}\sqrt{N}$ times rotates the system’s state toward the solution with high probability. While the speedup is “only” quadratic (unlike the exponential speedup of Simons or Shor’s algorithm), Grover’s result is provably optimal for unstructured search problems and forms the foundation for quantum approaches to SAT, constraint satisfaction, and combinatorial search.

---
## Goals for this investigation
### Conceptual Goals
1. Understand how amplitude amplification produces a quadratic speedup over classical search.
2. Distinguish between a bit-flip oracle and a phase oracle, and understand why Grover uses phase kickback.
3. Interpret Grover iterations geometrically as rotations in a two-dimensional subspace.
4. Understand how logical constraints (e.g., CNF clauses) can be encoded into quantum oracles.
5. Connect Grover’s search to SAT and 3-SAT as concrete computational problems.

### Technical/Coding Goals
1. Build and visualize simple quantum circuits in Qiskit.
2. Construct a 3-qubit phase oracle that marks specific computational basis states.
3. Simulate Grover’s algorithm using:
  * An ideal statevector simulator
  * A noisy or fake backend
4. Use Qiskit’s logical oracle tools (e.g., CNF-based encoding).
5. Implement a Grover-based solver for:
  * A constrained logic puzzle (e.g., dinner party problem)
  * A small 3-SAT instance
6. Compare classical brute-force search scaling vs. Grover scaling.
