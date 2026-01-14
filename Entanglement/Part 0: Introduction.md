#Part 0: Introduction to Entanglement in Many-Particle Quantum Systems

## Big Idea

In this lab you will explore **how entanglement scales and spreads** in quantum systems with many particles. You will discover that:

- A global quantum state can be **perfectly pure** while small subsystems look **mixed**
- Entanglement can be **fragile** (GHZ states) or **distributed** (W states)
- The **structure of entanglement changes with system size**
- Entropy and purity let us **map entanglement across many different partitions**

You will build and analyze systems from **2 up to 8 qubits**.

This lab is organized in **tiers**.  
Each tier has a **goal**, a **task**, and **things you must record**.

---

## What you will learn to do

- Build N-qubit states using `tensor`
- Convert kets to density matrices using `ket2dm`
- Compute reduced density matrices using `ptrace` (partial trace)
- Compute **purity** and **von Neumann entropy**
- Visualize the **internal structure of entanglement**
- Study how entanglement depends on **system size** and **subsystem size**
- Compare different *types* of multipartite entanglement (GHZ vs W)

---

## Setup
First install qutip
```python
pip install qutip
```
Then set up a specific environment
```python
import numpy as np
import matplotlib.pyplot as plt
from itertools import combinations

from qutip import (
    basis, tensor, qeye, ket2dm, ptrace,
    entropy_vn, purity
)

np.set_printoptions(precision=4, suppress=True)

