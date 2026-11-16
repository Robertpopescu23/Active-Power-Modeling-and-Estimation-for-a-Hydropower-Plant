
# Active Power Modeling and Estimation for a Hydropower Plant

## Comparing Fuzzy Inference Systems (FIS) and Regression Neural Networks (ANN) in MATLAB & Simulink

---

## 1. Introduction and Objectives

* **Context:** The electrical power system requires accurate and rapid estimation of active power generation from renewable sources, particularly hydropower plants, to ensure grid stability within the Romanian National Power System (SEN).
* **Objective:** Build, train, and compare two distinct artificial intelligence approaches—a **Fuzzy Inference System (FIS)** and a **Regression Neural Network (ANN)**—to predict active power output ($P$) based on hydraulic parameters.
* **Reference Data:** Models are calibrated and contextualized using official capacity data from the **ENTSO-E Transparency Platform**, specifically considering installed capacities in Romanian water reservoirs (~3.4 GW) and run-of-river plants (~2.7 GW).

---

## 2. Engineering Problem Description

The system models a representative hydropower generation unit governed by the fundamental hydraulic power equation:

$$
P = \eta \cdot \rho \cdot g \cdot Q \cdot H
$$

* **Input Variables ($X$):**
  * Water Discharge ($Q$) [$m^3/s$]
  * Hydraulic Head / Water Level ($H$) [$m$]
* **Output Variable ($Y$):**
  * Generated Active Power ($P$) [$MW$]

---

## 3. Methodology 1: Fuzzy Inference System (FIS)

* Developed using MATLAB's `fuzzyLogicDesigner` tool.
* Configured as a Mamdani-type fuzzy inference system with:
  * **2 Inputs:** Discharge ($Q$) and Head ($H$), each split into linguistic terms (e.g., *Low, Medium, High*).
  * **1 Output:** Active Power ($P$).
  * **Rule Base:** 9 intuitive IF-THEN rules reflecting expert engineering heuristics.

---

## 4. Methodology 2: Regression Neural Network (ANN)

* **Training Data:** Synthetic dataset generated using physical laws combined with realistic Gaussian noise to simulate sensor variations.
* **Architecture:** Feed-forward neural network for regression trained using the modern `fitrnet` function in MATLAB.
* **Performance Metric:** Evaluated using Mean Squared Error (MSE) and visual regression plots comparing targets versus predictions.

---

## 5. Simulink Implementation

* An integrated Simulink model (`.slx`) running both the Fuzzy controller and the trained Neural Network block in parallel.
* Inputs are fed simultaneously to both blocks, and outputs are visualized in real-time via a Scope block to evaluate dynamic responses and differences.

---

## 6. Repository Structure

* `/matlab_scripts/` – MATLAB scripts for data generation, ANN training (`fitrnet`), and error analysis.
* `/fuzzy_models/` – Exported Fuzzy system file (`.fis`).
* `/simulink_models/` – Comparative Simulink block diagram (`.slx`).
* `/data/` – Official ENTSO-E capacity CSV file used for system-level context.
