# Part 3: Single SAT Problem
When studying the basic quantum algorithms (Deutsch-Jozsa, Simon, Grover, etc) they all start to look a bit contrived.  They all make use of oracles, but to generate the oracle you need to "know the answer" already.  In the case of Deutsch-Jozsa - is the function constant or balanced, with Simon's - you need to know the secret key, and as we have seen with Grover's you need to know the state you are looking for, and then find it.  Its time to look at an example of Grover's algorithm where we use an oracle to confirm (or discover) a solution we don't know before we start the problem.

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
1. Represent Boolean variable using qubits.
2. Encode logical operations using quantum gates:
     * NOT via `X`
     * AND via `CCX`
     * OR via anti-controlled multi-qubit gates
3. Constructs a reversible logical oracle that:
     * Computes the SAT condition
     * Applies a phase flit to satisfying assignments
     * Uncomputes ancilla qubits
4. Run shot-based simulations with noise-less, and noisy backends to observe amplification.

---
Problem Statement:
A door has a lock with five switches (labeled A - E) that are either on or off.  To open the door, all the switches need to be placed into the correct position.  The correct switch position is given by the rules:
1. At least one of the switches A or B must be up
2. Either A is up or B is down
3. If B is up, then C must be up
4. If B is up, then C must be down
5. At least one of the switches C or D must be up
6. Either C is up or D is down
7. If D is up, then E must be up
8. If D is up, then E must be down

