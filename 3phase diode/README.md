# Three-Phase Diode Bridge Rectifier with Ohmic Load

This repository contains the complete MATLAB/Simulink design, simulation model, and analytical verification for an uncontrolled **Three-Phase Full-Wave Bridge Rectifier** operating under a pure resistive (ohmic) load[cite: 1]. 

This project was developed in accordance with the Industrial Electronics Laboratory curriculum guidelines at the **University of Kurdistan**, Faculty of Engineering[cite: 1].

---

##  Academic Profile
* **Student Name:** THON KUOL THON KOCH[cite: 1]
* **Student ID:** 40217001201[cite: 1]
* **Institution:** University of Kurdistan | Faculty of Engineering[cite: 1]
* **Department:** Electrical & Electronics Engineering[cite: 1]
* **Lab Instructor:** Eng. Omid Eslamipour[cite: 1]
* **Date of Simulation:** June 10, 2026[cite: 1]
* **Reference Document:** Industrial_Electronics_Lab_Report1.pdf

---

##  Core Project Objectives
1. **Model and Analyze:** Comprehensively model and evaluate the steady-state performance parameters of an uncontrolled 6-pulse diode rectifier configuration[cite: 1].
2. **Replicate Practical Conditions:** Establish a hardware-matched continuous simulation network using the MATLAB/Simulink ecosystem[cite: 1].
3. **Evaluate Non-Ideal Effects:** Measure explicit voltage drops and minor regulation losses induced by input line matching protection resistances ($R_s = 10\ \Omega$)[cite: 1].
4. **Validate Performance Metrics:** Verify empirical simulation values directly against classical power conversion derivations[cite: 1].

---

##  Theoretical Background & Circuit Topology

Three-phase uncontrolled rectifiers are widely utilized in heavy industrial power electronics, high-voltage DC (HVDC) transmission architectures, and variable frequency drive (VFD) front-ends[cite: 1]. Compared to conventional single-phase configurations, multi-phase rectification provides structural superiorities including a significantly higher average output voltage level, standard continuity of current flow without zero-crossing drop periods, and low voltage ripple percentages that mitigate the immediate necessity for complex smoothing filter banks[cite: 1].

### Conduction Mechanics & Commutation Matrix
The bridge topology utilizes a configuration consisting of six power diodes arranged in three vertical leg columns[cite: 1]. Commutation follows a strict natural potential hierarchy:
* **Upper Commutation Bank ($D_1, D_3, D_5$):** The specific diode attached to the phase carrying the highest instantaneous positive potential conducts[cite: 1].
* **Lower Commutation Bank ($D_4, D_6, D_2$):** The specific diode attached to the phase carrying the highest instantaneous negative potential conducts[cite: 1].

Consequently, every $60^\circ$ interval, a natural phase cross-over prompts commutation to a subsequent pair, yielding six output pulses per primary fundamental AC period ($f_{\text{ripple}} = 6f_{\text{source}}$)[cite: 1].

### Definitive Mathematical Formula Framework
To verify data compiled from the simulation engine, standard analytical derivations are applied assuming an ideal transformer interface where $V_m$ indicates the absolute peak phase voltage ($311\text{ V}$)[cite: 1]:

1. **Average (DC) Output Voltage:** 
   $$V_{o(DC)} = \frac{3}{\pi} \times V_{LL(peak)} = \frac{3\sqrt{3}}{\pi} \times V_m \approx 1.654 \times V_m$$
2. **Root-Mean-Square (RMS) Output Voltage:** 
   $$V_{o(RMS)} = V_m \times \sqrt{\frac{3}{2} + \frac{9\sqrt{3}}{4\pi}} \approx 1.6554 \times V_m$$
3. **Form Factor (F.F):** 
   $$F.F = \frac{V_{o(RMS)}}{V_{o(DC)}} \approx 1.0009$$
4. **Ripple Factor (R.F):** 
   $$R.F = \sqrt{F.F^2 - 1} \approx 0.042 \ (4.21\%)$$
5. **Conversion Efficiency ($\eta$):** 
   $$\eta = \frac{P_{DC}}{P_{AC}} = \frac{V_{o(DC)} \times I_{o(DC)}}{V_{o(RMS)} \times I_{o(RMS)}} \approx 99.81\%$$

---

## MATLAB/Simulink Implementation Environment

The experiment is constructed inside the Simscape Electrical / Specialized Power Systems blockset framework[cite: 1]. The workspace diagram mimics an industrial test stand layout by integrating explicit protection impedances[cite: 1].

### Model Component Breakdown
* **Three-Phase Source Block:** Configured in a star-grounded (Y) configuration, generating a true balanced three-phase system at a fundamental operating frequency of $f = 50\text{ Hz}$ and peak amplitude $V_m = 311\text{ V}$[cite: 1].
* **Line Matching/Protection Impedance:** Three discrete series resistive blocks inserted natively at each individual line connection point, set exactly to $R_s = 10\ \Omega$ to emulate internal physical transformer losses as dictated by page 36 of the lab manual[cite: 1].
* **Universal Bridge Module:** A 6-pulse block tailored strictly to Diode power electronics operations, enabling continuous non-controlled dynamic switching states[cite: 1].
* **Ohmic Load Branch:** A linear Series RLC block switched entirely to a pure resistive mode with an explicit impedance value of $R_L = 680\ \Omega$[cite: 1].
* **Signal Acquisition:** Integration of specialized Current and Voltage Measurement points routing instantaneous real-time metrics directly into an active Multi-channel Time Scope block through a continuous-mode Powergui execution block[cite: 1].

