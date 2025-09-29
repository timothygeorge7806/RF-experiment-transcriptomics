# RF-experiment-transcriptomics

## Overview
This project applies a **Random Forest classifier** to single-cell transcriptomics data from the Craig Venter Institute (Aevermann et al., 2018).  
The task is binary classification:  
- **Class 1** → e1 neuronal cluster  
- **Class 0** → non-e1 cluster  

Dataset summary:
- **Samples:** 871  
- **Features:** 608 (continuous gene expression values)  
- **Balanced classes:** 299 positives, 572 negatives  
- **No missing values**

---

## Data Preparation
- **Training DB:** 869 samples (571 negative, 298 positive)  
- **Verification DB:** 2 samples (1 positive, 1 negative)  
- Features preserved; label column = `Label`.

---

## Methods
- Implemented in **R** using the `randomForest` package.  
- **Evaluation:** Out-of-bag (OOB) error estimation.  
- **Hyperparameter grid search**:  
  - `NTREE`: 1000, 2000, 5000  
  - `MTRY`: 0.5√p, 1√p, 2√p (p = 608 features)  
  - `CUTOFF`: 0.3, 0.5, 0.7  
- Total of **27 configurations tested**.  
- Metrics: **Precision, Recall, F1 Score**.

---

## Results

### Best Model
- **NTREE:** 1000  
- **MTRY:** ~12  
- **CUTOFF:** 0.3  
- **Lowest OOB Error:** 0.46%  

### Confusion Matrix
|                  | Predicted Negative | Predicted Positive |
|------------------|--------------------|--------------------|
| **Ground Truth 0** | 568                | 3                  |
| **Ground Truth 1** | 1                  | 297                |

### Accuracy Metrics
- **Precision:** 0.9900  
- **Recall:** 0.9966  
- **F1 Score:** 0.9933  

---

## Feature Importance
Top 10 ranked features (Mean Decrease in Accuracy, MDA):

| Rank | Feature   | MDA      |
|------|-----------|----------|
| 1    | TESPA1    | 11.85    |
| 2    | KCNIP1    | 10.77    |
| 3    | SLC17A7   | 10.62    |
| 4    | NECAB1    | 9.73     |
| 5    | LINC00507 | 9.72     |
| 6    | ANKRD33B  | 9.63     |
| 7    | NPTX1     | 9.59     |
| 8    | SLIT3.2   | 9.55     |
| 9    | SLIT3     | 9.33     |
| 10   | ZNF536    | 9.33     |

**Biological validation:**  
- TESPA1, LINC00507, and SLC17A7 are known markers for the e1 cluster.  
- KCNIP1 absence is also a ground-truth marker.  
- Strong alignment confirms the model captures biologically meaningful patterns.

---

## Verification Test
Run-time predictions on Verification DB:

| Sample | Ground Truth | Predicted | P(Class 0) | P(Class 1) |
|--------|--------------|-----------|------------|------------|
| 1      | 1            | 1         | 0.005      | 0.995      |
| 2      | 0            | 0         | 0.965      | 0.035      |

- Both samples classified correctly with high confidence.

---

## References
- Aevermann et al., *Cell type discovery using single-cell transcriptomics*  
- R Project Documentation  
- ChatGPT-5 (for code snippets)

---

## Key Takeaways
- Random Forest achieved **~99% accuracy** with minimal error.  
- Identified key biological markers consistent with literature.  
- Verified model generalizes to unseen data with strong confidence.  