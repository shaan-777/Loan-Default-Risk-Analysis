# Loan Default Risk Analysis & Decision Dashboard

**An end-to-end credit risk analytics project using the Home Credit Default Risk dataset to investigate borrower payment difficulties, identify risk drivers, analyze applicant profiles, and evaluate credit decision policies through business-focused dashboards.**

## 📌 Project Overview

Financial institutions must balance lending growth with credit risk. Approving applicants without sufficient assessment can increase repayment difficulties, while excessively restrictive lending policies may exclude creditworthy applicants.

This project analyzes borrower applications and historical credit information to understand repayment patterns, identify risk factors, and translate findings into actionable business insights.

The project combines data quality assessment, multi-table feature engineering, exploratory data analysis, risk segmentation, policy scenario analysis, and Tableau-style business intelligence.

### Business Objectives

- Identify applicant characteristics associated with payment difficulties.
- Analyze income, credit amounts, employment, age, and housing characteristics.
- Integrate information from multiple historical credit and repayment tables.
- Evaluate risk segmentation and applicant decision categories.
- Examine the trade-off between approval volume and observed payment-difficulty rates.
- Identify data quality and measurement risks that could distort business decisions.
- Communicate findings through executive-level dashboards and supporting visualizations.

## 📊 Dataset

**Source:** [Home Credit Default Risk — Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk/data)

The dataset contains anonymized loan application information and associated historical credit records.

The main training dataset contains **307,511 applications and 122 columns**.

### Dataset tables

| Dataset | Purpose |
|---|---|
| `application_train.csv` | Applicant characteristics and target outcome |
| `application_test.csv` | Applications without the training target |
| `bureau.csv` | Credit history reported by other financial institutions |
| `bureau_balance.csv` | Monthly history of bureau credit accounts |
| `previous_application.csv` | Historical loan applications |
| `installments_payments.csv` | Historical installment repayment records |
| `POS_CASH_balance.csv` | Point-of-sale and cash loan history |
| `credit_card_balance.csv` | Credit card balances and account activity |

### Target Variable

The primary outcome is `TARGET`.

- `0` — No payment difficulties recorded under the dataset's target definition.
- `1` — Payment difficulties recorded under the dataset's target definition.

The target is a historical outcome indicator. It should not automatically be interpreted as a confirmed loan default, lifetime credit loss, or probability of future default.

### Baseline Portfolio Statistics

| Metric | Value |
|---|---:|
| Total training applications | 307,511 |
| Applications with `TARGET = 1` | 24,825 |
| Applications with `TARGET = 0` | 282,686 |
| Overall payment-difficulty rate | 8.07% |

These figures describe the training dataset and provide a baseline for subsequent comparisons.

## 🛠️ Technology Stack

| Technology | Application |
|---|---|
| Python | Data processing and analysis |
| Pandas | Data cleaning, joins, aggregation, and feature engineering |
| NumPy | Numerical calculations and transformations |
| Jupyter Notebook | Exploratory analysis and reproducible investigation |
| SQL | Analytical queries and business metric calculations |
| DuckDB | Local analytical querying |
| PyArrow | Columnar data processing |
| Statsmodels | Statistical analysis where applicable |
| Scikit-learn | Analytical and machine-learning utilities where applicable |
| Tableau | Dashboard design and business intelligence visualization |

The final implementation should distinguish between technologies used in completed analysis and tools reserved for future enhancements.

## 🔄 End-to-End Analysis Process

### 1. Data Ingestion and Understanding

The raw Kaggle files were organized into a project structure separating raw data, processed data, notebooks, SQL scripts, dashboard files, and reports.

The initial investigation covered:

- Dataset dimensions and column names.
- Data types and identifier fields.
- Target distribution and class imbalance.
- Relationships between the application table and historical tables.
- Differences between applicant-level records and transaction-level records.

**Why this matters:** Understanding the granularity of each table prevents incorrect joins and inflated applicant counts.

### 2. Data Quality Assessment

Data quality is important because inaccurate or incomplete information can produce misleading credit risk conclusions.

| Risk identified | Potential business impact | Validation approach |
|---|---|---|
| Missing values | Incomplete borrower profiles and biased comparisons | Measure missingness and assess its distribution |
| Duplicate records | Inflated counts and incorrect totals | Check duplicates using appropriate keys |
| Incorrect join cardinality | Applicants or transactions counted multiple times | Validate join relationships and resulting row counts |
| Incomplete join coverage | Historical information excluded from analysis | Calculate matched and unmatched applicant records |
| Invalid numerical values | Incorrect ratios and aggregates | Check invalid values, infinities, and calculation outputs |
| Target imbalance | Misleading interpretation of predictive performance | Use metrics appropriate for imbalanced outcomes |
| Inconsistent table granularity | Incorrect applicant-level KPIs | Aggregate historical records before combining them |

The purpose of the audit is to identify and quantify potential data problems, document their effects, and validate any corrections.

### 3. Multi-Table Feature Engineering

