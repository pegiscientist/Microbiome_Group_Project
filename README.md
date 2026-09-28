# BIOT670I Microbiome Group Project

This repository contains the code, analyses, documentation, and results for the BIOT670I microbiome group project (Capstone Project).

## Project Status

- Project planning: Complete
- Simulation framework: Complete
- Phase 1 – Null calibration: Complete
- Phase 2 – Signal simulation: Complete
- Phase 3 – Extended/real-data analysis: Next phase
- Final integration and interpretation: Planned

## Repository Structure

```text
Microbiome_Group_Project/
├── data/
│   ├── real/          # Reserved for approved real microbiome data
│   └── simulated/     # Generated microbiome simulation data
├── notebooks/
│   └── BIOT670I_Microbiome_Project.ipynb
├── results/
│   ├── phase1_final_calibration_summary.csv
│   ├── phase1_raw_type1_error.png
│   └── phase1_any_bh_false_discovery.png
├── scripts/           # Reusable analysis scripts and functions
├── .gitignore
└── README.md
```

## Phase 1 Results

Phase 1 evaluates the statistical analysis pipeline under the null hypothesis using repeated microbiome simulations.

Selected Phase 1 results, including empirical Type I error, Benjamini-Hochberg false-discovery behavior, and the final calibration summary, are available in the [`results`](results/) directory.
