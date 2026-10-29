
# Part 0: Introduction to Density Matrices and Quantum Coherence

## Big Idea

In this lab, you will investigate how **statistical uncertainty about the relative phase of a spin-½ state changes the predictions of quantum mechanics**. You have already used kets, projection operators, the Bloch sphere, and the Born rule. Density matrices let you describe both pure states and ensembles that cannot be represented by one ket.

You will construct ensembles in QuTiP, visualize them on the Bloch sphere, compute measurement probabilities, and apply your findings to an idealized Stern–Gerlach interferometer.

The emphasis is on **predicting, testing, and explaining physical behavior** rather than writing large amounts of code.

## Learning Goals

- Construct a pure-state density matrix as an outer product and a mixed-state density matrix as a weighted sum of projectors.
- Explain how averaging different relative phases changes off-diagonal matrix elements while leaving some measurement populations unchanged.
- Relate a spin state's density matrix to its Bloch vector and purity.
- Apply the density-matrix Born rule to predict spin measurement outcomes.
- Relate the magnitude of coherence to interference visibility and distinguish statistical phase averaging from which-path entanglement.

---

## Theoretical Background

### Pure States and Projectors

For a spin-½ system, work in the $z$ basis $\{|+z\rangle,|-z\rangle\}$. A general pure state is

$$
|\psi\rangle=\cos\frac{\theta}{2}|+z\rangle
+e^{i\phi}\sin\frac{\theta}{2}|-z\rangle.
$$

Its density operator is the rank-one projector

$$
\hat\rho=|\psi\rangle\langle\psi|.
$$

In the $z$ basis,

$$
\rho=
\begin{pmatrix}
\cos^2(\theta/2)&\frac12\sin\theta\,e^{-i\phi}\\
\frac12\sin\theta\,e^{i\phi}&\sin^2(\theta/2)
\end{pmatrix}.
$$

The diagonal entries give $P(+z)$ and $P(-z)$. The off-diagonal entries encode coherence in the $z$ basis, which affects measurements along other directions.

Changing a ket's global phase does not change its projector.

### Statistical Ensembles

If state $|\psi_k\rangle$ is prepared with probability $p_k$, the ensemble density operator is

$$
\hat\rho_{\mathrm{ensemble}}
=\sum_k p_k|\psi_k\rangle\langle\psi_k|,
\qquad
\sum_k p_k=1.
$$

We will initially fix $\theta=\pi/2$ and let the relative phase vary:

$$
|\psi(\phi)\rangle=
\frac{1}{\sqrt2}
\big(|+z\rangle+e^{i\phi}|-z\rangle\big).
$$

Generate $N$ phases $\phi_k$ and average their pure-state density matrices:

$$
\rho_N=\frac1N\sum_{k=1}^N
|\psi(\phi_k)\rangle\langle\psi(\phi_k)|.
$$

If phases are uniformly distributed over $[-\Delta,\Delta]$, the **large-ensemble** prediction is

$$
\rho(\Delta)=\frac12
\begin{pmatrix}
1&\gamma\\
\gamma&1
\end{pmatrix},
$$

where

$$
\gamma=
\begin{cases}
\dfrac{\sin\Delta}{\Delta},&\Delta\ne0\\
1,&\Delta=0.
\end{cases}
$$

Here the phase distribution is centered at zero, so $\gamma$ is real.

For a finite random sample, small complex off-diagonal fluctuations are normal. We will compare numerical results against analytical predictions rather than assume they must agree exactly.

### Purity, Measurement, and Interference

A valid density matrix is Hermitian, positive semidefinite, and has trace 1.

Its **purity** is

$$
\mathcal{P}=\mathrm{Tr}(\rho^2).
$$

For a single spin-½ system,

$$
\frac12\leq\mathcal{P}\leq1.
$$

A pure state has purity 1. A maximally mixed state has purity $1/2$.

For a projective spin measurement, the density-matrix Born rule is

$$
P(+j)=\mathrm{Tr}(\rho P_{+j}),
\qquad j=x,y,z.
$$

The same mathematical coherence that changes $P(+x)$ also controls interference in a suitably recombined Stern–Gerlach interferometer.

---

## Analytical Warm-Up

Before beginning the computational activities, consider an ensemble containing:

- 50% of particles prepared in $|+x\rangle$.
- 50% of particles prepared in $|+y\rangle$.

Calculate, by hand,

$$
\rho=
\frac12|+x\rangle\langle+x|
+\frac12|+y\rangle\langle+y|.
$$

Express your answer as a $2\times2$ matrix in the $z$ basis.

Then calculate

$$
P(+x)=\mathrm{Tr}(\rho P_{+x}).
$$

**Check your result with QuTiP.**

Does a mixed state necessarily have zero off-diagonal elements?

Complete this calculation in the **Analytical Warm-Up** section of your student notebook before beginning Part 1.

