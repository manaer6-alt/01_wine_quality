# 01_wine_quality

## Project goal

This project is part of `ML Projects Batch 01`.

The main educational goal is to understand how task framing affects the whole machine learning workflow.

The dataset target `quality` can be framed as:

1. regression target;
2. multiclass classification target;
3. binary classification target using a quality threshold.

For the first full iteration, the selected framing was regression.

## Dataset

Dataset: Wine Quality Dataset  
Source: Kaggle — `yasserh/wine-quality-dataset`

The dataset contains physicochemical wine measurements and a target quality score.

Dataset shape:

```text
1143 rows, 13 columns

Target:

quality

Excluded column:

Id

Feature columns:

fixed acidity
volatile acidity
citric acid
residual sugar
chlorides
free sulfur dioxide
total sulfur dioxide
density
pH
sulphates
alcohol
Task framing

The target quality is an ordinal score with observed values:

3, 4, 5, 6, 7, 8

The first iteration treats quality as a regression target.

This means the model predicts a continuous quality score.

Primary metrics:

MAE
RMSE
R²

Alternative task framings, such as multiclass classification and binary classification, were reserved for later experiments.

Target distribution

The target is imbalanced and concentrated mostly around quality values 5 and 6.

quality 3:   6 observations
quality 4:  33 observations
quality 5: 483 observations
quality 6: 462 observations
quality 7: 143 observations
quality 8:  16 observations

This imbalance is an important limitation, especially for rare quality values.

Project stages

Completed stages:

Stage 0 — Dataset audit and task framing
Stage 1 — Local project setup, .venv, Git/GitHub
Stage 2 — Initial EDA
Stage 3 — Baseline regression model
Stage 4 — Controlled model comparison
Stage 5 — Controlled hyperparameter tuning
Stage 6 — Final evaluation on held-out test set
Stage 7 — Save final pipeline and update README
Modeling process
Stage 3 baseline

Models evaluated with cross-validation on X_train only:

DummyRegressor
LinearRegression
Ridge with StandardScaler inside Pipeline
DecisionTreeRegressor(max_depth=3)

Best simple baseline:

Ridge / LinearRegression

Approximate CV metrics:

MAE  ≈ 0.513
RMSE ≈ 0.660
R²   ≈ 0.346
Stage 4 model comparison

Additional model families were compared using 5-fold cross-validation on X_train only:

RandomForestRegressor
ExtraTreesRegressor
GradientBoostingRegressor
HistGradientBoostingRegressor
SVR with StandardScaler inside Pipeline

Best Stage 4 model:

ExtraTreesRegressor

Approximate CV metrics:

MAE  ≈ 0.4252
RMSE ≈ 0.5883
R²   ≈ 0.4632

Main finding:

Tree-based ensemble models improved meaningfully over Ridge and LinearRegression, suggesting that nonlinear patterns and feature interactions matter for this dataset.

Stage 5 tuning

Tuned models:

ExtraTreesRegressor
RandomForestRegressor
GradientBoostingRegressor

Best tuned model:

ExtraTreesRegressor

Best parameters:

max_depth=None
max_features=1.0
min_samples_leaf=2
n_estimators=500
random_state=42
n_jobs=-1

Stage 5 CV metrics:

MAE  ≈ 0.4414
RMSE ≈ 0.5854
R²   ≈ 0.4688

Tuning did not give a clearly meaningful improvement:

RMSE improved only slightly
MAE became worse compared with the Stage 4 ExtraTrees baseline
Final model

Final frozen candidate:

ExtraTreesRegressor(
    max_depth=None,
    max_features=1.0,
    min_samples_leaf=2,
    n_estimators=500,
    random_state=42,
    n_jobs=-1,
)

The final saved artifact is a scikit-learn Pipeline containing the final ExtraTreesRegressor.

Artifact path:

models/final_extra_trees_pipeline.joblib

The model artifact is not tracked by Git.

Final test evaluation

The final frozen candidate was evaluated once on the held-out test set.

Train/test split:

test_size=0.2
random_state=42
stratify=y

Final test metrics:

MAE  ≈ 0.4137
RMSE ≈ 0.5844
R²   ≈ 0.4707

Comparison with Stage 5 CV:

CV MAE  ≈ 0.4414 → test MAE  ≈ 0.4137
CV RMSE ≈ 0.5854 → test RMSE ≈ 0.5844
CV R²   ≈ 0.4688 → test R²   ≈ 0.4707

Conclusion:

The final test metrics aligned well with cross-validation expectations. There were no strong signs of overfitting or instability.

Key limitations

The model works best for common quality values 5 and 6.

Error by true quality showed that rare and extreme quality values are harder:

quality 3: high error, very few examples
quality 4: high error, few examples
quality 8: high error, very few examples

The model tends to compress predictions toward common middle quality values.

Rounded prediction diagnostics showed that after rounding continuous predictions to integer quality labels, the model did not predict classes 3, 4, or 8.

This means the regression model is useful for approximate quality scoring, but it is not a strong exact-class predictor across all quality levels.

Leakage controls

Leakage controls used throughout the project:

Id was excluded from the feature matrix.
The holdout test set was not used during model comparison or tuning.
Cross-validation was performed only on X_train.
GridSearchCV was performed only on X_train.
No preprocessing was fitted on the full dataset before evaluation.
No outlier removal was performed.
No target transformation was applied.
No multiclass or binary framing was used in the first full iteration.
The final candidate was frozen before test evaluation.
X_test was evaluated exactly once.
No retuning was performed after seeing test results.
No parameter changes were made after test evaluation.
How to reproduce
1. Create and activate virtual environment
python -m venv .venv
.\.venv\Scripts\Activate.ps1
2. Install dependencies
pip install -r requirements.txt
3. Place dataset

Put the raw Kaggle dataset file here:

data/raw/WineQT.csv
4. Run notebooks in order
notebooks/00_dataset_audit_and_task_framing.ipynb
notebooks/01_initial_eda.ipynb
notebooks/02_baseline_regression.ipynb
notebooks/03_model_comparison.ipynb
notebooks/04_tuning.ipynb
notebooks/05_final_evaluation.ipynb
notebooks/06_save_final_pipeline.ipynb
Project structure
01_wine_quality/
├── data/
│   ├── raw/
│   └── processed/
├── models/
│   └── final_extra_trees_pipeline.joblib  # ignored by Git
├── notebooks/
│   ├── 00_dataset_audit_and_task_framing.ipynb
│   ├── 01_initial_eda.ipynb
│   ├── 02_baseline_regression.ipynb
│   ├── 03_model_comparison.ipynb
│   ├── 04_tuning.ipynb
│   ├── 05_final_evaluation.ipynb
│   └── 06_save_final_pipeline.ipynb
├── reports/
├── src/
├── .gitignore
├── README.md
└── requirements.txt
Current status

The first full regression iteration is complete.

Final model artifact was saved locally but is not tracked by Git.

Potential future experiments:

multiclass classification framing
binary classification framing with quality >= 7
ordinal-specific modeling
target transformation experiments
outlier handling experiments inside CV

---

## 5. Git checks before commit

После сохранения notebook и README выполни:

```powershell
git status

Важно проверить:

README.md должен быть untracked/modified
notebooks/06_save_final_pipeline.ipynb должен быть untracked/modified
models/final_extra_trees_pipeline.joblib не должен попадать в Git
.venv не должен попадать в Git