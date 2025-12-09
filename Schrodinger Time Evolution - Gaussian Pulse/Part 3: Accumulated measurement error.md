# Accumulating error due to jitter

In real quantum hardware, control pulses are never perfectly shaped: each pulse may be slightly too strong, too weak, too wide, or too narrow due to unavoidable noise in the electronics. These small variations—often called jitter—may seem insignificant for a single pulse, but their effects accumulate when many pulses are applied in sequence. Because each pulse contributes a small rotation to the qubit's state, even tiny fluctuations in pulse height (amplitude) or pulse width can cause the total rotation angle to drift away from its intended value. Over many pulses, these small errors compound, gradually pushing the qubit off its ideal trajectory on the Bloch sphere and degrading the fidelity of the implemented quantum operation.

To model realistic imperfections in a laboratory setting, we introduce small random fluctuations—or jitter—into both the pulse width and the pulse amplitude. In this simulation, the pulse width 
$\sigma$ varies by about 0.5% reflecting typical timing uncertainties in experimental control hardware, while the pulse amplitude $\Omega0$ fluctuates by about 0.1%, representing small drifts in the strength of the applied field. These values capture realistic levels of noise seen in many quantum control systems and allow us to explore how even modest imperfections can accumulate over repeated pulses.

## 
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
You are also going to hvae to make a helper function `draw_jittered_args()` that pulls new values of `Omega0` and `sigma` from normal distributions centered at 0, and with standard deviation given by `sigma_jitter` and `Omega0_jitter`.

Update your `for` loop to include a call to your new `draw_jittered_args()` function.  Then run your full code and produce both the plots of expectation values and Bloch Sphere.

***
After you made the above changes, and your code is working, here are some questions to consider:
- Increase `NumPulse = 50`, then look at the plots of expectation values.  What do you notice about these plots compared to the non-jittered plots?
- Try isolating each type of jitter individually (Omega0, sigma). Which one produces more drift in the expectation values?
- Look at your Bloch Sphere plot - in particular, look at the vectors near each axis (x, y, z).  It should look like the vectors are clustering around the axis, why?
***


