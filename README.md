# Cancer Drug Response Prediction

A research-oriented machine learning project for predicting cancer cell line sensitivity to drugs using multimodal biomedical data from the GDSC2 dataset.

## Objectives

- Predict drug sensitivity (LN_IC50) in cancer cell lines
- Compare classical ML vs deep learning approaches
- Integrate molecular drug structure via graph-based encoding
- Identify predictive biomarkers using explainable AI

## Dataset

- **GDSC2** (Genomics of Drug Sensitivity in Cancer, v2)
- Target variable: `LN_IC50` (natural log of half-maximal inhibitory concentration)
- Features: gene expression profiles, mutation data, copy number variations
- Drug representation: SMILES strings for molecular graph encoding

## Models

### Baseline
- **XGBoost** — gradient boosted trees for tabular genomic features

### Deep Learning
- **MLP** (Multi-Layer Perceptron) — feedforward network on expression + mutation features
- **GNN Drug Encoder** — graph neural network over drug molecular graphs (CANDELA-style), encodes SMILES strings into learned drug embeddings; combined with cell line features for final prediction

### Explainability
- **SHAP** — feature importance for XGBoost and MLP models
- **Integrated Gradients** — attribution scores for MLP inputs to identify predictive genomic features

## Tech Stack

- Python, PyTorch, Scikit-learn, Pandas, NumPy
- `torch_geometric` — GNN drug encoder
- `captum` — Integrated Gradients
- `shap` — SHAP explainability
- Google Colab (GPU runtime)

## Project Structure
├── data/ # GDSC2 expression, mutation, drug data
├── models/
│ ├── xgboost_baseline.py
│ ├── mlp_model.py
│ └── gnn_drug_encoder.py
├── explainability/
│ ├── shap_analysis.py
│ └── integrated_gradients.py
├── notebooks/ # Colab notebooks for training and analysis
└── outputs/ # Saved models, plots, results


## Results

| Model | RMSE | R² |
|-------|------|----|
| XGBoost | — | — |
| MLP | — | — |
| MLP + GNN Drug Encoder | — | — |

*Results to be updated after full training runs.*

## Usage

Open the notebooks in Google Colab with GPU runtime enabled. Ensure internet access is on for dataset and dependency downloads.
