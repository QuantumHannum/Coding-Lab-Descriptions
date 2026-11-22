# Part 0: Introduction to NV-Diamond Quantum Spin Dynamics

## Big Idea

In this lab, you will investigate **how quantum spins evolve in time, how they can be controlled using microwave radiation, and how their quantum states can be measured experimentally**.

You have already studied spin-½ systems, the Bloch sphere, Hamiltonians, and Schrödinger time evolution. You will now apply these concepts to a real quantum system: the **nitrogen-vacancy (NV) center in diamond**.

NV centers are particularly interesting for two reasons:

1. **They provide an excellent example of Schrödinger time evolution.** We can use the Hamiltonians and spin operators you have already studied to predict how an NV-center spin evolves in a magnetic field and responds to microwave radiation.
2. **They are directly relevant to research at TJHSST.** Students in the Quantum Information and Optics Research Lab (QLab) are actively developing magnetometers using NV centers in diamond. The quantum physics you will simulate in this lab is the same physics that underlies those instruments.

Using QuTiP, you will simulate the evolution of an NV-center spin, visualize its motion on the Bloch sphere, investigate resonant and off-resonant microwave driving, and predict the results of an optical measurement.

The emphasis is on **predicting, testing, and explaining physical behavior**, rather than writing large amounts of Python code.

---

## Learning Goals

By the end of this lab, you should be able to:

1. Represent an NV-center spin using a simplified two-level quantum system and visualize its state on the Bloch sphere.
2. Explain and simulate **Larmor precession**, including how a magnetic field changes relative phase without changing the populations of the spin states.
3. Simulate **resonant Rabi oscillations** and determine the microwave pulse duration needed to flip a spin.
4. Investigate how **microwave detuning** changes the effective rotation axis and reduces the maximum population transfer.
5. Connect quantum-state populations to **optical fluorescence measurements**.
6. Explain qualitatively how these quantum processes make NV centers useful for magnetic-field sensing.

---

## Theoretical Background

### The NV Center as a Two-Level Quantum System

A nitrogen-vacancy center is a defect in a diamond crystal consisting of a nitrogen atom next to a vacant carbon lattice site.

The negatively charged NV center has an electronic ground state with spin $s=1$, corresponding to three possible spin projections:

$$
m_s=0,\quad m_s=+1,\quad m_s=-1.
$$

In this lab, we will simplify the system by considering only two of these states:

$$
|0\rangle\equiv|m_s=0\rangle,
$$

$$
|1\rangle\equiv|m_s=-1\rangle.
$$

These states form a two-dimensional Hilbert space. We can therefore represent them mathematically using the same Pauli matrices and Bloch sphere that you have used for spin-½ systems.

**Important:** This is a two-level approximation of a physical spin-1 system. We are not claiming that the NV center itself is a spin-½ particle.

### Schrödinger Time Evolution

The time evolution of a quantum state is governed by the time-dependent Schrödinger equation:

$$
i\hbar\frac{d}{dt}|\psi(t)\rangle=\hat H|\psi(t)\rangle.
$$

For a time-independent Hamiltonian, the solution is

$$
|\psi(t)\rangle=e^{-i\hat Ht/\hbar}|\psi(0)\rangle.
$$

You have already used this equation to calculate how energy eigenstates acquire phase factors and how superpositions evolve in time.

In this lab, we will use QuTiP to perform these calculations numerically and visualize the resulting motion on the Bloch sphere.

We will investigate two important physical processes:

- **Larmor precession:** A static magnetic field changes the relative phase between components of a quantum superposition.
- **Rabi oscillations:** An oscillating microwave field drives transitions between two quantum states.

We will examine the Hamiltonians describing these processes in the individual activities.

### Connecting Quantum States to Measurement

In quantum mechanics, the state of a system determines the probabilities of possible measurement outcomes.

For our two-level NV model,

$$
|\psi(t)\rangle=a(t)|0\rangle+b(t)|1\rangle,
$$

the probabilities of measuring the two states are

$$
P_0(t)=|a(t)|^2,\qquad P_1(t)=|b(t)|^2.
$$

