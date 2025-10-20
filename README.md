# Real Estate Market Modeling (R) — UCLA Econ 104 (Data Science for Economists)

*Econometrics project applying regression modeling and diagnostic testing to U.S. housing data.*



**Author:** Cole Kazu Yanagisawa  
**Goal:** Explain variation in residential listing prices using property features and build an interpretable, statistically sound model.  
**Stack:** R, tidyverse, GGally, janitor, broom, car, lmtest, sandwich, MASS

---

## Overview

This project analyzes how structural attributes of homes (bedrooms, bathrooms, interior square footage, lot size) relate to **log(price)** across U.S. listings.  
We start with descriptive analysis, fit a baseline linear model, diagnose misspecification (RESET), add nonlinear terms and an interaction, and correct inference with robust standard errors.

**Final model (chosen):**

$$
\log(\text{price}) = \beta_0 + \beta_1\text{bed} + \beta_2\text{bath} + \beta_3\text{house\_size} + \beta_4\text{house\_size}^2 + \beta_5\text{acre\_lot} + \beta_6\text{acre\_lot}^2 + \beta_7(\text{bath} \times \text{house\_size}) + \epsilon
$$

**Why:** Captures diminishing returns for size/lot and the way bathrooms’ value scales with house size.

---

## Repo Structure

```
├── real-estate-regression-modeling-final.Rmd   # main analysis (knit)
├── real_estate_sample.csv                      # sample data required to run code
├── output/
│   └── real_estate_project_report.pdf          # final report (knit from Rmd)
└── README.md
```

- **Rmd in root** so the relative path `read_csv("real_estate_sample.csv")` works.  
- **Final PDF** lives in `/output` for easy viewing.

---

## Key Findings (Short)

- **Strongest drivers:** bathrooms and interior square footage.  
- **Diminishing returns:** larger homes/lots add value at a decreasing rate (negative squared terms).  
- **Bedrooms:** small negative coefficient once controlling for size/baths (overlap/collinearity).  
- **Lot size:** positive but modest effect relative to interior space.

**Performance (high level):**
- Adj. R² ≈ **0.361** (final model)  
- AIC/BIC improved materially vs. baseline (AIC ~ **45,443**, BIC ~ **45,514**)  
- Test-set errors (log scale): **MAE ≈ 0.499**, **RMSE ≈ 0.765**  
- Dollar-scale error (with Duan smearing): heavy-tail outliers inflate mean; **median AE ≈ \$175k** gives a realistic sense for typical listings.

---

## Methods & Diagnostics

- **Descriptives:** summaries, histograms (log scale for skewed vars), boxplots, correlations.  
- **Modeling:** baseline OLS on log(price); stepwise AIC check (no changes retained).  
- **Misspecification:** Ramsey **RESET** → add \( \text{house\_size}^2, \text{acre\_lot}^2 \) and **bath × house_size**.  
- **Heteroskedasticity:** Breusch–Pagan positive; **White (HC1)** robust SE used.  
- **Train/Test:** 80/20 split, `set.seed(104)` for reproducibility.  
- **Back-transform:** Duan smearing to express errors in dollars.

---

## Reproduce Locally

1. **R packages (from the Rmd header):**
   ```r
   install.packages(c(
     "tidyverse","GGally","glue","janitor","broom","scales",
     "readr","car","lmtest","sandwich","MASS"
   ))
   ```

2. Place files exactly as shown in the structure above (`.Rmd` and `.csv` in the root directory).

3.	Open real-estate-regression-modeling-final.Rmd in RStudio → Knit to PDF.

4.	**Output:** output/real_estate_project_report.pdf.


*Data note: If your full dataset is large/restricted, this repo includes a non-sensitive sample (real_estate_sample.csv) for reproducibility.*

---

## Deliverables

- Final Report (PDF): output/real_estate_modeling_project_report.pdf
- Source (R Markdown): real-estate-regression-modeling-final.Rmd

---

🏷 Citation (Data)

> Sakib, A. S. (2024). *USA Real Estate Dataset* [Data set]. Kaggle.  
> <https://doi.org/10.34740/KAGGLE/DS/3202774>

---

## Notes

- First portfolio repo spun out from an academic assignment; future projects will include expanded Git workflow (branching, smaller commits, CI).
- **Random seed:** set.seed(104) for repeatable splits/plots where applicable.
