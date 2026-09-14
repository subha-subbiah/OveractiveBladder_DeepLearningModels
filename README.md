# A Comparative Deep Learning Study for Symptom-Based Overactive Bladder Prediction with SHAP Explainability

[![Paper DOI](https://img.shields.io/badge/DOI-10.5220%2F0014996300005051-blue.svg)](https://doi.org/10.5220/0014996300005051)
[![Conference](https://img.shields.io/badge/Conference-ABH%202026-green.svg)](https://abh.scitevents.org/Home.aspx?y=2026)

Official repository for the paper: **"A Comparative Deep Learning Study for Symptom-Based Overactive Bladder Prediction with SHAP Explainability"**, published in the *Proceedings of the International Conference on Artificial Intelligence and Blockchain in Healthcare (ABH 2026)*, pages 223–236.

---

## Publication & Citation

If you use this code or findings in your research, please cite our paper:

> **Subbiah, S., & Ramachandran, M. (2026).**  
> *A Comparative Deep Learning Study for Symptom-Based Overactive Bladder Prediction with SHAP Explainability.*  
> In Proceedings of the International Conference on Artificial Intelligence and Blockchain in Healthcare (ABH 2026), pages 223–236.  
> **DOI:** [10.5220/0014996300005051](https://doi.org/10.5220/0014996300005051)  
> **ISBN:** 978-989-758-856-3 | **ISSN:** 3051-8377  
> **Publisher:** SCITEPRESS – Science and Technology Publications, Lda.

---

## Overview

Overactive Bladder (OAB) is a prevalent lower urinary tract dysfunction characterized primarily by urinary urgency, accompanied by frequency and nocturia. Early identification at a population level can facilitate timely clinical screening and improve patient quality of life.

This repository provides an end-to-end explainable deep learning framework that models symptom-based OAB classification using nationally representative data from the **National Health and Nutrition Examination Survey (NHANES 2021–2023)** cycle.

### Key Contributions:
1. **Clinical Symptom Labeling**: OAB label based on International Continence Society (ICS) criteria (`Urgency AND (Urge Incontinence OR Nocturia)`).
2. **Deep Learning Architectures**: Developed and evaluated three distinct neural network configurations:
   - **Deep Multi-Layer Perceptron (Deep MLP)**
   - **Residual Neural Network (ResNet with Skip Connections)**
   - **Wide & Deep Network (Joint Linear & Non-linear Feature Learning)**
3. **Class Imbalance Mitigation**: Applied Synthetic Minority Over-sampling Technique (**SMOTE**).
4. **Explainable AI (XAI)**: Integrated **SHAP (SHapley Additive exPlanations)** in logit space to deliver both global risk factor rankings and patient-level local waterfall explanations.

---

## Experimental Results

All three architectures achieved strong discriminative performance with high sensitivity, ideal for screening applications where minimizing false negatives is critical.

| Model Architecture | Accuracy | ROC-AUC | F1-Score | Precision | Recall | True Negatives | True Positives | False Positives | False Negatives |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Deep MLP** | **93.05%** | 0.9677 | **0.8942** | 0.8087 | **1.0000** | 641 | 296 | 70 | **0** |
| **Residual NN** | 92.65% | 0.9674 | 0.8875 | 0.8066 | 0.9865 | 641 | 292 | 70 | 4 |
| **Wide & Deep** | 92.95% | **0.9678** | 0.8926 | 0.8082 | 0.9966 | 641 | 295 | 70 | 1 |

### SHAP Explainability Highlights:
- **Global Importance**: Age (`RIDAGEYR`) emerged as the primary determinant across all models, followed by urgency operational proxy (`KIQ044`), urge incontinence (`KIQ042`), gender (`RIAGENDR`), and comorbidities (diabetes `DIQ010`, asthma `MCQ010`, kidney conditions `KIQ022`).
- **Target Leakage Mitigation**: Excluded defining label features (`KIQ042`, `KIQ481`) from predictors, while retaining `KIQ044` as a partial operational proxy to preserve clinical symptom-driven context.

---

## Repository Structure

```text
.
├── code/
│   ├── cleaning.ipynb    # Data preprocessing, filtering, binarization & label construction
│   └── models.ipynb      # Neural network architectures, SMOTE, training, evaluation & SHAP analysis
├── data/
│   ├── raw/
│   │   └── 2021_2023/    # NHANES 2021–2023 SAS transport (.xpt) raw files
│   └── final/
│       ├── nhanes_2021_2023.csv         # Processed intermediate dataset
│       └── full_cleaned_labelled.csv   # Final encoded dataset used for model training
└── README.md             # Project documentation and paper reference
```

---

## Installation & Setup

### 1. Prerequisites
Ensure you have Python 3.10+ installed.

### 2. Virtual Environment Setup
Create and activate a virtual environment:

```bash
# Create environment
python -m venv .venv

# Activate environment (macOS/Linux)
source .venv/bin/activate

# Activate environment (Windows)
.venv\Scripts\activate
```

### 3. Install Dependencies
Install the required packages:

```bash
pip install pandas numpy scikit-learn imbalanced-learn tensorflow shap matplotlib seaborn
```

---

## Execution Workflow

To reproduce the study's pipeline:

1. **Data Preprocessing & Cleaning**:
   Open and execute `code/cleaning.ipynb`. This notebook loads raw NHANES `.xpt` files from `data/raw/2021_2023/`, performs data imputation, symptom binarization, logical rule-based OAB label construction, and saves `data/final/full_cleaned_labelled.csv`.

2. **Model Training & Evaluation**:
   Open and execute `code/models.ipynb`. This notebook splits the dataset (80/20 stratified split with `SEED=42`), applies `StandardScaler` and `SMOTE`, builds and trains the three deep learning architectures, plots performance metrics (ROC curves, confusion matrices), and computes SHAP feature importance & waterfall plots.

---

## Clinical Disclaimer

This repository forms part of a methodological and population-level screening study using cross-sectional survey data. It is intended for research and educational purposes and does **not** constitute a deployment-ready clinical diagnostic tool. 

---
