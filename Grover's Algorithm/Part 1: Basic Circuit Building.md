# Part 1: Basic Curcuit Building
---
##Activity 1: Basic Circuits in Qiskit

Before we move on to Grover’s Algorithm, we need to be fluent in circuit construction, simulation, and visualization. This lab builds that technical foundation.

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
To initialize a quantum circuit in Qiskit, we first import the circuit construct from Qiskit, then initialize the number of qubits and classical outputs we need.
```python
from qiskit import QuantumCircuit
import matplotlib.pyplot as plt

qc = QuantumCircuit(2, 2)
```
In this example, we have initialized our circuit with the name `qc` with 2 qubits (first argument inside `QuantumCircuit()`, and 2 classical outputs (second argument).

We can now add some basic single-qubit gates to our circuit.
```python
qc.h(0)
qc.x(1)
qc.z(0)
```
This adds a `Hadamard` on the $0^{th}$ qubit (notice Qiskit indexes off 0), an `X` gate on the first, and a `Z` on the zeroth qubit.

We can draw this circuit using matplotlib
```python
qc.draw("mpl")
```
Make sure to take a brief moment to match the circuit diagram with the code.

We can also perform multi-qubit gates like `CNOT`

```python
qc = QuantumCircuit(2)   #make a new circuit and overwrite the last example

qc.h(0)        # put qubit 0 in superposition
qc.cx(0, 1)    # CNOT Gate with control = 0, target = 1

fig = qc.draw("mpl")
plt.show()
```
Or with a `CZ`.  It is important to note that when Qiskit draws a `CZ` it doesn’t draw the control and target in a way that is obvious.

```python
qc = QuantumCircuit(2)   #make a new circuit and overwrite the last example

qc.h(0)        
qc.h(1)
qc.cz(0,1)  #Control Z with control = 0, target = 1

fig = qc.draw("mpl")
plt.show()
```
We can also do multi-controlled CNOT gates, called `Toffoli` gate or `CCX` gates.

```python
qc = QuantumCircuit(3)

qc.ccx(0, 1, 2)   # controls = 0,1  target = 2

fig = qc.draw("mpl")
plt.show()
```
Performing more than two controls is not any more complicated; we just have to pass the `control` qubits as a list and use the `mcx([control],target)` structure.

```python
qc = QuantumCircuit(4)

controls = [0, 1, 2]              # Set control qubits
target = 3                        # Set target qubit
qc.mcx(controls, target)          # Perform a multi-controled X

from qiskit.circuit.library import ZGate     #import ZGate details

ccz =ZGate().control(2)           #make a custom Z Gate with two controls
qc.append(ccz,[0,1,2])            #add our new ccz gate to circut
                                  #gate has controls = [0,1], target = 2
fig = qc.draw("mpl")
plt.show()
```
Finally, we may need to perform an **anti-controlled** gate, which is a gate that triggers when the control bit is $|0\rangle$ rather than the normal $|1\rangle$.  Qiskit doesn’t have a native way of performing this, but we can just wrap a regular control with X gates to achieve the goal.  The code below performs a multi-controlled `X` gate with an anti-control on the zeroth qubit and a regular control on the first qubit.

```python
qc = QuantumCircuit(3)

qc.x(0)
qc.ccx(0, 1, 2)   # controls = 0,1  target = 2
qc.x(0)

fig = qc.draw("mpl")
plt.show()
```

The last thing we need to understand about building quantum circuits in Qiskit is how to perform measurements.  We can do that in two ways: 1) Direct Statevector Calculations, and 2)Shot-Based simulation.

**Statevector** calculations determine the exact quantum state of the system before measurement.  This is a purely mathematical object; it represents the full quantum state of the system.  Real quantum computers **CANNOT** directly output statevectors; it only works in a simulation and assumes a perfectly noiseless system.  This allows us to see not only the exact probabilities of each state, but also the probability amplitudes and phase information.

**Shot-Based** simulation is more what real devices do.  It runs the circuit many times (shots), where the outcome of each shot is probabilistically determined.  Then, after many shots, we can observe the full statistical distribution of all possible outcomes. Shot-based simulation only provides approximate statistics and does not provide any information about probability amplitudes or relative phases of states.

Here are basic ways to perform each type of evaluation.
```python
qc=QuantumCircuit(2)

qc.h(0)        #make a Bell State
qc.cx(0,1)

from qiskit_aer import Aer                    #import necessary Statevector methods
from qiskit.quantum_info import Statevector

sv = Statevector.from_instruction(qc)
print(sv)
```
The output of this should be a Bell State in the form:
```math
|\psi\rangle = \frac{1}{\sqrt{2}}|0_10_0\rangle + \frac{1}{\sqrt{2}}|1_11_0\rangle
```
Notice the indexing, and that the output statevector using complex numbers with **j** as the imaginary number.

We can change the circuit and make a shot-based measurement.  This will run the circuit 2000 times, make measurements of each qubit, and record their frequency of measuring to $|0\rangle$ and $|1\rangle$ independently.  It will print those frequency counts, as well as produce a histogram.

```python
from qiskit_aer import Aer

qc=QuantumCircuit(2)

qc.h(0)        #make a Bell State
qc.cx(0,1)
qc.measure_all()

backend = Aer.get_backend("aer_simulator")
result = backend.run(qc, shots=2000).result()
counts = result.get_counts()
print(counts)      #These counts should change slighly each time you exicute the code

from qiskit.visualization import plot_histogram
plot_histogram(counts)
plt.show()
```

