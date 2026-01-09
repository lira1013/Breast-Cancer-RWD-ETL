# Spatially-Informed Data Engineering for Breast Cancer RWD

This repository documents an engineering-first approach to processing complex multi-modal data (mFISHseq & Clinical RWD) from the **PRJNA1190437** dataset. It focuses on resolving **Biomarker Discordance** through robust ETL pipelines and spatial-temporal alignment.

---

## Domain Background & Clinical Significance

### 1. Addressing "Biomarker Discordance"
Biomarker discordance (inconsistent HER2/ER/PR status across different tumor regions) is a major clinical pain point caused by **Intratumoral Heterogeneity**. 
* **Engineering Goal:** Transition from binary "HER2+/-" classification to a continuous spatial variable: **HER2 Heterogeneity Score**, **Clone Proportion**, and **Immune Co-localization**.

### 2. Strategic Value for ADC Therapy (e.g., T-DXd)
Modern Antibody-Drug Conjugates (ADCs) like T-DXd leverage the **"Bystander Effect"**, where cytotoxic payloads kill neighboring HER2-negative cells. 
* **Project Impact:** By mapping spatial distribution rather than just average expression, this pipeline helps identify "HER2-low" or heterogeneous patients who may still benefit from ADC treatment.

---

##  Data Engineering Workflow

### 1. Clinical-First ETL (The "Precision" Strategy)
* **Visual Verification:** Utilized a manual-to-automated curation flow (Excel-to-Python) to ensure 100% accuracy in drug-regimen alignment before genomic merging.
* **Integrity Constraint:** Implemented strict matching protocols to ensure no samples were deleted during the complex 1-to-N (Patient-to-Sample) mapping.

### 2. RWD Standardization & Temporal Logic
* **Drug Normalization:** Resolved redundant trade names into generic pharmaceutical entities.
* **Temporal Aggregation:** Developed a logic for cumulative treatment duration where specific dosing timestamps were unavailable in the RWD.
* **Control Framework:** Integrated specific control samples (e.g., Patient 1 Sample 39 as a Positive Control; 12-individual pool as Negative Control) to establish a "Credible Backbone" for the experimental findings.

### 3. Spatial-Genomic Integration
* **Prior-Knowledge Screening:** Designed a TPM filtering pipeline guided by immune infiltration markers (CD8, NK, PD-L1) and TME (Tumor Microenvironment) features.

---

##  Engineering Lessons & Maturity (The "Pivot")

Initially, a predictive model (PBMF) was implemented. However, it was **deprecated** based on the following engineering reflections:

* **Avoiding "Mixed Population" Bias:** Without a clear experimental design and control arm, single-arm modeling on raw RWD carries a high risk of confounding variables. 
* **Patient-Centric vs. Sample-Centric:** Modeling at the sample level incorrectly assumes independence, ignoring the spatial correlation within a single patient. 
* **Industrial ROI:** In medical data engineering, the complexity of a model should not exceed the reliability of the underlying grouping logic. I pivoted to focus on **High-Fidelity Curation** over algorithmic complexity.

---

##  Project Structure
* `/scripts`: TPM filtering and drug-mapping automation.
* `/metadata`: Logic for treatment-based patient stratification and inclusion criteria.
* `/docs`: Detailed notes on LCM (Laser Capture Microdissection) and WSI (Whole Slide Imaging) coordinate mapping.

##  Future Exploration
* Implementation of **Virtual Control Arms** to resolve the lack of a control cohort in RWD.
* Development of a "Spatial Heterogeneity Score" as a continuous feature for treatment response prediction.
