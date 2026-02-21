# Part 3: Single SAT Problem
When studying the basic quantum algorithms (Deutsch-Jozsa, Simon, Grover, etc), they all start to look a bit contrived.  They all make use of oracles, but to generate the oracle, you need to "know the answer" already.  In the case of Deutsch-Jozsa, is the function constant or balanced? With Simon's, you need to know the secret key, and as we have seen with Grover's, you need to know the state you are looking for, and then find it.  It's time to look at an example of Grover's algorithm, where we use an oracle to confirm (or discover) a solution we don't know before we start the problem.

---
## Activity 3: Boolean Satisfiability Problems with Grover's

A Boolean Satisfiability Problem (SAT) asks whether there exists an assignment of truth values (True/False or 1/0) to a set of Boolean variables that makes a logical formula evaluate to True. Typically, the formula is written in Conjunctive Normal Form (CNF) — an AND of clauses, where each clause is an OR of literals (variables or their negations). For example:
```math
(x_1 \lor \lnot x_2) \land (x_2 \lor x_3)
```
Where $\lor$ is the symbol for `OR` and $\land$ is the symbol for `AND`.  The symbol for negation or `NOT` is $\lnot$.  The task is to determine whether there exists a bitstring $x=(x_1,x_2,...)$ that satisfies all clauses simultaneously.

Grover’s algorithm can be used to search for satisfying assignments by constructing a phase oracle that encodes the Boolean function:
```math
|x\rangle \rightarrow (-1)^{f(x)}|x\rangle
```

where $f(x)=1$ if the assignment satisfies the SAT formula and $f(x)=0$ otherwise. By placing all possible assignments into superposition and applying Grover’s amplitude amplification, the algorithm increases the probability of measuring a satisfying assignment.

### Conceptual Goals
1. Define a Boolean Satisfiability (SAT) problem and explain what it means for an assignment to satisfy a formula.
2. Interpret Conjunctive Normal Form (CNF) as a list of `AND` clauses composed of `OR` literals.
3. Explain how a Boolean function $f(x)$ can be encoded as a phase oracle:

$$
    |x\rangle \rightarrow (-1)^{f(x)}|x\rangle
$$

5. Predict how the number of satisfying solutions M of a SAT problem affects the number of necessary Grover iterations and the final measurement distribution.

### Technical/Coding Goals
1. Represent a Boolean variable using qubits.
2. Encode logical operations using quantum gates:
     * NOT via `X`
     * AND via `CCX`
     * OR via anti-controlled multi-qubit gates
3. Constructs a reversible logical oracle that:
     * Computes the SAT condition
     * Applies a phase flit to satisfying assignments
     * Uncomputes ancilla qubits
4. Run shot-based simulations with noiseless and noisy backends to observe amplification.

---
Problem Statement:
A door has a lock with five switches (labeled A - E) that are either switched up or down.  To open the door, all the switches need to be placed into the correct position. The correct switch position is given by the rules:
Clause Num.| Statement | Logical Form |
|:--------:|:----------|:-----------------------|
|1| At least one of switches **A** or **B** must be up. | $\(A \lor B\)$ |
|2| Either **A** is up or **B** is down. | $\(A \lor \lnot B\)$ |
|3| If **B** is up, then **C** must be up. | $\(\lnot B \lor C\)$ |
|4| If **B** is up, then **C** must be down. | $\(\lnot B \lor \lnot C\)$ |
|5| At least one of switches **C** or **D** must be up. | $\(C \lor D\)$ |
|6| Either **C** is up or **D** is down. | $\(C \lor \lnot D\)$ |
|7| If **D** is up, then **E** must be up. | $\(\lnot D \lor E\)$ |
|8| If **D** is up, then **E** must be down. | $\(\lnot D \lor \lnot E\)$ |

Take a few minutes and look at the table, and make sure you are clear about why the statement can be represented by the logical form.  This is going to be a key step later.

---
The claim is that Grover's algorithm can be used to devise all the possible correct positions of the switches.  If Gorver's is just a three-step process:
  1. Perform a Hadamard Transform on all qubits to generate a uniform superposition of all possible states.
  2. Apply the Grover Oracle to tag valid solutions with a phase
  3. Apply the diffuser to amplify the measurement probabilities of the valid states

Then the only part of the algorithm that every needs to change is step 2: Grover Phase Oracle.  All we have to do is translate the logical conditions of the problem into an oracle of the form:
```math
|x\rangle \rightarrow (-1)^{f(x)}|x\rangle
```
with $f(x)$ encodes all the logical conditions.

We need to first determine how many qubits we need for the problem.  We need one computational qubit for each variable (**A**-**E**).  We also need one ancilla qubit for each logical statement to record if that statement has been satisfied.  We then need one more ancilla bit to record if **ALL** logical statements have been sastified.  For this lock problem that means we need **14!** qubits.  

We also need an encoding for each qubit.  If computational qubit $|q_i\rangle = |0\rangle$ then lest assign that to mean the switch is in the **down** position.  If $|q_i\rangle = |1\rangle$, that switch is in the **up** position.  

Now let's look at the first logical statement $\(A \lor B\)$.  We need to check if 
**A** = {down, up}, i.e. $|A\rangle = |0\rangle$ or $|A\rangle = |1\rangle$, and specifically statement 1 is looking for when **A** is up. If $|A\rangle = |1\rangle$, then we need to record the first statement as satisfied.  Alternatively, we can record when the first statement **FAILS**.  So that would be when $|A\rangle = |0\rangle$. That is exactly what an anti-controlled `CX` does. As mentioned before, Qiskit doesn't have a native anti-control, but to achieve the same goal, we just have to wrap a regular control with `X` gates.  We can make a similar argument for switch **B**.  Because we need to consider both of these conditions **at the same time**, we need to use an Anti- `CCX`.  We then need to record the outcome of this logical check in the first ancilla bit.  That starts our Grover Oracle as:

