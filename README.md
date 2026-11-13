# Monitoring Digital Payment Reliability in Nigeria

> A data analytics project that profiles synthetic Nigerian retail banking transactions and demonstrates how operational monitoring indicators can help identify unusual changes in digital payment reliability.

---

## Project Overview

This project develops an exploratory analytics workflow using a synthetic Nigerian retail banking transaction dataset. It examines transaction behaviour, audits data quality, defines operational monitoring KPIs, and tests selected detection approaches against controlled scenarios.

> **Important:** The dataset is synthetic. The findings describe the dataset and its simulated scenarios. They do not measure actual Nigerian payment-system performance.

---

## Project Aim

> To demonstrate an exploratory analytics workflow for profiling transaction activity and evaluating monitoring KPIs on a synthetic Nigerian retail banking dataset, then translate the findings into practical operational monitoring recommendations.

---

## Objectives

- Profile transaction volume, value, outcomes, channels, transaction types, locations, merchant categories, time patterns, and account concentration.
- Assess data quality and identify fields that should be excluded from downstream analysis.
- Define operational KPIs for monitoring transaction reliability.
- Create controlled scenarios that simulate changes in selected transaction outcomes.
- Evaluate the implemented detection approach and document its limitations.
- Present monitoring recommendations for further testing with appropriate operational data.

---

## Dataset and Sources

