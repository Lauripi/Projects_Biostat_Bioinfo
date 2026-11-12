# 1. Breast Cancer Classification - Model Building, Predictor Selection and Validation - Python

A complete, leakage-aware machine learning pipeline for classifying breast tumors as malignant or benign. It is built not just to chase accuracy, but to produce a model whose feature selection and decision threshold can actually be defended and explained.

## Overview

- **Dataset:** Wisconsin Diagnostic Breast Cancer dataset (569 samples, 30 morphological features extracted from digitized biopsy images)
- **Task:** binary classification — malignant vs. benign tissue
- **Focus:** methodological rigor over a single headline number: handling multicollinearity properly, validating feature selection stability, and choosing a decision threshold aligned with clinical priorities

## Results at a glance

| Metric | Value |
|---|---|
| ROC-AUC | 0.996 |
| Accuracy | 97.4% |
| Sensitivity (malignant recall) | 95.2% |
| Specificity | 98.6% |
| Features used | 12 (reduced from 30) |

## Approach

1. **Exploratory analysis and correlation screening:** identified strong multicollinearity among size-related features (radius, perimeter, area are mathematically redundant); manually removed 9 redundant features before modeling, using training data only.\
   → *Multicollinearity diagnosis, correlation-based feature reduction, leakage-safe workflow*
2. **Model comparison:** benchmarked 6 classifiers (Logistic Regression, KNN, Linear/RBF SVM, Random Forest, Gradient Boosting) via 10-fold stratified cross-validation, with scaling fit within pipeline folds. Logistic Regression was selected for its interpretability, at no measurable cost in performance.\
   → *Systematic model benchmarking, cross-validation, scikit-learn pipelines*
3. **Regularization comparison:** compared L1, L2 and Elastic Net penalties; L1 produced the sparsest model (12 features) with no loss in ROC-AUC.\
   → *Regularized logistic regression, hyperparameter tuning (GridSearchCV), nested cross-validation*
4. **Threshold optimization:** compared a balanced (Youden) threshold against a minimum-sensitivity target, and selected the final threshold based on its best compromise between sensitivity and specificity. Chosen from training CV, then applied once to a held-out test set.\
   → *Clinical decision-analytic thinking, sensitivity/specificity trade-offs, cross-validated vs. held-out performance*
5. **Feature stability validation:** bootstrap-resampled the L1 feature selection (200 iterations) to separate genuinely robust predictors from features that only appeared due to one particular data split.\
   → *Bootstrap stability analysis*

## Tech stack

Python · scikit-learn · pandas · NumPy · matplotlib

## Files

- `1. Predictive_model_on_breast_cancer.ipynb` — full analysis notebook

---

# 2. Palmer Penguins - Data Cleaning, Visualization and Statistical Inference - R

A reproducible R workflow showing how to handle a fresh dataset: checked cleaning decisions, the right summary for each variable type, and tests chosen from verified assumptions.

## Overview

- **Dataset:** Palmer Penguins (344 penguins, 3 species, 4 body measurements plus sex and year)
- **Task:** clean and describe the data, visualize species differences, then test them statistically
- **Focus:** assumption checks before every test, with parametric and non-parametric results reported side by side

## Results at a glance

