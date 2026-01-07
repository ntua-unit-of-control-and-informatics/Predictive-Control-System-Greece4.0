# Predictive-Control-System-Greece4.0
## Overview
This repository documents a widget-driven, end-to-end workflow to generate RBFNNs as predictive models in a control framework and to integrate them into a nonlinear Model Predictive Control (MPC) scheme for closed-loop simulation and analysis, developed for the project "Greece4.0: Network of Excellence for the Development, Dissemination and Application of Digital Transformation Technologies in the Greek Manufacturing Industry".

## Table of Contents
- [Overview](#overview)
- [Notebooks](#notebooks)
- [Recommened Workflow](#recommended-workflow)
- [Execution Environment](#execution-environment)
- [License](#license)

The repository is organized around two main notebooks, each corresponding to a distinct stage of the control pipeline:
1. Predictive model generation (RBFNN training and benchmarking)
2. Closed-loop control using MPC with trained RBFNN models


## Notebooks

### 1. Predictive_RBFNNs_for_Control_Systems_Greece4_0.ipynb  

This notebook is used to **generate and evaluate RBFNN predictive models** from historical process data.

**Main functionality**
- Dataset loading and visualization  
- Automatic detection of input/output signals  
- Signal normalization  
- Time-series feature construction using past-value embedding  
- Training multiple RBFNN candidates with different fuzzy subset counts  
- Train / Validation / Test evaluation  
- Visualization of predictions and residuals  
- Performance reporting (e.g. R²)  
- Export of trained models to `.mat` files for downstream use in MPC  

**Required inputs**
- A dataset file (`.csv`, `.xlsx`, or `.xls`) containing:
  - Input signals (columns containing “input”)
  - Output signals (columns containing “output”)
  - A time column (optional but recommended)

**Outputs**
- Trained RBFNN models exported as:
  - `RBF_plant.mat` — used as the plant model
  - `RBF_mpc.mat` — used inside the MPC formulation

These files are **required** by the MPC notebook.

---

### 2. MPC_RBFNN_Control_System_Greece4_0.ipynb  
This notebook implements a **closed-loop nonlinear MPC controller** using the RBFNN models trained in the previous step.

The workflow is fully widget-driven and divided into logical sections.

**Main functionality**
- Loading RBFNN models (`RBF_plant.mat`, `RBF_mpc.mat`)
- Initial steady-state estimation (from dataset or manually)
- MPC tuning:
  - Tracking weights (Q)
  - Move suppression weights (R)
  - Soft constraint penalties
  - Control and prediction horizons
- Case study configuration:
  - Simulation time and sampling time
  - Setpoints
  - Output bounds
  - Input bounds
  - Feature toggles (SP tracking, soft constraints)
- Closed-loop simulation using CasADi-based optimization
- Visualization of outputs, inputs, setpoints, and constraints

**Required inputs**
- `RBF_plant.mat`
- `RBF_mpc.mat`

These are typically generated using  
`Predictive_RBFNNs_for_Control_Systems_Greece4_0.ipynb`.

Optionally:
- A dataset file for initializing steady-state conditions

**Outputs**
- Closed-loop simulation results stored in memory
- Interactive plots of:
  - Outputs vs setpoints
  - Inputs with bounds

No files are overwritten unless explicitly saved by the user.

---

## Recommended Workflow

1. Prepare your dataset with clearly labeled input and output signals  
2. Run the RBFNN training notebook to generate predictive models  
3. Export the trained models to `.mat` files  
4. Run the MPC notebook
5. Load the exported models, tune the controller, define a case study, and simulate

---

## Execution Environment

The notebooks are designed to run in:
- **Google Colab** (recommended)
- Local Jupyter environments with:
  - `numpy`, `scipy`, `pandas`
  - `matplotlib`
  - `ipywidgets`
  - `casadi`

> **Note:** GitHub preview does not execute widgets.  
> For full functionality, download the notebooks and run them locally or in Colab.

---

## License
This project is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License**.
See the file `LICENSE-CC-BY-NC-SA.txt` for details.
