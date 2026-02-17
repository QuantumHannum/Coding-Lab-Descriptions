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
1.  Build a phase oracle that marks a specific 3-qubit state (e.g. $|111\rangle$) and other more complicated states.
2.  Build and simulate a complete Grover algorithm including phase oracle and diffuser
3.  Run full Grover algorithm on:
     * Statevector mode
     * Shot-based measurment on both noise-less and noisy backends
---

Grover's algorithm is composed of three steps. 
1. Initialize all qubits into a superposition
2. Apply Phase Oracle to tag state that is being searched for
3. Apply Diffuser to amplify the measurement amplitude of the state being searched for

This can be seen in the basic circut diagram below:
<img width="850" height="364" alt="GroverCircuit" src="https://github.com/user-attachments/assets/7d0c587f-924f-47be-9e5d-c843acd5c9f6" />

---
Lets explore how to code Grover's algorithm using 3 qubits and extract the $|111\rangle$ state.
---
First, set up the circuit and perform Hadamards on all qubits
```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

N=3   #number of qubits
qc=QuantumCircuit(N)

#*********************
# You add in N Hardamards, try using a for loop
#*********************



fig = qc.draw("mpl")
plt.show()
```
The state of the system after appling all the Hadamards is:
```math
|\psi\rangle = \frac{1}{\sqrt{8}}\left[|000\rangle+|001\rangle+|010\rangle+|011\rangle+|100\rangle+|101\rangle+|110\rangle+|111\rangle \right]
```
If we are trying to `search` for the state $|111\rangle$ we need the Oracle to tag just this state with a negative i.e. in the above equation make $-|111\rangle$ (hence the oracle is called the phase oracle).  The easiest way to do this is with a multi-controled Z gate.  Remembering the index order, if the zeroth, and first qubits are $|1\rangle$, then apply a `Z` gate to qubit two.

Update your code now to include this phase Oracle.  

```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

N=3   #number of qubits
qc=QuantumCircuit(N)

#*********************
# You add in N Hardamards, try using a for loop
#*********************


qc.barrier()      #this just puts a verticle line in the circut to seperate the code
#*********************
# You add in the ccz gate based phase oracle
#*********************


qc.barrier()
#*********************
# This section just prints the statevector so you can confirm phase
#*********************
sv = Statevector.from_instruction(qc)
print("\nStatevector amplitudes (basis order q2 q1 q0):")
for bitstring, amplitude in sv.to_dict().items():
    print(f"{bitstring} : {amplitude}")


#********************
# Draw Circuit
#*******************
fig = qc.draw("mpl")
plt.show()
```

The Phase Oracle construction is dependent on which state we are searching for - the Diffuser on the other hand is always the same.  The Diffuser has the effect of flipping (or rotating) each part of the superposition state around the average of all the probability amplitudes.

In this 3-qubit case, the average probability amplitude is:
```math
Ave = \frac{1}{8} \times \left[\frac{7}{\sqrt{8}}-\frac{1}{\sqrt{8}}\right] \approx 0.2652
```
The state right before the Diffuser should be:
```math
|\psi\rangle = \frac{1}{\sqrt{8}}\left[|000\rangle+|001\rangle+|010\rangle+|011\rangle+|100\rangle+|101\rangle+|110\rangle-|111\rangle \right]
```
If we call the original probability amplitudes $P_{0}$, and the new probablity amplitudes $P_{new}$, $P_{new}$ can be calculated by:
```math
P_{new}=P_{0}-2\times\left(P_{0}-Ave\right)
```
This has the effect of **lowering** the probabilty of all the states with positive probablity amplitude, and **raising** the probabilty of the one target state with negative probablity amplitude.

We can construct this Diffuser with the following recipe:
1. Apply `H` gates to all qubits
2. Apply `X` gates to all qubits
3. Apply a multi-control Z gate with controls on the first $N-1$ qubits, with the target on the last qubit
4. Apply `X` gates to all qubits
5. Apply `H` gates to all qubits

Now update your code with a Diffuser
```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

N=3   #number of qubits
qc=QuantumCircuit(N)

#*********************
# You add in N Hardamards, try using a for loop
#*********************



qc.barrier()     
#*********************
# You add in the ccz gate based phase oracle
#*********************




qc.barrier()
#*********************
# You add in the Diffuser here
#*********************





#*********************
# This section just prints the statevector so you can confirm phase
#*********************
sv = Statevector.from_instruction(qc)
print("\nStatevector amplitudes (basis order q2 q1 q0):")
for bitstring, amplitude in sv.to_dict().items():
    print(f"{bitstring} : {amplitude}")

#********************
# Draw Circuit
#*******************
fig = qc.draw("mpl")
plt.show()
```
Run your code and confirm that the Diffuser is working by comparing the statevector amplitudes to the values you would expect if calculating them by hand with the above equation.

