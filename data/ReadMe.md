# Data Directory — Project 3 (m-height Prediction)

This directory contains all datasets, intermediate artifacts, and model outputs used in **Project 3 for CSCE 636: Deep Learning**.

The data is provided as part of the course assignment and is used strictly for academic and research purposes.

---

##  Directory Contents

###  Input Data
- **Training input pickle**
  - Contains tuples of the form:
    ```
    (n, k, m, P)
    ```
    where:
    - `n = 9` (fixed)
    - `k ∈ {4, 5, 6}`
    - `m ∈ {2, ..., n − k}`
    - `P` is a real-valued generator matrix of shape `(k, n − k)`

- **Training output pickle**
  - A list of corresponding **m-height values**
  - Target values are strictly positive and span several orders of magnitude

---

##  Prediction Target

The learning target is the **m-height** of matrix `P`, denoted as:

y = m-height(P)


To stabilize training, the models predict:

z = log2(y)


All evaluation metrics are reported in **log2 space**.

---

##  Derived Artifacts

Depending on the experiment, this directory may also contain:

- Preprocessed feature matrices (e.g., SVD-64 invariant features)
- Group-aware train/validation/test split indices
- Saved prediction outputs (`.pkl`)
- Calibration parameters for ensemble models

---

##  Model Outputs

- **Prediction files**
  - Stored as pickled lists of predicted m-height values
  - No post-processing or thresholding is applied
  - Final predictions are converted back from log2 space

---

##  Notes on Reproducibility

- All data splits are **group-aware and deterministic**
- No validation or test leakage occurs during augmentation
- Random seeds are fixed globally across experiments

---

## Usage Policy

This dataset is provided exclusively for coursework under:

**CSCE 636 – Deep Learning**  
**Texas A&M University**

Redistribution or commercial use is not permitted.

---

##  Author

**Shruti Singh**  
M.S. Management Information Systems  
Texas A&M University

---

##  Questions

For questions regarding data format, preprocessing, or model usage, please refer to the main project README or the accompanying notebooks.
