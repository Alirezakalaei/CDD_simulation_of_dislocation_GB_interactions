# Continuum Dislocation Dynamics: ADD Framework for Grain Boundary Interactions

This repository contains the MATLAB source code for simulating the effect of misorientation angles and grain size on the yield stress of crystalline materials. It utilizes a mesoscopic Continuum Dislocation Dynamics (CDD) model known as **"all-dislocation" density (ADD) dynamics**. 

This code forms the foundational data representation for Chapter 4 of my thesis and the associated publication in the *International Journal of Plasticity*.

## Background and Motivation

Grain boundaries (GBs) play a crucial role in the plasticity of polycrystalline materials. During plastic deformation, dislocations interact with GBs in various ways—they may transfer to adjacent grains, reflect back, or be absorbed—depending on the GB's properties. Historically, understanding these effects has predominantly relied on atomistic simulations, which are fundamentally limited in their spatiotemporal scales.

To bridge this gap, this repository exploits the ADD dynamics framework to simulate plasticity within grains and at GBs at the mesoscale. 

## Key Scientific Features

Based on our mathematically robust framework, this code implements:
* **Flux Boundary Condition:** A rigorously derived boundary condition for ADD that computes the dislocation density flux at the grain boundary, accounting for factors affecting intergranular slip transfer.
* **Rhombus Mesh Implementation:** Numerical implementation of the ADD framework on a rhombus mesh structure to accurately capture spatial dislocation flow.
* **Versatile Loading Conditions:** Capable of examining slip transfer under constant stress, constant stress rate, and constant strain rate conditions (this specific repository highlights the constant strain rate implementation).
* **Physical Phenomena Captured:**
  * **Hall–Petch Effect:** Demonstrates that plastic strain is inversely proportional to the square root of the grain size.
  * **Misorientation Effects:** Shows that lower misorientation angles lead to higher plastic strain and mobile dislocation density, while strain hardening intensifies with increasing GB misorientation due to reduced slip transfer.
  * **Hardening Mechanisms:** Investigates how dislocation mobility and initial dislocation density influence lattice resistance and GB strengthening.

## Repository Structure

### Main Scripts
* **`main_code.m`**: The entry point of the simulation. It initializes the parallel pool, loops over various misorientation angles, runs the CDD simulation, and generates Stress-Strain and Yield Stress plots.
* **`DDFD_single_plane.m`**: The core simulation setup for a single slip plane. It handles mesh generation, coarse-graining of initial dislocations, boundary conditions, and the main time-integration loop for strain rate control.
* **`comput_sec.m`**: The numerical solver step. It calculates dislocation velocities, fluxes, partial differential equation (PDE) updates, nucleation, annihilation, and resulting strains for each time step.

### Required Dependencies
To run the main scripts, the following custom MATLAB functions must be present in the working directory (included in this repository):
* `annihilation.m`, `border_maker.m`, `cthetacoarse.m`, `difff7.m`, `discoarse1.m`, `distance2curve.m`, `flux_flow_interaction2.m`, `grain_point_interaction.m`, `interaction_coeff_o.m`, `interaction_lr_mirror_o.m`, `local_max.m`, `strain_finder.m`, `tau_calculator1.m` (and/or `Tau_calculator1.m`), `theta_diff2.m`, `thetadiff.m`, `vel_modifier.m`.

**Data Files:**
* `initial_dislocation_8000b.mat`: Contains the initial dislocation configuration (`lines` cell array).

## System Requirements

* **MATLAB**: Recommended version R2021a or newer.
* **Parallel Computing Toolbox**: Required for the `parfor` loop in `main_code.m`.
* **Image Processing Toolbox**: Required for the `imgaussfilt` function used in velocity smoothing.
* **Statistics and Machine Learning Toolbox**: Required for the `normpdf` function used in nucleation spread.

## Usage

1. Clone the repository to your local machine.
2. Open MATLAB and navigate to the repository folder.
3. Ensure all required `.m` files and the `.mat` data file are in the current path.
4. Open `main_code.m`.
5. Adjust the `parpool` size according to your machine's hardware capabilities (default is set to 80 workers).
6. Run `main_code.m`.

*Note: The simulation is computationally intensive. Depending on your hardware and the number of parallel workers, it may take significant time to complete.*

## Outputs

The simulation will generate:
1. **Data Files**: `.mat` files (e.g., `result_summary_little_lower_strain.mat`) containing the history of stress, strain, dislocation density (`QQ`), and other state variables.
2. **Figures**:
   * Stress-Strain curves for various misorientation angles.
   * A plot of Yield Stress (\(\sigma / \mu\)) versus Misorientation Angle.

## Citation

If you use this code or framework in your research, please cite the following paper:

```bibtex
@article{KALAEI2026104554,
  title = {A flux boundary condition for grain boundary-dislocation interaction using all-dislocation density dynamics (ADD)},
  journal = {International Journal of Plasticity},
  volume = {196},
  pages = {104554},
  year = {2026},
  issn = {0749-6419},
  doi = {https://doi.org/10.1016/j.ijplas.2025.104554},
  author = {Alireza Kalaei and Jinxin Yu and Jian Han and David J. Srolovitz and Alfonso H.W. Ngan}
}
