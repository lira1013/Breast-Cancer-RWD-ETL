# Spatially-Informed Data Engineering for Breast Cancer RWD

This repository documents an engineering-first approach to processing complex multi-modal data (mFISHseq & Clinical RWD) from the **PRJNA1190437** dataset. 

The primary objective of this project is to **curate clean, model-ready datasets** from noisy Real-World Data (RWD) and develop a biology-driven feature engineering pipeline for biomarker discovery.

---

##  Data Engineering Workflow

### 1. Patient Entity Resolution (Fingerprinting Logic)
To resolve the lack of unified `Patient_ID` in RWD, I implemented a **Fingerprinting Algorithm** to reconstruct patient entities (derived from GSE181574):
* **Fingerprint Fields**: Used a 6-dimensional vector: `[Sex, Age, Menopausal Status, Geo-Location, Provider, Tumor Type]`.
* **Standardization**: Resolved inconsistent age strings and unified "missing" value representations.
* **ID Assignment**: Generated unique `Inferred_Patient_ID` (e.g., P00001) to enable precise 1-to-N (Patient-to-Sample) mapping for spatial analysis.

### 2. Stratified Gene Filtering (Biology-Driven Feature Selection)
I designed a tiered filtering strategy for TPM (Transcripts Per Million) data, specifically optimized for **Trastuzumab treatment & HER2-pathway** analysis:

* **Tier 1: Prior Core Genes**: Force-retained key biomarkers (e.g., ERBB2, ESR1) with a lenient threshold (TPM > 0) to ensure zero loss of known clinical markers.
* **Tier 2: HER2 Pathway Signaling**: For genes in the HER2 pathway (e.g., EGFR, PIK3CA, AKT1), implemented a sensitive threshold (TPM > 0.5 in ≥10% samples) to capture low-abundance regulatory signals.
* **Tier 3: Discovery Set**: For non-prior genes, applied a strict threshold (TPM > 1 in ≥10% samples) to eliminate noise, followed by **Variance Ranking** to select the top 500 highly variable genes (HVGs) capturing inter-sample heterogeneity.
* **Normalization**: Post-filtering, data underwent $log_2(TPM+1)$ transformation and Z-score normalization for downstream analysis.

### 3. Clinical-First ETL & Standardization
* **Hybrid Curation**: Utilized an Excel-to-Python flow to ensure 100% accuracy in drug-regimen alignment.
* **Temporal Logic**: Developed a logic for cumulative treatment duration (Duration Total) to handle the lack of granular dosing timestamps in RWD.

---

##  Project Structure

* **`/data`**: 
    - `prjna1190437_clinical_cleaned.xlsx`: Master Table with cleaned clinical features and `Inferred_Patient_ID`.
    - `Medication_Log_summary.xlsx`: Distribution of treatment cohorts and patient counts.
* **`/notebooks`**: 
    - `analysis_pipeline.ipynb`: Full implementation of the **Fingerprinting Logic** and the **Stratified Filtering Pipeline**.

---

##  Engineering Reflections & "The Pivot"

Initially, predictive modeling was explored. However, the project shifted focus toward **High-Fidelity Data Curation** based on the following engineering insights:

1. **Complexity vs. Validity**: Due to the inconsistent nature of PFS (Progression-Free Survival) and complex experimental designs in RWD, a "distribution-first" approach was taken.
2. **Visualizing Cohorts**: Instead of forced modeling on noisy labels, I focused on visualizing patient-treatment distributions and exploring the behavior of filtered gene sets.
3. **Biological Logic Over Algorithms**: The stratification logic (Prior vs. Pathway vs. Noise) provides more industrial value for drug response exploration than a "black-box" model on uncleaned data.

---

##  Future Exploration
*probably implementation of **Virtual Control Arms** to resolve cohort imbalances
* Refinement of the **Spatial Heterogeneity Score** as a continuous variable for ADC (e.g., T-DXd) response prediction.v
