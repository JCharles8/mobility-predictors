# Mobility Predictors: Identifying Structural Conditions for Upward Economic Mobility

**Author:** Jeff Charles (JC)
**Course:** DSE6311 — Capstone, Merrimack College
**Date:** May 2026
**Stakeholders:** Urban Institute and National Low Income Housing Coalition (NLIHC)

---

## Project Overview

This project uses the Panel Study of Income Dynamics (PSID) to identify which structural baseline conditions best predict whether a household in the bottom 40 percent of the U.S. income distribution in 2010 crossed the national median income threshold by 2021. Three supervised classification models (logistic regression, random forest, and XGBoost) were applied to a panel-linked sample of 2,375 households. SHAP analysis was used for post-hoc interpretability on the best-performing model.

**Research Question:**
Among households in the bottom 40 percent of the U.S. income distribution in 2010, which combination of structural baseline conditions (including wealth, housing cost burden, family structure, educational attainment, employment status, geography, and demographic characteristics) best predicts whether a household crosses the national median income threshold by 2021?

**Key Findings:**
- Age of household head is the dominant predictor across all three models (RF Gini importance: 32.9%, SHAP mean |value|: 0.79)
- Race: White (Non-Hispanic) ranks second in all three models after controlling for wealth, education, income, housing cost, and geography
- The racial mobility gap (33.2% White vs. 13.3% Black) survives every structural control applied
- XGBoost achieves the best cross-validated AUC: 0.811
- SHAP dependence analysis finds education and wealth operate as roughly independent contributors; Prediction 4 (education conditional on wealth) is partially disconfirmed

---

## Quick View (No Setup Required)

Open **Final_Code.html** in any browser to view the fully rendered notebook with all outputs and figures already populated. No Python installation needed.

---

## Repository Structure

```
mobility-predictors/
├── Final_Code.ipynb                          # Main analysis notebook (EDA + preprocessing + modeling + SHAP)
├── Final_Code.html                           # Rendered HTML version — open in any browser
├── psid_panel.csv                            # Panel-linked analytic dataset (2,375 households)
├── Mobility_Predictors_Final_Report.docx     # Final written deliverable
├── Mobility_Predictors_Final_Report.pdf      # PDF version of the final report
├── output/                                   # Generated figures
│   ├── shap_fig10_summary_bar.png
│   ├── shap_fig11_beeswarm.png
│   ├── shap_fig12_dependence_edu_wealth.png
│   └── (additional EDA figures)
└── README.md
```

---

## Running the Notebook

**Requirements:** Python 3.8+, Jupyter

**Install dependencies:**

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn xgboost shap jupyter
```

**Clone the repo and run:**

```bash
git clone https://github.com/JCharles8/mobility-predictors.git
cd mobility-predictors
mkdir -p output
jupyter notebook Final_Code.ipynb
```

Once Jupyter opens, run all cells in order via Kernel > Restart & Run All.

`psid_panel.csv` must be in the same directory as the notebook. It is included in the repository, so no additional data download is needed.

**Expected runtime:** 10-20 minutes, dominated by RandomizedSearchCV for Random Forest and XGBoost (n_iter=50 each).

---

## Notebook Structure

| Section | Cells | Description |
|---------|-------|-------------|
| Custom Functions | 1 | `sign_preserving_log`, `mobility_summary` defined at top for global availability |
| Setup & Load | 2-3 | Load data, define colors and category orders |
| EDA Sections 1-5 | 4-8 | Group comparisons, chi-squared tests, correlations, outlier detection, interaction effects |
| EDA Figures 1-9 | 9-17 | KDE plots, mobility rate charts, correlation matrix, beeswarm, dependence |
| Preprocessing | 18-23 | Train-test split, missing indicator, scaling, MICE imputation, K-Means clustering |
| Validation | 24-25 | Leakage audit, assertion-based pipeline checks |
| Logistic Regression | 26 | Baseline model with 5-fold CV, coefficients |
| Random Forest | 27 | OOB error curve, RandomizedSearchCV tuning |
| XGBoost | 28 | RandomizedSearchCV tuning |
| Model Comparison | 29 | Performance table and feature importance comparison |
| SHAP Analysis | 30-34 | TreeExplainer, Figure 10 (summary bar), Figure 11 (beeswarm), Figure 12 (dependence) |

---

## Validation

Cell 25 runs five assertion-based checks to confirm pipeline integrity:

1. Train/test positive class rates preserved within tolerance (+/-0.01)
2. No missing values remain after MICE imputation
3. Cluster labels only contain valid values (0, 1, 2)
4. `sign_preserving_log` preserves sign direction
5. Train and test sets have non-overlapping indices

All five pass on the author's machine (self-tested May 2026).

---

## Custom Functions

**`sign_preserving_log(x)`**

```python
def sign_preserving_log(x):
    return np.sign(x) * np.log1p(np.abs(x))
```

Applies a log transformation that handles negative values by preserving sign. Used on household net worth, which includes negative values (debt exceeds assets) for roughly 25 percent of the sample. Reduces skewness from approximately 17.0 to approximately 4.5.

```python
sign_preserving_log(np.array([-1000, 0, 1000]))
# array([-6.908,  0.   ,  6.908])
```

---

**`mobility_summary(df, group_col, target_col='above_median_2021', order=None)`**

```python
def mobility_summary(df, group_col, target_col='above_median_2021', order=None):
    grp = df.dropna(subset=[group_col]).groupby(group_col)[target_col]
    summary = grp.agg(['mean', 'count']).rename(columns={'mean': 'mobility_rate', 'count': 'n'})
    summary['mobility_rate_pct'] = (summary['mobility_rate'] * 100).round(1)
    summary['ci_95'] = (1.96 * np.sqrt(
        summary['mobility_rate'] * (1 - summary['mobility_rate']) /
        summary['n'].replace(0, np.nan)
    ) * 100).round(2)
    return summary[['mobility_rate_pct', 'n', 'ci_95']]
```

Computes mobility rate, count, and 95 percent confidence interval by group. Used throughout EDA for consistent group-level summaries across categorical predictors.

```python
mobility_summary(df, 'education', order=edu_order)
mobility_summary(df, 'race_ethnicity')
```

---

## Data

**Source:** Panel Study of Income Dynamics (PSID), University of Michigan Institute for Social Research
**Access:** [psidonline.isr.umich.edu](https://psidonline.isr.umich.edu) (free registration required)

`psid_panel.csv` is the cleaned, panel-linked analytic dataset included in this repository. The raw PSID source files (FAM2011ER, FAM2021ER, IND2023ER) are not redistributable and must be downloaded directly from the PSID portal if reproducing the data construction step.

---

## Citation

Charles, J. (2026). *Mobility predictors: Identifying structural conditions for upward economic mobility* [Capstone project]. Merrimack College, DSE6311.

---

## References

Chetty, R., Hendren, N., Kline, P., & Saez, E. (2014). Where is the land of opportunity? *Quarterly Journal of Economics, 129*(4), 1553-1623.

Desmond, M. (2016). *Evicted: Poverty and profit in the American city.* Crown Publishers.

Lundberg, S. M., & Lee, S.-I. (2017). A unified approach to interpreting model predictions. *Advances in Neural Information Processing Systems, 30.*

Putnam, R. D. (2015). *Our kids: The American dream in crisis.* Simon & Schuster.

University of Michigan Institute for Social Research. (2023). *Panel Study of Income Dynamics* [public use dataset]. Survey Research Center. https://psidonline.isr.umich.edu