Because the state is normalized,

$$
P_0(t)+P_1(t)=1.
$$

An NV center has an unusual and useful property: its electron spin state influences the intensity of red fluorescence emitted when it is illuminated with green light.

This allows researchers to infer information about the spin state by measuring light rather than directly measuring the spin with a Stern–Gerlach apparatus.

We will investigate this optical measurement technique in Part 3.

---

## Analytical Warm-Up

Before beginning the computational investigations, consider a two-level quantum system initially prepared in the state

$$
|\psi(0)\rangle=\frac{1}{\sqrt2}\left(|0\rangle+|1\rangle\right).
$$

The system evolves under the time-independent Hamiltonian

$$
\hat H=\frac{\hbar\omega}{2}\sigma_z,
$$

where

$$
\sigma_z=\begin{pmatrix}1&0\\0&-1\end{pmatrix}.
$$

**Calculate by hand:**

1. Determine the energy eigenvalues associated with $|0\rangle$ and $|1\rangle$.
2. Write the time-dependent state $|\psi(t)\rangle$.
3. Calculate $P_0(t)$ and $P_1(t)$.
4. Explain what happens to the relative phase between the two basis states.

**Prediction:** How will the Bloch vector move as time increases? Will the populations of the two states change?

**Check your results with QuTiP.**

Complete this calculation in the **Analytical Warm-Up** section of your student notebook before beginning Part 1.

---

## Organization of the Lab

The lab consists of three computational investigations:

**Part 1: Larmor Precession and Quantum Phase**

Investigate how a static magnetic field produces relative phase evolution. You will simulate spin precession on the Bloch sphere and compare the trajectory with the populations of the two states.

**Part 2: Microwave Driving and Rabi Oscillations**

Investigate how microwave radiation drives transitions between spin states. You will simulate resonant Rabi oscillations, determine the duration of a $\pi$-pulse, and explore how microwave detuning changes the spin dynamics.

**Part 3: Optical Readout and NV Magnetometry**

Use your simulated spin populations to predict red fluorescence measurements. You will connect your computational results to the principles underlying NV-diamond magnetometers.

---

## Download Your Student Notebook

A structured Jupyter notebook has been provided for this lab. It contains the code stems, prediction questions, spaces for figures, numerical verification tasks, physics-analysis questions, and AI Development Record.

**[Download the NV-Diamond Quantum Spin Dynamics Student Notebook](NV_Spin_Dynamics_Student_Template.ipynb)**

**Use this notebook for all three activities.** You do not need to create a new notebook or copy the assignment instructions into a separate file.

The notebook includes:

- **Part 0:** Coding environment setup and analytical warm-up.
- **Part 1:** Larmor precession, including Figure 1.
- **Part 2:** Resonant and off-resonant microwave driving, including Figures 2–3.
- **Part 3:** Optical fluorescence readout, including Figure 4.
- **AI Development Record:** Spaces to document your LLM interactions.
- **Final AI Reflection:** A short reflection on AI use and the physics you learned.
- **Submission Check:** An automated verification of required computational results.

Throughout the notebook, you will find code cells marked `TODO` and Markdown cells with spaces for your written responses. Complete these sections as you work through the activities.

**Important:** Record your predictions before running the corresponding simulations. Do not delete the supplied code stems, questions, or section headings. You may add cells as needed.

Save your working notebook regularly.

---

## Coding Environment Setup

You may use **Google Colab**, the school's **OnDemand Jupyter system (JupyterHub)**, or **VS Code**. Choose one environment; you do not need all three.

All activities use Python, NumPy, Matplotlib, and QuTiP. The downloadable notebook already contains the basic imports, basis definitions, and code stems.

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
6. Save your working notebook to Google Drive as you work.

**Remember:** Colab runtimes are temporary. You may need to reinstall packages when starting a new session.

### Option B: School OnDemand Jupyter Notebook

