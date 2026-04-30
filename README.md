# CPSM_COAD — Colorectal Cancer Survival Analysis

MTLR-based survival modeling for TCGA colorectal adenocarcinoma (COAD) using the [CPSM](https://bioconductor.org/packages/CPSM/) Bioconductor package.

## Overview

This repository applies the Cancer Patient Survival Model (CPSM) pipeline to TCGA-COAD, identifying prognostic genes and building predictive survival models using Multi-Task Logistic Regression (MTLR).

**Key features:**
- Overall Survival (OS) endpoint with ~14% event rate
- AJCC pathologic staging (I–IV) as the primary clinical feature
- Five MTLR model architectures comparing clinical, genomic, and combined predictors
- 1,157 univariate survival-significant genes identified (p<0.05)

## Pipeline

| Step | Function | Description |
|------|----------|-------------|
| 1 | Data acquisition | TCGA-COAD via TCGAbiolinks |
| 2 | `data_process_f()` | Survival time conversion (days → months) |
| 3 | `tr_test_f()` | 80/20 train-test split |
| 4 | `train_test_normalization_f()` | Log2 + quantile normalisation |
| 5a | `Lasso_PI_scores_f()` | LASSO feature selection + PI score |
| 5b | `Univariate_sig_features_f()` | Univariate Cox screening |
| 6 | `MTLR_pred_model_f()` | 5 MTLR models |
| 7–11 | Visualisation | Survival curves, KM overlay, nomogram, risk group prediction |

## Results

| Model | Train C-index | Test C-index |
|-------|:---:|:---:|
| Model 1 — Clinical (Age + Stage) | 0.78 | 0.65 |
| Model 4 — Univariate genes | 0.78 | 0.63 |
| Model 5 — Genes + Clinical | 0.85 | 0.68 |

Best model: **Model 5 (Genes + Clinical)** — test C-index 0.68. Combining gene expression with clinical staging provides modest improvement over either alone.

## Requirements

- R ≥ 4.4
- [CPSM](https://bioconductor.org/packages/CPSM/), TCGAbiolinks, SummarizedExperiment, survival, glmnet, MTLR

## Usage

```r
source("CPSM_TCGA_COAD_Pipeline.R")
```

## Data

- **Source:** [TCGA-COAD](https://portal.gdc.cancer.gov/projects/TCGA-COAD) via TCGAbiolinks
- **Samples:** ~350 primary tumour patients (after deduplication)
- **Endpoint:** OS — ~49 events (14%)
- **Genes:** Top 20,000 by variance → 1,157 univariate significant (p<0.05)

## Part of Pan-Cancer Analysis

This pipeline is one of seven TCGA cohorts (COAD, BRCA, GBM, LUAD, LUSC, PAAD, PRAD) analysed for survival genes.
## References

1. Liu J et al. An Integrated TCGA Pan-Cancer Clinical Data Resource. *Cell*. 2018;173(2):400-416. [doi:10.1016/j.cell.2018.02.052](https://doi.org/10.1016/j.cell.2018.02.052)
2. Kaur H, Das P, Camphausen K, Shankavaram U. CPSM: R-package of an Automated Machine Learning Pipeline for Predicting the Survival Probability of Single Cancer Patient. *bioRxiv*. 2024. [doi:10.1101/2024.11.14.623597](https://doi.org/10.1101/2024.11.14.623597)

## Author

Denver Ncube
