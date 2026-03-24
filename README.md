# mobility-predictors

**Which structural conditions predict whether a low-income household reaches above-median income over a 10-year period?**

This project uses the Panel Study of Income Dynamics (PSID) to identify the baseline structural factors most associated with upward economic mobility. It is a classification project using logistic regression, random forest, and XGBoost — with feature importance as the primary analytical output.

---

## Research Question

Which baseline structural factors are most associated with whether lower-income households reach above median income over a 10-year period (2011–2021)?

This is a descriptive and predictive modeling project. Findings are framed as associations, not causal claims.

---

## Project Identity

This is a social mobility research project using data science tools. The goal is not to test many algorithms — it is to identify which starting conditions most predict upward mobility and produce one clear, defensible takeaway.

> *One dataset. One target. Three solid models. One clear takeaway.*

---

## Dataset

**Panel Study of Income Dynamics (PSID)**
- Longitudinal household survey tracking income, wealth, and demographics since 1968
- Baseline window: **2011** (aligns with PSID wealth supplement wave)
- Follow-up window: **2021**
- Access: [https://psidonline.isr.umich.edu](https://psidonline.isr.umich.edu) — free with registration

> **Note:** Raw PSID data is not included in this repository. Follow the access instructions above to download the data and place it in `data/raw/`.
