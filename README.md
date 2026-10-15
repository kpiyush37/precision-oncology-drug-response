# Cancer Drug Response Prediction

A research-oriented machine learning project for predicting cancer cell line sensitivity to drugs using multimodal biomedical data from the GDSC2 dataset.

## Objectives

- Predict drug sensitivity (LN_IC50) in cancer cell lines
- Compare classical ML vs deep learning approaches
- Integrate molecular drug structure via graph-based encoding
- Identify predictive biomarkers using explainable AI

## Overview

This project identifies differentially expressed genes between breast tumour and normal breast
tissue using publicly available microarray data from NCBI GEO. The analysis spans the full
workflow — from raw data download and quality control through statistical testing, annotation,
and visualisation.

**Research question:** Which genes are significantly up- or down-regulated in breast cancer
tissue compared to normal breast tissue, and what biological processes do they implicate?

---

## Dataset

| Property | Value |
|---|---|
| GEO Accession | GSE42568 |
| Platform | GPL570 — Affymetrix Human Genome U133 Plus 2.0 Array |
| Total samples | 121 (17 Normal, 104 Tumour) |
| Probes on array | 54,675 |
| Data type | RMA-normalised log2 expression values |

---

## Methods

- **Preprocessing:** IQR ≥ 0.5 variance filter removed ~59% of low-variance probes, retaining
  22,426 informative probes from 54,675. No additional normalisation applied — GEO GSE42568
  is already RMA log2-normalised.
- **QC:** PCA on standardised expression confirmed clear separation between Normal and Tumour
  sample groups along PC1.
- **Differential expression:** Welch two-sample t-test (`equal_var=False`) per probe, chosen
  for robustness to unequal group sizes (17 vs 104) and unequal variances.
- **Multiple testing:** Benjamini-Hochberg FDR correction across all 22,426 probes.
- **Significance thresholds:** FDR < 0.05 AND |log2FC| ≥ 1 (≥ 2-fold change).
- **Annotation:** Probe IDs mapped to HGNC gene symbols and Entrez IDs via GPL570 annotation.
  Gene-level results retain the best probe per gene (lowest adjusted p-value).

---

## Results

| Metric | Value |
|---|---|
| Probes tested | 22,426 |
| Significant (FDR < 0.05) | 13,893 (61.9%) |
| Upregulated (FDR < 0.05, logFC ≥ 1) | 3,420 |
| Downregulated (FDR < 0.05, logFC ≤ −1) | 2,615 |

Over 60% of variably expressed probes reached statistical significance after FDR correction,
reflecting the strong and consistent transcriptional difference between breast tumour and
normal tissue in this cohort.

### Top differentially expressed probes

| Probe | logFC | adj p-value | Direction |
|---|---|---|---|
| 205941_s_at | +3.59 | 5.3 × 10⁻³³ | Upregulated |
| 223540_at | +1.86 | 3.9 × 10⁻²⁶ | Upregulated |
| 217428_s_at | +3.56 | 6.3 × 10⁻²² | Upregulated |
| 213909_at | +3.64 | 9.5 × 10⁻²⁰ | Upregulated |
| 203878_s_at | +3.40 | 1.3 × 10⁻²¹ | Upregulated |

All top-ranked probes are upregulated in tumour tissue. The top probe (205941_s_at) shows a
3.59 log2FC, corresponding to approximately 12-fold higher expression in tumour vs normal,
with an adjusted p-value of 5.3 × 10⁻³³.

### Interpretation

The strong upregulation signal relative to downregulation (3,420 vs 2,615 probes) is consistent
with the known transcriptional activation patterns in breast cancer — proliferation-associated
genes, cell cycle regulators, and DNA replication machinery are characteristically over-expressed
in tumour tissue. The magnitude of fold changes (several probes exceeding log2FC = 3, i.e.
8-fold linear) indicates robust biological effect sizes, not marginal statistical artefacts.

The PCA and MA plot confirm that the expression differences are distributed uniformly across
expression levels, with no evidence of normalisation artefacts or batch effects.

---

## Outputs

| File | Description |
|---|---|
| `outputs/differential_expression_results.csv` | Full probe-level DE results with annotation |
| `outputs/differential_expression_results_gene_level.csv` | Gene-level results (best probe per gene) |
| `outputs/top_30_differentially_expressed_features.csv` | Top 30 probes by adjusted p-value |
| `outputs/analysis_summary.txt` | Key statistics from the run |
| `figures/pca_tumor_vs_normal.png` | PCA of all 121 samples |
| `figures/volcano_plot_tumor_vs_normal.png` | Volcano plot (logFC vs −log10 FDR) |
| `figures/ma_plot_tumor_vs_normal.png` | MA plot (average expression vs logFC) |
| `figures/top_50_deg_heatmap.png` | Z-score heatmap of top 50 DE probes |

---

## Tech Stack

- Python, Pandas, NumPy, SciPy, statsmodels, scikit-learn
- Matplotlib, Seaborn
- Google Colab (CPU runtime, ~3–5 min end-to-end)

---

*Data source: NCBI GEO — https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE42568*
