# Monitoring Digital Payment Reliability in Nigeria

### A Baseline Profile of Transaction Concentration, Temporal Patterns, and Scenario-Based KPI Detection Using Synthetic Transaction Data

**PairBud Project:** Emmanuel Olawumi and Favour Obajuaye  
**Last Updated:** November 2026

---

## 1. Project Overview

This project explores how data analytics can support the monitoring of digital payment reliability.

It examines transaction outcomes, account-level activity, transaction volume patterns, and the distribution of activity across channels, merchant categories, and geographical locations.

It also uses controlled scenario simulations to test how selected monitoring indicators respond to changes in transaction outcomes.

> ** The project uses a synthetic Nigerian retail banking transaction dataset. Therefore, its findings describe the generated dataset and do not represent the actual performance of Nigerian banks, fintech companies, or payment systems.**

The main objective is to demonstrate a reproducible analytical workflow and translate the findings into practical transaction monitoring recommendations.

---

## 2. Problem Statement

Failed, pending, and reversed transactions can create operational challenges for payment providers and financial institutions.

Effective monitoring requires indicators that help operations teams identify unusual changes in transaction outcomes and activity patterns.

However, publicly available transaction-level banking data is limited. This project uses a synthetic dataset to demonstrate how transaction profiling, controlled simulations, and monitoring KPIs can support this process.

> ** The project does not attempt to identify the causes of payment failures. The available data does not contain failure reason codes or the operational information required for causal analysis.**

---

## 3. Project Aim and Objectives

### Aim

> To profile transaction activity in a synthetic Nigerian retail banking dataset and evaluate monitoring KPIs under explicitly defined reliability scenarios.

### Objectives

- Examine the distribution of successful, failed, pending, and reversed transactions.
- Measure transaction concentration across customer accounts.
- Analyse transaction volume and temporal patterns.
- Examine transaction composition across channels, merchant categories, and states.
- Investigate data quality and document variables unsuitable for analysis.
- Simulate controlled changes in transaction outcomes.
- Evaluate monitoring methods using detection and false-alert measures.
- Recommend practical KPIs for transaction reliability monitoring.

---

## 4. Dataset and Sources

### Primary Dataset

**Nigerian Banking Retail Transactions**

