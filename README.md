# Biotransport Course Tasks (SBEG201)

## Course Overview

The **Biotransport (SBEG201)** course introduces the fundamental transport phenomena that govern biological and biomedical systems. The course integrates principles of **fluid mechanics**, **heat transfer**, and **mass transfer**, focusing on how momentum, energy, and mass are transported in living systems. Throughout the course, theoretical foundations are linked to real biomedical applications through analytical problem solving and computational simulations.

The course is divided into three main parts: biofluid mechanics, bioheat transfer, and mass transfer.

---

## Biofluid Mechanics

In the **first part of the course**, the focus is on **biofluid mechanics**, which studies the behavior of fluids in biological systems, particularly blood flow in the cardiovascular system. This part is based mainly on lecture slides and the textbook **_Biofluid Mechanics: Principles and Applications_**, in addition to core fluid mechanics concepts.

Topics covered include fluid properties, stress and strain in fluids, viscosity, Newtonian and non-Newtonian fluid behavior, pressure, shear stress, and velocity profiles. Fundamental conservation laws such as **conservation of mass**, **linear momentum**, and the **Navier–Stokes equations** are introduced and applied to biological flows. Dimensionless analysis, especially the **Reynolds number**, is used to characterize flow regimes and understand laminar and disturbed flows in vessels.

These concepts are later applied using computational simulations to analyze velocity fields, pressure distributions, wall shear stress, and flow disturbances in idealized arterial geometries.

---

## Task 01: Biofluid Mechanics Simulation Task

**Task 01** applies the biofluid mechanics concepts studied in the first part of the course. The task focuses on modeling and simulating fluid flow in an idealized arterial geometry.

The task includes simulation results that illustrate velocity contours, pressure distributions, and flow behavior inside the artery. The folder structure contains simulation figures, source files used for the analysis, graphical user interface (GUI) files, and screenshots for visualization. The report explains the problem formulation, assumptions, simulation setup, and interpretation of the results.

This task demonstrates the application of theoretical biofluid mechanics concepts to a computational biomedical problem.


**Folder explanation:**

- **src**  
  Contains the MATLAB source code used for calculations, plotting velocity profiles, and friction factor analysis.

- **TaskGUI**  
  Includes the MATLAB graphical user interface files that allow interactive input of fluid properties and automatic flow regime identification.

- **report**  
  The final written report describing the methodology, theory, results, discussion, and conclusions of the task.

- **Simulation Pictures** and **Simulation Pictures2/**  
  CFD simulation results showing velocity contours, pressure distributions, and flow development in different geometries.

- **screenshots of app**  
  Screenshots of the MATLAB application interface and generated plots.

- **statement**  
  The original assignment description and task requirements provided for Task 01.

---

## Task 02: Stenosis Flow Analysis (Newtonian vs Non-Newtonian)

**Task 02** extends the biofluid mechanics analysis by studying blood flow in an arterial model with different degrees of stenosis. The task compares **Newtonian and non-Newtonian flow models** under multiple conditions, including normal geometry, mild stenosis, and severe stenosis.

The **Figures** folder contains simulation outputs such as velocity contours, pressure contours, total pressure distributions, velocity streamlines, recirculation regions, and wall shear stress results. These figures are organized according to stenosis severity and rheological model.

The **report** folder includes the technical report describing the modeling approach, simulation methodology, and analysis of the results. The **statement** folder contains the task description and requirements.

This task highlights the impact of stenosis severity and blood rheology on flow patterns, pressure losses, and hemodynamic quantities, reinforcing the importance of accurate modeling in biomedical fluid mechanics.


**Folder explanation:**

- **Figures**  
  Contains all CFD post-processing results, including:
  - No stenosis case (0%)
  - Mild stenosis case (50%)
  - Severe stenosis (Newtonian) (75%)
  - Severe stenosis (Non-Newtonian) (75%)
  - Pressure drop plots
  - Velocity contours, streamlines, recirculation zones, and wall shear stress visualizations

- **report**  
  The final report presenting the simulation setup, assumptions, results, analysis, and biomedical interpretation.

- **statement**  
  The original assignment description and task requirements provided for Task 02.


---
## Task 03: Transient Bioheat Transfer in Multilayer Human Skin (Cryotherapy)

**Task 03** focuses on the **bioheat transfer** part of the course. This task investigates **transient heat conduction in multilayer human skin during cryotherapy**, with emphasis on how cooling evolves with **time and depth** under surface cooling conditions.

A three-dimensional numerical model is used to represent a simplified skin structure composed of the **epidermis, dermis, and subcutaneous fat** layers. The tissue is initially at body temperature and exposed to surface cooling through a convective boundary condition for **10 minutes**. The analysis examines temperature evolution within each layer and evaluates the effect of the **convective heat transfer coefficient** on cooling rate and penetration depth.

A **baseline cooling case** is used for detailed visualization, while additional cases with different convective coefficients are analyzed comparatively. Results include temperature–depth profiles, temperature–time histories at selected depths, cross-sectional temperature contours, and quantitative metrics such as minimum temperature, time to reach clinical thresholds (10°C and 15°C), and cooling penetration depth.

This task demonstrates the application of transient heat transfer principles to a biomedical problem and highlights the insulating role of subcutaneous fat and the dominant influence of surface convection on superficial tissue cooling.

---

### Folder explanation:

- **Figures**  
  Contains all simulation results organized by convective cooling case:
  - **Case 1** – Baseline cooling (h = 50 W/m²·K)
  - **Case 2** – Mild cooling (h = 20 W/m²·K)
  - **Case 3** – Aggressive cooling (h = 100 W/m²·K)

  Each case folder includes:
  - Temperature vs. depth plots at selected times (0, 60, 300, 600 s)
  - Temperature vs. time plots at selected depths (epidermis, dermis, fat)
  - Cross-sectional temperature contour at t = 600 s
  - Extracted numerical data used for post-processing and analysis

- **report**  
  The final technical report describing the model formulation, governing equations, numerical setup, results, comparative analysis across convective coefficients, and physiological interpretation of the cooling behavior.

- **statement**  
  The original task description and requirements provided for Task 03.

---
## Team Members

- Yomna Sabry  
- Zeyad Hamed  
- Abdelrahman Emad  
- Sulaiman Alfozan  
- Hamdy Ahmed  
---

Contact me if needed: yomnasabry252@gmail.com
