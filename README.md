
# Loan Recipient Pattern Analysis (R)

**Goal:** Explore patterns in small-business **loan approvals** and build a simple logistic regression model to understand which factors are associated with approval outcomes.

> **Target variable:** `Funded` (1 = approved/funded, 0 = not funded)

## Repo Structure

```
.
├── README.md
├── loan_recipient_patterns.Rmd      # R Notebook: EDA + logistic regression + plots
├── data/
│   ├── loan_recipient_patterns.csv  # (Add your anonymized CSV here)
│   └── dissstrategycase1227.342a.xlsx (optional; if present, Rmd writes CSV)
└── outputs/
    ├── model_logit_odds_ratios.csv  # saved model coefficients (odds ratios)
    └── predictions.csv              # predicted probabilities & classes
```

## Getting Started

1. **Install R packages** (one-time):

```r
install.packages(c("tidyverse", "janitor", "skimr", "broom", "readxl"))
```

2. **Add data**  
   - Preferred: place `loan_recipient_patterns.csv` in `data/`.  
   - Optional: if you have the Excel, place `dissstrategycase1227.342a.xlsx` in `data/` and the notebook will export a CSV for reproducibility.

3. **Run the analysis**  
   - Open `loan_recipient_patterns.Rmd` in RStudio (or R).  
   - Knit to HTML or run chunks interactively.

## Notes on Variables (typical fields in the dataset)

- `Funded` – approval outcome (1 yes / 0 no)  
- `RequestLoanAmt`, `ReqAmt`, `LoanAmount` – requested amount fields (the script creates `req_amt_any`)  
- `AnnualGross` – annual revenue (script creates `log_annual_gross`)  
- `AgeofBusiness` – firm age in years (script creates `age_of_business`)  
- `Race`, `Gender` – simplified into `race_simplified`, `gender_simplified`  
- `City`, `metro`, `referrals` – geography/support context

## Methods

- **EDA**: outcome distribution, grouped rates by demographics, summary tables  
- **Model**: baseline logistic regression:
  ```
  Funded ~ log(RequestAmt) + log(AnnualGross) + AgeOfBusiness + Race + Gender + Metro
  ```
- **Outputs**: model coefficients (odds ratios), basic predictions for inspection

## Ethics & Privacy

- Data are **anonymized** and used for educational/research purposes.  
- Avoid uploading any personally identifiable information (PII) or sensitive attributes.  
- Consider fairness and bias: approval models can reflect historical inequities. Use results responsibly.

## License

You may choose a license suited to your goals. For open sharing, consider **MIT** or **CC BY 4.0**. Update this README accordingly.

## Attribution

Adapted from Desha Elliott’s dissertation research on access to capital and accelerator outcomes; SICSS training & TA experience informed the workflow.

## Results Tables

<details>
<summary>📊 Table 5. Case Summary</summary>

| Unweighted Cases       | N    | Percent |
|------------------------|------|---------|
| Included in Analysis   | 580  | 26.7%   |
| Missing Cases          | 1593 | 73.3%   |
| Total                  | 2173 | 100.0%  |
| Unselected Cases       | 0    | 0.0%    |
| Total                  | 2173 | 100.0%  |

**Interpretation:** Out of 2,173 applications, about 27% had complete data and were used for analysis. Missing data represented the majority (73%).  
</details>

---

<details>
<summary>📊 Table 6. Frequency Table</summary>

| Variable               | N    | Mean/Mode | S.D.    | Minimum | Maximum   |
|------------------------|------|-----------|---------|---------|-----------|
| Age of Business        | 2109 | 3.65      | 0.926   | 0.01    | 45.4      |
| Location: Inside Metro | 744  | --        | --      | --      | --        |
| Location: Outside Metro| 1429 | --        | --      | --      | --        |
| Requested Amount       | 2173 | 31230.57  | 45767.43| 2082.00 | 500,000.00|
| Referral: Private      | 433  | --        | --      | --      | --        |
| Referral: Public       | 111  | --        | --      | --      | --        |
| Referral: Nonprofit    | 56   | --        | --      | --      | --        |
| Referral: Unknown      | 1573 | --        | --      | --      | --        |
| Funded Loans           | 95   | 15216.71  | 13906.06| 2082.00 | 90,000.00 |

