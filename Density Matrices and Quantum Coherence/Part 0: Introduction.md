# Part 0: Introduction to Density Matrices and Quantum Coherence

## Big Idea

In this lab, you will investigate how **statistical uncertainty about the relative phase of a spin-½ state changes the predictions of quantum mechanics**. You have already used kets, projection operators, the Bloch sphere, and the Born rule. Density matrices let you describe both pure states and ensembles that cannot be represented by one ket.

You will construct ensembles in QuTiP, visualize them on the Bloch sphere, compute measurement probabilities, and apply your findings to an idealized Stern–Gerlach interferometer. The emphasis is on **predicting, testing, and explaining physical behavior** rather than writing large amounts of code.

## Learning Goals

- Construct a pure-state density matrix as an outer product and a mixed-state density matrix as a weighted sum of projectors.
- Explain how averaging different relative phases changes off-diagonal matrix elements while leaving some measurement populations unchanged.
- Relate a spin states density matrix to its Bloch vector and purity.
- Apply the density-matrix Born rule to predict spin measurement outcomes.
- Relate the magnitude of coherence to interference visibility and distinguish statistical phase averaging from which-path entanglement.

## Theoretical Background

### Pure states and projectors

For a spin-½ system, work in the $z$ basis $\{|+z\rangle,|-z\rangle\}$. A general pure state is

```math
|\psi\rangle=\cos\frac{\theta}{2}|+z\rangle+e^{i\phi}\sin\frac{\theta}{2}|-z\rangle.
```

Its density operator is the rank-one projector $\hat\rho=|\psi\rangle\langle\psi|$. In the $z$ basis,

```math
\rho=\begin{pmatrix}
\cos^2(\theta/2)&\tfrac12\sin\theta\,e^{-i\phi}\\
\tfrac12\sin\theta\,e^{i\phi}&\sin^2(\theta/2)
\end{pmatrix}.
```

The diagonal entries give $P(+z)$ and $P(-z)$. The off-diagonal entries encode coherence in the $z$ basis, which affects measurements along other directions. Changing a ket's global phase does not change its projector.

### Statistical ensembles

If state $|\psi_k\rangle$ is prepared with probability $p_k$, the ensemble density operator is

```math
\hat\rho_{\mathrm{ensemble}}=\sum_k p_k|\psi_k\rangle\langle\psi_k|,\qquad \sum_k p_k=1.
```

We will fix $\theta=\pi/2$ initially and let the relative phase vary:

```math
|\psi(\phi)\rangle=\frac{1}{\sqrt2}\big(|+z\rangle+e^{i\phi}|-z\rangle\big).
```

Generate $N$ phases $\phi_k$ and average their pure-state density matrices:

```math
\rho_N=\frac1N\sum_{k=1}^N|\psi(\phi_k)\rangle\langle\psi(\phi_k)|.
```

If phases are uniformly distributed over $[-\Delta,\Delta]$, the *large-ensemble* prediction is

```math
\rho(\Delta)=\frac12\begin{pmatrix}1&\gamma\\\gamma&1\end{pmatrix},\qquad
\gamma=\begin{cases}\dfrac{\sin\Delta}{\Delta},&\Delta\ne0\\1,&\Delta=0.\end{cases}
```

Here the phase distribution is centered at zero, so $\gamma$ is real. For a finite random sample, small complex off-diagonal fluctuations are normal. We will compare numerical results against the analytical prediction rather than assume they must agree exactly.

### Purity, measurement, and interference

A valid density matrix is Hermitian, positive semidefinite, and has trace 1. Its **purity** is

$$
\mathcal{P} = \mathrm{Tr}(\rho^2)
$$

For one spin state, $1/2\le\mathcal P\le1$. A pure state has purity 1. For a projective spin measurement,

$$
P(+j) = \mathrm{Tr}(\rho P_{+j}), \qquad j=x,y,z
$$

