# Customer Churn Prediction

A banking classification project by **Patrick Felder**, completed as TripleTen coursework. The goal is to identify customers likely to leave, providing a starting point for retention analysis. This is a learning project, not a deployed retention system.

## Results

| Model | Evaluation set | F1 | ROC-AUC |
|---|---|---:|---:|
| Logistic Regression | Validation | 0.3215 | 0.7874 |
| Class-weighted Logistic Regression | Validation | 0.5109 | 0.7918 |
| Tuned class-weighted Random Forest | Validation | 0.6456 | 0.8699 |
| Final Random Forest | Held-out test | **0.6492** | **0.8671** |

The final test set contains **2,000 customers**. The model exceeds the coursework F1 target of 0.59. These are saved results from the submitted notebook, not a new execution of this package. Exact values are in `results.json`.

## Data and approach

- 10,000 banking customer records; 14 original columns; 10 model input features.
- 20.37% of customers have the `Exited` label; `Tenure` contains 909 missing values.
- Excluded row numbers, customer IDs, and surnames from the model inputs.
- Used stratified 60/20/20 training, validation, and test splits with random seed 12345.
- Fitted median imputation and scaling for numeric inputs, and most-frequent imputation and one-hot encoding for categories within scikit-learn pipelines.
- Compared a baseline with class weighting and tuned Random Forest using five-fold GridSearchCV scored on F1. Refitted the selected model on training plus validation data before test evaluation.

Best Random Forest settings: 600 trees, maximum depth 12, minimum leaf size 4, and balanced class weights. No resampling is implemented.

## Run locally

Use Python 3.11 or 3.12 with a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Then install dependencies and open Jupyter:

```bash
python -m pip install -r requirements.txt
python -m jupyterlab
```

Place an authorized `Churn.csv` in `data/` and open `customer_churn.ipynb`. Alternatively set `CHURN_DATA_PATH` to the CSV path. The original TripleTen `/datasets/Churn.csv` path is also supported.

Grid search evaluates 36 parameter combinations across five folds and may take time. Dependencies are not pinned because the original execution environment versions were not captured; fresh results may differ.

## Files

- `customer_churn.ipynb`: readable workflow with original recorded results and a portable dataset loader.
- `results.json`: exact recorded evaluation results and split details.
- `requirements.txt`: Python dependencies.
- `data/README.md`: instructions for the dataset, which is not bundled.

## Limits and next steps

F1 balances precision and recall, while ROC-AUC measures ranking performance. No real-world retention uplift or deployment performance was measured. Useful next steps include threshold selection based on business costs, calibration, and subgroup evaluation.

The notebook code was checked for syntax during packaging. End-to-end retraining was not performed because the CSV was not supplied.
