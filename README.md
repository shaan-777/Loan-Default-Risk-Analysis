# Loan Default Risk Analysis & Decision Dashboard

**A data-driven credit risk analytics project using the Home Credit Default Risk dataset to investigate borrower repayment difficulties, identify risk drivers, evaluate lending policies, and support portfolio-level decisions.**

## 📌 Project Overview

Lending institutions must balance loan growth with the risk of borrowers experiencing repayment difficulties. Approving every application without evaluating risk can increase credit losses, while overly restrictive lending policies can reject creditworthy applicants.

This project analyzes borrower application data and historical credit behavior to identify risk patterns, segment applicants, evaluate approval policies, and develop business recommendations.

The project combines **Python-based data analysis, multi-table feature engineering, risk segmentation, policy analysis, and dashboard-oriented business intelligence**.

## 🎯 Business Objectives

- Identify borrower and credit characteristics associated with repayment difficulties.
- Analyze application, credit bureau, previous loan, and repayment history.
- Develop applicant-level analytical features from multiple related tables.
- Segment applicants by risk and compare observed payment-difficulty rates.
- Evaluate an illustrative credit decision policy using risk thresholds.
- Investigate data quality, missing information, duplicate records, and join coverage.
- Develop a ₹10 crore investment allocation analysis based on modeled business priorities.
- Translate analytical findings into actionable recommendations for credit and portfolio management.

## 📊 Dataset

**Source:** [Home Credit Default Risk — Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk/data)

The dataset contains anonymized loan application information and linked historical credit records. The main training dataset contains 307,511 applications and 122 columns.

### Dataset tables

| Dataset | Purpose |
|---|---|
| `application_train.csv` | Applicant characteristics and the target outcome |
| `application_test.csv` | Applications without the training target |
| `bureau.csv` | Credit history from other financial institutions |
| `bureau_balance.csv` | Monthly history of bureau credit accounts |
| `previous_application.csv` | Historical loan applications |
| `installments_payments.csv` | Historical installment payment behavior |
| `POS_CASH_balance.csv` | Historical point-of-sale and cash loan balances |
| `credit_card_balance.csv` | Historical credit card balances and activity |

**Target variable:** `TARGET`

- `0` — No recorded payment difficulties under the dataset's target definition.
- `1` — Payment difficulties recorded under the dataset's target definition.

The target should not automatically be interpreted as a legally confirmed default or a measure of lifetime loan losses.

### Baseline portfolio profile

| Metric | Value |
|---|---:|
| Total training applications | 307,511 |
| Applications with payment difficulties | 24,825 |
| Applications without recorded payment difficulties | 282,686 |
| Baseline payment-difficulty rate | 8.07% |

These figures describe the training dataset, not the performance of a deployed lending institution.

## 🛠️ Technology Stack

| Technology | Role |
|---|---|
| Python | Data analysis and processing |
| Pandas | Data cleaning, joins, aggregation, and feature engineering |
| NumPy | Numerical calculations and data transformations |
| Jupyter Notebook | Exploratory analysis and reproducible investigation |
| SQL | Analytical querying and business metric calculations |
| DuckDB | Local analytical querying of structured data |
| PyArrow | Efficient columnar data handling |
| Statsmodels | Statistical analysis where applicable |
| Scikit-learn | Analytical and machine-learning utilities where applicable |
| Tableau | Planned or implemented visual reporting, depending on the final workbook |

Only tools used in the final workflow should be marked as fully implemented technologies. The presence of a package in the environment does not, by itself, establish that a model was trained or evaluated.

## 🔄 End-to-End Analysis Workflow

The analysis follows a structured workflow from raw data to business recommendations.

### 1. Data ingestion and understanding

The raw Kaggle files were organized into a project directory with separate locations for raw data, processed data, notebooks, SQL, dashboards, and reports.

Initial exploration covered:

- File dimensions and column names.
- Data types and target distribution.
- Identifier columns and relationships between tables.
- Missing values and potential data-quality issues.
- The difference between applicant-level and historical transaction-level data.