Historical credit and repayment records can contain multiple observations for a single applicant. The project therefore includes applicant-level feature aggregation.

Processed feature tables created during the workflow include:

- `installment_features.csv`
- `bureau_features.csv`
- `previous_application_features.csv`
- `pos_cash_features.csv`
- `credit_card_features.csv`
- `bureau_balance_features.csv`
- `bureau_history_features.csv`

These tables support analysis of historical repayment behavior, previous applications, credit exposure, and account activity.

**Key analytical control:** Historical tables should be aggregated at the appropriate key before being joined to the application table. Otherwise, one-to-many joins can duplicate applicants and distort metrics.

### 4. Exploratory Data Analysis

The analysis investigates the relationship between borrower characteristics and the recorded target outcome.

Key dimensions include:

- Income groups.
- Age groups.
- Employment characteristics.
- Housing type.
- Credit amount bands.
- Historical credit and repayment information.
- Missingness and data completeness.

The analysis focuses on identifying associations and differences between groups. Observational patterns do not, by themselves, establish causation.

### 5. Risk Identification

The project considers several categories of risk.

**Borrower affordability risk**

Differences in income and credit obligations may be associated with different payment-difficulty rates. These patterns can help identify areas requiring further affordability analysis.

**Historical repayment risk**

Previous installment and credit records can provide additional context about repayment behavior, subject to data quality and historical coverage.

**Credit exposure risk**

Historical credit information can help describe an applicant's broader credit profile and existing obligations.

**Data and measurement risk**

Missing values, duplicates, incorrect joins, and inconsistent denominators can produce unreliable KPIs.

**Credit policy risk**

Decision thresholds can change the distribution of applicants across approval, manual-review, and decline categories. These choices involve trade-offs between risk management and lending access.

**Model and interpretation risk**

Observed segment rates and manually defined scores should not be presented as a validated predictive model. Any production model requires separate training, validation, calibration, and fairness assessment.

### 6. Risk Segmentation and Decision Analysis

The project uses an illustrative three-tier decision framework.

| Risk score | Decision category | Intended treatment |
|---|---|---|
| ≤ 25 | Auto Approve | Lower-score segment for streamlined assessment |
| > 25 to 40 | Manual Review | Intermediate segment requiring additional assessment |
| > 40 | Decline | Higher-score segment for stricter review |

These thresholds are analytical assumptions for scenario analysis, not universal banking standards.

#### Illustrative Segment Analysis

| Segment | Applicant count | Observed payment-difficulty rate |
|---|---:|---:|
| Auto Approve | 124,532 | 5.85% |
| Manual Review | 102,673 | 11.52% |
| Decline | 80,306 | 16.67% |
| Overall training population | 307,511 | 8.07% |

The counts sum to the training population. The segment rates should be interpreted only in the context of the score definition, segment assignment, and target-availability rules used to calculate them.

**Business interpretation**

- The auto-approve segment has a lower observed payment-difficulty rate than the overall baseline.
- The manual-review segment has a higher observed rate, suggesting an opportunity to investigate additional assessment criteria.
- The decline segment has the highest observed rate under the illustrative segmentation.

These observations do not prove that the proposed policy will improve future lending outcomes. Before implementation, the policy must be evaluated against approval volumes, false approvals, false declines, expected losses, and applicant fairness.

## 🖥️ Dashboard Suite

The four dashboards present the analysis from different business perspectives, progressing from overall portfolio health to borrower risk drivers, applicant characteristics, and risk-focused decision support.

### 1. Executive Overview

<img width="1670" height="942" alt="Loan Portfolio Risk Analysis Dashboard" src="https://github.com/user-attachments/assets/4db5f033-b1cc-4bd5-978f-466c89105e92" />

**Purpose:** Provide a high-level summary of the applicant portfolio and recorded payment difficulties.

**Key components**

- Total applications.
- Overall payment-difficulty rate.
- Distribution of target outcomes.
- Applications by illustrative risk segment.
- Monthly application volume and payment-difficulty trend.
- Key portfolio metrics.

**Business interpretation**

This dashboard establishes the baseline against which other segments and analyses can be compared. It helps stakeholders understand portfolio size, outcome distribution, and the overall scale of recorded payment difficulties.

The segment distribution illustrates how applicants are assigned under the defined decision framework. It should not be confused with the actual approval decisions made by Home Credit.

### 2. Risk Driver Analysis

<img width="1555" height="1012" alt="Tableau Loan Portfolio Dashboard" src="https://github.com/user-attachments/assets/741b7ae3-55e5-4e5b-af7d-87f17f5aa8c6" />

**Purpose:** Examine borrower characteristics associated with different payment-difficulty rates.

**Key components**

- Payment-difficulty rate by income band.
- Payment-difficulty rate by age band.
- Employment and housing type heatmap.
- Payment-difficulty rate by credit amount band.
- Filters for applicant characteristics and risk segments.

**Business interpretation**

The income and age comparisons help identify groups with different historical outcome rates. The employment–housing heatmap highlights combinations worth investigating, while the credit amount chart examines how outcome rates vary across credit bands.

