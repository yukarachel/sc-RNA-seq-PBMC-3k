# Single-Cell RNA-seq Cell Type Classification

## Overview

This project analyzes the **PBMC 3k single-cell RNA-seq dataset** to identify immune-cell populations and develop a machine-learning model for cell-type classification.

The project combines single-cell bioinformatics with supervised and unsupervised machine learning.

### Workflow

Gene expression data  
↓  
Quality control & normalization  
↓  
PCA  
↓  
Cell-cell neighborhood graph  
↓  
Leiden clustering  
↓  
Marker-gene analysis & cell-type annotation  
↓  
Logistic regression classification  

## Biological Question

Can gene-expression profiles be used to distinguish different immune-cell populations?

A secondary machine-learning question is:

> How much classification information is retained when thousands of gene-expression features are reduced to a smaller number of principal components?

---

## Dataset

**PBMC 3k (Peripheral Blood Mononuclear Cells)**

The PBMC 3k dataset is a commonly used single-cell RNA-seq dataset containing gene-expression profiles from peripheral blood immune cells.

The AnnData object used in this project contains approximately:

- **2,700 cells**
- **32,738 genes** before feature selection
- A reduced set of highly variable genes for downstream analysis

---

## Single-Cell Analysis

### 1. Quality Control

Cell-level quality metrics were examined, including:

- Number of genes detected per cell
- Total RNA counts per cell
- Mitochondrial gene expression

### 2. Normalization

Gene-expression counts were normalized across cells and log-transformed to make expression levels more comparable and reduce the influence of highly skewed values.

### 3. PCA

Principal Component Analysis (PCA) was used to reduce the dimensionality of the gene-expression data.

The principal components capture major sources of variation across cells and were used for downstream neighborhood analysis.

### 4. Cell-Cell Similarity

A nearest-neighbor graph was constructed using the PCA representation.

This graph represents which cells have similar gene-expression profiles.

### 5. UMAP

UMAP was used to visualize the cell-cell neighborhood structure in two dimensions.

UMAP was used for visualization rather than determining the cell-type labels.

### 6. Leiden Clustering

Leiden clustering was applied to the neighborhood graph to identify groups of transcriptionally similar cells.

The resulting clusters were then interpreted using marker-gene analysis.

### 7. Marker-Gene Analysis

Differential expression analysis was used to identify genes enriched within each cluster.

Examples include:

| Cluster | Example marker genes | Interpretation |
|---|---|---|
| 0 | CD79A, CD79B, MS4A1 | B cells |
| 3 | S100A8, FCN1, TYROBP | Classical monocytes |
| 8 | NKG7, GNLY, GZMB, PRF1 | NK cells |
| 9 | LST1, FCGR3A, FCER1G | FCGR3A+ monocytes |
| 12 | TUBB1, PF4, GNG11 | Megakaryocytes |

---

# Machine Learning

## Cell-Type Classification

After identifying and annotating cell populations, the cluster-derived cell-type labels were used as targets for supervised machine learning.

```text
X = gene-expression features
y = cell type
