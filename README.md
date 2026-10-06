# Freight Rate Prediction Challenge

Machine learning solution for predicting freight rates from shipment, route, equipment, weight, and market-related features.

## Overview

This project develops a freight-rate regression model using the provided historical load data. The workflow includes data validation, chronological train/validation splitting, feature engineering, model comparison, final model training, validation prediction generation, and December 2025 forecasting.

The final model uses **CatBoost Regressor** with categorical handling for pickup city, delivery city, and equipment type.

## Final Model

* **Model:** CatBoost Regressor
* **Iterations:** 200
* **Depth:** 8
* **Learning rate:** 0.05
* **Loss function:** MAE
* **Random seed:** 42
* **Categorical features:** `pickup`, `delivery`, `equipment`

### Features

* `pickup`
* `delivery`
* `equipment`
* `distance`
* `weight`
* `month`
* `day_of_week`
* `day_of_year`

Invalid negative weights were treated as missing values rather than removing the affected loads.

## Validation Approach

A chronological hold-out was used to simulate future prediction:

* **Training:** January 1 – September 30, 2025
* **Validation:** October 1 – October 31, 2025
* **Training rows:** 43,147
* **Validation rows:** 4,853

The final model was subsequently retrained on all 48,000 labeled training rows before generating predictions for the 12,000 validation loads.

## Model Performance

The selected model achieved an internal validation MAE of approximately **$116** and RMSE of approximately **$647**.

A route-based model achieved a slightly lower MAE, but the route feature provided effectively zero feature importance. The simpler model was therefore selected for the final fit.

## Project Files

* `train_model.ipynb` — complete data analysis, model development, evaluation, and prediction workflow
* `requirements.txt` — Python dependencies
* `score.py` — provided assessment scoring script
* `validation-predictions.csv` — predictions for the 12,000 validation loads
* `december-chart-inputs.csv` — predictions for the fixed December 2025 inputs
* `scorer_results/candidate_december.png` — generated December prediction chart

The original assessment datasets are not included in this repository.

## Setup

Python 3.10+ is recommended.

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Running the Model

Open `train_model.ipynb` in Jupyter Notebook or JupyterLab and run the cells in order.

The notebook performs:

1. Data validation and exploration
2. Chronological train/validation splitting
3. Feature engineering
4. CatBoost model training and comparison
5. Final model training on all labeled data
6. Generation of validation predictions
7. Generation of December predictions

The assessment datasets should be placed in the project directory before running the notebook.

## Running the Scorer

After generating the prediction files, run:

```bash
python score.py --predictions validation-predictions.csv --december-predictions december-chart-inputs.csv
```

A successful run validates the 12,000 final predictions, the 31 December predictions, and generates:

```text
scorer_results/candidate_december.png
```

## Notes

The official final validation metric is calculated by the assessment platform after submission. The reported MAE and RMSE are from the internal October hold-out used for model development and selection.
