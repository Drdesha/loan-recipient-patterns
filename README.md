
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
