# Accumulating error due to jitter

In real quantum hardware, control pulses are never perfectly shaped: each pulse may be slightly too strong, too weak, too wide, or too narrow due to unavoidable noise in the electronics. These small variations—often called jitter—may seem insignificant for a single pulse, but their effects accumulate when many pulses are applied in sequence. Because each pulse contributes a small rotation to the qubit's state, even tiny fluctuations in pulse height (amplitude) or pulse width can cause the total rotation angle to drift away from its intended value. Over many pulses, these small errors compound, gradually pushing the qubit off its ideal trajectory on the Bloch sphere and degrading the fidelity of the implemented quantum operation.

To model realistic imperfections in a laboratory setting, we introduce small random fluctuations—or jitter—into both the pulse width and the pulse amplitude. In this simulation, the pulse width 
$\sigma$ varies by about 0.5% reflecting typical timing uncertainties in experimental control hardware, while the pulse amplitude $\Omega0$ fluctuates by about 0.1%, representing small drifts in the strength of the applied field. These values capture realistic levels of noise seen in many quantum control systems and allow us to explore how even modest imperfections can accumulate over repeated pulses.

## Activity 6: Add statistical jitter
We can model these random variations with some simple code:
```python
# Statistical Jitter
sigma_jitter_us = sigma_us * .005         # .5% error units microsecond
sigma_jitter    = sigma_jitter_us * 1e-6  # convert to seconds

Omega0_jitter = Omega0 * .001             # 0.1% error units rad/s
```
We only need to update the `args` dictionary with argmented values. But, we need to keep track of the original values in `args`, so lets make two new variables:

```python
#build args list for each case
args_nominal = {"Omega0": Omega0, "sigma": sigma, "t0": t0}
args_jitter = draw_jittered_args()            
```
You are also going to have to make a helper function `draw_jittered_args()` that pulls new values of `Omega0` and `sigma` from normal distributions centered at 0, and with standard deviation given by `sigma_jitter` and `Omega0_jitter`.

Update your `for` loop to include a call to your new `draw_jittered_args()` function.  Then run your full code and produce both the plots of expectation values and Bloch Sphere.

***
After you made the above changes, and your code is working, here are some questions to consider:
- Increase `NumPulse = 50`, then look at the plots of expectation values.  What do you notice about these plots compared to the non-jittered plots?
- Try isolating each type of jitter individually (Omega0, sigma). Which one produces more drift in the expectation values?
- Look at your Bloch Sphere plot - in particular, look at the vectors near each axis (x, y, z).  It should look like the vectors are clustering around the axis, why?
***

## Activity 7: Divergence of measurement error due to jitter
To quantify how far a jittered state drifts from the ideal trajectory, we use a standard measure from quantum information theory called the trace distance. For two pure states $|\psi_{ideal}\rangle$ and $|\psi_{noisy}\rangle$, the trace distance simplifies to half the length of the difference between their Bloch vectors:
```math
D = \frac{1}{2}||\vec{B}_{ideal}-\vec{B}_{noisy}||
```
This quantity has a direct operational meaning: the trace distance is equal to the maximum possible difference in measurement outcomes between the ideal and noisy states. In other words, $D$ tells us the largest probability error that noise could introduce in any single-qubit measurement, making it a powerful tool for evaluating how control imperfections accumulate over multiple pulses.

Because we have the expectation values as part of our model, we can calculate the Bloch vector directly:

```math
\vec{B} = \left( <\sigma_x>, <\sigma_y>, <\sigma_z> \right)
```

The final step in our code will be to track the evolution of an ideal pulse and a noisy pulse over all `NumPulse`.  Lets add to our empty arrays that we initialize before our main loop.

```python
#initialize arrays  *** BE CAREFULE WITH DATATYPES IN QuTip
pulseNum = []             # list to store the index of pulses
times_all_ideal = None    # 1D numpy array; don't really need two here
times_all_noisy = None
states_all_ideal = []     # list of Qobj (specal QuTip datatype)
states_all_noisy = []
expect_all_ideal = None   # list of many numpy arrays
expect_all_noisy = None
blochvec_all_ideal = []   # list of many numpy arrays
blochvec_all_noisy = [] 
```
As well as split our initial state into an ideal and noisy Qobj to track.

```python
psi0 = (basis(2, 0) + basis(2, 1)).unit()
psi_ideal = psi0      #split into two vectors to track
psi_noisy = psi0
```

You will also need to make two new helper functions.  The first is `getBlochVector()` that takes in the last state of `result.states[-1]` and outputs a numpy array. The second is `trace_distance()` that takes in two vectors give by `getBlochVector()` and outputs the Bloch distance.

Update your main `for` loop to keep track of both an ideal and nosiy evolution.  Only calculate the Bloch vector and trace distance for the last state after each pulse, not all states over the duration of the pulse.

Change your output plots:
- plot only $<\sigma_z>$ vs. time for both the ideal and noisy states as functions of time.
- don't plot the Bloch sphere at all
- plot the Trace Distance vs. Pulse Number

***
THE ONLY THINGS YOU SHOULD TURN IN FOR THIS ASSIGNMENT:

**Upload to Schoology the follwing things**
- An exicutable version of your final code from Actiity 7
- Plot of $<\sigma_z>$ vs. Time showing both the ideal and noisy evolution
- Plot of the Trace Distance vs. Pulse Number
- Answers to the following questions:
  -- example

***