---

## Download Your Student Notebook

A structured Jupyter notebook has been provided for this lab. It contains the code stems, prediction questions, spaces for figures, numerical verification tasks, physics analysis questions, and AI Development Record.

**[Download the Density Matrices and Quantum Coherence Student Notebook](Density_Matrices_Quantum_Coherence_Student_Template.ipynb)**

**Use this notebook for all three activities.** You do not need to create a new notebook or copy the assignment instructions into a separate file.

The notebook includes:

- **Part 0:** Coding environment setup and analytical warm-up.
- **Part 1:** Constructing mixed states, including Figures 1–2.
- **Part 2:** Measuring coherence, including Figure 3.
- **Part 3:** Quantum interference, including Figure 4.
- **AI Development Record:** Spaces to document your LLM interactions for each activity.
- **Final AI Reflection:** A short reflection on your use of AI and what you learned.

Throughout the notebook, you will find code cells marked `TODO` and Markdown cells with spaces for your written responses. Complete these sections as you work through the activities.

The notebook also contains reference diagrams of the Stern–Gerlach experiments. These diagrams help you understand the experimental arrangements; they do not replace the four figures you must generate yourself.

**Important:** Record your predictions before running the corresponding simulations. Do not delete the supplied code stems, questions, or section headings. You may add cells as needed.

Save your working notebook regularly. Before submitting, rename your completed file to include your name, for example:

`Density_Matrices_Quantum_Coherence_Lastname.ipynb`

---

## Coding Environment Setup

You may use **Google Colab**, the school's **OnDemand Jupyter system**, or **VS Code**. Choose one environment; you do not need all three.

All activities use Python, NumPy, Matplotlib, and QuTiP. The downloadable student notebook already contains the basic imports, basis definitions, and code stems.

Internet access may be needed to install packages initially.

### Option A: Google Colab