1. Download the student notebook using the link above.
2. Sign in to the school's OnDemand Jupyter portal using your assigned credentials.
3. Upload the `.ipynb` file into your Jupyter working directory.
4. Open the notebook and select an available Python kernel.
5. Run the setup cell to check whether QuTiP, NumPy, and Matplotlib are available.
6. If QuTiP is not installed, you may try:

   ```python
   %pip install qutip
   ```

7. Restart the kernel if necessary and rerun the setup cell.
8. If package installation is restricted, contact your instructor rather than modifying the shared environment.
9. Save your completed notebook in your persistent school directory.

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
9. Run the notebook's setup cell to verify that the required packages are available.

### Common Import and Verification

The downloaded notebook already contains an import and environment-check cell. Run that cell before beginning Part 1.

The following is an additional test you may use if necessary:

```python
import numpy as np
import matplotlib.pyplot as plt
import qutip as qt

zero = qt.basis(2, 0)
one = qt.basis(2, 1)

psi_test = (zero + one).unit()

P0 = abs(zero.overlap(psi_test))**2
P1 = abs(one.overlap(psi_test))**2

print("QuTiP:", qt.__version__)
print("P(0):", P0)
print("P(1):", P1)
print("Total probability:", P0 + P1)

assert np.isclose(P0 + P1, 1)
```

A successful test produces $P_0=0.5$, $P_1=0.5$, and total probability approximately equal to 1.

---

## Using a Generative AI (LLM) as a Coding Assistant

You may use a Generative AI (GAI/LLM) to write, revise, explain, or debug code. However, **you must decide what physical quantities to calculate, verify the numerical results, and understand every figure you submit.**

**Using a GAI to write your answers to the physics-analysis questions is not allowed.**

As LLMs become increasingly capable of producing code, an important scientific skill is learning which parts of a computational problem to delegate to AI and which decisions must remain your own.

Your responsibility is to direct the investigation, evaluate the results, and draw conclusions from the physics—not simply obtain working code.

When asking an LLM for programming assistance, provide the relevant equations, QuTiP version, and intended variables. Ask it to build on your existing notebook rather than replacing earlier work.

Before trusting a simulation, verify that the quantum state is normalized, measurement probabilities are physically valid, and results agree with analytical predictions or known limiting cases.

### AI Development Record

To document how you use AI as a scientific tool, you will maintain a short **AI Development Record** in your Jupyter notebook.

For each of the three activities, document **one meaningful interaction with an LLM**.

Choose an interaction that illustrates how you used AI to develop, improve, debug, or validate your computational investigation. You do not need to submit your entire conversation history.

For each entry, include:

1. **Objective:** What were you trying to accomplish scientifically or computationally?
2. **Prompt:** Copy the actual prompt you submitted to the LLM.
3. **AI Contribution:** Briefly describe what the LLM produced or suggested.
4. **Verification:** Explain how you checked, tested, corrected, or modified the LLM's response.
5. **Revision (if applicable):** Describe a meaningful improvement you made after testing the result.

If you complete an activity without using an LLM, briefly describe how you developed and verified your code instead.

Your entries do not need to be long. A few sentences explaining your objective, the AI's contribution, and your verification are sufficient.

The notebook includes designated Markdown cells for all three entries.

You must also complete a short **Final AI Reflection** (approximately 100 words) addressing:

- How did AI assistance improve or accelerate your computational investigation?
- Describe one mistake, limitation, or unexpected result in the AI-generated code that you investigated or corrected.
- What physics insight did you develop through interpreting the simulations rather than simply executing the code?

If you did not use AI, reflect instead on your independent coding process, the challenges you encountered, and how you verified your results.

**Assessment:** Your AI Development Record will contribute to the clarity and reproducibility portion of the assignment. You will not be graded on how many prompts you used, how elaborate your prompts were, or which LLM you selected.

The emphasis is on demonstrating that you directed the computational work and independently verified its scientific validity.

Do not submit complete AI conversation transcripts or share links. Do not include personal information or unrelated conversations in your record.

---

## Organization and Submission

Complete the three activities in order, using the provided student notebook:

1. **Part 1: Larmor Precession and Quantum Phase**
2. **Part 2: Microwave Driving and Rabi Oscillations**
3. **Part 3: Optical Readout and NV Magnetometry**

