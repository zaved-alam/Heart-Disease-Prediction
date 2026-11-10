# Heart Disease Prediction (CSE570 Skill Based Assignment)

End-to-end machine-learning solution for predicting the presence of heart
disease from the UCI Cleveland dataset (303 records, 14 attributes).

This repository is organised so that development is **incremental**: every
workflow stage is a small notebook combining multiple cells, so you can commit,
review and explain each stage individually. This maps directly to **Q4 (GitHub,
20 marks)** — meaningful progressive commits, branch-based pull requests, and a
clean repository.

## Assignment coverage

| Rubric section | Marks | Covered by |
|---|---|---|
| Q1A Problem Statement + Dataset | 10 | `report/` + README |
| Q2B-1a Data Cleaning & Visualization | 5 | `01_dataprep_*` |
| Q2B-1b EDA & Statistical Analysis | 10 | `02_eda_*` |
| Q2B-1c Model Development & Evaluation | 15 | `03_modeling_*` |
| Q2B-2 Report + Technical Writing | 10 | `report/` |
| Q2B-3 Viva | 20 | — |
| Q3 LinkedIn | 10 | `linkedin/` evidence |
| Q4 GitHub | 20 | this repository |

## Project workflow / run order

These notebooks must be run **in order** because each reads the output file of
the previous stage. Each notebook groups together the multiple cells of one
workflow stage.

### Phase 1 — Data Preparation & Visualization (`01_dataprep_*`)
| Run | Notebook | Stage |
|---|---|---|
| 1 | `01_dataprep_01_setup_and_loading` | Load UCI Cleveland data (+ schema) |
| 2 | `01_dataprep_02_data_cleaning` | Missing values, duplicates, dtypes, target binarization, outliers |
| 3 | `01_dataprep_03_train_test_split` | Stratified 80/20 split (leakage-safe) |
| 4 | `01_dataprep_04_preprocessing_pipeline` | ColumnTransformer pipeline |
| 5 | `01_dataprep_05_visualizations` | Target / numeric / categorical / correlation plots |
| 6 | `01_dataprep_06_save_clean` | Save `heart_disease_clean.csv` |

### Phase 2 — EDA & Statistical Analysis (`02_eda_*`)
| Run | Notebook | Stage |
|---|---|---|
| 7 | `02_eda_01_setup_and_load` | Load cleaned data |
| 8 | `02_eda_02_univariate_analysis` | Descriptive stats, skewness, distributions |
| 9 | `02_eda_03_numeric_bivariate` | Group comparison, boxplots, Welch / Mann-Whitney tests |
| 10 | `02_eda_04_categorical_bivariate` | Chi-square, Cramér's V, proportion bars |
| 11 | `02_eda_05_multivariate_analysis` | Correlation matrix, pairplot |
| 12 | `02_eda_06_feature_selection` | Backward elimination |
| 13 | `02_eda_07_summary_and_save` | Summary table + save stats CSVs |

### Phase 3 — Model Development & Evaluation (`03_modeling_*`)
| Run | Notebook | Stage |
|---|---|---|
| 14 | `03_modeling_01_setup_split_pipeline` | Setup, split, pipeline |
| 15 | `03_modeling_02_baseline_dummy` | Baseline (majority class) |
| 16 | `03_modeling_03_model_zoo_cv` | 6 models, 5-fold stratified CV |
| 17 | `03_modeling_04_hyperparameter_tuning` | GridSearchCV (scoring = F1) |
| 18 | `03_modeling_05_final_model_selection` | Select final model by CV only |
| 19 | `03_modeling_06_test_evaluation` | Test metrics, confusion matrices, ROC, report, bootstrap CIs |
| 20 | `03_modeling_07_model_interpretation` | Feature importance / coefficients |
| 21 | `03_modeling_08_save_model` | Persist model (`best_heart_disease_model.joblib`) |
| 22 | `03_modeling_09_limitations` | Limitations & improvements |

## Dependencies

`requirements.txt` (install with `pip install -r requirements.txt`):
numpy, pandas, matplotlib, seaborn, scipy, scikit-learn, statsmodels, joblib.

## Dataset

- **Source:** UCI Machine Learning Repository — Heart Disease (Cleveland,
  303 records). https://archive.ics.uci.edu/dataset/45/heart+disease
- **File:** `processed.cleveland.data` (comma-separated, no header, missing
  values encoded as `?`). If the download in `01_dataprep_01` fails, place the
  file next to the notebooks.

## Suggested GitHub workflow (for Q4 marks)

Develop each phase on its own branch and merge with a pull request:

```bash
git init
git add README.md requirements.txt split_notebooks.py processed.cleveland.data
git commit -m "chore: initial repo skeleton, dataset and requirements"

git checkout -b feature/data-preparation
# commit each dataprep notebook individually, with a specific message, e.g.:
git add 01_dataprep_01_setup_and_loading.ipynb
git commit -m "add: load UCI Cleveland heart-disease data with dtype schema"
git add 01_dataprep_02_data_cleaning.ipynb
git commit -m "add: clean data - missing values, duplicates, dtypes, target binary, outliers"
git add 01_dataprep_03_train_test_split.ipynb
git commit -m "add: leakage-safe stratified 80/20 train-test split"
# ... and so on for every stage ...
git checkout main
git merge --no-ff feature/data-preparation -m "feat(data-preparation): complete Phase 1"

git checkout -b feature/eda-statistical
# similarly commit 02_eda_* one by one, then PR/merge

git checkout -b feature/modeling
# similarly commit 03_modeling_* one by one, then PR/merge
```

On GitHub: open a pull request per branch with a meaningful title/description
(e.g. "Add Phase 2 — EDA & statistical analysis"). That earns the *Pull
Requests (5 marks)* and *Commits (10 marks)* components.

## Regenerating the step notebooks

`split_notebooks.py` regenerates all stage notebooks from the three original
notebooks. If you edit the originals, re-run it and re-commit the updated files.

```
python split_notebooks.py   # rebuilds the notebooks_steps/ folder
```

> Each output notebook groups a stage's markdown + code into one file without
> splitting individual cells. During regeneration, the script also strips
> internal workflow annotations (e.g. `Day 1-2`, `Unit I/II/IV`, syllabus
> alignment notes) from headings and body text so the submitted notebooks are
> clean. The repo is not yet a git repository — run `git init` to begin
> committing.