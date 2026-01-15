# Part 2: Visualizing Entanglement

## Activity 5
### Goals
- Go beyond "how much entanglement is there?" and ask - **Where does the entanglement live** inside the system.

### Specific Tasks
1.  For an N-qubit system, we will build an $N x N$ matrix M where:
   - Diagonal entries will be $M[i,i]=S(\rho_i)
   - Off-diagonal entries will be $M[i,j] = S(\rho_{ij})
   - We will plot this matrix as a color map


To generate this matrix $M$ we can define a function

```python
def entropy_map_2body(rho,N):
  M = np.zeros((N,N))
  for i in range (N):
      # Calculate the diagonal elements of M
  for i in range (N):
      # Calculate the off-diagonal elements of M

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
    plt.show()

```
---