As is, the code produces the Statevector, which is enought to see that the probablity of randomly selecting the target state of $|111\rangle$ is now much higher than the non-target states.  But we should complete our code and run shot-based trails and produce a histogram of outcomes. We can't however track the statevector and do the shot-based measurement, so you will need to remove the statevector part of the code.

```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

N=3   #number of qubits
qc=QuantumCircuit(N)

#*********************
# You add in N Hardamards, try using a for loop
#*********************




qc.barrier()      
#*********************
# You add in the ccz gate based phase oracle
#*********************




qc.barrier()
#*********************
# You add in the Diffuser here
#*********************



qc.barrier()
#********************
# Perform Shot-based Measurements
#*******************
qc.measure_all()                                  #add in measurements for all qubits

backend = Aer.get_backend("aer_simulator")
result = backend.run(qc, shots=4000).result()
counts = result.get_counts()

plot_histogram(counts)
plt.show()

#********************
# Draw Circuit
#*******************
fig = qc.draw("mpl")
plt.show()
```

You should see a very nice histogram showing the shot distribution and the overwhelming selection of the target state.

This `aer_simulator` is **perfect**, it does not include any random errors in the qubits when in reality quantum computers are noisy. To see the effect of real hardware we can import and use a `fake backend`.  IBM very carefully measures all the error rates of their computers, and users can import their properties to run noisy simulations that mirror results of running on real hardware.  To do this we need to import a few new things.

```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit_aer import Aer
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

#*********************
# New Imports
#*********************
from qiskit import transpile
from qiskit_aer import AerSimulator
from qiskit_ibm_runtime.fake_provider import FakeManilaV2   #get the IBM Manila info

N=3   #number of qubits
qc=QuantumCircuit(N)

#*********************
# You add in N Hardamards, try using a for loop
#*********************


qc.barrier()      
#*********************
# You add in the ccz gate based phase oracle
#*********************


qc.barrier()
#*********************
# You add in the Diffuser here
#*********************



qc.measure_all()          
#********************
# Perform Shot-based Measurements with Fake Backend
#*******************

#*** #FakeManila is one of IBM's real Quantum Computers
fake_backend = FakeManilaV2()       
sim_backend = AerSimulator.from_backend(fake_backend)

#**** When using real hardware, the circuit must be converted into the universal
#**** gate set of the specific computer.  We are going to talk more about this later
tqc = transpile(qc, sim_backend, optimization_level=1)
 
result = sim_backend.run(tqc, shots=4000).result()
counts = result.get_counts()

plot_histogram(counts)
plt.show()

#********************
# Draw Circuit
#*******************
fig = qc.draw("mpl")
plt.show()
```
IBM offers many different Fake Backends. After you get your code working, try other backends to see the difference in the error rates as shown in the final measurement histogram.

| Backend Name        | Qubits | Single Qubit Error | Two-Qubit Error | Readout Error |
|---------------------|--------|--------------------|-----------------|---------------|
| FakeManilaV2        | 5      | ~0.02% – 0.05%     | ~0.5% – 1.5%    | ~1% – 3%      |
| FakeLimaV2          | 5      | ~0.02% – 0.06%     | ~0.7% – 1.8%    | ~1% – 4%      |
| FakeBelemV2         | 5      | ~0.03% – 0.07%     | ~1% – 2%        | ~2% – 5%      |
| FakeAthensV2        | 5      | ~0.02% – 0.05%     | ~0.5% – 1.5%    | ~1% – 3%      |
| FakeJakartaV2       | 7      | ~0.02% – 0.06%     | ~1% – 2%        | ~2% – 5%      |
| FakeCasablancaV2    | 7      | ~0.03% – 0.08%     | ~1% – 3%        | ~3% – 6%      |
| FakeHanoiV2         | 27     | ~0.03% – 0.1%      | ~1% – 3%        | ~3% – 6%      |
| FakeKolkataV2       | 27     | ~0.03% – 0.1%      | ~1% – 3%        | ~3% – 6%      |
| FakeTorontoV2       | 27     | ~0.03% – 0.1%      | ~1.5% – 3%      | ~3% – 6%      |
| FakeMontrealV2      | 27     | ~0.03% – 0.1%      | ~1% – 3%        | ~3% – 6%      |
| FakeWashingtonV2    | 127    | ~0.05% – 0.15%     | ~1% – 3%        | ~3% – 7%      |

---
Making use of what you have learned, here are some more specific challenges to do on your own.  You will turn these in as part of your final submission:

For each of the following, produce **Full Circuit Diagram** and **Histogram**.  If you use a FakeBackend, make sure to note which one on your histograms.

1. Change your Grover Oracle so that it selects out the state $|100\rangle$.
2. Change your Grover Oracle so that it selects out two states $|101\rangle$ and $|110\rangle$
3. Change your Grover Oracle so that it selects out any states where exactly two qubits are $|1\rangle$, specifically this would be the states: $|110\rangle$, $|101\rangle$, $|011\rangle$