**Why this matters:** A correct understanding of table granularity is essential before combining multiple credit histories. Otherwise, joins can duplicate applicants and distort portfolio KPIs.

### 2. Data quality audit

The quality review focuses on the following risks:

| Risk | Potential impact | Analytical response |
|---|---|---|
| Missing values | Incomplete borrower profiles and biased comparisons | Measure missingness and investigate its distribution |
| Duplicate records | Inflated application or transaction counts | Check duplicates at the appropriate key level |
| Incorrect join cardinality | Applicants counted multiple times | Validate one-to-one and one-to-many relationships |
| Missing applicant matches | Historical records excluded from analysis | Measure join coverage before interpreting results |
| Invalid or infinite values | Unreliable ratios and aggregates | Validate derived features and numerical outputs |
| Target imbalance | Misleading performance interpretations | Compare metrics appropriate for imbalanced outcomes |
| Inconsistent table granularity | Incorrect applicant-level metrics | Aggregate historical tables before joining to applications |

**Important distinction:** A quality check identifies a potential problem; it does not automatically prove that the underlying records are erroneous. Corrections should be documented and validated.

### 3. Multi-table feature engineering

Historical records are summarized into applicant-level analytical features before being combined with the main application data.

Processed feature tables include:

- `installment_features.csv`
- `bureau_features.csv`
- `previous_application_features.csv`
- `pos_cash_features.csv`
- `credit_card_features.csv`
- `bureau_balance_features.csv`
- `bureau_history_features.csv`

These tables provide a foundation for analyzing repayment behavior, historical credit exposure, previous application outcomes, and credit account activity.

**Why aggregation matters:** One applicant can have multiple previous loans, installments, or monthly credit records. Joining all raw tables directly can multiply rows and inflate totals. Applicant-level aggregation helps preserve the intended unit of analysis.

### 4. Exploratory data analysis

The analytical investigation examines:

- Payment-difficulty rates across borrower characteristics.
- Income and credit amount distributions.
- Employment and demographic patterns.
- Historical credit activity and repayment behavior.
- Differences between applicant segments.
- Relationships between missing data and borrower characteristics.
- Potential outliers and inconsistent records.

The purpose is to identify meaningful associations, not to assume that a correlation establishes causation.

### 5. Risk identification and prioritization

The project evaluates several categories of credit risk.

**Borrower affordability risk**

Investigate whether income, loan amount, and repayment obligations are associated with higher payment-difficulty rates.

**Historical repayment risk**

Examine available installment records and prior credit behavior for evidence of repayment difficulties.

**Credit exposure risk**

Review historical credit accounts and application activity to understand the borrower's broader credit profile.

**Data and measurement risk**

Assess missing values, duplicates, join coverage, and the possibility of misleading metrics.

**Decision-policy risk**

Evaluate whether a lending policy creates an appropriate balance between accepting applicants and managing repayment risk.

**Model and interpretation risk**

Avoid treating an observed association or a manually defined risk score as a validated predictive model. Any predictive model would require separate validation, threshold selection, and appropriate performance evaluation.

### 6. Risk segmentation and policy analysis

The project uses a three-tier decision framework to examine how different risk segments could be handled.

| Risk score | Illustrative decision | Intended treatment |
|---|---|---|
| ≤ 25 | Auto Approve | Lower-score segment for streamlined review |
| > 25 to 40 | Manual Review | Intermediate segment requiring additional assessment |
| > 40 | Decline | Higher-score segment for stricter credit review |

The score thresholds are **project policy assumptions**, not universal banking standards. They should only be used with a clearly documented score definition and validation process.

#### Observed segment rates

| Segment | Observed payment-difficulty rate |
|---|---:|
| Auto Approve | 5.85% |
| Manual Review | 11.52% |
| Decline | 16.67% |
| Overall baseline | 8.07% |

These project figures illustrate the risk-segmentation analysis. The rates are meaningful only when the score, segment assignment, and denominator are defined consistently. They should not be interpreted as the proven performance of a production credit policy.

