# NLDBD-Search-Using-Multi-Detector-Spectral-Analysis

# Search for Neutrinoless Double Beta Decay Using Multi-Detector Spectral Analysis

This repository contains the analysis code and results for a
multi-detector search for **neutrinoless double beta decay (0νββ)**
using real spectral data from four detectors (A, B, C, and Target). The
project implements multiple statistical inference methods---**1D
Bayesian**, **2D Bayesian**, and **Unbinned MLE**---to estimate the
possible NLDBD signal strength and derive 90% confidence-level (CL)
limits.

------------------------------------------------------------------------

## Dataset

The notebook expects four CSV files corresponding to four detectors:

| Detector | File | Columns |
|---------|------|---------|
| Detector A | `DetectorA.csv` | EventID, Score, Energy |
| Detector B | `DetectorB.csv` | EventID, Score, Energy |
| Detector C | `DetectorC.csv` | EventID, Score, Energy |
| Target Detector | `DetectorTarget.csv` | EventID, Score, Energy |

Each dataset contains calibrated event-level energy measurements and ML-based discrimination scores.


------------------------------------------------------------------------

## Project Objectives

1.  Identify spectral features near the 0νββ Q-value (≈2039 keV)
2.  Optimize discrimination thresholds using auxiliary background peaks
3.  Estimate NLDBD signal strength using multiple inference techniques
4.  Compute 90% CL upper limits
5.  Validate consistency across statistical methods

------------------------------------------------------------------------

## Environment Setup

### Install Dependencies

``` bash
conda create -n nldbdenv python=3.10
conda activate nldbdenv
pip install numpy scipy pandas matplotlib seaborn pymc arviz tqdm
```

### Directory Structure

    .
    ├── dsc291_project.ipynb
    ├── DetectorA.csv
    ├── DetectorB.csv
    ├── DetectorC.csv
    └── DetectorTarget.csv

------------------------------------------------------------------------

## Methodology Overview

### Background Peak Calibration

-   Detector A's **1592 keV** peak is used to optimize the ML score
    threshold
-   Detector B's **2103 keV** peak is used strictly for validation
-   This avoids optimization bias and ensures generalization

------------------------------------------------------------------------

### 1D Bayesian Analysis (Energy Only)

-   Binned Poisson likelihood in energy
-   Priors applied to background and signal rates
-   Posterior sampled using PyMC
-   Outputs:
    -   Posterior mean of signal rate
    -   90% credible upper limit

**Pros:** Stable, interpretable\
**Cons:** Ignores score information

------------------------------------------------------------------------

### 2D Bayesian Analysis (Energy + Score)

-   Joint 2D Poisson likelihood over (Energy, Score)
-   Incorporates ML discrimination power
-   More sensitive in theory, variance-dominated in practice

------------------------------------------------------------------------

### Unbinned Maximum Likelihood (MLE)

-   KDE-based continuous likelihood
-   Signal and background PDFs from calibration detectors
-   Maximizes likelihood to extract best-fit signal rate

------------------------------------------------------------------------

## Results Summary

| Method       | Signal Estimate θₙ | 90% CL Upper Limit | Role                 |
| ------------ | ------------------ | ------------------ | -------------------- |
| 1D Bayesian  | 7.74               | 14.81              | Primary result       |
| 2D Bayesian  | –                  | 27.77              | Advanced cross-check |
| Unbinned MLE | 8.34               | –                  | Robustness test      |


No statistically significant excess compatible with 0νββ was observed.

------------------------------------------------------------------------

## Running the Analysis

Open the notebook and run all cells sequentially:

    dsc291_project.ipynb

All figures, histograms, and posterior distributions will be generated
automatically.

------------------------------------------------------------------------

## Key References

-   Arnquist, I. J., et al. (Majorana Collaboration). *Majorana
    Demonstrator data release for AI/ML applications*. arXiv:2308.10856\
-   Agostini, M., et al. (GERDA Collaboration). *Background-free search
    for neutrinoless double beta decay*. Phys. Rev. Lett.\
-   Abgrall, N., et al. (LEGEND Collaboration). *Search for neutrinoless
    double beta decay with LEGEND-200*, Phys. Rev. Lett. 129\
-   Saleh, G., et al. (LEGEND Collaboration). *First results from
    LEGEND-200*, arXiv:2509.21166

------------------------------------------------------------------------

## Future Work

-   Full detector systematic modeling
-   Profile likelihood limit setting
-   Signal template from full MC
-   Multi-detector joint likelihood
-   Time-dependent background modeling
