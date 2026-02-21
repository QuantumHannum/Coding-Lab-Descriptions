# Part 4: 3-SAT Problem
The 3-SAT problem asks whether a Boolean formula written in 3-conjunctive normal form (3-CNF) has at least one satisfying assignment. Each clause contains exactly three literals (variables or their negations), and the full formula is an AND of many such clauses. While checking a single assignment is easy, determining whether any satisfying assignment exists is computationally hard (NP-complete). Remarkably, when we generate random 3-SAT instances with $n$ variables and $m$ clauses, their behavior changes sharply as we vary the clause density $\alpha = m/n$.  For small $\alpha$, instances are almost always satisfiable (SAT), but for large $\alpha$, they are almost always unsatisfiable UNSAT).  Near a critical threshold $T$, the probabilty of satisfiablity drops abruptly - a phenomenon known as **computational phase transition**.  In this final activity, you will experimentally explore this transition using Grover's algorithm as a quantum search subroutine applied to randomly generated 3-SAT instances.

## Activity 4: 3-SAT and UNSAT

### Conceptual Goals
1. Define a 3)CNF formula and interpret clauses written in DIMACS CNF format.
2. Describe what a SAT-UNSAT phase transition means in probabilistic terms.
3. Connect Boolean satisfiability to a phase oracle in Grover's algorithm.
4. Using computational data, explore which instances are the hardest computationally.

### Technical Goals
1. Generate random valid 3-SAT clauses without tautologies or repeated variables.
2. Construct full DIMACS CNF files programmatically.
3. Convert CNF formulas into Qiskit Boolean expressions for phase-oracle construction
4. Apply Gorver's algorithm to search for satisfying assignments.
5. Run parameter sweeps over clause count $m$ and compute empirical success probabilities.

---
#### From logical clauses to random 3-SAT instances
To explore the transition from SAT to UNSAT, we need to generate many Grover Oracles with clauses of different lengths encoded.  In the previous section, you manually translated logical constraints into a phase oracle. While that process builds deep understanding, it quickly becomes laborious as the number of variables and clauses grows. For large random 3-SAT instances, hand-building Boolean expressions is impractical.

Instead, we will use a standard encoding format called DIMACS CNF, which allows us to automatically generate Boolean formulas and feed them directly into a phase oracle. This shifts our focus from manually constructing logic to studying statistical structure — specifically, how satisfiability changes as we vary the clause density $\alpha =m/n$.

In this section, you will build the machinery that generates ensembles of random 3-SAT instances.  A 3-SAT clause looks like $(x_i \lor x_j \lor x_k)$ where the three variables can also appear negated. For example:
```math
(1 \lor \lnot 2 \lor 3)
``` 
can be encoded more compactly as:
```math
1 -2 \: 3
``` 
where these numbers have a space between them.  To begin a 3-SAT simulation, we need to create all possible clauses composed of 3 literals, including negations, but excluding repeated variables and tautologies.  For example, a repeated variable error would look like $(1\or 1\lor \lnot 3)$ and a tautological error would be $(4 lor \lnot 4 \or 6)$.

Define a function `PreCalc(k, num_vars)` that makes k literal clauses using variables -num_vars - num_vars.

``` python
import itertools

def PreCalc(k, num_vars):
    """
    Generate all valid k-literal clauses over variables ±1..±num_vars.
    Excludes repeated variables and tautologies.
    """

    literals = list(range(-num_vars, 0)) + list(range(1, num_vars + 1))
    clauses = []

    for combo in itertools.combinations(literals, k):
        # Exclude repeated variables (same absolute index)
        if len({abs(x) for x in combo}) == k:
            clauses.append(combo)

    return clauses
```
We now need to put all of these 3-literal clauses into a `.cnf` file, which is a **Conjuctivie Normal Form** which we can automatically convert into a Phase Oracle.  A CNF File has a very particular structure.
```python
p cnf <num_vars> <num_clauses>
a b c 0
d e f 0
...
```
It must have the header shown "p cnf <num_vars> <num_clauses>" followed by a list of logical clauses with a trailing 0 at the end of each line.

Complete the following code to generate DIMACS CNF files that make use of your previous `PreCalc` output as input **all_possible**.
```python
import random

def generate_dimacs_cnf(m, all_possible, num_vars, file_path):
    """
    Generate a random 3-SAT instance with m clauses.
    Write it to a DIMACS CNF file at file_path.
    """

    chosen = random.sample(all_possible, m)

    with open(file_path, "w") as f:
        f.write(f"p cnf {num_vars} {m}\n")

        for clause in chosen:
            line = " ".join(str(lit) for lit in clause)
            f.write(line + " 0\n")
```
#### Checkpoint
After you complete the two functions above, run:
```python
clauses = PreCalc(3, 6)
generate_dimacs_cnf(10, clauses, 6, "test.cnf")
```
Open `test.cnf` and verify:
  * Heder matches: **p cnf 6 10**
  * Each line ahs exactly 3 integers + a zero
  * No variable repeats within a clause
