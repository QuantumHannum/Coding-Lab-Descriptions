# Part 1: Spatial Dependencies

## Activity 2
The infinite square well produced an exact equation for the eigenenergies:

```math
E_n=\frac{n^2\pi^2\hbar^2}{2mL^2}
```
As we discovered in the introduction, there is no closed-form equation for the eigenenergies for the finite square well; however, we can still explore some functional dependencies.

---
What you need to turn in for this activity
1. Using the parameters provided in activity 1, produce a plot of the bound state eigenenergies vs. quantum number n.  Fit a curve of the form $E = Ax^B$ using ``scipy.optimize.curve_fit()``.  How close is this fit to the infinite square well prediction?  Produce a second and third, similar plot where you double, then triple the value of $V_0$.  What do these other plots indicate about the functional dependence of the eigenenergies, quantum number, and potential height?
2. Using the parameters provided in activity 1, produce a plot of the ground state energy as you vary the size of the well $2a \in \[1.2, 10\]$
