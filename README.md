# MultiomeLatent

**Analysis code accompanying:**  
**"Context-Dependent Duality of a BCG-Induced HSPC Reprogramming Signature: Protective in Blood, Prognostic in Tumor"**  
* Sadia Ruhama*

This repository contains the analysis code used to derive and validate a BCG-induced hematopoietic stem and progenitor cell (HSPC) transcriptional program from paired **scRNA-seq** and **scATAC-seq** data using **MOFA+**. The study integrates multimodal single-cell analysis with external validation across independent immune and bladder cancer cohorts.

---

## Dataset

- **Discovery cohort:** GSE295308 (paired scRNA-seq + scATAC-seq from BCG-treated NMIBC patient HSPCs)
- **External validation cohorts:**
  - TCGA-BLCA (survival analysis)
  - GSE199471 (baseline vs. recurrence)
  - GSE184241 (healthy BCG-vaccinated donor monocytes)

---

# Pipeline

```
RNA preprocessing.py
        │
        ▼
ATAC.py
        │
        ▼
Cell Annotation.py
        │
        ▼
MOFA+.py
        │
        ├──────────────► Biological Validation.py
        │
        ├──────────────► tcga_blca_validation (1).py
        │
        ├──────────────► gse199471_recurrence_validation (2).py
        │
        └──────────────► gse184241_external_cohort_for_trained_immunity_validation (6).py
```

---

## Script Overview

### 1. `RNA preprocessing.py`

- Performs preprocessing


---

### 2. `ATAC.py`

Single-cell ATAC preprocessing.

Includes

- Quality control
- Peak calling
- Peak filtering
- Peak matrix construction
- Creation of the ATAC AnnData object used downstream

---

### 3. `Cell Annotation.py`

Cell clustering and annotation.

Includes

- Leiden clustering
- Marker-gene identification
- Manual cell-type annotation
- Selection of the HSPC population

The discovery analysis selected **cluster 6** after biological marker inspection rather than using an automated rule.

---

### 4. `MOFA+.py`

Core multimodal integration pipeline.

Performs

- Joint MOFA+ factor analysis
- RNA + ATAC integration
- Factor quality assessment
- Technical confound testing
- Signature gene identification
- Signature peak identification
- Multiple signature definitions
  - Core
  - Standard-100
  - Extended
  - Signed
- Leave-one-patient-out retraining
- AUCell scoring
- GSEA
- Over-representation analysis (ORA)

Outputs generated here are consumed by all downstream validation scripts.

---

### 5. `Biological Validation.py`

Internal validation using the discovery cohort.

Reads outputs produced by **MOFA+.py** to verify the biological consistency of the identified signature.

---

### 6. `tcga_blca_validation (1).py`

External validation in **TCGA-BLCA**.

Includes

- Signature scoring
- Kaplan–Meier survival analysis
- Cox proportional hazards modeling

Uses the signature generated in Step 4.

---

### 7. `gse199471_recurrence_validation (2).py`

Independent validation in **GSE199471**.

Compares

- Baseline tumors
- Recurrent tumors

using the derived BCG signature.

---

### 8. `gse184241_external_cohort_for_trained_immunity_validation (6).py`

Validation in an independent trained immunity dataset.

Evaluates the signature in

- Healthy BCG-vaccinated donors
- Monocyte populations

---



This repository is provided primarily for **methodological transparency** .



# Dependencies

### Python

- scanpy
- anndata
- mudata
- mofapy2 (MOFA+)
- scikit-learn
- scipy
- statsmodels
- gseapy

### R

- Seurat





---

## Code Availability

This repository is released to provide methodological transparency and document the statistical analyses used in the accompanying manuscript. It is intended as a reference implementation of the analytical workflow.
