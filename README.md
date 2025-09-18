# CPSM_COAD
This repository contains a comprehensive implementation of the CPSM (Cancer Patient Survival Model) package for analyzing survival outcomes in colorectal cancer (COAD) using TCGA data.The analysis includes multiple approaches to improve model performance, particularly focusing on optimizing the C-index through various train-test split ratios and feature selection methods.
Key Features
1. Data Processing

a) Downloads and processes TCGA-COAD RNA-seq data using TCGAbiolinks

b) Handles missing values and performs quality control

c) Implements log2 transformation and normalization

d) Filters genes based on variance and expression levels

2. Model Development
The pipeline implements four CPSM model types:

Model 1: Clinical features only (baseline)

Model 2: Prognostic Index (PI) score only (from LASSO regression)

Model 3: Clinical features + PI score

Model 4: Clinical features + individual genes + PI score

3. Feature Selection Methods

Univariate Cox regression: Identifies genes significantly associated with survival
LASSO regression: Creates a Prognostic Index (PI) score from selected genes
Variance-based filtering: Selects top variable genes for analysis

4. Performance Optimization

Implements both 80/20 and 70/30 train-test splits

Enhanced handling of factor levels and missing data

Robust error handling for LASSO regression failures

Multiple approaches to maximize C-index performance

Results
Original Implementation (80/20 split)

Clinical only model: C-index = 0.65

Combined model: C-index = 0.57 (unexpected decrease)

Issues with LASSO regression preventing full model evaluation

Optimized Implementation (70/30 split)

Larger test set provides more stable C-index estimates

Better event distribution for discrimination measurement

Expected C-index improvement to 0.58-0.65 range

Key Findings

Identified significant survival-associated genes including:

Protective genes (HR < 1): MAPKAPK3, ATOH1, SERPINA1, FDFT1

Risk genes (HR > 1): TRIP10, SURF6, PHGDH, EGFL7, ANGPTL4


Very strong associations found (HR ranging from 0.03 to 22.21)

Additional Analyses
IBS (Integrated Brier Score) Calculation

Comprehensive survival metrics including IBS for overall prediction accuracy

Time-dependent AUC calculations at multiple time points

Brier scores for calibration assessment

Gene Annotation

Conversion of ENSEMBL IDs to gene symbols
Pathway enrichment analysis recommendations
Identification of cancer-related genes

Technical Details

Dependencies
- BiocManager
- CPSM
- TCGAbiolinks
- survival
- survminer
- glmnet
- dplyr
- ggplot2
- org.Hs.eg.db
- AnnotationDbi

Data Structure

Samples: ~393 TCGA-COAD samples after quality control
Features:

Clinical: Age, gender, tumor stage

Genomic: 5,000-40,000 gene expression features


Outcome: Overall survival (OS) time and status

Usage

Basic CPSM Analysis:

results <- run_tcga_coad_cpsm_analysis_fixed()

70/30 Split Analysis:

results_70_30 <- run_cpsm_pipeline_70_30()

Calculate Comprehensive Metrics:

rsurvival_metrics <- calculate_comprehensive_survival_metrics(results)

Key Improvements Implemented

Data Quality: Enhanced missing value handling and normalization

Feature Selection: Multiple approaches to identify predictive genes

Model Robustness: Error handling for LASSO failures

Performance Metrics: Comprehensive evaluation including IBS and time-dependent AUC

Reproducibility: Seed setting and consistent factor level handling

Bug Fixes: Corrected gene indexing in feature selection to start at the proper
           column, ensuring only gene expression data are considered

Future Directions

Validate findings in independent TCGA cohorts

Perform pathway analysis on identified genes

Implement ensemble methods combining multiple gene sets

Explore deep learning approaches for survival prediction

Functional validation of top candidate genes

Notes

The LASSO regression module occasionally fails due to data formatting issues or multicollinearity

Alternative approaches (Ridge regression, Elastic Net) are provided as fallbacks

Gene expression data requires log2 transformation for optimal performance

Larger test sets (30%) provide more stable C-index estimates

Citation
If using this analysis, please cite:

CPSM package: https://doi.org/10.1101/2024.11.14.623597
CPSM: R-package of an Automated Machine Learning Pipeline for Predicting the Survival Probability of Single Cancer Patient Harpreet Kaur, Pijush Das, Kevin Camphausen, Uma Shankavaram

TCGA data: The Cancer Genome Atlas Network
TCGAbiolinks: Colaprico et al., 2016