Source: [Hugging Face Dataset Repository](https://huggingface.co/datasets/electricsheepafrica/nigerian-banking-retail-transactions)

Direct Parquet file: [Download Dataset](https://huggingface.co/datasets/electricsheepafrica/nigerian-banking-retail-transactions/resolve/main/nigerian_retail_transactions_full.parquet)

| Attribute | Description |
|---|---|
| Records | 5,000,000 |
| Original columns | 16 |
| Period | January 2023 to December 2024 |
| Coverage | 730 calendar days |
| Currency | Nigerian Naira (₦) |
| Format | Parquet |
| Data type | Synthetic transaction data |

The dataset contains transaction timestamps, amounts, transaction types, channels, merchant information, locations, transaction statuses, account identifiers, and a synthetic fraud indicator.

### External Context

[Central Bank of Nigeria, e-Payment Statistics](https://www.cbn.gov.ng/PaymentsSystem/ePaymentStatistics.html)

CBN statistics provide separate aggregate context on digital payment activity in Nigeria.

> They are not merged with the synthetic transaction-level dataset and are not used to calculate transaction-level reliability metrics.

---

## 5. Tools and Technologies

| Tool | Purpose |
|---|---|
| Python | Data analysis and processing |
| Pandas | Data manipulation and profiling |
| NumPy | Numerical operations |
| Matplotlib | Data visualisation |
| Jupyter Notebook | Reproducible analytical workflow |
| GitHub | Project documentation and version control |
| Power BI | Planned dashboard and reporting layer |

---

## 6. Analytical Workflow

The project follows five main phases.

### Phase 1: Environment and Data Acquisition

- Set up the Python environment.
- Acquire the source Parquet dataset.
- Inspect the dataset structure and data types.
- Apply memory-conscious data handling.

### Phase 2: Data Understanding and Quality Audit

- Examine transaction records and distributions.
- Check missing values and duplicate transaction identifiers.
- Audit transaction amounts and balance fields.
- Examine associations between transaction outcomes and selected variables.
- Document analytical limitations.
- Prepare the working dataset.

### Phase 3: Baseline Transaction Analysis

- Profile the four transaction outcomes.
- Examine transaction volume and value.
- Measure account-level transaction concentration.
- Analyse hourly and daily transaction patterns.
- Examine channel, merchant-category, and geographical composition.

### Phase 4: Scenario Simulation and Detection

- Define controlled transaction reliability scenarios.
- Apply scenario transformations to copies of the baseline data.
- Preserve the original transaction records.
- Develop monitoring methods.
- Evaluate detection behaviour and false alerts.

### Phase 5: Recommendations and Reporting

- Define operational monitoring KPIs.
- Interpret baseline and scenario findings.
- Document limitations and recommendations.
- Prepare the final report and visualisations.
- Develop a Power BI dashboard as a presentation layer.

---

## 7. Data Quality Findings

The initial data audit identified the following characteristics.

| Audit Area | Finding | Analytical Decision |
|---|---|---|
| Dataset structure | 5 million records and 16 original columns | Retain the dataset |
| Missing values | No missing values in audited fields | Retain |
| Duplicate IDs | No duplicate transaction IDs | Retain |
| Date coverage | 730 consecutive calendar days | Retain |
| Transaction amounts | All amounts are positive and multiples of ₦100 | Document as a synthetic data characteristic |
| Balance fields | Balance movements do not reliably reflect transaction outcomes | Exclude both balance fields |
| Fraud indicator | Approximately 0.8% prevalence with negligible association with transaction status | Descriptive use only |

After excluding the two balance fields, the analytical dataset contains 5 million records and 14 columns.

### Important Data Limitations

**Balance fields**

The `balance_before_ngn` and `balance_after_ngn` fields do not reliably represent transaction outcomes. They are excluded from subsequent analysis.

> The project does not use them to infer successful posting, settlement, financial loss, or ledger behaviour.

**Fraud indicator**

The `fraud_flag` field is a synthetic indicator. Its observed relationship with transaction outcomes is negligible.

> The project does not treat flagged records as confirmed fraud or use the field for fraud prediction.

**Transaction failure patterns**

The failure rate remains nearly constant across the tested dimensions. The available data does not support meaningful failure-cause analysis.

---

## 8. Baseline Findings

> The following figures describe the synthetic dataset.

### 8.1 Transaction Status Distribution

| Status | Transactions | Share |
|---|---:|---:|
| Success | 4,599,963 | 92.00% |
| Failed | 349,765 | 7.00% |
| Pending | 40,383 | 0.81% |
| Reversed | 9,889 | 0.20% |
| **Total** | **5,000,000** | **100%** |

### 8.2 Account-Level Transaction Concentration

Transaction activity is concentrated among a relatively small proportion of accounts.

| Account Group | Share of Total Transactions |
|---|---:|
| Top 1% | 33.9% |
| Top 5% | 50.6% |
| Top 10% | 59.6% |
| Top 20% | 70.0% |
| Top 50% | 86.4% |

> This describes transaction concentration within the generated dataset. It does not establish actual customer activity patterns in Nigeria.

### 8.3 Channel Composition

Mobile, POS, and ATM account for approximately 85% of the transaction records.

This indicates that transaction volume is concentrated in these three channels within the dataset.

> Higher transaction volume does not automatically indicate higher failure risk.

### 8.4 Temporal Patterns

The baseline analysis identifies:

- Higher transaction activity around 18:00 to 20:00.
- Lower activity between 00:00 and 05:00.
- A narrow daily transaction-volume range across the 730-day period.

> These patterns describe the generated data and should not be interpreted as actual Nigerian payment trends.

### 8.5 Statistical Association

The statistical audits show negligible observed associations between transaction status and several tested dimensions, including channel, transaction type, and the synthetic fraud indicator.

For example:

- Channel and status: Cramér's V = 0.000738.
- Transaction type and status: Cramér's V = 0.000628.
- Fraud flag and status: Cramér's V = 0.000064.

> The results do not provide evidence of meaningful associations in the tested data. They do not establish that these variables are independent in real payment systems.

---

## 9. Scenario-Based Monitoring Tests

The project uses three controlled scenarios to examine how monitoring indicators respond to selected changes in transaction outcomes.

All scenario transformations are applied to copies of the baseline data. The original dataset remains unchanged.

> **🧪 All simulated results must be labelled: Scenario test output, not observed data.**

### Scenario A: Peak-Hour Mobile Failure Increase

| Parameter | Definition |
|---|---|
| Channel | Mobile |
| Event period | 17 to 30 June 2024 |
| Time window | 18:00 to 19:59 |
| Baseline failure probability | 7% |
| Simulated failure probability | 12% |
| Change | Increase in failed transaction probability |

This scenario tests whether monitoring can identify a sustained increase in failed mobile transactions during a defined peak-hour window.

### Scenario B: Pending and Reversal Increase

| Parameter | Definition |
|---|---|
| Channel | All channels |
| Event date | 15 September 2024 |
| Time window | 12:00 to 17:59 |
| Pending adjustment | Increase by 3 percentage points |
| Reversal adjustment | Increase by 2 percentage points |
| Failed transactions | Unchanged |

This scenario tests whether monitoring can identify changes in pending and reversed transaction shares when the failure rate remains unchanged.

It demonstrates why monitoring only failed transactions may overlook other important changes in transaction outcomes.

### Scenario C: Telecom Merchant Category Failure Increase

| Parameter | Definition |
|---|---|
| Merchant Category Code | MCC 4814 |
| Event date | 28 June 2023 |
| Time window | 06:00 to 17:59 |
| Baseline failure rate | Approximately 7.24% |
| Simulated target failure rate | 15% |
| Change | Increase in failed transactions within the selected merchant category |

This scenario tests whether monitoring can identify a failure-rate increase within a specific merchant category.

> The simulation uses the same target population before and after injection. The resulting comparison is a controlled scenario test, not an independent-sample experiment.

---

## 10. Detection Methodology

The project considers three candidate monitoring approaches.

| Method | Description | Current Status |
|---|---|---|
| Daily threshold monitoring | Compares daily KPI rates against a baseline-derived upper control limit | ✅ Implemented in the notebook |
| Rolling seven-day baseline | Compares current activity with a rolling historical baseline | 🔄 Planned extension |
| Hourly CUSUM | Sequentially monitors hourly changes against a reference level | 🔄 Planned extension |

### Daily Threshold Monitoring

The implemented daily threshold method uses a binomial upper control limit:

$$
UCL_d = p_0 + k\sqrt{\frac{p_0(1-p_0)}{n_d}}
$$

Where:

- $p_0$ is the pre-event baseline rate.
- $k$ is the selected control-limit multiplier.
- $n_d$ is the eligible transaction count for the monitoring day.

The method compares experimental values of $k=3$ and $k=4$.

Calibration uses pre-event observations. Event and post-event periods are excluded from baseline calibration.

### Evaluation Measures

The detection evaluation focuses on:

- Whether the monitoring method detects the simulated event.
- Detection lag in calendar days.
- Number of event days flagged.
- Number of event days evaluated.
- False-alert days.
- False-alert rate across eligible post-event control days.

> The project will report only metrics that have been calculated and validated in the notebook.

The rolling seven-day and hourly CUSUM methods remain planned extensions. They should not be treated as completed or compared as evaluated methods until their implementations and results are available.

---

## 11. Monitoring KPI Catalogue

The project considers the following indicators for operational monitoring.

| KPI | Definition | Monitoring Purpose |
|---|---|---|
| Transaction Volume | Number of eligible transactions | Track activity levels |
| Success Rate | Successful transactions divided by eligible transactions | Monitor successful outcomes |
| Failure Rate | Failed transactions divided by eligible transactions | Identify increases in failed transactions |
| Pending Share | Pending transactions divided by eligible transactions | Monitor unresolved transaction outcomes |
| Reversal Share | Reversed transactions divided by eligible transactions | Track changes in recorded reversals |
| Account Concentration | Share of transactions generated by ranked account groups | Monitor activity concentration |
| Failed Transaction Value | Sum of failed transaction amounts | Support prioritisation, not financial-loss measurement |

> The KPIs should be interpreted with their denominators, monitoring windows, and relevant transaction segments clearly stated.

---

## 12. Limitations

The project has several limitations.

1. **Synthetic data:** The dataset does not represent actual Nigerian payment performance.
2. **Generated patterns:** Transaction distributions and temporal patterns may reflect the dataset's generation process.
3. **No failure reasons:** The dataset cannot support failure-cause analysis.
4. **No settlement timestamps:** Settlement delays cannot be evaluated.
5. **Defective balance fields:** Balance movements cannot support reliable ledger or settlement conclusions.
6. **Synthetic fraud flag:** The fraud indicator is not suitable for confirmed fraud classification or predictive modelling.
7. **Scenario simulations:** Injected changes test monitoring behaviour. They are not observations of real payment incidents.
8. **External aggregate data:** CBN statistics are kept separate and cannot validate transaction-level outcomes in the synthetic dataset.

> The project therefore focuses on descriptive analysis, monitoring design, and controlled method evaluation rather than causal inference or claims about actual payment reliability.

---

## 13. Repository Structure

The proposed repository structure is:

```text
payment-reliability-project/
│
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
│
├── notebooks/
│   └── Monitoring_Digital_Payment_Reliability.ipynb
│
├── data/
│   └── README.md
│
├── figures/
│   ├── baseline/
│   └── scenarios/
│
├── docs/
│   ├── 01_project_methodology.md
│   ├── 02_data_dictionary.md
│   ├── 03_data_quality_audit.md
│   ├── 04_research_questions.md
│   ├── 05_limitations.md
│   └── 06_monitoring_kpis.md
│
├── reports/
│   └── project_summary.md
│
└── powerbi/
    └── README.md
```

The repository will contain the notebook, documentation, and selected analytical outputs.

> The original 5-million-row Parquet dataset will not be uploaded to GitHub. Users should obtain it directly from the Hugging Face source.

## 14. Reproducing the Analysis

Requirements

Python 3.10 or later.

Jupyter Notebook.

Pandas.

NumPy.

Matplotlib.

A compatible Parquet engine, such as PyArrow.

## Installation

Clone the repository:

git clone https://github.com/your-username/payment-reliability-project.git
cd payment-reliability-project

Install the dependencies:

pip install -r requirements.txt

Open Jupyter Notebook:

jupyter notebook

Open the notebook in the notebooks/ directory.

The notebook checks for the dataset in the configured project directory and downloads the Parquet file from Hugging Face if it is not already available.

Update the project directory in the notebook to match your local setup.

> Because the dataset contains five million records, sufficient system memory is required.

## 15. Current Project Status

Component

Status

Data acquisition and initial profiling

Developed

Data quality audit

Developed

Analytical dataset preparation

Developed

Baseline transaction analysis

Developed

Scenario A simulation

Developed

Scenario B simulation

Developed

Scenario C simulation

Developed

Daily threshold method

Implemented in notebook

Rolling seven-day method

Planned

Hourly CUSUM method

Planned

Final detection evaluation

Requires clean-kernel execution and validation

Final report and documentation

In progress

Power BI dashboard

Planned

The notebook has undergone a methodological review. A complete restart-and-run execution against the source Parquet file is still required before final numerical outputs are treated as verified.

## 16. Future Development

The next stages of the project include:

- Complete a clean-kernel execution of the notebook.
- Validate all scenario outputs and detection metrics.
- Implement and evaluate the rolling seven-day method.
- Implement and evaluate hourly CUSUM.
- Compare the methods using validated detection results.
- Finalise the monitoring KPI catalogue and recommendations.
- Publish the supporting documentation and selected figures.
- Develop a Power BI dashboard for reporting and exploration.

The planned Power BI dashboard will organise the findings into four pages:

1. Executive Overview.
2. Transaction Behaviour.
3. Reliability Monitoring.
4. Scenario Testing.

Python and Jupyter Notebook will remain the primary analytical workflow. Power BI will serve as the presentation and reporting layer.

## 17. Contributors

Emmanuel Olawumi
Project design, data preparation, exploratory analysis, statistical interpretation, and documentation.

Favour Obajuaye
Project collaboration, analytical development, and scenario-based monitoring work.

## 18. Acknowledgements and References

- [Nigerian Banking Retail Transactions, Hugging Face](https://huggingface.co/datasets/electricsheepafrica/nigerian-banking-retail-transactions)
- [Central Bank of Nigeria, e-Payment Statistics](https://www.cbn.gov.ng/PaymentsSystem/ePaymentStatistics.html)

## Disclaimer

This project is an educational and analytical demonstration using synthetic transaction data.

The results do not represent actual Nigerian banking transactions, customer behaviour, payment-system performance, or financial institution reliability.

All simulated results are controlled test outputs and must not be interpreted as observed payment incidents.

The project does not use confidential employer, customer, or production transaction data.