<img width="97" height="300" alt="grover_circuit" src="https://github.com/user-attachments/assets/b83dd5c0-02d1-49d7-a427-c083db720309" />

Moving on to the second logical clause the only difference is that **B** is $\lnotB$.  that means the success criteria is $|B\rangle=|0\rangle$, and the failure is $|B\rangle=|1\rangle$.  That requires an `CNOT` on qubit B to recored the failure.   After the second clause is added, the Grover Oracle is:

<img width="162.4" height="300" alt="grover_circuit" src="https://github.com/user-attachments/assets/b21bac00-b63d-4812-a1ef-f4c4b5cb2666" />

We can finish out the rest of the logical clauses to produce the circuit below.  Barriers have been placed in the circuit to isolate each clause.

<img width="545.8" height="600" alt="grover_circuit" src="https://github.com/user-attachments/assets/dab81f6a-887e-4217-9b8b-e507ea3b9fee" />

The Oracle needs to flip the phase of all states that fit the logical conditions, but we have constructed our oracle off **FAILURES**.  We now have to check which conditions **haven't** failed (i.e. in the 0 state) with another anti-mult-control X gate on all of the ancilla, then add a `Z` gate to filp the phase of the states that haven't failed our logical tests.  You can see these last two steps in the above circuit.

We could stop now - but we won't.  As it stands, we just stored a values $\{0,1\}$ in all the ancilla.  We should clean these values up and make sure they all get reset back to their initial values.  This process is called **UNCOMPUTING** the ancilla.  To reset all these values, we just have to reverse the process we used to set the ancilla, i.e., we do all the same gates we did, but in reverse order.  This makes the full Oracle very long, but it is essential.  The final Grover Oracle, including all the uncompute steps, is shown below.


<img width="548.65" height="900" alt="grover_circuit" src="https://github.com/user-attachments/assets/483078ca-d5d1-40e9-b05d-325be74d8e9a" />

---
Ok, your turn.  Using the Oracle described above, code it in Qiskit.  You should define a function for the Oracle and Diffuser rather than hard-code them to make it easier to reuse them (*hint, hint*).  You can effectively define one custom gate `oracle_gate()` and `diffuser_gate()`, which contain all the smaller gates internally.

```python
def oracle_gate(n_comp: int, n_anc: int):
    n_total = n_comp + n_anc
    qc_oracle = QuantumCircuit(n_total)
    # ... build oracle using qubits:
    # comp = range(n_comp)
    # anc  = range(n_comp, n_total)



    return qc_oracle.to_gate(label="Oracle")

def diffuser_gate(n_comp: int):
    qc_diff = QuantumCircuit(n_comp)
    # ... build diffuser using only computational qubits



    return qc_diff.to_gate(label="Diffuser")

```
Here is how you use these custom gates:
```python
n_comp = 5
n_anc  = 9
n_total = n_comp + n_anc

qc = QuantumCircuit(n_total)

comp = list(range(n_comp))
anc  = list(range(n_comp, n_total))

O = oracle_gate(n_comp, n_anc)
D = diffuser_gate(n_comp)

backend = Aer.get_backend("aer_simulator")
shots = 4000

# Perform Hadamard Transform
qc.h(comp)

# Add oracle and diffuser to circuit
qc.append(O, comp + anc)   # oracle needs all qubits it was built for
qc.append(D, comp)         # diffuser only on computational register

# measure computational qubits only
qc.measure(comp, list(range(n_comp)))

# because we made custom gates we need to transpile them for Aer
qc_run = transpile(qc, backend, optimization_level=0)

result = backend.run(qc_run, shots=shots).result()
counts = result.get_counts()

fig = plot_histogram(counts)
plt.show()
```
If you are successful, your histogram should look like this:

<img width="1190" height="712" alt="1iteration" src="https://github.com/user-attachments/assets/421fde05-550f-48d2-8c54-aa1a9960b725" />

This result doesn't seem to have extracted a solution to the problem! As it turns out, when there are so many basis states ($ 2^5=32$ in this case), the amplitude amplification done by the Diffuser doesn't have much effect because the normalization probability has to be distributed over so many states.  This leads to the final part of Grover's.  You must do iterations of the oracle and diffuser until a maximum probability of selection is reached.

So one more code addition.  Wrap your circuit construction with a for loop that, on each loop, adds one more oracle+diffuser pair to the circuit.  At the conclusion of each for loop iteration, output a histogram.  Run 1, 2, 3,...,6 iterations, and based on your histogram, which iteration provides the best results?

For confirmation, you should discover that there are two correct switch positions that open the door. 

|Histogram Result| Lock Positions |
|:--------|:-----------------------|
|00101      | E = down, D = down, C = up, B = down, A = up |
|10101      | E = up,   D = down, C = up, B = down, A = up |


Theoretically, the optimal number of oracle + diffuser iterations is given by:
```math
I_{optimal}=\frac{\pi}{4}\sqrt{\frac{N}{M}}
```
Where N is the total number of basis states $(2^5=32)$ and M is the number of selected states.  For this problem, $I_{optimal} \approx 3.14$ - which means somewhere between 3 and 4 iterations should be optimal.
