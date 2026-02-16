# Part 1: Building Oracles By Hand
---
## Activity 1: Basic Circuits in Qiskit

Before we move on to Grover's Algorithm we need need fluency in circuit construction, simulation, and visualization. This lab builds that technical foundation.

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
To initial a quantum circuit in Qiskit we first import the circuit construct from qiskit, then initial the number of qubits and classical outputs we need.
```python
from qiskit import QuantumCircuit
import matplotlib.pyplot as plt

qc = QuantumCircuit(2, 2)
```
In this example we have initialize our circuit with name `qc` with 2 qubits (first argument inside `QuantumCircuit()`, and 2 classical outputs (second argument).

We can now add some basic single-qubit gates to our circuit.
```python
qc.h(0)
qc.x(1)
qc.z(0)
```
This adds a `Hadamard` on the $0^{th}$ qubit (notice Qiskit indexes off **0**), an `X` gate on the first, and a `Z` on the zeroth qubit.

We can draw this circuit using matplotlib
```python
qc.draw("mpl")
```
Make sure to take a brief moment to match the circiut diagram with the code.

We can also perform multi-qubit gates like `CNOT`

```python
qc = QuantumCircuit(2)   #make a new circuit and overwrite the last example

qc.h(0)        # put qubit 0 in superposition
qc.cx(0, 1)    # CNOT Gate with control = 0, target = 1

fig = qc.draw("mpl")
plt.show()
```
Or with a `CZ`.  It is important to note that when Qiskit draws a `CZ` it doesn't draw the control and target in a way that is obvious.

```python
qc = QuantumCircuit(2)   #make a new circuit and overwrite the last example

qc.h(0)        
qc.h(1)
qc.cz(0,1)  #Control Z with control = 0, target = 1

fig = qc.draw("mpl")
plt.show()
```
We can also do multi-controled CNOT gates, called `Toffoli` gate or `CCX` gates.

```python
qc = QuantumCircuit(3)

qc.x(0)
qc.x(1)
qc.ccx(0, 1, 2)   # controls = 0,1  target = 2

fig = qc.draw("mpl")
plt.show()
```
Performing more than two controls is not any more complicated, we just have to pass the `control` qubits as a list and use the `mcx([control],target)` structure.

```python
qc = QuantumCircuit(4)

controls = [0, 1, 2]
target = 3
qc.mcx(controls, target)

fig = qc.draw("mpl")
plt.show()
```
