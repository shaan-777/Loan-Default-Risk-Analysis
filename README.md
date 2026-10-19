# Loan Default Risk Analysis

## Project Overview

An end-to-end data analytics project focused on understanding loan default risk, analyzing borrower characteristics, and identifying patterns associated with repayment difficulties using the **Home Credit Default Risk** dataset from Kaggle.

The project aims to transform raw lending data into actionable insights that support credit risk assessment, portfolio monitoring, and data-driven lending decisions.

## Objectives

- Analyze borrower demographics, income, employment, and credit characteristics.
- Identify patterns associated with loan defaults.
- Examine historical credit bureau records and previous loan applications.
- Analyze installment repayment behavior and credit utilization.
- Segment applicants into meaningful risk categories.
- Develop business KPIs and insights to support lending decisions.

## Dataset

**Source:** [Home Credit Default Risk — Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk/data)

The dataset contains multiple related tables covering current applications, previous applications, external credit history, installment payments, and credit card activity.

Key files include:

- `application_train.csv` — training applications and default labels
- `application_test.csv` — applications without training labels
- `bureau.csv` — previous credits reported by other financial institutions
- `bureau_balance.csv` — monthly bureau credit history
- `previous_application.csv` — historical loan applications
- `installments_payments.csv` — installment payment records
- `POS_CASH_balance.csv` — point-of-sale and cash loan history
- `credit_card_balance.csv` — credit card balances and payment history

## Tech Stack

- **Python** — data processing and exploratory analysis
- **Pandas & NumPy** — data cleaning and feature engineering
- **SQL** — analytical queries and KPI calculations
- **Tableau** — planned data visualization and dashboard development

## Analytical Approach

1. **Data Quality Assessment:** Inspect missing values, duplicate records, data types, and class imbalance.
2. **Feature Engineering:** Aggregate related credit and payment tables at applicant level.
3. **Exploratory Data Analysis:** Investigate default patterns across borrower and loan characteristics.
4. **Risk Segmentation:** Compare borrower groups using repayment behavior and credit indicators.
5. **Business Analysis:** Develop measures for default rates, exposure, repayment performance, and portfolio risk.
6. **Visualization:** Present key findings through decision-oriented charts and dashboards.

## Business Questions

- Which borrower characteristics are associated with higher default rates?
- How do income and credit amounts vary across risk segments?
- Does previous repayment behavior provide useful risk signals?
- What patterns appear in historical credit and installment records?
- Which applicant segments may require closer credit review?
- How can portfolio-level risk indicators support lending decisions?

## Project Status

Data exploration, data quality checks, and applicant-level feature engineering have been initiated. Further analysis and dashboard development are part of the project workflow.

## Disclaimer

This project is for educational and analytical purposes. Findings should not be interpreted as a validated credit-scoring model or used as the sole basis for real lending decisions.

## Author


**Krith Thakker**

[GitHub Repository](https://github.com/shaan-777/Loan-Default-Risk-Analysis)
