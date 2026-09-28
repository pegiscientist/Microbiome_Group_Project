# Analysis Results
This directory contains results generated from the BIOT670I microbiome analyses.
Results may include summary tables, figures, and selected simulation outputs used to evaluate the statistical methods in the project.
Large intermediate files should not be committed unless they are necessary to reproduce or document the analysis.

## Phase 1: Null Calibration
Phase 1 evaluates the statistical pipeline under the null hypothesis, where no true association is present.

### Raw Type I Error
The figure below summarizes the empirical raw Type I error across repeated null simulations. The 5% reference level represents the nominal significance threshold.
![Phase 1 Raw Type I Error](phase1_raw_type1_error.png)

### Benjamini-Hochberg False Discovery
The figure below summarizes the probability of observing any false discovery after Benjamini-Hochberg multiple-testing correction across repeated null simulations.
![Phase 1 BH False Discovery](phase1_any_bh_false_discovery.png)
