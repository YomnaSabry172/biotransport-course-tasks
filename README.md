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

- **src/**  
  Contains the MATLAB source code used for calculations, plotting velocity profiles, and friction factor analysis.

- **TaskGUI/**  
  Includes the MATLAB graphical user interface files that allow interactive input of fluid properties and automatic flow regime identification.

- **report/**  
  The final written report describing the methodology, theory, results, discussion, and conclusions of the task.

- **Simulation Pictures/** and **Simulation Pictures2/**  
  CFD simulation results showing velocity contours, pressure distributions, and flow development in different geometries.

- **screenshots of app/**  
  Screenshots of the MATLAB application interface and generated plots.

- **statement/**  
  The original assignment description and task requirements provided for Task 01.

---

## Task 02: Stenosis Flow Analysis (Newtonian vs Non-Newtonian)

**Task 02** extends the biofluid mechanics analysis by studying blood flow in an arterial model with different degrees of stenosis. The task compares **Newtonian and non-Newtonian flow models** under multiple conditions, including normal geometry, mild stenosis, and severe stenosis.

The **Figures** folder contains simulation outputs such as velocity contours, pressure contours, total pressure distributions, velocity streamlines, recirculation regions, and wall shear stress results. These figures are organized according to stenosis severity and rheological model.

The **report** folder includes the technical report describing the modeling approach, simulation methodology, and analysis of the results. The **statement** folder contains the task description and requirements.

This task highlights the impact of stenosis severity and blood rheology on flow patterns, pressure losses, and hemodynamic quantities, reinforcing the importance of accurate modeling in biomedical fluid mechanics.


**Folder explanation:**

- **Figures/**  
  Contains all CFD post-processing results, including:
  - No stenosis case
  - Mild stenosis case
  - Severe stenosis (Newtonian)
  - Severe stenosis (Non-Newtonian)
  - Pressure drop plots
  - Velocity contours, streamlines, recirculation zones, and wall shear stress visualizations

- **report/**  
  The final report presenting the simulation setup, assumptions, results, analysis, and biomedical interpretation.

- **statement/**  
  The original assignment description and task requirements provided for Task 02.


---
## Team Members

- Yomna Sabry  
- Zeyad Hamed  
- Abdelrahman Emad  
- Sulaiman Alfozan  
- Hamdy Ahmed  
---

Contact me if needed: yomnasabry252@gmail.com