The activity descriptions provide the detailed physics background, computational instructions, and assessment requirements.

The student notebook provides the workspace for completing those tasks.

### Submission Requirements

**Submit one PDF copy of your completed Jupyter notebook.**

Your PDF must include:

1. **Analytical Warm-Up:** Your calculation from Part 0 and its verification using QuTiP.
2. **Four Required Figures:**
   - Figure 1: Bloch-sphere visualization of Larmor precession and corresponding populations.
   - Figure 2: Resonant Rabi oscillations, including a Bloch-sphere trajectory and population graph.
   - Figure 3: Comparison of resonant and off-resonant microwave driving.
   - Figure 4: Expected red fluorescence versus microwave pulse duration.
3. **Predictions and Verification:** Complete prediction sections before running each activity's main simulations. Compare your results with analytical predictions or known physical limits.
4. **Physics Analysis:** Answer all six numbered analysis questions (two per activity) independently, without using an LLM to generate your answers.
5. **Numerical Results:** Include the calculated quantities and comparisons requested in Parts 1–3.
6. **AI Development Record:** Complete the three entries provided at the end of the notebook.
7. **Final AI Reflection:** Complete the approximately 100-word reflection.
8. **Reproducible Code:** Include all Python code required to regenerate the calculations and figures.
9. **Submission Check:** Include the results of the automated verification cell.

### Automated Submission Check

The final executable cell of your student notebook contains an instructor-provided **Submission Check**.

It will automatically verify that:

- Figures 1–4 have been generated.
- Spin-state populations are normalized.
- Larmor precession preserves the populations.
- Resonant driving produces the expected spin flip.
- Off-resonant driving reduces maximum population transfer.
- Fluorescence values remain within the physically allowed range.
- Required numerical results are available.

Each check will report **PASS**, **FAIL**, or **NOT RUN**.

If a check fails, investigate the cause and rerun the relevant simulation.

The Submission Check cannot verify that your written responses are complete or that your exported PDF is formatted correctly. You must check those separately.

### Presentation and Assessment

Clearly label figures with descriptive titles, axes, units where applicable, legends, and captions. Include numerical values and comparisons where requested.

Your submission will be evaluated using the following weights:

| Component | Weight |
|---|---:|
| Physics predictions and interpretation | 40% |
| Accurate, labeled visualizations | 30% |
| Correct, validated calculations | 20% |
| Clarity, reproducibility, and AI Development Record | 10% |

Code elegance, prompt sophistication, and the amount of code written without assistance are **not** grading objectives.

### Exporting Your Notebook to PDF

**Google Colab**

1. Open your completed notebook.
2. Select **File → Print**.
3. In the browser's print dialog, select **Save as PDF**.
4. Save the file to your computer.

**School OnDemand Jupyter**

1. Open your completed notebook.
2. Use your browser's Print command (`Ctrl+P` or `Cmd+P`).
3. Select **Save as PDF** as the destination.
4. Save the file to your computer.

**VS Code**

1. Save your completed notebook.
2. Use the notebook export options to export to HTML, if available.
3. Open the exported HTML file in a browser.
4. Use **Print → Save as PDF**.

If the HTML export option is unavailable, ask your instructor for assistance rather than installing additional export software.

### Before Submitting

1. Save your completed notebook.
2. Restart the kernel and run the notebook from beginning to end.
3. Confirm that all four required figures appear correctly.
4. Check that every prediction, verification task, physics-analysis question, and AI Development Record entry has been completed.
5. Complete the Final AI Reflection.
6. Run the automated Submission Check and review any failed or incomplete tests.
7. Export the completed notebook to PDF.
8. Open the PDF and verify that all figures, code, numerical results, and written responses are visible and readable.

Rename your completed PDF:

`NV_Spin_Dynamics_Lastname.pdf`

**Submit only the PDF to the LMS.** No separate code files, screenshots, AI transcripts, or written reports are required.

Keep your original `.ipynb` notebook in case you need to revise your work or your instructor requests it for verification.