| Question | Test | Result |
|---|---|---|
| Do body measurements differ between species? | ANOVA, Kruskal–Wallis, Tukey post hoc | Yes: Gentoo differ from both other species on every measurement; Adelie and Chinstrap differ only by bill length |
| Are Gentoo males heavier than females? | Welch t-test | Yes, by ~800 g (95% CI 700–910 g, p < 0.001) |
| Is sex balanced across species? | Chi-square test of independence | Yes (p = 0.98) |
| Are flipper length and body mass linked? | Pearson correlation | Strong positive correlation (r = 0.87, 95% CI 0.84–0.89) |
| Are bill length and bill depth linked? | Scatter plots, pooled vs by species | Negative when pooled, positive within each species (Simpson's paradox) |

## Approach

1. **Cleaning:** missing values per variable, the 2 empty rows identified by code and dropped, birds of unknown sex kept, range check on every numeric variable.\
   → *Data wrangling in R: tidyverse pipelines (dplyr, tidyr, purrr)*
2. **Description and visualization:** counts for categorical variables, mean ± SD and median (Q1/Q3) per species for continuous ones; density, scatter and violin plots, including a textbook Simpson's paradox on bill shape.\
   → *ggplot2, patchwork, long/wide reshaping, confounding and Simpson's paradox*
3. **Inference:** Shapiro–Wilk normality check, ANOVA and Kruskal–Wallis side by side, Tukey post hoc, Welch t-test, chi-square, Pearson correlation.\
   → *Assumption checking, omnibus vs post hoc tests, reproducible reporting (Quarto, flextable)*

## Tech stack

R · tidyverse · Quarto · flextable · patchwork · ggbeeswarm · wrappedtools

## Files

- `2. Data_Cleaning_Visualisation_Statistical_Inference.md` — rendered results, viewable on GitHub
- `Data_Cleaning_Visualisation_Statistical_Inference_files/` — tables and graphs used by the .md file

---

# 3. Laboratory and Greenhouse Databases - Database Design and Stored Procedures - SQL

MySQL exercises focused on relational database design and basic SQL programming, created to practice structuring relational databases and applying SQL concepts to scientific datasets.

## Overview

- **Dataset:** two databases built from scratch — a **laboratory database** (experiments, employees, tools, results) and a **greenhouse database** (plants, gardeners, greenhouses) — plus stored procedures on a protein-structure database
- **Task:** design the tables, link them with keys, and write reusable queries
- **Focus:** clean relational design, with data integrity enforced by constraints

## Databases at a glance

| Database | Tables | Highlights |
|---|---|---|
| Laboratory | 7 (incl. 3 linking tables) | Experiments linked to employees, tools and results (many-to-many) |
| Greenhouse | 5 (incl. 2 linking tables) | Gardeners and plants linked to greenhouses (many-to-many) |
| Protein structures | queried only | 7 stored procedures, incl. subqueries |

## Approach

1. **Stored procedures:** wrote reusable procedures to query organisms, proteins and structures, using filters and subqueries.\
   → *Stored procedures, subqueries and filtering*
2. **Database design:** created the laboratory and greenhouse databases, with primary keys, foreign keys and linking tables for many-to-many relationships.\
   → *Relational database design, primary and foreign keys, many-to-many relationships*
3. **Data integrity:** added `CHECK`, `UNIQUE`, `NOT NULL` and `DEFAULT` constraints (e.g. e-mail format, positive salaries, allowed weekdays).\
   → *Data validation with constraints*

## Tech stack

SQL · MySQL

## Files

- `3. SQL-Stored_Procedures_and_Database-Creation.sql` — SQL code only

---

# 4. Crime Incidents - Data Cleaning, Exploration and Statistical Analysis - Python

A Python workflow applied to a messy, simulated crime dataset: from raw data with realistic quality problems to an analysis-ready table, followed by exploratory analysis and statistical tests. Cleaning decisions are checked and explained, and imputed values are flagged rather than hidden.

## Overview

- **Dataset:** simulated crime incident dataset from [Kaggle](https://www.kaggle.com/datasets/sananshaikh/messy-crime-dataset-for-data-cleaning-practice), 5,250 records describing crimes, locations, victims, suspects, officers, arrests, property loss and case status
- **Task:** clean the raw data, then use it to answer three analysis questions
- **Focus:** transparent cleaning decisions, checks before and after each transformation, and statistical tests chosen to fit the data

## Cleaning at a glance

| Problem | Action | Result |
|---|---|---|
| Duplicate records | Removed | 5,250 → 5,050 rows |
| Typos, abbreviations and synonyms | Similarity detection (`difflib`) + manual review | e.g. crime type: 58 spellings → 4 categories; district: 16 → 10 |
| Malformed amounts (e.g. `35446.9.0`) | Repaired before numeric conversion | 136 values recovered |
| Three date formats, incl. US month-first | Each format converted explicitly | No swapped days and months |
| Impossible values (negative numbers, ages up to 298, latitudes > 90) | Sign corrected or set to missing | 331 ages and 176 coordinates set to missing |
| Missing victim/suspect gender | Inferred from first names (`gender-guesser`) | 1,964 values imputed and flagged |

## Results at a glance

| Question | Test | Result |
|---|---|---|
| Do incident counts differ between cities? | Chi-square goodness-of-fit | Yes (χ² = 15.6, p = 0.03); Riverside has the most incidents (705) |
| Does the crime-type mix differ between cities? | Chi-square test of independence | No (p = 0.26) |
| Did incidents change over time (2018–2024)? | Poisson regression on monthly counts | No significant trend (+1.1% per year, p = 0.12) |
| Do incidents vary by season? | Chi-square goodness-of-fit | No (p = 0.32) |
| Is crime severity linked to property loss? | Kruskal–Wallis, Spearman correlation | No (p = 0.79; ρ = 0.01, p = 0.39) |

## Approach

1. **Initial data audit:** inspected datatypes, missing values, duplicates, categorical levels and numeric ranges to identify the main data-quality problems.\
   → *Data quality assessment, duplicate detection*
2. **Text and datatype standardization:** trimmed whitespace, standardized capitalization, stored identifiers as text and converted wrongly stored numbers to numeric values.\
   → *String normalization, datatype conversion*
3. **Categorical cleaning:** detected spelling errors by string similarity, reviewed every suggested correction manually, harmonized abbreviations and synonyms, and grouped crime types into four broad categories.\
   → *Typo detection, synonym harmonization, manual quality control*
4. **Missing-data handling:** assessed missingness variable by variable instead of applying one imputation rule; imputed gender from first names with explicit imputation and uncertainty flags, and left other missing values as missing.\
   → *Selective imputation with explicit flags*
5. **Numeric and geographic validation:** checked distributions and plausible ranges, corrected sign errors and set impossible values to missing.\
   → *Range validation*
6. **Date and time preprocessing:** split date and time, standardized three date formats, and created time-of-day and season variables.\
   → *Mixed date formats, feature engineering*
7. **Exploration and analysis:** described the main variables (summary statistics, bar charts, histograms, box plots, trend plots), then answered three questions with statistical tests. Numeric variables are not normally distributed, so non-parametric tests were used where needed.\
   → *Chi-square tests, Poisson regression for count data, non-parametric tests (Kruskal–Wallis, Spearman), matplotlib*

## Tech stack

Python · pandas · NumPy · SciPy · statsmodels · matplotlib · difflib · gender-guesser

## Files

- `4. Crimes_cleaning_data.ipynb` — full analysis notebook

---

# 5. PBC2 Clinical Trial - Survival Analysis and Prognostic Factors - R (in progress)

A survival analysis of a randomized clinical trial with repeated patient visits. The aim is to test whether the treatment improved survival, and to find which patient characteristics predict the risk of death. Every choice (data cleaning, missing data, variable selection) is explained and justified.

> **Status:** the baseline analysis is done; the analysis of repeated measurements over time is coming next.

## Overview

- **Dataset:** `pbc2` from the R package JMbayes2 — Mayo Clinic randomized trial in primary biliary cirrhosis (PBC), D-penicillamine vs placebo. 312 patients, 1,945 visits (1 to 16 per patient, median 5), 20 variables (lab values, clinical signs, treatment, outcome)
- **Task:** compare survival between the two arms, then identify prognostic factors for death
- **Focus:** careful data checks, understanding why data are missing, and a Cox model with no more variables than the number of deaths allows

## Results at a glance

| Question | Method | Result |
|---|---|---|
| Did D-penicillamine improve survival? | Kaplan–Meier, log-rank test | No, the curves overlap (p = 0.99) |
| Any treatment effect after adjusting for risk factors? | Cox regression | No (HR 0.84, 95% CI 0.59–1.19, p = 0.33) |
| What increases the risk of death? | Cox regression | Older age (HR 1.04 per year), higher bilirubin (HR 2.48 per log unit), histologic stage 4 vs 1–2 (HR 1.85) |
| What lowers the risk of death? | Cox regression | Higher albumin (HR 0.52 per g/dL) |
| How well does the model rank patients by risk? | Concordance index | 0.84 (good discrimination) |

## Approach

1. **Data audit:** checked dimensions, data types, duplicates (0), empty rows (0) and missing values; counted patients and visits per patient.\
   → *Longitudinal data handling (one row per visit)*
2. **Distributions:** histograms and summary tables at baseline and across all visits. Skewed lab values (bilirubin, alkaline phosphatase, SGOT) were log-transformed for modelling.\
   → *Baseline extraction, tidyverse pipelines, flextable tables*
3. **Missing data:** checked whether missing values depend on treatment arm, visit number, outcome or disease stage. Cholesterol (42% missing over follow-up) was excluded. Missing alkaline phosphatase and platelet values are linked to disease severity: not MCAR, consistent with MAR (MNAR cannot be excluded).\
   → *Missing-data mechanisms (MCAR / MAR / MNAR), missingness by visit*
4. **Survival comparison:** Kaplan–Meier curves by arm with a log-rank test. Death is the event; patients alive or transplanted at the end of follow-up are censored.\
   → *Kaplan–Meier curves, log-rank test, censoring, comparing treatment arms*
5. **Variable selection:** 140 deaths for 18 candidate coefficients = 7.8 events per coefficient, below the usual minimum of 10 (Peduzzi et al., 1995). Reduced to 8 coefficients (17.5 events each), with no patient dropped, using literature published before this trial, correlation checks (edema dropped, redundant with ascites) and merging of small categories (histologic stages 1 and 2).\
   → *Events-per-variable rule, Spearman and Cramér's V, selecting prognostic factors from the literature*
6. **Cox regression:** main model on baseline data (treatment, age, bilirubin, albumin, histologic stage, hepatomegaly, ascites), compared with an extended model adding 4 lab values (likelihood-ratio test and AIC, same 308 patients).\
   → *Cox regression, hazard ratios, forest plots, nested model comparison*

## Coming next

- Interpretation of the main vs extended Cox model comparison
- Use of the repeated measurements over follow-up, not only baseline values

## Tech stack

R · tidyverse · survival · survminer · flextable · corrplot · DescTools · Quarto

## Files

- `5. PBC2_clin-trial_longitudinal-survival-IN-PREPARATION.md` — rendered analysis and interpretation, viewable on GitHub (in progress)
- `5.-PBC2_clin-trial_longitudinal-survival-IN-PREPARATION_files/` — graphs used by the .md file
