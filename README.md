HIT140 Assessment 3 — Objective 2: FIFA World Cup 2026

Group 64 · HIT140 Foundations of Data Science · Charles Darwin University

This repository contains the code, data and report for Objective 2 of Assessment 3. The aim is to test whether facts known before kick-off (team rankings, past scoring, squad value and rest days) can predict:

Objective 2.1: the goal difference in each match (104 matches).
Objective 2.2: the number of goals each team scores (208 team-match rows, two per match).

Both objectives use eight predictors and linear regression, with Ridge and a degree-2 polynomial model for comparison.

Repository contents
File	Purpose
HIT140_World_Cup_2026_FINAL.xlsx	Data. Sheet Objective_2.1 (104 matches) and sheet Objective_2.2 (208 team-match rows).
EDA.ipynb	Exploratory analysis: data types, missing values, summary statistics, skew and kurtosis, outliers (IQR rule), draw and win counts, predictor histograms.
Visualization.ipynb	Charts for both objectives: correlation heatmaps, coefficient bar charts, actual-versus-predicted plots, residual plots, target distributions.
Optimization.ipynb	Model tuning: exhaustive search over all 219 subsets of 3–8 predictors, plus polynomial and Ridge experiments. Results are in-sample.
notebooks_traintest.ipynb	Single 80/20 train/test split (seed 42) for linear, Ridge and polynomial models. Reports held-out R², MAE and gap.
eval_grouped_cv.py	Grouped and cross-validated evaluation: row-level and grouped splits, five-fold KFold (2.1) and GroupKFold by match (2.2), Ridge α tuned by cross-validation, and the mean-predictor baseline.
HIT140_Objective2_Report.docx	Group report (four pages).
Requirements

Python 3 with the following packages:

pandas
numpy
scikit-learn
scipy
matplotlib
seaborn
openpyxl (needed to read the Excel file)

The notebooks include %pip install lines for seaborn and openpyxl. If you are running them somewhere those are already installed, you can skip those cells.

How to reproduce the results

Place the notebooks, the script and the workbook in the same folder, then run them in this order:

EDA.ipynb: data checks and summary statistics.
Visualization.ipynb: charts.
Optimization.ipynb: feature search and in-sample comparison.
notebooks_traintest.ipynb: single-split held-out results.
eval_grouped_cv.py: grouped split and cross-validation (run from the command line with python eval_grouped_cv.py).

The script first checks that each pair of 2.2 rows forms one match, using the 2.1 team names. If that check fails, it stops and asks for a Match_ID column in the Objective_2.2 sheet.

All random splits use random_state = 42.

Main results (held-out)

Mean over five folds. R² is higher-is-better. MAE is in goals.

Model	2.1 CV R²	2.1 CV MAE	2.2 grouped CV R²	2.2 grouped CV MAE
Mean baseline	−0.093	1.244	−0.039	1.126
Linear regression	−0.058	1.177	0.263	0.912
Ridge (α = 1)	−0.055	1.176	0.263	0.911
Polynomial (degree 2)	−5.067	2.003	−0.210	1.137

Tuned Ridge: α = 100 for 2.1 (CV R² 0.04, at the top of the grid tested) and α = 10 for 2.2 (CV R² 0.27).

In short, goals scored (2.2) can be predicted moderately well out of sample. Goal difference (2.1) is close to unpredictable with these predictors.

Known issues
Mirrored rows in 2.2. Each match appears as two rows, so a random row-level split can leak information between the two teams. Use the grouped split for 2.2. The row-level test R² (0.376) is optimistic compared with the grouped figure (0.326).
Tuning on the same folds. Ridge α is tuned on the same folds it is scored on, so the tuned scores are slightly optimistic. Nested cross-validation would remove this.
Ridge grid edge. The best α for 2.1 is 100, the top of the grid. Extend the grid before relying on it.
Hard-coded baselines. Optimization.ipynb contains baseline values (0.449 and 0.287) that cannot be reproduced from the code. They are not used in the report.
Undefined functions. A "Model 2" cell in Optimization.ipynb calls evaluate_model and result_m1, which are not defined in the notebook.
Negative average goals. Avg_Goals_Scored has a minimum of −0.50 in the 2.2 sheet, which is not possible. Trace it to the source.
Team order. In 2.1, 73 of 104 margins are positive for Team A. Check whether the Team A and Team B ordering is consistent with the source.
Predictor sign conventions. Some signs (for example, Elo difference in 2.1) are counter-intuitive. Check variance inflation factors for Elo and FIFA ranking.




Generative AI use

[Team to complete, consistent with the signed AI Usage Declaration Form.]