The same mathematical coherence that changes $P(+x)$ also controls interference in a suitably recombined Stern–Gerlach interferometer.

---

## Coding Environment Setup

You may use **Google Colab**, the school's **OnDemand Jupyter system**, or **VS Code**. Choose *one* environment; you do not need all three. All activities use Python, NumPy, Matplotlib, and QuTiP. Internet access may be needed to install packages initially. If the school's environment restricts installs, ask the instructor to provide or enable QuTiP.

### Option A: Google Colab

1. Open [Google Colab](https://colab.research.google.com/) and create a **New notebook**.
2. Rename it `Density_Matrices_Quantum_Coherence.ipynb`.
3. In the **first code cell**, run the following, then run the common import cell below.

```python
%pip install -q qutip
```

4. If importing QuTiP fails immediately after installation, restart the session/runtime and rerun the cells.
5. Save your notebook to Drive, or download the `.ipynb` file for submission. A Colab runtime is temporary: rerun the setup cell in a fresh session.

### Option B: School OnDemand Jupyter Notebook

1. Sign in to the school's OnDemand portal using your assigned credentials.
2. Launch a Jupyter notebook or JupyterLab session using an available Python kernel. Your school's launch screens and resource choices may differ; follow instructor or school documentation.
3. Create `Density_Matrices_Quantum_Coherence.ipynb` in your working directory.
4. Run this **in a notebook cell** if QuTiP is not already installed:

```python
%pip install qutip
```

5. Restart the kernel if necessary and run the import/test cell below. If installation is blocked, **do not repeatedly modify the shared environment**; contact the instructor or system administrator.
6. Keep your notebook in the persistent course/home directory specified by the school. Download a copy for submission if required.

### Option C: VS Code on your computer

1. Install Python 3 and [Visual Studio Code](https://code.visualstudio.com/).
2. Install Microsoft's **Python** and **Jupyter** extensions in VS Code.
3. Open a project folder and create a local environment in the VS Code terminal:

```bash
python -m venv .venv
```

4. Activate the environment:

**macOS/Linux:**
```bash
source .venv/bin/activate
```

**Windows PowerShell:**
```powershell
.venv\Scripts\Activate.ps1
```

5. Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install numpy matplotlib qutip ipykernel
```

6. Create a `.ipynb` notebook. Select the `.venv` Python kernel using **Select Kernel** in VS Code. You can use ordinary `.py` files if you prefer, but your final submission must include the figures and written analysis.

### Common import and verification cell (all environments)

```python
import numpy as np
import matplotlib.pyplot as plt
import qutip as qt

up = qt.basis(2, 0)      # |+z>
down = qt.basis(2, 1)    # |-z>

psi_test = (up + down).unit()
rho_test = qt.ket2dm(psi_test)
print('QuTiP:', qt.__version__)
print('Trace:', rho_test.tr())
print('Purity:', (rho_test * rho_test).tr().real)
assert np.isclose(rho_test.tr(), 1)
assert np.isclose((rho_test * rho_test).tr().real, 1)
```

A successful test prints trace approximately 1 and purity approximately 1.



### Using a Generative AI (LLM) as a Coding Assistant

You may use a Generative AI (GAI/LLM) to write, revise, explain, or debug code. However, **you must decide what physical quantities to calculate, verify the numerical results, and understand every figure you submit.** <ins>Using a GAI to write your answers to the physics analysis questions is not allowed.</ins> More importantly, doing so would undermine the purpose of this investigation.

As LLMs become increasingly capable of producing code, an important scientific skill is learning **which parts of a computational problem to delegate to an AI and which decisions must remain your own**. Your responsibility is to direct the investigation, evaluate the results, and draw conclusions from the physics—not simply obtain working code.

When asking an LLM for programming assistance, provide the relevant equations, QuTiP version, and intended variables. Ask it to build on your existing notebook rather than replacing earlier work. Before trusting a simulation, verify that the density matrices are Hermitian, have trace 1, and produce physically valid measurement probabilities. Compare numerical results with analytical predictions whenever possible.

#### AI Development Record

To document how you use AI as a scientific tool, you will maintain a short **AI Development Record** in your Jupyter notebook.

For each of the three activities, document **one meaningful interaction with an LLM**. Choose an interaction that illustrates how you used AI to develop, improve, debug, or validate your computational investigation. You do not need to submit your entire conversation history.

For each entry, include:

1. **Objective:** What were you trying to accomplish scientifically or computationally?
2. **Prompt:** Copy the actual prompt you submitted to the LLM.
3. **AI Contribution:** Briefly describe what the LLM produced or suggested.
4. **Verification:** Explain how you checked, tested, corrected, or modified the LLM's response.

If you complete an activity without using an LLM, briefly describe how you developed and verified your code instead.

**Example AI Development Record — Activity 2**

> **Objective:** Determine how the purity of a mixed state changes as the range of random relative phases increases.
>
> **Prompt:** "Using the density matrices already generated in my QuTiP notebook, write Python code to calculate the purity Tr(rho^2) for each phase distribution. Plot purity against the phase-spread parameter Delta. Include the theoretical prediction on the same graph so I can compare the simulation with the analytical result. Do not rewrite my existing state-generation functions."
>
> **AI Contribution:** The LLM generated a function for calculating purity and suggested plotting the numerical results alongside the theoretical curve.
>
> **Verification:** I checked that a fixed-phase ensemble produced purity 1 and that a uniformly random phase distribution approached purity 0.5. I also compared the simulated values with the analytical prediction and increased the number of samples to reduce numerical fluctuations.
>
> **Revision:** The original graph used too few phase-spread values to show a smooth trend. I increased the number of points and added axis labels indicating that the phase spread was measured in radians.

Your entries do not need to be long. A few sentences explaining your objective, the AI's contribution, and your verification are sufficient. The example above illustrates the expected level of detail.

Place your three AI Development Record entries in **Markdown cells at the end of your Jupyter notebook**, under the heading `AI Development Record`. Include the activity number with each entry.

You must also include a short **Final AI Reflection** (approximately 100 words) addressing:

- How did AI assistance improve or accelerate your computational investigation?
- Describe one mistake, limitation, or unexpected result in the AI-generated code that you investigated or corrected.
- What physics insight did you develop through interpreting the simulations rather than simply executing the code?

**Assessment:** Your AI Development Record will contribute to the clarity and reproducibility portion of the assignment. You will not be graded on how many prompts you used, how elaborate your prompts were, or which LLM you selected. Instead, the emphasis is on demonstrating that you directed the computational work and independently verified its scientific validity.

Do not submit complete AI conversation transcripts or share links. Do not include personal information or unrelated conversations in your record.


---

## Analytical Warm-up (before coding)


Suppose an ensemble contains 50% $|+x\rangle$ and 50% $|+y\rangle$.

Calculate, by hand,

$$
\rho = \frac{1}{2}|+x\rangle\langle+x|
     + \frac{1}{2}|+y\rangle\langle+y|
$$

Then calculate

$$
P(+x) = \mathrm{Tr}(\rho P_{+x})
$$

**Check your result with QuTiP.** Does a mixed state necessarily have zero off-diagonal elements?


---

## Organization and Submission

Complete these activity files in order:

1. [Part 1: Constructing Mixed States](Part%201%3A%20Constructing%20Mixed%20States.md)
2. [Part 2: Measuring Coherence](Part%202%3A%20Measuring%20Coherence.md)
3. [Part 3: Quantum Interference](Part%203%3A%20Quantum%20Interference.md)

**Submit one notebook** with all three activities, **four required figures**, the analytical warm-up, and **six numbered analysis answers** (two per activity). Use Markdown cells for your explanations. Clearly label axes, units, legends, and figure captions. The notebook must run from top to bottom in a fresh environment once its required packages are installed. If you use an LLM, include a brief statement describing how it assisted you.
