# Part 1: Basic Code

We will be making use of a phython package that has been specifically designed to simulate quantum mechancis.  [QuTip](https://qutip.org) is a very powerful package that manages the complex(number) and matrix mathematics in a simple way.  The first thing you need to do is to install it into your environment:

```python
pip install qutip
```
Once you have done that, copy the rest of this code to simulate the first time evolution:

```python
import numpy as np
import matplotlib.pyplot as plt
import random

from qutip import (
    expect, basis, sigmax, sigmay, sigmaz,
    sesolve, Bloch, Options)

# ============================================================
# 1. Parameters
#    Work numerically with ħ = 1; H has units of rad/s.
# ============================================================

Omega0 = 2 * np.pi * 10e3   # 2π × 10 kHz ~ 6.283e4 rad/s

# Sigma in microseconds:
sigma_us = 10.0             # 10 microseconds
sigma    = sigma_us * 1e-6  # convert to seconds

# ============================================================
# 2. Time grid: positive-only times 0 → 10σ, pulse centered at 5σ
# ============================================================

t0 = 5 * sigma          # center the Gaussian at 5σ (in seconds)
t_start = 0.0
t_end   = 10 * sigma    # window [0, 10σ] in seconds

tlist    = np.linspace(t_start, t_end, 20)   # seconds
tlist_us = tlist * 1e6                        # microseconds for plotting

args = {"Omega0": Omega0, "sigma": sigma, "t0": t0}

# ============================================================
# 3. Time-dependent Hamiltonian H(t) = Omega_z(t) * Sz
# ============================================================

def Omega(t, args):
    """Z-rotation rate Omega_z(t) in rad/s (Gaussian pulse)."""
    Omega0 = args["Omega0"]
    sigma  = args["sigma"]
    t0     = args["t0"]
    return Omega0 * np.exp(-(t - t0)**2 / (2 * sigma**2))

# Spin operators
Sx = 0.5 * sigmax()
Sy = 0.5 * sigmay()
Sz = 0.5 * sigmaz()

# In QuTiP units, with ħ = 1, a Hamiltonian in rad/s is fine:
# H(t) = Omega(t) * Sz
H = [[Sz, Omega]]

# ============================================================
# 4. Initial state and time evolution
#    Start in |+x> = (|0> + |1>)/sqrt(2).
# ============================================================

psi0 = (basis(2, 0) + basis(2, 1)).unit()

# Track expectations of sigma_x, sigma_y, sigma_z
e_ops = [2*Sx, 2*Sy, 2*Sz]  # gives <σx>, <σy>, <σz>

# Force QuTiP to store the states as well as expectations
opts = Options(store_states=True)

result = sesolve(H, psi0, tlist, e_ops=e_ops, args=args, options=opts)
sx_t, sy_t, sz_t = result.expect

# Optional: plot expectation values vs time in microseconds
plt.figure()
plt.plot(tlist_us, sx_t, label=r'$\langle \sigma_x \rangle$')
plt.plot(tlist_us, sy_t, label=r'$\langle \sigma_y \rangle$')
plt.plot(tlist_us, sz_t, label=r'$\langle \sigma_z \rangle$')
plt.xlabel('time (µs)')
plt.ylabel('expectation value')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# ============================================================
# 5. Bloch sphere trajectory
# ============================================================

b = Bloch()
b.add_states(result.states[1:400])
b.show()
```
The key line in the above code is:
```python
result = sesolve(H, psi0, tlist, e_ops=e_ops, args=args, options=opts)
```
This is the line where QuTip solves the time-dependent Schrodinger equation using the Hamiltonian defined in the line:

```python
H = [[Sz, Omega]]
```
One **VERY** important feature of QuTip is that the Hamiltonian isn't actually calculated (using the definition above) until it is used inside the **sesolve()** line based on the values contained in **args**.  So you can change the values in **args** and it doesn't require recalculating the Hamiltonian.

It is also worth noting that the output of the *sesolve()*, **result** has two parts:
- **result.state** which is a list of Qobj containing the quantum state vectors after each time step given in *tlist*
- **result.expect** which is a list of numpy arrays, with each array containing the expectation value ($S_x$, $S_y$, $S_z$) at each time step in *tlist* 

***
As you complete the following activities, take time to look at the Bloch sphere output and the plots of the expextation values.  It is worth your time to make sure you understand how the changes you make to the code are reflected in the two outputs of the code.
***

## Activity 1: Change initial State
Change the code so that the inital state of the system is in the following:
- $|\psi_0\rangle = |+z\rangle$
- $|\psi_0\rangle = \frac{1}{\sqrt{2}}\left[|+z\rangle-|-z\rangle \right]$
- $|\psi_0\rangle = \frac{1}{\sqrt{2}}\left[|+z\rangle+i|-z\rangle \right]$ Note: In python imaginary number "i" is must be written as $1j$

## Activity 2: Change rotation axis
Change the code so that the rotation axis isn't just about the $z$-axis.

## Activity 3: Change the pulse duration and strength
Put the code back to rotations about the $z$-axis with initial state $|\psi_0\rangle = \frac{1}{\sqrt{2}}\left[|+z\rangle+|-z\rangle \right]$

Then change the duration of the Gaussian pulse by changing the width of the pulse:
- 5 $\mu s$
- 20 $\mu s$
- What pulse width is necessary to perform a full $2\pi$ revolution of the state? Confirm your answer with your code.

Return the pulse width back to 10 $\mu s$.  Then change the maximum strength of the pulse by altering the value of $Omega0$
- What value of $Omega0$ is necessary to induce a full $\pi$ rotation?
- What value of $Omega0$ is necessary to induce a full $4\pi$ rotation?