**Interpretation:** Businesses averaged ~3.6 years old. Most were outside metro areas. Loan requests averaged $31K but ranged widely. Referrals were mostly “unknown.”  
</details>

---

<details>
<summary>📊 Table 7. Model Summary</summary>

| Model | R     | R Square | Adjusted R Square | Std. Error of the Estimate |
|-------|-------|----------|-------------------|----------------------------|
| 1     | 0.211 | 0.044    | 0.043             | 0.19348                    |

**Interpretation:** The model explains about 4% of the variance in loan funding outcomes — statistically significant but modest power.  
</details>

---

<details>
<summary>📊 Table 8. ANOVA of Continuous Variables</summary>

| Model      | Sum of Squares | df   | Mean Square | F      | Sig.  |
|------------|----------------|------|-------------|--------|-------|
| Regression | 3.659          | 2    | 1.830       | 48.880 | <.001 |
| Residual   | 78.834         | 2106 | 0.037       | --     | --    |
| Total      | 82.493         | 2108 | --          | --     | --    |

**Interpretation:** Age of business and requested amount together significantly predict funding (p < .001).  
</details>

---

<details>
<summary>📊 Table 9. Coefficients of Continuous Variables</summary>

| Variable        | B         | Std. Error | Beta   | t      | Sig.  |
|-----------------|-----------|------------|--------|--------|-------|
| Constant        | 0.018     | 0.006      | --     | 2.917  | --    |
| ReqAmt          | -3.533E-7 | 0.000      | -0.082 | -3.827 | <.001 |
| Age of Business | 0.009     | 0.001      | 0.199  | 9.338  | <.001 |

**Interpretation:** Older businesses were more likely to get funded. Higher requested amounts slightly decreased approval chances.  
</details>

---

<details>
<summary>📊 Table 10. Variables in the Equation</summary>

| Variable        | B     | S.E.  | Wald   | df | Sig.  | Exp(B)   |
|-----------------|-------|-------|--------|----|-------|----------|
| Age of Business | 0.136 | 0.027 | 26.078 | 1  | <.001 | 1.146    |
| metro (1)       | 0.534 | 0.319 | 2.814  | 1  | 0.093 | 1.707    |
| ReqAmt          | 0.000 | 0.000 | 28.382 | 1  | <.001 | 1.000    |
| referrals (1)   | 5.161 | 1.063 | 23.555 | 1  | <.001 | 174.425  |
| referrals (2)   | 6.967 | 1.073 | 42.182 | 1  | <.001 | 1060.692 |
| referrals (3)   | 7.562 | 1.087 | 48.362 | 1  | <.001 | 1923.054 |
| Constant        | -7.692| 1.112 | 47.816 | 1  | <.001 | 0.000    |

**Interpretation:** Referral source was the strongest predictor. Private, public, and nonprofit referrals hugely increased approval odds. Metro location trended positive but wasn’t statistically significant.  
</details>

---

<details>
<summary>📊 Table 11. Hosmer and Lemeshow Test</summary>

| Chi-Square | df | Sig  |
|------------|----|------|
| 11.984     | 8  | 0.152|

**Interpretation:** The p-value (> .05) indicates the model fits the data reasonably well.  
</details>

---

<details>
<summary>📊 Table 12. Model Fit Summary</summary>

| -2 Log likelihood | Cox & Snell R Square | Nagelkerke R Square |
|-------------------|-----------------------|---------------------|
| 346.826           | 0.162                 | 0.560               |

**Interpretation:** The Nagelkerke R² of 0.560 suggests the model explains ~56% of variance in funding likelihood — a strong fit.  
</details>

## 🔑 Key Insights

- **Business age matters:** More years in operation increased funding odds significantly.  
- **Requested loan amount:** Higher requests slightly decreased approval likelihood.  
- **Referral source dominates:** Private, public, and nonprofit referrals drastically boosted approval chances.  
- **Metro location:** Showed a positive trend but was not statistically significant at conventional levels.  
- **Model fit:** Logistic regression explained over half of the variance (Nagelkerke R² = 0.56), confirming a meaningful though not perfect predictive model.  