**Business interpretation:**

- A lower observed rate in the auto-approve segment can support evaluating a faster approval process.
- An intermediate segment may benefit from additional income, affordability, or credit-history checks.
- A higher observed rate in the decline segment can motivate stricter review, subject to validation and fairness requirements.

A real implementation should also quantify approval volume, false approvals, false declines, expected losses, and the effects of the policy on eligible applicants.

## 🖥️ Dashboard Suite

The dashboard suite is organized around four business questions. Add your own locally saved dashboard screenshots using the filenames below.

### Dashboard 1 — Executive Credit Risk Overview

**Image placeholder**

Save your screenshot as `images/01-executive-credit-risk-overview.png`.
<img width="1670" height="942" alt="Loan Portfolio Risk Analysis Dashboard" src="https://github.com/user-attachments/assets/4db5f033-b1cc-4bd5-978f-466c89105e92" />


![Executive Credit Risk Overview — add screenshot](images/01-executive-credit-risk-overview.png)

**What it signifies**

Provides a high-level view of the applicant portfolio, the baseline payment-difficulty rate, and the distribution of applicants across decision categories.

**Recommended KPIs**

- Total applications.
- Overall payment-difficulty rate.
- Auto-approve, manual-review, and decline counts.
- Share of applicants in each risk segment.
- Segment-level payment-difficulty rates.

**Business use:** Helps decision-makers understand the overall risk profile before investigating individual drivers.

### Dashboard 2 — Product preformance

**Image placeholder**

Save your screenshot as `images/02-risk-driver-analysis.png`.

![Risk Driver Analysis — add screenshot](images/02-risk-driver-analysis.png)
<img width="1555" height="1012" alt="Tableau Loan Portfolio Dashboard" src="https://github.com/user-attachments/assets/741b7ae3-55e5-4e5b-af7d-87f17f5aa8c6" />


**What it signifies**

Investigates which borrower characteristics and historical credit indicators are associated with different payment-difficulty rates.

**Recommended visualizations**

- Payment-difficulty rate by income or credit bands.
- Applicant characteristics by target outcome.
- Credit history and repayment behavior comparisons.
- Missing-data and data-quality indicators.
- Segment-level comparisons of key risk variables.

**Business use:** Helps analysts identify where additional investigation or better underwriting information may be useful. These relationships are observational and do not prove causation.

### Dashboard 3 — Application Profile

**Image placeholder**

Save your screenshot as `images/03-credit-policy-simulator.png`.
<img width="1554" height="1012" alt="Applicant Profile Loan Analytics Dashboard" src="https://github.com/user-attachments/assets/164bc971-030b-44ac-aff6-6335c452bc33" />


![Credit Policy Simulator — add screenshot](images/03-credit-policy-simulator.png)

**What it signifies**

Explores how changing risk-score thresholds can change the number of applicants assigned to each decision category.

**Recommended visualizations**

- Adjustable auto-approve and decline thresholds.
- Applicant counts by decision category.
- Observed payment-difficulty rates by segment.
- Approval-versus-risk trade-off.
- Comparison of alternative policy scenarios.

**Business use:** Supports discussion of policy trade-offs rather than relying on one fixed threshold.

A threshold simulator is a decision-support tool, not a guarantee of future default performance. Actual implementation requires validated scores and a defined evaluation methodology.

### Dashboard 4 — RISK ANALYSIS

**Image placeholder**

Save your screenshot as `images/04-investment-allocation-strategy.png`.
<img width="1672" height="940" alt="Loan Portfolio Risk Analysis Dashboard (1)" src="https://github.com/user-attachments/assets/bfa43ee1-3417-43ab-a05d-bb666766e18d" />


![Investment Allocation Strategy — add screenshot](images/04-investment-allocation-strategy.png)

**What it signifies**

Presents the project's modeled ₹10 crore allocation analysis and connects proposed resource allocation with business priorities.

**Recommended visualizations**

- Allocation by recommended investment category.
- Budget share and allocation amount.
- Expected business impact, where defensible estimates exist.
- Priority ranking and supporting rationale.
- Scenario comparisons and sensitivity analysis.

