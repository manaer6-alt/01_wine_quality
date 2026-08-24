# Wine Quality Regression

A compact regression case study that predicts expert wine-quality scores from physicochemical measurements. It demonstrates systematic model comparison, honest held-out evaluation, and interpretation of regression errors.

## Results

| Metric | Held-out test |
|---|---:|
| MAE | 0.4137 |
| RMSE | 0.5844 |
| R² | 0.4707 |

The final model is an `ExtraTreesRegressor`. The error level is meaningful relative to the narrow scoring scale, while the moderate R² also makes the dataset's irreducible uncertainty explicit.

## Workflow

1. Audit distributions, correlations, and target imbalance.
2. Build simple baselines before testing non-linear ensembles.
3. Compare models using consistent validation.
4. Evaluate the selected model once on held-out data.
5. Inspect residuals and feature importance.

## Main finding

Like many regressors trained on imbalanced ordinal targets, the model tends to pull rare extreme ratings toward the center. That limitation is documented rather than hidden behind a single aggregate score.

## Repository guide

- `notebooks/` — EDA, experiments, and final evaluation.
- `src/` — reusable code, where available.
- `reports/` — figures and outputs.
- `requirements.txt` — recorded environment.

## Reproduce

Create a Python environment, install `requirements.txt`, and run the notebooks in order.

## Limitations and next steps

The target is subjective and concentrated around middle ratings. Useful extensions include ordinal modeling, calibrated uncertainty intervals, repeated cross-validation, and evaluation across wine types or vintages.

## Stack

Python · pandas · NumPy · scikit-learn · Matplotlib · Seaborn
