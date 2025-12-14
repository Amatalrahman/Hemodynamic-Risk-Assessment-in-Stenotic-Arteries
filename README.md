
# Hemodynamic Risk Assessment in Stenotic Femoral Artery
**Course:** SBEG201 – Biotransport  
**Department:** Systems and Biomedical Engineering  
**Software:** ANSYS Fluent (Student Version R2025)  


---

## 📌 Project Overview
In clinical practice, pressure drops across stenotic arteries are often estimated using simplified mathematical relations such as Bernoulli’s equation. However, these approaches neglect critical physiological factors including blood viscosity, flow separation, and turbulence, leading to systematic underestimation of hemodynamic risk.

This project employs **Computational Fluid Dynamics (CFD)** using **ANSYS Fluent** to investigate blood flow through stenotic arteries and to demonstrate why simplified analytical models fail in diseased conditions.

A comparative study is conducted on the **Common Femoral Artery (CFA)** under varying stenosis severities and rheological assumptions.

---

## 🎯 Objectives
- Quantify pressure drop variations with stenosis severity.
- Examine the non-linear relationship between stenosis percentage and pressure loss.
- Compare Newtonian and Non-Newtonian blood models.
- Analyze Wall Shear Stress (WSS) and recirculation zones.
- Demonstrate the physiological implications of incorrect modeling assumptions.

---
```
Hemodynamic-Risk-Assessment/
│
├── README.md
│
├── Report/
│   ├── Hemodynamic Risk Assessment in Stenotic Femoral Artery.pdf
│   └── Figures/
│
├── Geometry&Meshing/
│   ├── Femoral_Geom&Mesh_3Cases.wbpj
│   └── Femoral_Geom&Mesh_3Cases_files/
│
├── Newtonian_Model/
│   ├── Normal/
│   ├── Moderate_Stenosis/
│   ├── Severe_Stenosis/
│   └── Pressure_vs_Stenosis.txt
│
├── Non_Newtonian_Model/
│   ├── Velocity_Streamlines.png
│   ├── WSS_NonNewtonian.xlsx
│   └── WSS.png
│
└── Resources.docx
```
---

## 🧠 Methodology Overview

### 🔹 Artery Selection
The **Common Femoral Artery (CFA)** was selected due to its clinical relevance in **Peripheral Arterial Disease (PAD)** and its susceptibility to atherosclerotic plaque formation.

- Diameter: **6.9 mm**
- Inlet Velocity: **0.153 m/s**
- Blood Density: **1060 kg/m³**
<img src="https://github.com/user-attachments/assets/bac0ec42-0dc4-42b3-b4a4-08ac0a82aa26" width="200" height="auto"/>


All physiological inputs are justified using peer-reviewed literature.

---
## 🧩 Geometry Creation & Meshing

### Geometry Construction
The arterial geometry was developed using an **axisymmetric modeling approach** to reduce computational cost while preserving the essential three-dimensional flow physics.  
A **2D planar profile** representing half of the arterial cross-section was first created and then **revolved about the central axis** to generate the full 3D fluid domain.
<img width="1431" height="753" alt="Image" src="https://github.com/user-attachments/assets/4a5866e9-1a72-409c-a295-80ef671aa7bd" />
<img width="1257" height="584" alt="Image" src="https://github.com/user-attachments/assets/c4868034-1899-4157-9cbf-cfe9aeca9f0c" />

To enable precise boundary condition assignment and localized mesh control, the artery was divided into **three longitudinal regions**:
- **Inlet section**
- **Stenosis region**
- **Outlet section**
<img width="1436" height="756" alt="Image" src="https://github.com/user-attachments/assets/aa2f25f7-8a79-46af-a889-8fe898b9f72e" />

Each boundary surface was defined as a **named selection**, including:
- inlet  
- outlet  
- Healthy wall
- Stenosis Wall 
- Symmetry Wall  

The stenotic wall region was treated separately from the healthy wall to allow **targeted refinement in the diseased segment**.


---

### Stenosis Definition
Stenosis severity was defined based on **percentage cross-sectional area reduction**, according to:

**% Area Reduction** = (1 − *A*ₜₕᵣₒₐₜ / *A*ᵢₙₗₑₜ) × 100


Using this definition, three arterial models were generated:
- **Healthy artery:** 0% stenosis  
- **Moderate stenosis:** 50% area reduction  
- **Severe stenosis:** 75% area reduction  

To ensure biological realism and avoid artificial flow disturbances, the stenosis geometry was implemented using a **smooth cosine-shaped profile**, rather than sharp geometric transitions.

<img width="2297" height="603" alt="Image" src="https://github.com/user-attachments/assets/00ef5c52-0eca-4bab-b917-2708f884973e" />

---

