# Part 3: Single SAT Problem
When studying the basic quantum algorithms (Deutsch-Jozsa, Simon, Grover, etc) they all start to look a bit contrived.  They all make use of oracles, but to generate the oracle you need to "know the answer" already.  In the case of Deutsch-Jozsa - is the function constant or balanced, with Simon's - you need to know the secret key, and as we have seen with Grover's you need to know the state you are looking for, and then find it.  Its time to look at an example of Grover's algorithm where we use an oracle to confirm (or discover) a solution we don't know before we start the problem.

---
## Activity 3: Booliean Satisfiablity Problems with Grover's

A Boolean Satisfiability Problem (SAT) asks whether there exists an assignment of truth values (True/False or 1/0) to a set of Boolean variables that makes a logical formula evaluate to True. Typically, the formula is written in Conjunctive Normal Form (CNF) — an AND of clauses, where each clause is an OR of literals (variables or their negations). For example:
```math
(x_1 \lor \lnot x_2) \land (x_2 \lor x_3)
```
Where $\lor$ is the symbol for `OR` and $\and$ is the symbol for `AND`.  The task is to determine whether there exists a bitstring $x=(x_1,x_2,...)$ that satisfies all clauses simultaneously.

Grover’s algorithm can be used to search for satisfying assignments by constructing a phase oracle that encodes the Boolean function:
```math
|x\rangle \rightarrow (-1)^{f(x)}|x\rangle
```
where $f(x)=1$ if the assignment satisfies the SAT formula and $f(x)=0$ otherwise. By placing all possible assignments into superposition and applying Grover’s amplitude amplification, the algorithm increases the probability of measuring a satisfying assignment.

### Conceptual Goals
