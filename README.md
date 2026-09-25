# aidams-lab2-bennjima-gueddas-mallat-sanver-tanaci

AIDAMS Lab 2: predicting 2024 plant-level crude steel production from the Global Energy Monitor [Global Iron and Steel Tracker (GIST)](https://globalenergymonitor.org/projects/global-iron-steel-tracker), across the full ML lifecycle.

## What's in the notebook (`lab_2.ipynb`)

1. **Data:** polars joins the three sheets into one row per plant (257 labelled plants), runs a Pandera schema check, cleans the data and engineers features.
2. **Baselines:** a median dummy and a scikit-learn pipeline, then a linear regression with coefficients interpreted.
3. **Model selection:** 5-fold CV comparing Linear Regression, Ridge and Random Forest, then `RandomizedSearchCV` tuning.
4. **Lifecycle:** MLflow tracking (SQLite), Optuna (30 trials with `MLflowCallback`), and the best pipeline saved and reloaded with joblib.
5. **Deployment and drift:** schema-gated prediction, and a Kolmogorov-Smirnov drift check.

Every task has a written "critical thinking" answer below it. The TabFM bonus was skipped.

**Result:** production is about 80 % of built capacity. The 8-variable linear model is selected, with CV RMSE ≈ 840 ttpa and test R² ≈ 0.90. Ridge and a tuned Random Forest perform about the same, within fold noise.

## How to run

1. Install [uv](https://docs.astral.sh/uv/), then run `uv sync`.
2. Download the GIST release and put `Plant-level_data_Global_Iron_and_Steel_Tracker_June_2026_V1.xlsx` in `gem-download/`. The data is not in the repo.
3. Open `lab_2.ipynb` with the `.venv` kernel and run all cells, or from a terminal:
   `uv run --with nbconvert jupyter nbconvert --to notebook --execute --inplace lab_2.ipynb`
4. Optional: browse the experiments with `uv run mlflow ui --backend-store-uri sqlite:///mlflow.db`.

Running the notebook regenerates `mlflow.db`, `mlruns/` and `models/best_pipeline.joblib`. All three are git-ignored.
