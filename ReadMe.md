# m-Height Prediction using Deep Learning  
*(CSCE 636 – Deep Learning)*

---

##  Overview

This project addresses the problem of **predicting the m-height of generator matrices** using deep learning.  
The m-height is a mathematically defined quantity arising in coding theory and algebraic structures, and its exact computation is **computationally expensive** for large or complex matrices.

The goal of this project is to **learn an accurate surrogate model** that predicts the m-height directly from the generator matrix structure, achieving high accuracy while being orders of magnitude faster than exact methods.

This work was completed as **Project 3** for *CSCE 636: Deep Learning* and received an **A grade**.

---
## Problem Setting

Each training sample consists of:

- **A fixed parameter:**  
  $n = 9$

- **Variable parameters:**  
  $k \in \{4,5,6\}$, $m \in \{2,\dots,n-k\}$

- **A generator matrix:**  
  $P \in \mathbb{R}^{k \times (n-k)}$

The target value is the **m-height**, defined as:

- **m-height:**  
  $y = \mathrm{m\text{-}height}(P)$

Since the m-height spans several orders of magnitude, the model predicts the log-transformed value:

- **Prediction target:**  
  $z = \log_2(y)$

---

##  Modeling Approach

### Permutation-Invariant Feature Engineering (SVD-64)

A key challenge is that the m-height is **invariant under row and column permutations** of the matrix \(P\).  
To respect this structure, the project uses a **64-dimensional permutation-invariant feature representation**, derived from:

- Row and column norms
- Distributional statistics (mean, variance, inequality measures)
- Singular Value Decomposition (SVD)
- Spectral energy and entropy measures

This feature design ensures:
- Robustness to matrix permutations  
- Strong inductive bias aligned with the underlying mathematics  

---

###  Mixture of Experts (MoE) by \((k,m)\)

Instead of training a single global model, the problem is decomposed into **nine subproblems**, one for each valid \((k,m)\) pair.

For each pair:
- A dedicated neural network expert is trained
- Harder pairs use deeper architectures and Huber loss
- Easier pairs use lighter architectures

Predictions are combined using ensemble averaging, followed by pair-specific linear calibration:

**Calibration equation:**  
$z_{\text{cal}} = a \cdot z_{\text{pred}} + b$

This design significantly improves performance on difficult regimes.

---

###  Targeted Data Augmentation

To address data imbalance across \((k,m)\) pairs:

- **Train-only oversampling** is applied
- Hard pairs receive heavier augmentation
- Validation and test splits remain clean to ensure unbiased evaluation

Because the features are permutation-invariant, augmentation is **safe and leakage-free**.

---

###  Transformer Baseline (Exploratory)

A global **Transformer-based model** was also implemented using:

- Column-wise tokenization of matrix \(P\)
- Self-attention across columns
- Fusion with invariant features

While conceptually powerful, this model **did not outperform** the Mixture-of-Experts approach on the evaluation metric and was therefore not selected as the final submission.

---

##  Results

### Overall Performance

- **Test metric:**  
$\log_2\text{-MSE} = 0.838$

- Strong performance across all difficulty levels
- Particularly robust on hard \((k,m)\) pairs

The final model was validated against a **held-out test set provided by the instructor**, and predictions were verified for correctness and stability.

---

## Repository Structure

```text
├── Project3.ipynb        # Final, clean, runnable notebook
├── Proj3_SVD64_GlobalAug # Trained MoE models, scalers, calibration
├── README.md             # This file
```
---


##  Key Takeaways

- **Domain-aware feature engineering** can outperform generic deep learning models when strong mathematical invariances are present.
- **Mixture-of-Experts (MoE)** architectures are highly effective for structured mathematical problems with heterogeneous difficulty regimes.
- **Careful train-only data augmentation and per-bucket calibration** significantly improve generalization without introducing data leakage.
- **Deep learning can successfully approximate complex algebraic quantities**, such as m-heights, that are otherwise expensive to compute exactly.

---

##  Author

**Shruti Singh**  
M.S. Management Information Systems  
Texas A&M University  

---

##  Acknowledgements

- **Course:** CSCE 636 – Deep Learning  
- **Instructor:** Prof. Anxiao (Andrew) Jiang
- **Notes:** Dataset and evaluation protocol were provided as part of the coursework.

---

##  Next Steps

This project can be extended in several directions:

- Extension to larger values of  
  \[
  n
  \]
- Integration with **symbolic computation** or **coding-theoretic pipelines**
- Further research into **learning algebraic invariants** using deep learning

---
