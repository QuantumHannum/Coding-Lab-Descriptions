# Part 2: Visualizing Entanglement

## Activity 5
### Goals
- Go beyond "how much entanglement is there?" and ask - **Where does the entanglement live** inside the system.

### Specific Tasks
1.  For an N-qubit system, we will build an $N x N$ matrix M where:
   - Diagonal entries will be $M[i,i]=S(\rho_i)
   - Off-diagonal entries will be $M[i,j] = S(\rho_{ij})
   - We will plot this matrix as a color map


To generate this matrix $M$, we can define a function.

```python
def entropy_map_2body(rho,N):
  M = np.zeros((N,N))
  for i in range (N):
      # Calculate the diagonal elements of M

  for i in range (N):
      # Calculate the off-diagonal elements of M

         M[i, j] = vn_entropy_subset(rho, [i, j])
  return M
```
What does \( S(\rho_{ij}) \) mean?

> The **von Neumann entropy of the two-qubit subsystem consisting of qubits i and j**.

More explicitly:

1. You start with the full N-qubit density matrix $\rho$.
2. You keep only qubits i and j and trace out (discard) all the other qubits.
3. You obtain a two-qubit reduced density matrix:

```math
   \rho_{ij} = Tr_{\text{all except}-i,j}(\rho)
```
   
5. Then you compute:

```math
   S(\rho_{ij}) = -Tr(\rho_{ij} \log_2 \rho_{ij})
```

What does this mean physically?  $S(\rho_{ij})$ measures how entangled the pair ${i, j}$ is with the rest of the system***.
- It does **NOT** directly measure how entangled qubit $i$ is with qubit $j$.
- Instead, it tells you:

> “If I treat qubits i and j together as a *single subsystem*, how much information is shared between that pair and everything else?”

So in the heatmap:

- The **diagonal entries** tell you:
  > How entangled each *single qubit* is with the rest of the system.

- The **off-diagonal entries** tell you:
  > How entangled each *pair of qubits, treated together*, is with the rest of the system.

Here is some code to produce the heatmap once you have the matrix $M$

```python
def plot_entropy_heatmap(M, title):
    plt.figure(figsize=(5,5))
    im = plt.imshow(M, interpolation="nearest")
    plt.colorbar(im, label="Entropy (bits)")
    plt.xticks(range(M.shape[0]))
    plt.yticks(range(M.shape[0]))
    plt.title(title)
    plt.tight_layout()
    plt.colorbar()
    plt.show()
```
---
Making use of the above code, here are some questions to consider:
1.  For the number of qubits N, with $3 \le N \le 8$, generate:
   - A simple product state $|\psi\rangle = |000..0\rangle
   - A $GHZ_N$ state
   - A $W_N$ state

   For each value of N, Plot heatmaps for the generated states.

2. Which state shows no entanglement structure? Which state shows the global structure? Which state shows a distributed entanglement structure?
3. What do these heatmaps say about where the entanglement lives in these systems?

---
## Activity 6: Bipartitions and many-body entanglement structure
### Goals
- Study how entanglement is distributed across different-sized subsystems.

### Specific Tasks
1.  For $N=8$ and for $k=1,2,3,..7$, compute
   - the entrooy of all k-qubit subsets.
   - the average of these entropies.

So far, you have looked at:

- Single qubits entangled with the rest of the system
- Pairs of qubits entangled with the rest of the system (via heatmaps)

Now we take the next and most important conceptual step:

> Instead of asking about **one qubit** or **two qubits**, we will ask about **arbitrary-sized chunks of the system**.

This is how physicists think about **many-body entanglement**.

A **bipartition** means splitting the full system into two parts:

```math
\text{(k qubits)} \; | \; \text{(N - k qubits)}
```

For example, in an 8-qubit system:

- k = 1 means: 1 qubit vs 7 qubits  
- k = 2 means: 2 qubits vs 6 qubits  
- k = 4 means: 4 qubits vs 4 qubits  

For each such split, we can:

1. Keep the k-qubit subsystem
2. Trace out the other N−k qubits
3. Compute the entropy $S(\rho_k)$

This entropy tells us:

> **How entangled that k-qubit block is with the rest of the system.**

Why is this a powerful idea?

This lets us answer questions like:

- Is the entanglement mostly:
  - concentrated in small subsystems?
  - or spread across large chunks of the system?

- Does the system behave like:
  - a few particles strongly entangled with the rest?
  - or a genuinely collective, many-body entangled object?

In many-body physics, this kind of analysis is how people distinguish between:
- simple few-body entanglement
- and **true many-body entanglement**

We only need a small addition to our code to calculate the bipartite entropies.

```python
def avg_entropy_for_k(rho, N, k):
    entropies = []
    for keep in combinations(range(N), k):
        entropies.append(vn_entropy_subset(rho, list(keep)))
    return np.mean(entropies)
```

Use this code to produce a plot of Average Entropy vs. k for both $GHZ_8$ and $W_8$ states.

---
Making use of the above code, here are some questions to consider:
1.  Describe the overall shape of each curve. At what value(s) of $k$ is the entropy largest? Where is it the smallest?
2.  Do the patterns you saw in the heatmaps help explain the shape of the average entropy vs. k curves?
3.  For each state, is entanglement stored in a single global correlation, or is it spread throughout the system in overlapping pieces?
