# Part 1: Building N-qubit states


## Activity 1
### Goals
Learn how to:
- Construct multi-qubit Hilbert spaces
- Build simple product states
- See how the dimension of the state space scales as $2^N$

### Specific Tasks
1. Define the single-qubit basis states
2. Write a function that builds the N-qubit state $|00...0\rangle$
3. Test your function for various values of N

We first need to define our basic kets $|0\rangle$ (i.e. spin up) and $|1\rangle$ (i.e. spin down).  To me this always seems backwards, but this was decided long before me.

We can do this easily in QuTip

```python
zero = basis(2,0)
one = basis(2,1)
```
It is then very easy to construct the tensor product states with a simple function

```python
def zero_state(N):
  return tensor([zero]*N)
```
This will produce the product state $|00...0\rangle$ where there are N zeros.

---
Making use of the above code, here are some questions to consider:
1. How does the dimension of the product state vector scale with N?  Use your code to confirm that they scale as $2^N$.  As a suggestion, use a print statement like
   ``` python
   print(zero_state(N).shape)
   ```
for different values of N.

---
## Activity 2: Density matrices, partial trace, purity, and entropy

### Goals
Understand the difference between:
- A pure global state
- A reduced (subsystem) state
- How mixedness is detected using purity and entropy

### Specific Tasks
1.  Convert a pure N-qubit state into a density matrix.
2.  Use `ptrace' to extract the partial trace of one qubit from a N-qubit product state.
3.  Compute the `purity' and von Neumann `entropy' of a reduced state.
4.  Explore purity and entropy for different values of N

