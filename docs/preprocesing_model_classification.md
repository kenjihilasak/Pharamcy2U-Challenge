# Pipeline Summary

## `preprocesing.ipynb`

Location: `notebooks/preprocesing.ipynb`

This notebook takes the raw PDE events CSV from `data/raw/` and builds modeling-ready tables while avoiding future information leakage.

Main flow:
- Load the original dataset and convert `SRVC_DT` into `service_date`.
- Normalize `PROD_SRVC_ID` and derive `ndc11` and `ndc9`.
- Use `ndc9` as `drug_group_key` to better group refills from the same treatment.
- Sort each history by patient, drug, and date to rebuild the refill sequence.
- Compute dates and targets:
  - `run_out_date`
  - `next_fill_date`
  - `target_refill_gap_days`
  - `target_is_late_refill_7d`
- Mark whether the classification label is observable with `late_refill_label_observable`, so end-of-window censored cases can be excluded.
- Generate leakage-safe features using only the current fill and prior history:
  - quantity, days supplied, costs
  - calendar variables
  - gap relative to the previous fill
  - changes versus the previous fill
  - prior historical averages per patient-drug pair

Outputs:
- Files are saved under `data/processed/` at the project root.
- `challenge_a_regression_dataset.csv`
- `challenge_a_regression_numeric.csv`
- `challenge_a_classification_dataset.csv`
- `challenge_a_classification_numeric.csv`

## `model_classification.ipynb`

Location: `notebooks/model_classification.ipynb`

This notebook uses `data/processed/challenge_a_classification_dataset.csv` to predict `target_is_late_refill_7d`.

Main flow:
- Load the classification dataset and add a few helper features derived from the previous gap.
- Exclude identifiers, dates, and future target columns so only valid numeric features remain.
- Create a time-based split:
  - train: 2008-01-01 to 2009-12-31
  - validation: 2010-01-01 to 2010-06-30
  - test: 2010-07-01 to 2010-12-31
- Train and compare three models:
  - Logistic Regression
  - Random Forest
  - XGBoost
- Select the best model by `PR-AUC` on validation.
- Search for the best threshold using `F1` on validation.
- Retrain on train + validation and evaluate once on test.
- Generate a quick calibration check and a table of the most influential variables.

Outputs:
- Input files are read from `data/processed/`.
- Result files are saved to `data/outputs/`.
- model comparison on validation and test
- threshold search
- final metrics for the best model
- test predictions
- calibration check
- top drivers for the selected model