1. Download the student notebook using the link above.
2. Open [Google Colab](https://colab.research.google.com/).
3. Select **File → Upload notebook** and choose the downloaded `.ipynb` file.
4. Run the setup cell near the beginning of the notebook. If the required packages are not installed, run:

```python
%pip install -q qutip numpy matplotlib
```

5. If importing QuTiP fails immediately after installation, restart the runtime and rerun the setup cell.
6. Save your working notebook to Google Drive. When finished, select **File → Download → Download .ipynb** to obtain your submission file.

**Remember:** Colab runtimes are temporary. You may need to reinstall packages when starting a new session.

### Option B: School OnDemand Jupyter Notebook

1. Download the student notebook using the link above.
2. Sign in to the school's OnDemand portal using your assigned credentials.
3. Upload the `.ipynb` file into your Jupyter working directory.
4. Open the notebook and select an available Python kernel.
5. Run the setup cell to check whether QuTiP, NumPy, and Matplotlib are available.
6. If QuTiP is not installed, you may try the following in a notebook cell:

```python
%pip install qutip
```

7. Restart the kernel if necessary and rerun the setup cell.
8. If package installation is restricted, contact your instructor rather than modifying the shared environment.
9. Save the completed notebook in your persistent school directory and download a copy for submission.

### Option C: VS Code on Your Computer

1. Download the student notebook using the link above.
2. Install Python 3 and [Visual Studio Code](https://code.visualstudio.com/).
3. Install Microsoft's **Python** and **Jupyter** extensions in VS Code.
4. Open a project folder and create a Python virtual environment:

```bash
python -m venv .venv
```

5. Activate the environment.

**macOS/Linux:**

```bash
source .venv/bin/activate
```

**Windows PowerShell:**

```powershell
.venv\Scripts\Activate.ps1
```

6. Install the required packages:

```bash
python -m pip install --upgrade pip
python -m pip install numpy matplotlib qutip ipykernel
```

7. Open the downloaded `.ipynb` notebook in VS Code.
8. Use **Select Kernel** to choose your Python virtual environment.
9. Run the notebook's setup cell to verify that the packages are available.

### Common Import and Verification

The downloaded notebook already contains an import and environment-check cell. Run that cell before beginning Part 1.

The following is an additional test you may use if necessary:

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

A successful test produces trace approximately 1 and purity approximately 1.

---

## Using a Generative AI (LLM) as a Coding Assistant

You may use a Generative AI (GAI/LLM) to write, revise, explain, or debug code. However, **you must decide what physical quantities to calculate, verify the numerical results, and understand every figure you submit.**

<ins>Using a GAI to write your answers to the physics analysis questions is not allowed.</ins> More importantly, doing so would undermine the purpose of this investigation.

As LLMs become increasingly capable of producing code, an important scientific skill is learning **which parts of a computational problem to delegate to an AI and which decisions must remain your own**.

Your responsibility is to direct the investigation, evaluate the results, and draw conclusions from the physics—not simply obtain working code.

When asking an LLM for programming assistance, provide the relevant equations, QuTiP version, and intended variables. Ask it to build on your existing notebook rather than replacing earlier work.

Before trusting a simulation, verify that the density matrices are Hermitian, have trace 1, and produce physically valid measurement probabilities. Compare numerical results with analytical predictions whenever possible.

### AI Development Record

To document how you use AI as a scientific tool, you will maintain a short **AI Development Record** in your Jupyter notebook.

For each of the three activities, document **one meaningful interaction with an LLM**.

Choose an interaction that illustrates how you used AI to develop, improve, debug, or validate your computational investigation. You do not need to submit your entire conversation history.

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

Your entries do not need to be long. A few sentences explaining your objective, the AI's contribution, and your verification are sufficient.

The example above illustrates the expected level of detail.

The student notebook already includes an **AI Development Record** section near the end. Complete the three entries in the provided Markdown cells, identifying the corresponding activity.

You must also complete a short **Final AI Reflection** (approximately 100 words) addressing:

- How did AI assistance improve or accelerate your computational investigation?
- Describe one mistake, limitation, or unexpected result in the AI-generated code that you investigated or corrected.
- What physics insight did you develop through interpreting the simulations rather than simply executing the code?

If you did not use AI, reflect instead on your independent coding process, the challenges you encountered, and how you verified your results.

**Assessment:** Your AI Development Record will contribute to the clarity and reproducibility portion of the assignment. You will not be graded on how many prompts you used, how elaborate your prompts were, or which LLM you selected.

Instead, the emphasis is on demonstrating that you directed the computational work and independently verified its scientific validity.

Do not submit complete AI conversation transcripts or share links. Do not include personal information or unrelated conversations in your record.

---

## Organization and Submission

Complete the three activities in order, using the provided student notebook:

1. [Part 1: Constructing Mixed States](Part%201%3A%20Constructing%20Mixed%20States.md)
2. [Part 2: Measuring Coherence](Part%202%3A%20Measuring%20Coherence.md)
3. [Part 3: Quantum Interference](Part%203%3A%20Quantum%20Interference.md)

The activity descriptions provide the detailed physics background, computational instructions, and assessment requirements.

The student notebook provides the workspace for completing those tasks.

**[Download the Student Notebook Template](Density_Matrices_Quantum_Coherence_Student_Template.ipynb)**

### Submission Requirements

**Submit one completed, executable Jupyter notebook (`.ipynb`).**

Use the provided Markdown and code cells to organize your work. You may add additional cells, but retain the original section structure so your work is easy to follow.

Your notebook must include:

1. **Analytical Warm-Up:** Your calculation from Part 0 and its verification using QuTiP.

2. **Four Required Figures:**
   - Figure 1: Bloch-sphere visualization of the quantum-state ensembles.
   - Figure 2: Off-diagonal coherence versus phase spread.
   - Figure 3: Purity and measurement probabilities versus phase spread.
   - Figure 4: Stern–Gerlach interference fringes.

3. **Predictions and Verification:** Complete the prediction sections before running each activity's main simulations. Compare your results with your predictions and verify numerical results against analytical calculations or known physical limits.

4. **Physics Analysis:** Answer all six numbered analysis questions (two per activity) independently, without using an LLM to generate your answers.

5. **Numerical Results:** Include the density matrices, tables, and quantitative comparisons requested in Parts 1–3.

6. **AI Development Record:** Complete the three entries provided at the end of the notebook, one for each activity. Include your actual prompt, objective, AI contribution, and verification or revision. If you did not use an LLM, describe your independent development and verification process instead.

7. **Final AI Reflection:** Complete the approximately 100-word reflection in the designated Markdown cell.

8. **Reproducible Code:** Complete the necessary code cells so the notebook can regenerate the calculations and figures from start to finish.

### Presentation and Assessment

Clearly label figures with descriptive titles, axes, units where applicable, legends, and captions. Include numerical results and tables where requested.

Your submission will be evaluated using the following weights:

| Component | Weight |
|---|---:|
| Physics predictions and interpretation | 40% |
| Accurate, labeled visualizations | 30% |
| Correct, validated calculations | 20% |
| Clarity, reproducibility, and AI Development Record | 10% |

Code elegance, prompt sophistication, and the amount of code written without assistance are **not** grading objectives.

### Before Submitting

1. Save your completed notebook with your name in the filename.
2. Restart the kernel/runtime and run the notebook from beginning to end.
3. Confirm that all four required figures appear and all numerical results have been generated.
4. Check that every prediction, verification, analysis question, and AI Development Record entry has been completed.
5. Complete the Final AI Reflection.
6. Download or save the final `.ipynb` file.

**Submit only your completed Jupyter notebook.** No separate code files, AI transcripts, or written reports are required.

