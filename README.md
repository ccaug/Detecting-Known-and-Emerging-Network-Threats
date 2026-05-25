# Detecting-Known-and-Emerging-Network-Threats

## Overview

This project implements an Intrusion Detection System (IDS) using a fine-tuned ModernBERT model to classify security logs into **26 different attack types**. The system includes multiple classification approaches:

- **RAG (Retrieval-Augmented Generation) Classifier** — Combines prototype-based classification with FAISS similarity search
- **Prototype-based Classifier** — Uses class prototypes (mean embeddings)
- **FAISS-based Classifier** — Nearest neighbor retrieval
- **Neural Networks** — CNN and LSTM classifiers
- **Traditional ML Models** — Random Forest, XGBoost, LightGBM, and SVM

---

## Features

- Multi-model comparison with comprehensive evaluation metrics
- RAG approach combining prototype similarity and retrieval
- Confidence calibration and uncertainty quantification
- Visual analytics including:
  - Confusion matrices
  - ROC curves and Precision-Recall curves
  - Confidence distribution analysis
  - Per-class performance bar charts
  - Radar charts for model comparison
  - Feature importance analysis
  - Calibration plots
  - Data distribution analysis
  - Misclassification pattern analysis

---

## Dataset Requirements

Upload the following CSV files:

| File | Description |
|---|---|
| `training_dataset.csv` | Contains known attack types (9 classes) |
| `master_security_dataset.csv` | Used for building the RAG index (35K+ samples) |
| `sample_security_dataset.csv` | Contains additional attack types (17 total classes) |
| `UNSW_NB15_testing-set.csv` | Optional external testing dataset |

### Key Columns

- `Log` — Log message text (**required**)
- `Attack Type` — Attack classification label

---

## Supported Attack Classes (26 Total)

- Analysis
- Anomaly
- Backdoor
- Benign
- Bot
- Brute_Force
- DDoS
- DNS Fast-Flux
- DoS
- DoS + Brute-Force
- Dos Attacks-Goldeneye
- Exploits
- FTP Brute-Force / Data Exfiltration
- Fuzzers
- Generic
- HTTP C2
- ICMP Flood
- IRC C2
- Normal
- P2P / UDP Scan
- Portscan
- Reconnaissance
- Shellcode
- Spam
- Web_Attack
- Worms

---

## Results Summary

### Model Performance Comparison

| Model | Accuracy | F1 Macro | Precision | Recall |
|---|---|---|---|---|
| XGBoost | 72.36% | 0.632 | 0.625 | 0.654 |
| Random Forest | 72.36% | 0.632 | 0.626 | 0.654 |
| SVM | 68.17% | 0.588 | 0.582 | 0.617 |
| RAG Classifier | 66.43% | 0.570 | 0.560 | 0.601 |
| FAISS-only | 66.43% | 0.570 | 0.560 | 0.601 |
| Prototype-only | 49.44% | 0.441 | 0.465 | 0.490 |
| LightGBM | 24.67% | 0.189 | 0.275 | 0.212 |
| CNN | 21.60% | 0.131 | 0.154 | 0.171 |
| LSTM | 14.84% | 0.042 | 0.036 | 0.092 |

---

### Per-Class Accuracy Highlights

| Attack Type | Accuracy | Samples |
|---|---|---|
| Bot | 100% | 50 |
| DDoS | 100% | 50 |
| DoS + Brute-Force | 100% | 30 |
| Dos Attacks-Goldeneye | 100% | 50 |
| Portscan | 100% | 7 |
| ICMP Flood | 96.7% | 30 |
| IRC C2 | 96.7% | 30 |
| Spam | 96.7% | 30 |
| Web_Attack | 96.0% | 50 |

---

## How to Run

### 1. Open in Google Colab

Upload the notebook or run directly in Colab.

### 2. Install Dependencies

Dependencies are automatically installed in **Cell 1**.

### 3. Upload Datasets

Use the file upload widgets in **Cell 4**.

### 4. Run All Cells

Execute the notebook sequentially.

---

## Key Notebook Cells

| Cell | Function |
|---|---|
| 1 | Install packages and imports |
| 2 | Load fine-tuned ModernBERT model |
| 3 | Define embedding extraction |
| 4 | Upload and load datasets |
| 5-6 | Build label mappings |
| 7-11 | Build prototypes and FAISS index |
| 12-13 | Train comparison models |
| 14-15 | Prepare test data and generate embeddings |
| 16 | Evaluate all models |
| 17-28 | Generate visualizations |

---

## Output Files

The notebook generates the following visualization files:

| File | Description |
|---|---|
| `model_comparison.csv` | Performance metrics table |
| `confusion_matrix.png` | Confusion matrix for RAG classifier |
| `confidence_analysis.png` | Confidence distribution plots |
| `precision_recall_curves.png` | Precision-Recall curves by attack class |
| `roc_curves.png` | ROC curves by attack class |
| `per_class_accuracy_barchart.png` | Per-class accuracy visualization |
| `performance_radar_chart.png` | Radar chart comparing models |
| `confidence_calibration.png` | Calibration curve |
| `data_distribution.png` | Train/test data distribution |
| `misclassification_patterns.png` | Error analysis heatmap |
| `feature_importance.png` | Random Forest feature importance |

---

## Repository Structure

```text
├── README.md
├── notebook.ipynb
├── datasets.csv
```

---

## Methodology

### 1. Feature Extraction

- ModernBERT model generates 768-dimensional embeddings
- Mean pooling with L2 normalization

### 2. Prototype Construction

- Class prototypes are computed using mean embeddings per attack type
- Intra-class similarity statistics are calculated

### 3. RAG Classifier

The RAG classifier combines:

- Prototype similarity with temperature scaling
- FAISS retrieval using `k = 30` nearest neighbors
- Weighted voting with uncertainty estimation

### 4. Evaluation Metrics

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC
- PR-AUC
- Brier Score (Calibration)
- Per-class performance metrics

---

## Dependencies

```text
faiss-cpu
sentence-transformers
transformers
datasets
scikit-learn
torch
xgboost
lightgbm
pandas
numpy
matplotlib
seaborn
tqdm
psutil
gputil
```
---

## Acknowledgments

- ModernBERT by Answer.ai
- UNSW-NB15 Dataset (optional testing dataset)

---

## Notes

- GPU is recommended for embedding generation (CUDA supported)
- RAG classifier uses `temperature = 0.1` for confidence calibration
- FAISS index uses inner product similarity (cosine similarity after L2 normalization)
- Neural network models use 20 epochs for quick comparison training
