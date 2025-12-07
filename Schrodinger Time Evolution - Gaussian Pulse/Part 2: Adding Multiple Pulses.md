# Combining Multiple Gaussian Pulses

In quantum computing, gates are implemented by applying controlled rotations to a qubit, and these rotations are often constructed by combining several pulses in sequence. By stringing multiple pulses together, we can build more complex operations than a single pulse can provide—for example, performing a rotation about an arbitrary axis, decomposing a gate into simpler components, or creating sequences that move the qubit around the Bloch sphere in precise and intentional ways. Exploring how pulses add together to produce cumulative rotations helps reveal how quantum gates are engineered in practice and how control sequences shape the evolution of a quantum state.

## Activity 4: String pulses together
Lets alter your basic code from Part 1 to accomodate multiple pulses to the initial state.

Add in some empty lists
```python
#Number of pulses
NumPulse = 3

#initialize arrays  *** BE CAREFULE WITH DATATYPES IN QuTip
pulseNum = []             # list to store the index of pulses
times_all = None    # 1D numpy array; don't really need two here
states_all = []     # list of Qobj (specal QuTip datatype)
expect_all = None   # list of many numpy arrays
```
Then make a for loop to loop NumPulse times using a general structure:

```python
for k in range(NumPulse):
    pulseNum.append(k+1)                      #get index of loop

    H = [[H_axis,Omega]]                      #build Hamiltonian - not evaluated until sesolve()

    # Solve Schrodinger Equation for current (k) pulse

    # Set final state of sesolve() to be intial state for next loop

    #concatenation data from each pulse into big lists
```

Because of the datastructures that QuTip uses, here is a nice helper function to perform the concatenation:

```python
# Concatinates time, state, and expectation value lists after each pulse
def concat_data(result, times_all, states_all, expect_all):
    times_k = np.array(result.times)      # convert result.times to numpy array for t_offset

    # check for first pulse empty data
    if times_all is None:
        # first pulse: take everything as-is
        times_all  = times_k
        states_all = list(result.states)
        expect_all = [np.array(e) for e in result.expect]

    # for all pulses after first pulse
    else:
        # shift times of this pulse so it follows previous pulse
        t_offset  = times_all[-1]
        new_times = times_k[1:] + t_offset  # skip first to avoid duplicate

        # concatenate times
        times_all = np.concatenate([times_all, new_times])

        # concatenate states (skip first to avoid duplicate)
        states_all.extend(result.states[1:])

        # concatenate expectation values (each expect[j] is already an array)
        for j in range(len(expect_all)):
            expect_all[j] = np.concatenate(
                [expect_all[j], np.array(result.expect[j][1:])]
            )
    return times_all, states_all, expect_all
```
