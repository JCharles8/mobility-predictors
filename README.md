# Mobility Predictors: Identifying Structural Conditions for Upward Economic Mobility
Author: Jeff Charles
Program: Master of Science in Data Science — Merrimack College

Mobility Predictors: Which Baseline Structural Conditions Are Most Associated with Upward Economic Mobility?
---
## Brief Background

The United States has long positioned upward economic mobility as a cornerstone of national identity, the idea that hard work and opportunity, regardless of starting point, can lead to a better life. However, the empirical record increasingly challenges this narrative. As Chetty et al. (2017) demonstrate, the fraction of children earning more than their parents has fallen from approximately 90% for those born in 1940 to just 50% for those born in 1984. Further, a child born into the bottom income quintile has only a 7.5% chance of reaching the top quintile, among the lowest rates of any peer nation (Chetty et al., 2014).

These findings point toward structural conditions rather than individual effort as the primary drivers of mobility outcomes. Wealth, housing cost burden, geographic opportunity, and access to stable employment shape trajectories in ways that compound over time and vary substantially across demographic groups. Understanding which of these structural conditions most predict mobility is not only an academic question, it has direct implications for housing policy, workforce development, and social safety net design.

This project uses the Panel Study of Income Dynamics (PSID), the gold standard longitudinal household survey in the United States, to examine which baseline structural conditions are most associated with whether a lower-income household reaches above median income over a 10-year period. The analysis is predictive and descriptive, not causal.

---

## Research Question

**Which baseline structural factors are most associated with whether lower-income households reach above median income over a 10-year period (2011 to 2021)?**

More specifically: among households in the bottom 40% of the income distribution in 2011, which combination of structural conditions including wealth, cost burden, family structure, education, employment, geography, and demographics best predicts whether a household crosses the national median income threshold by 2021? This question is motivated by a recognized gap between what the public believes drives mobility and what the data actually shows (Chetty et al., 2014), with direct relevance to policymakers, researchers, and organizations working on economic equity.

---

## Hypothesis and Predictions

**Primary hypothesis:** Structural wealth and cost burden at baseline are stronger predictors of upward income mobility than educational attainment alone, and their predictive power compounds across demographic groups.

This hypothesis is grounded in Chetty et al.'s (2014) finding that neighborhood-level structural conditions outperform individual characteristics in predicting long-run income outcomes. It is further supported by evidence that parental wealth, distinct from income, buffers households against shocks and enables investment, while housing cost burdens function as a structural constraint on savings and mobility capacity regardless of earnings (Desmond, 2016).

**Specific predictions:**

- Household wealth proxy at baseline will rank among the top three most important features in the Random Forest and XGBoost models, outperforming educational attainment. This is supported by research showing wealth's compounding intergenerational effect on opportunity (Chetty et al., 2014).
- Housing cost burden will be negatively associated with mobility outcomes and will have stronger predictive power than geographic region alone. Households spending more than 30% of income on housing have structurally limited capacity to save or invest, constraining upward movement regardless of other advantages (Desmond, 2016).
- The interaction between low wealth and high cost burden will surface a distinct high-burden/low-asset household profile through K-Means clustering, with substantially lower mobility rates than other groups.
- Educational attainment will retain predictive value but will be conditional on baseline wealth, reflecting the well-documented finding that education amplifies existing structural advantage rather than independently substituting for it (Putnam, 2015).

---

## Dataset

**Panel Study of Income Dynamics (PSID)**
- Longitudinal household survey tracking income, wealth, and demographics since 1968
- Baseline window: **2011** (aligns with PSID wealth supplement wave)
- Follow-up window: **2021**
- Access: [https://psidonline.isr.umich.edu](https://psidonline.isr.umich.edu) - free with registration

> **Note:** Raw PSID data is not included in this repository. Follow the access instructions above to download the data and place it in `data/raw/`.

---

## References

Chetty, R., Grusky, D., Hell, M., Hendren, N., Manduca, R., & Narang, J. (2017). The fading American dream: Trends in absolute income mobility since 1940. *Science, 356*(6336), 398-406.

Chetty, R., Hendren, N., Kline, P., & Saez, E. (2014). Where is the land of opportunity? The geography of intergenerational mobility in the United States. *Quarterly Journal of Economics, 129*(4), 1553-1623.

Desmond, M. (2016). *Evicted: Poverty and profit in the American city.* Crown Publishers.

Putnam, R. D. (2015). *Our kids: The American dream in crisis.* Simon & Schuster.

University of Michigan Institute for Social Research. (2023). *Panel Study of Income Dynamics* [public use dataset]. Survey Research Center. https://psidonline.isr.umich.edu

---

*This is a living document and will be updated throughout the project as analysis progresses.*

*Last updated: March 2026*