The project uses the [Nigerian Banking Retail Transactions dataset](https://huggingface.co/datasets/electricsheepafrica/nigerian-banking-retail-transactions), published by `electricsheepafrica` on Hugging Face.

| Property | Value |
|---|---|
| Records | 5,000,000 |
| Original columns | 16 |
| Period | January 2023 to December 2024 |
| Currency | Nigerian naira (NGN) |
| Data type | Synthetic transaction data |

The dataset includes transaction timestamps, amounts, transaction types, channels, merchant categories, locations, devices, statuses, and other transaction attributes.

The initial audit uses all 16 columns. Two balance fields are then excluded from downstream analysis, leaving 14 analytical columns.

### Downloading the dataset

> **[Download the Parquet dataset](https://huggingface.co/datasets/electricsheepafrica/nigerian-banking-retail-transactions/resolve/main/nigerian_retail_transactions_full.parquet)**

The full dataset is not included in this repository because of its size. Place the downloaded file in the location expected by the notebook, or update the notebook's data path. See `data/README.md` for setup instructions.

The Central Bank of Nigeria's [ePayment Statistics](https://www.cbn.gov.ng/PaymentsSystem/ePaymentStatistics.html) may be used for separate aggregate context. CBN statistics are not merged with the synthetic transaction-level dataset and do not validate its outcome rates.

---

## Methodology

The project follows an exploratory workflow:

1. **Data acquisition and understanding:** Load the dataset and inspect its structure, fields, and time coverage.
2. **Data quality audit:** Check missing values, duplicate transaction identifiers, amount patterns, status behaviour, and balance-field consistency.
3. **Data preparation:** Create analytical features and exclude fields that fail the reliability checks required for this study.
4. **Baseline profiling:** Examine transaction outcomes, volumes, values, channels, merchant categories, locations, time patterns, and account concentration.
5. **Scenario design:** Apply controlled changes to copies of the baseline data. Preserve the original transaction status and record simulated outcomes separately.
6. **Detection evaluation:** Apply the implemented daily threshold approach to scenario data and assess detection and false-alert behaviour.
7. **Interpretation and recommendations:** Explain the results, state limitations, and propose monitoring indicators for further testing.

The analysis is implemented in Python using Jupyter Notebook. Power BI is planned as a presentation layer for selected findings.

---

## Key Data Quality Findings

- The source dataset contains 5 million records and 16 original columns.
- No duplicate transaction IDs or null values were identified in the audited fields. Blank merchant-category strings were treated separately from null values.
- Transaction amounts are non-negative and all recorded amounts are multiples of ₦100. This should be treated as a synthetic-data characteristic.
- The balance fields do not reliably represent transaction outcomes. Failed transactions also show balance movement, and the balance calculations follow a generator rule that is unsuitable for interpreting settlement or ledger behaviour.
- The two balance fields are excluded from downstream analysis.

> **The balance fields must not be used to infer settlement, customer loss, successful posting, or reversal behaviour.**

---

## Baseline Findings

The dataset contains four transaction statuses:

| Status | Records | Share |
|---|---:|---:|
| Successful | 4,599,963 | 92.00% |
| Failed | 349,765 | 6.995% |
| Pending | 40,383 | 0.808% |
| Reversed | 9,889 | 0.198% |

The analysis retains all four outcomes and reports them separately.

Other baseline observations:

- Mobile, POS, and ATM account for approximately 85% of records in this synthetic dataset.
- USSD and agent transactions account for approximately 2.01% of records in this dataset.
- Transaction volume is highest in the evening, peaking around 18:00 to 20:00.
- Transaction amounts are strongly right-skewed. The median is more representative of a typical transaction than the mean alone.
- Transaction activity is concentrated among a subset of accounts.
- Failure rates vary only slightly across the tested channels, transaction types, locations, amount bands, and time groupings. The observed effect sizes are negligible in the tested comparisons.

> **These findings describe this dataset. They are not estimates of the Nigerian banking population and do not show that any channel, location, or transaction type causes transaction failures.**

---

## Scenario-Based Testing

The project uses controlled scenarios to test whether monitoring methods can identify selected changes in transaction outcomes.

| Scenario | Simulated change | Evaluation focus |
|---|---|---|
| **A** | Increased mobile-channel failure probability during a defined evening event window | Failure and non-success monitoring |
| **B** | Increased pending and reversed outcomes during a defined Sunday event window | Pending, reversed, and non-success monitoring |
| **C** | Increased failure probability for merchant category code 4814 during a defined event window | Failure and non-success monitoring |

Scenario transformations are applied to separate copies of the baseline data. The original status is retained, and the simulated status is stored separately.

> **Scenario results are simulation outputs, not observed payment-system incidents.**

### Detection method status

The daily threshold method has been implemented and evaluated, including a comparison of experimental threshold multipliers.

The rolling seven-day method and hourly CUSUM method remain planned extensions. They should not be treated as completed or evaluated methods until implementation and results have been validated.

> **Threshold settings in this project are experimental. They are not production alert thresholds.**

---

## Monitoring KPIs

The proposed monitoring framework includes:

- **Transaction volume:** Number of transactions within a defined time interval.
- **Failure count:** Number of transactions with a failed status.
- **Failure rate:** Failed transactions divided by total transactions, multiplied by 100.
- **Failed-value exposure:** Sum of the amounts associated with failed transactions.
- **Pending share:** Pending transactions divided by total transactions, multiplied by 100.
- **Reversal share:** Reversed transactions divided by total transactions, multiplied by 100.
- **Peak-hour volume:** Transaction count within selected high-activity hours.
- **Account concentration:** Share of transaction activity attributable to the most active accounts.

> **Failed-value exposure is a monitoring measure. It does not represent confirmed financial loss, unsettled funds, or customer compensation.**

---

## Limitations

- The transaction dataset is synthetic and cannot establish actual Nigerian payment performance.
- The generated outcome patterns show little variation across the tested dimensions. The analysis does not support causal explanations for transaction failures.
- The fraud indicator does not provide useful separation across the tested dimensions. It is not used as a fraud-prediction target, and flagged records are not treated as confirmed fraud.
- The balance fields are excluded because their behaviour is inconsistent with reliable transaction-outcome interpretation.
- Scenario tests are controlled simulations. Their results do not represent real incidents.
- The daily threshold method is experimental. Further validation is required before operational use.
- Rolling seven-day and hourly CUSUM comparisons are not yet complete.
- The project does not analyse settlement delays or identify root causes of failed transactions because the dataset does not provide the required evidence.

---

## Repository Structure

```text
payment-reliability-project/
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── notebooks/
│   └── Monitoring_Digital_Payment_Reliability.ipynb
├── data/
│   └── README.md
├── figures/
│   ├── baseline/
│   └── scenarios/
├── docs/
│   ├── 01_project_methodology.md
│   ├── 02_data_dictionary.md
│   ├── 03_data_quality_audit.md
│   ├── 04_research_questions.md
│   ├── 05_limitations.md
│   └── 06_monitoring_kpis.md
├── reports/
│   └── project_summary.md
└── powerbi/
    └── README.md
```

## Tools and Technologies

- Python
- Jupyter Notebook
- pandas
- NumPy
- Matplotlib
- SciPy
- Parquet
- Power BI (planned)

## Running the Notebook

1. Clone or download this repository.
2. Download the Parquet dataset using the link above.
3. Place the file in the location expected by the notebook, or update the notebook's dataset path.
4. Install the dependencies listed in `requirements.txt`.
5. Open the notebook in Jupyter and run the cells in order.

```bash
git clone <repository-url>
cd payment-reliability-project
pip install -r requirements.txt
jupyter notebook
```

Replace `<repository-url>` with your GitHub repository URL.

## Project Status

**Current stage: Exploratory analysis and documentation.**

- Data profiling and quality audit: implemented.
- Feature engineering and baseline analysis: substantially implemented.
- Scenario design: implemented.
- Daily threshold evaluation: implemented, with experimental sensitivity checks.
- Rolling seven-day detection: pending.
- Hourly CUSUM detection: pending.
- Final clean execution and output verification: pending.
- Power BI report: planned.

The repository will be updated as the remaining evaluation work and Power BI report are completed.

## Future Work

- Complete and validate the rolling seven-day and hourly CUSUM methods.
- Compare detection performance across the three scenarios using consistent evaluation criteria.
- Re-run the full notebook from a clean environment and verify all reported outputs.
- Build the planned Power BI report with pages for Executive Overview, Transaction Behaviour, Reliability Monitoring, and Scenario Testing.
- Add final charts, a project summary, and supporting documentation.