---

##  Experimental Simulation Protocol & Waveform Strategy

The simulator configuration utilizes continuous variable-step integration solvers (specifically optimized via `ode23tb`) over a complete time frame of $t = 0.1\text{ s}$ to capture exactly five full structural line frequency cycles[cite: 1].

When evaluating the generated visualization graphs from the measurement scope, the following structural characteristics are verified:
1. **Input Source Profiles ($V_a, V_b, V_c$):** Symmetrical sinusoidal phase paths separated cleanly by a balanced $120^\circ$ phase shift displacement[cite: 1].
2. **DC Rail Output ($V_o$):** A highly continuous DC voltage profile oscillating steadily at six pulses per cycle, remaining uniformly elevated ($460\text{ V} - 520\text{ V}$) and never approaching zero[cite: 1].
3. **Diode Overvoltage Exposure ($V_{D1}$):** Traces showing near-zero forward conduction voltage drop for a span of $120^\circ$, falling rapidly into alternating reverse-blocking profiles peaking at $-538\text{ V}$[cite: 1].
4. **Sampling Resistance Profile ($V_1$):** The isolated voltage drop across the line protection block that maps exactly to the jagged, discontinuous rectangular line current pulses traversing that phase[cite: 1].

---

## Quantitative Laboratory Data Logging & Validation

The quantitative evaluation grid below catalogs and assesses simulated values directly against mathematical derivations[cite: 1].

| Operational Parameter | Theoretical Metric Formula | Theoretical Value (Ideal) | Logged Simulink Data | Percentage Deviation Error (%) |
| :--- | :--- | :--- | :--- | :--- |
| **Average Output DC Voltage ($V_{o(DC)}$)** | $1.654 \times V_m$ | 514.40 V | 497.2 V | -3.34% (Due to $10\ \Omega$ line drop)[cite: 1] |
| **Total Output RMS Voltage ($V_{o(RMS)}$)** | $1.6554 \times V_m$ | 514.83 V | 497.7 V | -3.33% (Due to $10\ \Omega$ line drop)[cite: 1] |
| **System Form Factor (F.F)** | $V_{o(RMS)} / V_{o(DC)}$ | 1.0009 | 1.0010 | +0.01% (Excellent match)[cite: 1] |
| **Inherent Ripple Factor (R.F)** | $\sqrt{F.F^2 - 1}$ | 0.042 (4.21%) | 4.47% | +6.18% (Normal impedance var.)[cite: 1] |
| **Rectification Efficiency ($\eta$)** | $(V_{o(DC)} / V_{o(RMS)})^2$ | 99.81% | 99.80% | -0.01% (Identical)[cite: 1] |
| **Line Protection Voltage RMS ($V_{1(AC)}$)** | Dependent on Line Flow | Experimental Baseline | 5.12 V | N/A[cite: 1] |

---

## Academic Evaluation & Structural Questions

### Q1: Contrast the properties of this three-phase bridge against conventional single-phase full-wave topologies.
**Analytical Response:** The structural transition from a single-phase bridge to a three-phase bridge introduces highly superior performance dynamics[cite: 1]. A single-phase full-wave system yields a significantly depressed average DC profile ($0.636 \times V_m$) and exhibits severe cyclic dips that force the output voltage to touch the zero-potential line twice per primary line cycle[cite: 1]. This induces an extreme theoretical ripple factor of $48.21\%$[cite: 1]. Conversely, the three-phase bridge topology splits the conduction load across a multi-phase system, lifting the minimum output profile to the peak line-to-line envelope threshold[cite: 1]. It ensures the voltage never drops below $1.5 \times V_m$, dramatically minimizing the ripple factor to a minor $4.21\%$[cite: 1]. Consequently, three-phase configurations achieve high-quality, flat DC profiles without needing oversized filter capacitors, which reduces cost and improves reliability in industrial applications[cite: 1].

### Q2: Formulate the analytical significance of the $10\ \Omega$ inline protection resistors regarding real circuit operation.
**Analytical Response:** In completely ideal textbook topologies, lines possess zero internal source impedance, resulting in instantaneous, infinite current transitions during diode commutation. By integrating the explicit $10\ \Omega$ matching resistors, the model accounts for the physical reality of transformer winding resistance and line cables[cite: 1]. During active conduction intervals, the load current drawing through the active phase legs creates a continuous proportional voltage drop across these protection blocks[cite: 1]. This introduces clear thermal losses that slightly decrease the overall operational efficiency and cause an observable down-shift in the DC output voltage rail compared to ideal mathematical models[cite: 1]. Furthermore, in non-ideal circuits, these line resistances interact with source inductances to introduce minor commutation overlap delays, dampening rapid voltage step variations[cite: 1].

---

## Execution Guide
1. Open **MATLAB** (Simulink environment).
2. Load the file `three_phase_rectifier.slx`.
3. Set simulation parameters to variable-step continuous solver (`ode23tb`) with a stop time of `0.1`.
4. Run simulation and access the Scope blocks to verify real-time waveforms.
