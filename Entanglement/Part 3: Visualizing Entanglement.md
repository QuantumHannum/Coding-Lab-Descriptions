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