### Mesh Generation Strategy
Mesh generation was performed using the **MultiZone meshing method** combined with a **sweepable body approach**, enabling the use of predominantly **hexahedral elements**.

Hexahedral elements were selected due to their:
- Superior numerical accuracy  
- Improved convergence behavior  
- Enhanced resolution of velocity and shear gradients  

The mesh strategy included:
- **Global element size:** 0.3 mm  
- **Body sizing:** 0.2 mm for improved mesh uniformity  
- **Local face sizing at stenosis wall:** \(5 \times 10^{-2}\) mm to resolve high velocity and pressure gradients at the throat and post-stenotic region  

![Image](https://github.com/user-attachments/assets/9e4eef1c-074e-4e36-b111-869196bd91b6)
---

### Near-Wall Refinement (Inflation Layers)
To ensure accurate computation of **Wall Shear Stress (WSS)**, inflation layers were applied along all arterial walls.

Inflation settings:
- **First layer thickness:** \(5 \times 10^{-3}\) mm  
- **Number of layers:** 8  
- **Growth rate:** 1.2  

This configuration ensured proper resolution of the **viscous sublayer**, allowing reliable capture of near-wall velocity gradients critical for WSS analysis.


---

### 🔹 Geometry Models
Four computational models were developed:

| Model | Description | Blood Model |
|-----|------------|-------------|
| Model 1 | Healthy (0% stenosis) | Newtonian |
| Model 2 | Moderate (50% stenosis) | Newtonian |
| Model 3 | Severe (75% stenosis) | Newtonian |
| Model 4 | Severe (75% stenosis) | Non-Newtonian (Carreau) |

---

### 🔹 Blood Rheology
- **Newtonian Model:**  
  Constant viscosity:  
  \[
  \mu = 0.0035 \, \text{kg/m·s}
  \]

- **Non-Newtonian Model:**  
  Carreau viscosity model to capture shear-thinning behavior:

| Parameter | Value |
|---------|-------|
| Zero-shear viscosity (μ₀) | 0.056 kg/m·s |
| Infinite-shear viscosity (μ∞) | 0.00345 kg/m·s |
| Time constant (λ) | 1.902 s |
| Power-law index (n) | 0.22 |

---

### 🔹 Flow Regime & Solver
- **Pressure-Based Solver**
- **Laminar Flow:** Healthy, Moderate, and Non-Newtonian Severe cases
- **Turbulent Flow:** Severe Newtonian case (k-ω SST), where \( Re > 2000 \)
- **Mesh Constraint:** ≤ 512,000 cells (ANSYS Student limit)

---

## 📊 Key Analyses

<img width="530" height="198" alt="Image" src="https://github.com/user-attachments/assets/ab9d673e-679d-43ab-9b00-3a44c8545881" />
<img width="523" height="195" alt="Image" src="https://github.com/user-attachments/assets/10d8da96-b5af-41f6-9a27-caf263a160f2" />
### 1️⃣ Severity Study (Young’s Curve)
- Pressure drop vs. stenosis percentage
- Strong non-linear behavior observed
- Small increases in stenosis lead to disproportionately large pressure losses

### 2️⃣ Rheological Comparison
- Velocity streamlines comparison
- Recirculation zone length analysis
- Wall Shear Stress (WSS) distribution
- Identification of low-WSS regions prone to plaque progression

---

## 🧬 Clinical Implications
- Non-linear pressure behavior explains sudden cardiovascular events despite mild symptoms.
- Newtonian assumptions can misestimate WSS, affecting disease progression predictions.
- Low-WSS regions correlate strongly with further plaque accumulation.

---

## ⚠️ Assumptions & Limitations
- Rigid arterial walls
- Steady-state flow
- No fluid–structure interaction
- Resting physiological conditions only

Each assumption introduces modeling errors that are discussed in the final report.

---

## 📚 References
1. Nichols, W. W., O’Rourke, M. F., & Vlachopoulos, C., *McDonald's Blood Flow in Arteries*, 6th ed., 2011.  
2. Ku, D. N., “Blood flow in arteries,” *Annual Review of Fluid Mechanics*, 1997.  
3. Fung, Y. C., *Biomechanics: Circulation*, Springer, 1997.

---

## 👥 Team Contribution
This project was completed as a **group assignment (5 members)**.
1. Amatalrahman Sayed 
2. Alaa Essam
3. Engy Elsarta
4. Youssef Magdy
5. Youssef Mojahed
---

## 🛠 Software
- ANSYS Workbench & Fluent (Student Version R2025)
---
## 📎 Learning Resources
Educational materials and tutorials used to support the modeling and simulation workflow are documented separately:

➡️https://docs.google.com/document/d/1wKA1NrtM9OKQweMLkNLeD_OG-EYKFQnhS25aCXq0p9s/edit?tab=t.0
