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
1\quad -2\quad 3
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
---
#### Checkpoint
After you complete the two functions above, run:
```python
clauses = PreCalc(3, 6)
generate_dimacs_cnf(10, clauses, 6, "test.cnf")
```
Open `test.cnf` and verify:
  * Heder matches: **p cnf 6 10**
  * There are 10 lines of 3 integers each + zero
  * all integers range between -6...6
  * No variable repeats within a clause
  * No clause contains a tautology.
---
You generated random 3-SAT instances and saved them as DIMACS CNF files. In this part, we load the CNF file and let Qiskit build the Boolean expression and phase oracle automatically. The core function you’ll use is `find_sat(...)`: it (1) reads the CNF from disk, (2) converts it to a Boolean expression, (3) turns that into a PhaseOracleGate that marks satisfying assignments, (4) runs Grover with an “optimal” iteration count based on an assumed number of solutions, and (5) returns both the measured bitstring and the oracle depth (a useful diagnostic for “how complex” the oracle circuit is). This is the key bridge between “SAT instance as a text file” and “Grover running on a quantum circuit.”

**You do not need to modify this code at all** but you will use it later in larger code.

```python
# ------------------------------------------------
# Qiskit imports
# ------------------------------------------------
from qiskit_algorithms import AmplificationProblem, Grover
from qiskit.primitives import StatevectorSampler
from qiskit.circuit.library.phase_oracle import PhaseOracleGate

# BooleanExpression import path can vary across Qiskit versions
try:
    from qiskit.synthesis.boolean.boolean_expression import BooleanExpression
except Exception:
    from qiskit.circuit.classicalfunction.boolean_expression import BooleanExpression

def find_sat(cnf_path: Path, numSols: int, num_vars: int):
    bexpr = BooleanExpression.from_dimacs_file(str(cnf_path))
    oracle_gate = PhaseOracleGate(bexpr.expression)

    try:
        depth = oracle_gate.definition.depth()
    except Exception:
        depth = oracle_gate.decompose().depth()

    problem = AmplificationProblem(oracle_gate)

    sampler = StatevectorSampler()

    iterations = Grover.optimal_num_iterations(
        num_solutions=numSols,
        num_qubits=num_vars,   # <- key fix: Grover iterations depend on the search-space qubits (variables)
    )

    grover = Grover(sampler=sampler, iterations=iterations)
    result = grover.amplify(problem)

    return result.top_measurement, depth
```
---
#### Checkpoint
After you complete the two functions above, run:
```python
num_vars, clauses = read_dimacs("test.cnf")
candidate, depth = find_sat("test.cnf", numSols=1, num_vars=num_vars)

print("Candidate:", candidate)
print("Depth:", depth)
print("Satisfies?", assignment_satisfies(candidate, clauses))
```
  * If `Satisfied` is `True`, Govers found a satisfying assignment 
  * If `False`, either the instance is UNSAT or teh assumed `numSol was wrong (we handle that next in Part).
  * If `False` generate a new test.cnf and try again

---
#### Design an 3-SAT experiment

In this part, we move from solving a single CNF instance to modeling a **statistical experiment**. For a fixed number of variables $n$ we generate a random 3-SAT formula with $m$ clauses, attempt to find a satisfying assignment using Grover’s algorithm, and record whether a solution was found. We then repeat this process many times for the same clause count $m$ to estimate the probability that a random instance of that size is satisfiable. After collecting statistics for that value of $m$, we increase the clause count slightly and repeat the entire procedure. By sweeping $m$ from small to large values, we experimentally observe how the likelihood of satisfiability changes — revealing the sharp SAT $\rightarrow$ UNSAT phase transition as the clause density $\alpha=m/n$ increases.

##### Experimental Structure
|Step|Process|
|:--:|:----|
|1|Fix the nummber of variables $n$|
|2| Choose a claus count $m$|
|3| Generate many independent random CNF instances of size $m$|
|4| For each instance, attempt to find a satisfying assignment using Grover|
|5| Record whether a solution was found and other interesting stats|
|6| Estimate P(SAT) $\rightarrow$ Probablity of satisfyablity for variable $m$|
|7| Repeaat for larger values of $m$|

Step 4 above has a nuance to it.  Because we do not know in advance how many satisfying assignments a random formula may have (if any), we incorporate a retry strategy that incrementally increases the assumed number of solutions for Grover’s iteration count. This practical detail becomes especially important as we move toward and beyond the phase transition region.

Alter your `find_sat()` function to perform the described experiment.  Look for the commented areas in the code to alter/add.

```python
def find_success_prob(m: int, full_set, num_vars: int, trials: int, max_retries: int):
    # -------------------------------------------------------
    # Initialize experiment statistics
    # -------------------------------------------------------
    successful = 0              # number of SAT instances detected
    time_data = []              # optional: runtime diagnostics
    oracle_depth = []           # optional: oracle complexity diagnostics

    pid = os.getpid()

    # -------------------------------------------------------
    # Loop over independent random CNF trials
    # -------------------------------------------------------
    for trial in range(trials):

        # ---------------------------------------------------
        # YOUR TURN: Generate a fresh random CNF instance
        # ---------------------------------------------------
        cnf_path = CNF_DIR / f"tempCNF_m{m}_pid{pid}_t{trial}.cnf"
        
        generate_dimacs_cnf(....)
   

        # ---------------------------------------------------
        # Attempt Grover solve with a retry strategy, looking for multiple solutions
        # ---------------------------------------------------
        state = False
        retries = 1

        while (state is False) and (retries <= max_retries):

            state, t_ms, depth = find_sat(
                cnf_path,
                retries,              # assumed number of solutions
                num_vars=num_vars
            )

            retries += 1

        # ---------------------------------------------------
        # YOUR TURN: Record whether this instance was SAT
        # ---------------------------------------------------
        if ............:
            successful += 1
        time_data.append(t_ms)
        oracle_depth.append(depth)

        # ---------------------------------------------------
        # Clean up temporary CNF file
        # ---------------------------------------------------
        try:
            cnf_path.unlink(missing_ok=True)
        except Exception:
            pass

    # -------------------------------------------------------
    # YOUR TURN: Compute the empirical SAT probability for this m
    # -------------------------------------------------------
    prob = ...........

    print(f"m = {m}  |  SAT fraction = {prob:.3f}", flush=True)

    return prob, m, successful, trials, time_data, oracle_depth
```
