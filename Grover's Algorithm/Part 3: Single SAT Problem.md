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

Now lets look at the first logical statement $(A \lor B)$.  We need to check if **A** = {down, up}, i.e. $|A\rangle = |0\rangle$ or $|A\rangle = |1\rangle$, and specifically statement 1 is looking for when **A** is up. If $|A\rangle = |1\rangle$, then we need to record the first statement as satisfied.  That is exactly what a `CX` does.  We can make a similar argument for switch **B**.  Because we need to consider both of these conditions **at the same time**, we need to use a `CCX`.  We then need to record the outcome of this logical check in the first ancilla bit.  That starts our Grover Oracle as:

<img width="68.48" height="300" alt="Screenshot 2026-02-18 at 8 36 08 PM" src="https://github.com/user-attachments/assets/7ead6081-0050-439d-a433-4aaf4faec04c" />

Moving on to the second logical clause the only difference is that **B** is $\lnotB$.  that means we are interested in the case where $|B\rangle=|0\rangle$.  That requires an `anti-control` on qubit B.  As mentioned before, Qiskit doesn't have a native anti-control, but to achieve the same goal, we just have to wrap a regular control with `X` gates.  After the second clause is added, the Grover Oracle is:

<img width="122.54" height="300" alt="Screenshot 2026-02-18 at 8 45 49 PM" src="https://github.com/user-attachments/assets/7bc88417-ab50-4a5e-9146-7514e198f25e" />

We can finish out the rest of the logical clauses to produce the circuit below.  Barriers have been placed in the circuit to isolate each clause.

<img width="2369" height="2628" alt="grover_circuit" src="https://github.com/user-attachments/assets/f429bc0d-3f39-4dfe-a02c-99d059787e46" />

The Oracle needs to flip the phase of all states that fit the conditions.  We now have to check that **ALL** conditions have been met with another mult-control X gate on all of the ancilla, then add a `Z` gate to filp the phase.  You can see these last two steps in the above circuit.

We could stop now - but we won't.  As it stands, we just stored a values $\{0,1\}$ in all the ancilla.  We should clean these values up and make sure they all get reset back to their initial values.  This process is called **UNCOMPUTING** the ancilla.  To reset all these values, we just have to reverse the process we used to set the ancilla, i.e., we do all the same gates we did, but in reverse order.  This makes the full Oracle very long, but it is essential.  The final Grover Oracle, including all the uncompute steps, is shown below.

<img width="2369" height="3815.72" alt="Screenshot 2026-02-18 at 9 23 08 PM" src="https://github.com/user-attachments/assets/67124bba-7230-4dc2-8689-cc215fd980c5" />

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

# Perform Hadamard Transform
qc.h(comp)

# Add oracle and diffuser to circuit
qc.append(O, comp + anc)   # oracle needs all qubits it was built for
qc.append(D, comp)         # diffuser only on computational register

```

To make the outcomes more realistic, you should use a FakeBackend to produce a histogram of the results.  Make sure to choose a FakeBackend that has enough qubits (i.e., Manilla is too small). 