These charts are descriptive. Differences between groups may reflect multiple interacting characteristics and should not be interpreted as causal effects.

### 3. Applicant Profile Analysis

<img width="1554" height="1012" alt="Applicant Profile Loan Analytics Dashboard" src="https://github.com/user-attachments/assets/164bc971-030b-44ac-aff6-6335c452bc33" />

**Purpose:** Provide a more detailed view of applicant characteristics and how they relate to the recorded target outcome.

**Key components**

- Applicant profile comparisons.
- Demographic and financial characteristics.
- Risk-related segmentation.
- Comparative outcome analysis.
- Interactive filtering by relevant applicant dimensions.

**Business interpretation**

This dashboard supports deeper investigation of applicant groups and helps analysts move from portfolio-level patterns to more specific characteristics.

When reviewing results, it is important to distinguish between applicant counts, percentages, and target rates. A group with a high payment-difficulty rate may have a small population, so both rate and sample size should be considered.

### 4. Risk Analysis Dashboard

<img width="1672" height="940" alt="Loan Portfolio Risk Analysis Dashboard (1)" src="https://github.com/user-attachments/assets/bfa43ee1-3417-43ab-a05d-bb666766e18d" />

**Purpose:** Bring the risk analysis together to support prioritization, comparison, and business recommendations.

**Key components**

- Risk-focused metrics.
- Comparative analysis of relevant applicant segments.
- Visual identification of higher-risk patterns.
- Supporting charts and analytical summaries.
- Business-oriented interpretation of the findings.

**Business interpretation**

This dashboard helps translate descriptive findings into areas for further investigation. Higher observed payment-difficulty rates can inform where additional verification, monitoring, or affordability analysis may be worth evaluating.

Any policy recommendation should be supported by validated calculations and a clear explanation of its assumptions.

## 📈 KPI Definitions

| KPI | Definition | Business significance |
|---|---|---|
| Total Applications | Count of distinct application identifiers | Measures the size of the analyzed population |
| Payment-Difficulty Rate | Applications with `TARGET = 1` divided by applications with known targets | Measures historical target-outcome frequency |
| Segment Share | Applicants in a segment divided by the eligible applicant population | Shows how the population is distributed across segments |
| Segment Payment-Difficulty Rate | Applications with `TARGET = 1` within a segment divided by applications with known targets in that segment | Compares historical outcome rates across groups |
| Missing-Value Rate | Missing observations divided by eligible observations | Measures data completeness |
| Join Coverage | Matched eligible applicant records divided by the relevant applicant population | Measures completeness of linked data |
| Application Volume | Count of applications for the selected period or group | Supports comparisons across populations and time |

For meaningful comparisons, the dashboard should use consistent denominators and clearly communicate the population and filters behind each KPI.

## 💡 Business Recommendations

1. **Investigate affordability patterns:** Compare payment-difficulty rates across income and credit amount groups before proposing underwriting changes.
2. **Use historical credit data carefully:** Validate data coverage, aggregation logic, and feature quality before using historical information in decisions.
3. **Evaluate differentiated review:** Test whether risk-based review categories improve decision efficiency without creating unacceptable risk or unfair outcomes.
4. **Strengthen data controls:** Monitor missingness, duplicate records, join coverage, and metric consistency.
5. **Validate policy thresholds:** Compare approval volume, observed risk, and potential decision errors before operational deployment.
6. **Separate association from causation:** Use the dashboards to identify patterns for investigation rather than claiming that one characteristic directly causes repayment difficulties.
7. **Apply responsible lending principles:** Assess the fairness, explainability, and governance implications of any proposed risk-based decision process.

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

This represents the intended project organization. Retain only files and folders that exist in the repository. Raw datasets can be downloaded from Kaggle rather than committed to GitHub, particularly where file sizes are large.

## ⚠️ Limitations

- The analysis uses historical data and may not generalize to other lenders or future applicants.
- The target represents the dataset's defined payment-difficulty outcome, not every form of loan default or financial loss.
- Missing values and unmatched historical records may affect the conclusions.
- Risk thresholds are illustrative policy assumptions and require validation before operational use.
- Observed relationships do not establish causality.
- Dashboard metrics should be reproducible from the underlying data and calculations.
- A dashboard is a decision-support tool; it does not replace credit policy governance or model validation.

## 🚀 Future Enhancements

- Validate dashboard calculations against the processed datasets.
- Add additional drill-downs for borrower and historical credit characteristics.
- Implement policy threshold sensitivity analysis with documented assumptions.
- Evaluate predictive models using appropriate validation metrics, if model development is undertaken.
- Add calibration, explainability, and fairness assessments for any trained model.
- Document reproducible steps for regenerating analytical outputs.

## 👤 Author

**Krith Thakker**

[GitHub Profile](https://github.com/shaan-777)

[Project Repository](https://github.com/shaan-777/Loan-Default-Risk-Analysis)

## 📚 Data Source

[Home Credit Default Risk — Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk/data)
