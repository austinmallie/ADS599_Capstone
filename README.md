# Hospital Ransomware Analysis: A Predictive Framework for Cybersecurity Risk

> A capstone project for the ADS-599 Applied Data Science Program at the University of San Diego

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Python](https://img.shields.io/badge/Python-3.10-blue)

---

## Table of Contents
- [Project Overview](#project-overview)
- [Key Results](#key-results)
- [Repository Structure](#repository-structure)
- [Data Sources](#data-sources)
- [Installation](#installation)
- [How to Run](#how-to-run)
- [Methods](#methods)
- [Technologies](#technologies)
- [Contributors](#contributors)
- [License](#license)

---

## Project Overview

Hospital cybersecurity is fundamentally reactive. Defenses exist — but they activate after an attack has already occurred. After patients are harmed. After systems go offline. After the damage is done.

This project asks a different question: **what if we could identify which hospitals are most vulnerable before an attack happens?** Could the financial and operational data hospitals already report to the federal government serve as an early warning system?

The research literature paints a consistent picture. Larger hospitals face elevated breach risk due to more complex IT environments. Urban location, non-profit status, high inpatient workloads, and specialized care all correlate with higher breach probability. Financial profile matters too — profitable hospitals are attractive ransom targets, while under-resourced ones cannot afford adequate protections. The most rigorous prior work (Dolezel et al., 2023) achieved 78–83% accuracy, but was far better at identifying hospitals that were not breached than catching the ones that were. High specificity, weak recall — the wrong trade-off when missing a vulnerable hospital has real patient safety consequences. No prior study produced a reproducible, longitudinal predictive framework. That gap is exactly where this project lives.

---

## Key Results

| Model | ROC-AUC | PR-AUC | Recall | Precision | F1 |
|---|---|---|---|---|---|
| Logistic Regression | 0.787 | 0.104 | 0.751 | 0.067 | 0.122 |
| XGBoost | **0.975** | **0.722** | **0.884** | **0.391** | **0.542** |
| Random Forest | 0.945 | 0.457 | 0.691 | 0.322 | 0.439 |
| SVM | 0.929 | 0.283 | 0.867 | 0.149 | 0.254 |

XGBoost achieved the highest performance, successfully identifying **88% of breached hospitals** in the 2021 temporal holdout year. Results support the hypothesis that financial health ratios, hospital size indicators, and year-over-year trajectory variables are statistically significant predictors of cybersecurity vulnerability.

---

## Repository Structure

```
ADS599_Capstone/
│
├── Data-Folder/
│   ├── Cost-Report-Data/          # Annual CMS cost report CSV files (2014–2023)
│   ├── Hospital_General_Clean_Final.csv
│   └── breach_provider_CCN.csv
│
├── EDA/
|   ├── Breach_EDA.ipynb
|   ├── Cost_Report_EDA.ipynb
|   ├── HospitalGeneral_EDA.ipynb
|   ├── hospital_ransomware_eda.ipynb # Final EDA of the combined data sets
|
|
├── Feature_Engineering/
|   ├── HospitalGeneral_FE.ipynb
|   ├── Hospital_Cost_Report_Feature_Engineered.ipynb
|
|
├── Models/
|   ├── Confusion Matrix/
|      ├── confusion_matrix_baseline.png
|      ├── confusion_matrix_rf.png
|      ├── confusion_matrix_svm.png
|      ├── confusion_matrix_xgb.png
|   ├── Feature Importance/
|      ├── feature_importance_final.csv
|      ├── feature_importance_rf.csv
|      ├── feature_importance_xgb.csv
|   ├── claude_ransomware_model_analysis.ipynb
|   ├── hospital_breaches_predictive_modeling.ipynb
|   ├── hospital_breaches_predictive_modeling_app_data.ipynb # Final model notebook
|   ├── hospital_ransomware.parquet # The data file is too large for github to handle so it was compressed using as a parquet file
|   ├── model_performance_final 3.21.26.csv
|
|
├── Capstone_Streamlit.ipynb
├── README.md
└── .gitignore
```

---

## Data Sources

All three datasets are publicly available at no cost.

| Dataset | Source | Access |
|---|---|---|
| CMS Hospital Provider Cost Reports (2014–2023) | [CMS HCRIS](https://www.cms.gov/data-research/statistics-trends-and-reports/cost-reports) | Bulk CSV download |
| CMS Hospital General Information | [CMS Care Compare](https://data.cms.gov/provider-data/dataset/xubh-q36u) | Single CSV download |
| HHS OCR Breach Portal | [HHS OCR](https://ocrportal.hhs.gov/ocr/breach/breach_report.jsf) | CSV download |

The three sources are joined on the **CMS Provider Certification Number (CCN)**, producing a longitudinal panel of **61,444 hospital-year observations** covering **6,696 unique hospitals**.

---

## Installation

### Prerequisites
- Python 3.10 or higher
- Google Colab (recommended) or a local Jupyter environment

### Clone the repository

```bash
git clone https://github.com/austinmallie/ADS599_Capstone
cd ADS599_Capstone
```

### Install dependencies

```bash
pip install -r requirements.txt
```

---

## How to Run
### Option 1: Google Colab (Recommended)

This project is designed to run seamlessly in Google Colab, which avoids local environment setup and handles the larger datasets more efficiently.

**Setup**

Clone the repository:

```bash
git clone https://github.com/austinmallie/ADS599_Capstone
```

Upload the repository to Google Colab or mount your Google Drive:

```python
from google.colab import drive
drive.mount('/content/drive')
```

Navigate to the project directory:

```python
import os
os.chdir('/content/ADS599_Capstone')
```

Install required dependencies:

```bash
pip install -r requirements.txt
```

**Execution Order**

Run the notebooks in the following sequence to reproduce the full pipeline:

1. Exploratory Data Analysis
   - `EDA/hospital_ransomware_eda.ipynb`

2. Feature Engineering
   - `Feature_Engineering/Hospital_Cost_Report_Feature_Engineered.ipynb`
   - `Feature_Engineering/HospitalGeneral_FE.ipynb`

3. Modeling and Evaluation
   - `Models/hospital_breaches_predictive_modeling_app_data.ipynb`

**Data Requirements**

The final modeling dataset is included as a compressed parquet file:

```
Models/hospital_ransomware.parquet
```

To rebuild the dataset from raw sources:
- Download the datasets listed in the [Data Sources](#data-sources) section
- Place them in the `Data-Folder/` directory
- Execute the notebooks in the order listed above

**Outputs**

Running the full pipeline will generate:
- Model performance metrics (AUC, Recall, Precision, F1)
- Confusion matrices (stored in `Models/Confusion Matrix/`)
- Feature importance outputs (stored in `Models/Feature Importance/`)

---

### Option 2: Local Environment

1. Clone the repository and install dependencies as above
2. Update `DATA_DIR` at the top of each notebook to point to your local data folder
3. Update `DRIVE_DIR` to your preferred local output directory
4. Run notebooks in the execution order listed above


--- 

## Methods

- Longitudinal panel construction and multi-source data integration
- Missing value profiling and threshold-based column removal
- Outlier detection and domain-validated row removal
- Feature engineering: financial ratios, per-unit efficiency metrics, solvency indicators, year-over-year temporal delta features
- StandardScaler normalization for distance-based models
- Median imputation for remaining intermittent missingness
- Temporal holdout validation strategy (train on 2014–2020, test on 2021)
- Cost-sensitive learning to address 32:1 class imbalance
- Binary classification: Logistic Regression, Random Forest, SVM, XGBoost
- Distributional analysis, correlation analysis, multicollinearity assessment
- Data visualization

---

## Technologies

| Category | Tools |
|---|---|
| Language | Python 3.10 |
| Data manipulation | pandas, numpy |
| Machine learning | scikit-learn, xgboost |
| Visualization | matplotlib, seaborn |
| Environment | Google Colab, Jupyter |
| Version control | Git, GitHub |

---

## Contributors

| Name | GitHub |
|---|---|
| Austin Mallie | [@austinmallie](https://github.com/austinmallie) |
| Cynthia Portales-Loebell | [@cploebell](https://github.com/cploebell) |
| Sasha Libolt | [slibolt](https://github.com/slibolt) |
---

## License



---

*University of San Diego — Applied Data Science Program — ADS-599 Capstone*


