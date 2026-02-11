# Bulk Deconvolution and Single-Cell Clustering
Machine Learning for Genomics (Fall 2025) - Project 2

## Project Overview
The objective of this project is to analyze the tumor microenvironment (TME) of patients with esophageal adenocarcinoma.  Using a combination of single-cell RNA sequencing (scRNA-seq) and bulk RNA-seq data, the project aims to:
1. Identify biologically meaningful cell clusters to discover reliable biomarkers across patients.
2. Estimate cell-type fractions in bulk samples to compare TME compositions between pre- and post-chemotherapy treatments.

## Dataset
- Single-Cell Data: 10 samples (6 train, 4 test) containing immune and stromal cells.
- Bulk Data: 32 samples (12 train witho ground truth, 20 test).
- Cell Types: Nine distinct types, including T cells, B cells, Endothelial cells, Fibroplasts, NK cells and Myeloid cells.

## Technical Approach
1. **Single-Cell Clustering**
   
To identify cell populations in the unannotated test dataset, we implemented a pipeline using the scanpy library:
   - **Preprocesssing**: Filtered cells by gene count, normalized total counts and applied log transformation (log(1+x)).
   - **Feature Selection**: Identified highly variable genes to focus on biologically informative signals.
   - **Dimensionality Reduction**: Used PCA followed by neighborhood graph construction.
   - **Clustering**: Applied the Leiden algorithm to the training data.
   - **Classification**: Trained a Random Forest Classifier on the annotated training clusters to predict cell-type labels for the test single-cell dataset.
2. **Bulk Deconvolution**
     
   The goal was to estimate the proportion of the nine cell types within the 20 test bulk samples.
  - **Method**: We utilized Non-Negative Least Squares (NNLS).
  - **Process**: 1. Calculated a signature matrix (expression profiles) from the training bulk data and true proportions using a pseudo-inverse approach. 2. Solved the NNLS problem for each test bulk sample to find the coefficients (proportions) that best represent the bulk expression as a weighted sum of the signature matrix. 3. Normalized the resulting coefficients so that the sum of proportions for each patient equals 1.

## Evaluation Metrics
- **Clustering**: Evaluated using Adjusted Rand Index (ARI) and V-measure score.
- **Deconvolution**: Evaluated using the average Root Mean Squared Error (RMSE) across all cell types.


