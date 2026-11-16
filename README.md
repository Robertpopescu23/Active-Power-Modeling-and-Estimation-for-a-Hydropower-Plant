# Active Power Modeling and Estimation for a Hydropower Plant

## Comparing Fuzzy Inference Systems (FIS) and Regression Neural Networks (ANN) in MATLAB & Simulink

---

## 1. Introduction and Objectives

- **Context:** The electrical power system requires accurate and rapid estimation of active power generation from renewable sources, particularly hydropower plants, to ensure grid stability within the Romanian National Power System (SEN).
- **Objective:** Build, train, and compare two distinct artificial intelligence approaches—a **Fuzzy Inference System (FIS)** and a **Regression Neural Network (ANN)**—to predict active power output ($P$) based on hydraulic parameters.
- **Reference Data:** Models are calibrated and contextualized using official capacity data from the **ENTSO-E Transparency Platform**, specifically considering installed capacities in Romanian water reservoirs (~3.4 GW) and run-of-river plants (~2.7 GW).

---

## 2. Engineering Problem Description

The system models a representative hydropower generation unit governed by the fundamental hydraulic power equation:

$$
P = \eta \cdot \rho \cdot g \cdot Q \cdot H
$$

- **Input Variables ($X$):**
  * Water Discharge ($Q$) [$m^3/s$]
  * Hydraulic Head / Water Level ($H$) [$m$]
- **Output Variable ($Y$):**
  * Generated Active Power ($P$) [$MW$]

---

## 3. Methodology 1: Fuzzy Inference System (FIS)

- Developed using MATLAB's `fuzzyLogicDesigner` tool.
- Configured as a Mamdani-type fuzzy inference system with:
  * **2 Inputs:** Discharge ($Q$) and Head ($H$), each split into linguistic terms (*Low, Medium, High*).
  * **1 Output:** Active Power ($P$) with 3 membership functions (*Low, Medium, High*).
  * **Rule Base:** 9 intuitive IF-THEN rules reflecting expert engineering heuristics.

### Fuzzy Inference Control Surfaces (3D & 2D)
The control surfaces demonstrate how the Mamdani system maps the non-linear relationship between inputs and output. The color gradients (ranging from dark blue/purple for low power up to green, orange, and bright yellow for peak power generation) illustrate the smooth linguistic transition across operational regimes:

![Fuzzy 2D Control Surface](Screenshot%202026-11-16%20at%2017.37.41.png)
*Figure 3.1: 2D Fuzzy surface mapping Discharge ($Q$) versus Head ($H$).*

![Fuzzy 3D Control Surface with Active Power](Screenshot%202026-11-16%20at%2017.38.07.png)
*Figure 3.2: 3D Fuzzy control surface mapping inputs to Active Power ($P$).*

---

## 4. Methodology 2: Regression Neural Network (ANN)

- **Training Data:** Synthetic dataset generated using physical laws combined with realistic Gaussian noise to simulate sensor variations ($Q \in [10, 220]\text{ m}^3/\text{s}$, $H \in [20, 65]\text{ m}$).
- **Architecture:** Feed-forward neural network for regression trained using the modern `fitrnet` function in MATLAB ($2 \to 12 \to 6 \to 1$ layout).
- **Performance Metric:** Evaluated using Mean Squared Error (MSE) and visual regression plots comparing targets versus predictions, achieving smooth nonlinear regression.

---

## 5. Simulink Implementation & Architecture

### Logical System Architecture
The framework is designed in a **parallel architecture**, where both the ANN block and the Mamdani Fuzzy Controller simultaneously receive the exact same dynamic input signals ($Q$ and $H$) to independently estimate active power before comparison:

![Logical Block Architecture](691f9d07.jpeg)
*Figure 5.1: High-level parallel logical architecture block diagram.*

### Simulink Block Diagram
The operational model built in MATLAB/Simulink integrates sinusoidal test signal generators, signal multiplexers (`Mux`), the ANN MATLAB function block, the Fuzzy Logic Controller block, and the comparative scope:

![Simulink Implementation Model](Screenshot%202026-11-16%20at%2018.32.29.png)
*Figure 5.2: Actual Simulink block diagram implementing the parallel comparison.*

### Comparative Scope Results
The simulation scope visualizes the dynamic behavior of both models under identical transient inputs:
- **Blue Curve (ANN):** Smooth, continuous nonlinear trajectory accurately tracking peak loads (~107 MW) and low-flow regimes (~8 MW).
- **Yellow Curve (Fuzzy FIS):** Rule-based approximation showing characteristic horizontal plateaus (e.g., around 62.5 MW due to membership function centroids) and stable linguistic zones.

![Scope Comparison Results](Screenshot%202026-11-16%20at%2018.32.05.png)
*Figure 5.3: Comparative scope output showing ANN (blue) vs. Fuzzy FIS (yellow) dynamic responses.*

---

## 6. Repository Structure

- `/matlab_scripts/` – MATLAB scripts for data generation, ANN training (`fitrnet`), and error analysis.
- `/fuzzy_models/` – Exported Fuzzy system file (`hydro_fuzzy_controller.fis`).
- `/simulink_models/` – Comparative Simulink block diagram (`hydro_simulink_model.slx`).
- `/resources/` – Official ENTSO-E capacity CSV file used for system-level context (`Date_ENTO-E_Hydropower_2025-2026.csv`).
- `/images/` – Visual assets, control surfaces, and Simulink diagrams.