**Business use:** Helps stakeholders compare proposed allocation choices and the assumptions behind them.

The ₹10 crore figure represents an analytical planning scenario, not evidence of an actual investment or guaranteed financial return. Any expected-impact metric should be accompanied by its calculation and assumptions.

## 📈 KPI Definitions

| KPI | Definition | Interpretation |
|---|---|---|
| Total Applications | Count of distinct application identifiers | Portfolio size |
| Payment-Difficulty Rate | Applications with `TARGET = 1` divided by applications with a known target | Historical outcome frequency |
| Segment Share | Applications in a segment divided by all eligible applications | Distribution of applicants across decisions |
| Segment Risk Rate | Applications with `TARGET = 1` in a segment divided by applications with known targets in that segment | Historical risk observed in that segment |
| Missing-Value Rate | Missing observations divided by eligible observations | Data completeness |
| Join Coverage | Matched eligible applicant records divided by the relevant applicant population | Completeness of linked historical data |
| Budget Allocation | Amount assigned to a category under the modeled scenario | Distribution of the planning budget |

For every KPI, the dashboard should make its denominator, filters, and date or population scope clear. Historical rates and modeled estimates must be labeled separately.

## 💡 Business Recommendations

1. **Strengthen affordability analysis:** Compare repayment difficulty across income and credit-obligation segments before proposing underwriting changes.
2. **Use historical credit information carefully:** Incorporate available repayment behavior only after validating coverage, aggregation logic, and data quality.
3. **Apply differentiated review:** Evaluate whether the proposed risk tiers improve decision efficiency without creating unacceptable credit losses or unfair outcomes.
4. **Monitor data quality:** Track missingness, duplicate records, and join coverage as part of ongoing analytical controls.
5. **Validate before deployment:** Evaluate any predictive score using held-out data, appropriate metrics, calibration checks, and threshold analysis.
6. **Document allocation assumptions:** Link every modeled budget recommendation to a stated objective, calculation, and sensitivity assessment.
7. **Review fairness and governance:** Investigate whether decisions create unjustified differences across applicant groups and ensure compliance with applicable lending requirements.

## 📁 Project Structure

```text
Loan-Default-Risk-Analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│       ├── installment_features.csv
│       ├── bureau_features.csv
│       ├── previous_application_features.csv
│       ├── pos_cash_features.csv
│       ├── credit_card_features.csv
│       └── bureau_history_features.csv
│
├── notebooks/
├── sql/
├── tableau/
├── reports/
├── README.md
└── .gitignore
```

This structure describes the intended organization; keep only files and folders that exist in the final repository. Large raw datasets should generally be downloaded from Kaggle rather than committed to GitHub.

## ⚠️ Limitations

- The dataset is historical and does not establish that the same patterns will hold for another lender or future applicants.
- The target represents the dataset's defined payment-difficulty outcome, not every possible form of default or financial loss.
- Missing values and unmatched historical records may affect the analysis.
- Risk thresholds are analytical policy assumptions and require validation before operational use.
- The ₹10 crore allocation is a modeled planning exercise, not a realized investment.
- Dashboard screenshots should reflect the actual workbook and calculated data. A design mockup should not be presented as a functioning interactive dashboard.

## 🚀 Future Enhancements

- Complete and validate the dashboard workbook.
- Add interactive filters for risk segment and borrower characteristics.
- Add threshold sensitivity and scenario analysis.
- Evaluate predictive models using appropriate validation metrics, if modeling is undertaken.
- Add explainability, calibration, and fairness analysis for any trained model.
- Document reproducible steps for regenerating all analytical outputs.

## 👤 Author

**Krith Thakker**

[GitHub Profile](https://github.com/shaan-777)

[Project Repository](https://github.com/shaan-777/Loan-Default-Risk-Analysis)

## 📚 Data Source

Home Credit Default Risk — Kaggle

https://www.kaggle.com/competitions/home-credit-default-risk/data
