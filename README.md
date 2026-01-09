# Breast-Cancer-RWD-ETL
An ETL and data engineering workflow for the PRJNA1190437 dataset, integrating spatial mFISHseq with breast cancer Real-World Data (RWD) to resolve biomarker discordance and optimize treatment grouping.
# Spatially-Informed Data Engineering for Breast Cancer RWD

This repository documents an engineering-first approach to processing complex clinical and genomic data from the **PRJNA1190437** dataset.

## Key Engineering Contributions

* **Clinical-First ETL:** Prioritized clinical drug-regimen alignment (Excel-to-Python) to ensure 100% logic integrity before genomic merging.
* **RWD Standardization:** Resolved 1-to-N relationships between patients and tissue samples; standardized drug aliases and calculated treatment durations.
* **Spatial Feature Integration:** Mapped mFISHseq data to ROI coordinates to quantify HER2 spatial heterogeneity (moving beyond binary +/- classification).
* **Analytical Pivot:** Successfully matched data with the PBMF model for single-arm analysis. However, identified that single-arm biological significance was limited. Shifted focus to **High-Quality Data Curation** and the design of **Virtual Control Arms** to improve predictive power for ADC therapy (e.g., T-DXd).

##  Project Highlights

* **Data Source:** BioProject PRJNA1190437 (mFISHseq & Clinical RWD).
* **Core Logic:** Replaced simple "medication-based grouping" with "prior-knowledge-based stratification."
* **Gene Filtering:** Implemented a TPM screening pipeline focused on immune infiltration (CD8, NK, PD-L1).

## Use Case
Designed for medical data engineers to bridge the gap between messy raw clinical records and model-ready multi-omic datasets.
