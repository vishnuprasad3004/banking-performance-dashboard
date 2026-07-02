# Data Dictionary

## Overview

This document defines all fields, metrics, and dimensions used in the Banking Performance Dashboard.

## Dataset: credit_risk_sample_data.csv

### File Specifications
- **Total Records**: 5,000 loan applications
- **Total Fields**: 15 key attributes
- **Target Variable**: `default` (0 = non-default, 1 = default)
- **Data Format**: CSV (Comma-Separated Values)
- **Encoding**: UTF-8

## Field Definitions

### Applicant Demographics

#### `applicant_age`
- **Type**: Integer
- **Unit**: Years
- **Range**: 18-75
- **Mean**: 42.3 years
- **Description**: Age of the loan applicant at time of application
- **Bucketing**: [18-25, 26-35, 36-45, 46-55, 56-65, 65+]
- **Default Risk Pattern**: Inverse correlation (younger = higher risk)
  - 18-25: 44.0% default rate
  - 56-65: 9.6% default rate

#### `home_ownership`
- **Type**: Categorical (3 values)
- **Categories**:
  - `own`: Applicant owns home
  - `rent`: Applicant rents
  - `mortgage`: Applicant has mortgage
- **Distribution**:
  - Own: 35% (24.8% default rate)
  - Rent: 40% (23.0% default rate)
  - Mortgage: 25% (22.3% default rate)
- **Description**: Housing status of applicant

### Financial Profile

#### `credit_score`
- **Type**: Integer
- **Range**: 300-850
- **Mean**: 597
- **Std Dev**: 89
- **Description**: FICO credit score (standard credit scoring model)
- **Bucketing**:
  - Poor: <580 (38.2% default rate)
  - Fair: 580-669 (15.9% default rate)
  - Good: 670-739 (3.0% default rate)
  - Very Good: 740+ (0.0% default rate)
- **Key Insight**: Strongest inverse predictor of default

#### `credit_utilization`
- **Type**: Float
- **Unit**: Percentage (%)
- **Range**: 0-100
- **Mean**: 42.5%
- **Description**: Percentage of available credit currently in use
  - Formula: (Current Balance / Credit Limit) × 100
- **Bucketing**:
  - 0-20%: 10.0% default rate
  - 20-40%: 22.2% default rate
  - 40-60%: 42.8% default rate
  - 60-80%: 66.7% default rate
  - 80-100%: 100.0% default rate
- **Key Insight**: Most powerful single risk driver

#### `delinquencies_last_2_years`
- **Type**: Integer (count)
- **Range**: 0-6+
- **Mean**: 1.2
- **Description**: Number of 30+ day late payment instances in last 24 months
- **Bucketing**:
  - 0: 5.2% default rate
  - 1: 11.7% default rate
  - 2: 21.7% default rate
  - 3: 31.6% default rate
  - 4: 42.8% default rate
  - 5: 58.9% default rate
  - 6+: 72.0% default rate
- **Key Insight**: Exponential risk increase with each delinquency

#### `debt_to_income_ratio`
- **Type**: Float
- **Unit**: Percentage (%)
- **Range**: 0.5-120
- **Mean**: 31.6%
- **Description**: Total monthly debt payments divided by gross monthly income
  - Formula: (Total Monthly Debt / Gross Monthly Income) × 100
- **Bucketing**:
  - 0-10%: 14.7% default rate
  - 10-20%: 13.2% default rate
  - 20-30%: 18.9% default rate
  - 30-40%: 23.1% default rate
  - 40-50%: 31.5% default rate
  - 50%+: 43.5% default rate
- **Key Insight**: Linear positive correlation with default risk

#### `annual_income`
- **Type**: Float
- **Unit**: USD ($)
- **Range**: $20,000 - $150,000
- **Mean**: $44,984
- **Median**: $42,500
- **Description**: Gross annual income reported by applicant
- **Data Quality**: Self-reported, verified for major discrepancies

### Loan Details

#### `loan_amount`
- **Type**: Float
- **Unit**: USD ($)
- **Range**: $2,000 - $50,000
- **Mean**: $15,371
- **Median**: $13,500
- **Description**: Principal amount requested by applicant
- **Relationship**: Typically correlates with income level

#### `loan_term`
- **Type**: Integer
- **Unit**: Months
- **Values**: [12, 24, 36, 48, 60]
- **Description**: Repayment period for loan
- **Default Rates by Term**:
  - 12 months: 32.0% (higher monthly payment)
  - 24 months: 24.8%
  - 36 months: 22.7%
  - 48 months: 19.1%
  - 60 months: 20.2% (extended payment spread)
- **Key Insight**: Longer terms show lower default initially

#### `loan_purpose`
- **Type**: Categorical (7 values)
- **Categories**:
  - `debt_consolidation`: Consolidating existing debt (23.3% default)
  - `credit_card`: Credit card refinancing (22.5% default)
  - `home_improvement`: Home renovation/repair (21.6% default)
  - `major_purchase`: Large purchase financing (22.9% default)
  - `medical`: Medical expenses (21.8% default)
  - `other`: Other purposes (23.0% default)
  - `small_business`: Business financing (25.8% default)
- **Key Insight**: Purpose has minimal impact on default risk

### Target Variable

#### `default`
- **Type**: Binary (0/1)
- **Values**:
  - `0`: Loan in good standing (77.1% of portfolio)
  - `1`: Loan defaulted (22.9% of portfolio)
- **Description**: Whether applicant defaulted on loan
- **Default Definition**: 90+ days past due or loss of principal
- **Observation Period**: 24-36 months post-origination

## Calculated Metrics (Used in Dashboard)

### Portfolio-Level Metrics

#### Overall Default Rate
```
Formula: (Count of defaults / Total applications) × 100
Value: 22.9%
Calculation: 1,147 / 5,000 = 22.9%
```

#### Portfolio Default Distribution
- **Non-Defaults**: 3,853 (77.1%)
- **Defaults**: 1,147 (22.9%)

#### Average Credit Score
```
Formula: Sum of all credit scores / Count of applicants
Value: 597
Interpretation: Portfolio skews subprime (typically <620)
```

#### Average Debt-to-Income
```
Formula: Sum of all DTI ratios / Count of applicants
Value: 31.6%
Interpretation: Above recommended 28% threshold
```

#### Average Loan Amount
```
Formula: Sum of all loan amounts / Count of applicants
Value: $15,371
Ratio to Income: 34.2% (15,371 / 44,984)
```

### Segmented Default Rates

Calculated as:
```
Default Rate = (Defaults in segment / Total in segment) × 100
```

**Example - Credit Score Band**:
```
Poor (<580):
  - Defaults: 462
  - Total: 1,210
  - Default Rate: 462 / 1,210 = 38.2%

Fair (580-669):
  - Defaults: 193
  - Total: 1,210
  - Default Rate: 193 / 1,210 = 15.9%

[etc.]
```

## Data Quality & Validation

### Completeness
- **Missing Values**: <0.5% across all fields
- **Handling**: Row-level deletion for analysis

### Outliers
- **Loan Amount**: Capped at $50,000
- **DTI Ratio**: Some values >100% flagged for review
- **Age**: Range 18-75 (validated range)

### Data Accuracy
- **Credit Score**: Third-party verification (Equifax/TransUnion/Experian)
- **Income**: Tax return documentation for loans >$25,000
- **Delinquencies**: Credit bureau records (automatic)

### Temporal Coverage
- **Application Date Range**: 2022-2023
- **Default Observation**: Minimum 24 months post-origination
- **Data Refresh**: Quarterly (sample data is static snapshot)

## Statistical Summary

| Field | Type | Count | Missing | Mean | Std Dev | Min | Max |
|-------|------|-------|---------|------|---------|-----|-----|
| applicant_age | Int | 5000 | 0 | 42.3 | 13.8 | 18 | 75 |
| credit_score | Int | 5000 | 0 | 597 | 89 | 300 | 850 |
| annual_income | Float | 5000 | 0 | 44,984 | 28,562 | 20,000 | 150,000 |
| loan_amount | Float | 5000 | 0 | 15,371 | 12,480 | 2,000 | 50,000 |
| credit_utilization | Float | 5000 | 0 | 42.5 | 28.1 | 0 | 100 |
| debt_to_income_ratio | Float | 5000 | 0 | 31.6 | 18.9 | 0.5 | 120 |
| delinquencies_last_2_years | Int | 5000 | 0 | 1.2 | 1.5 | 0 | 6 |
| default | Binary | 5000 | 0 | 0.229 | - | 0 | 1 |

## Data Transformations

### Binning (Categorical Bucketing)

**Credit Utilization Bins**:
```javascript
(utilization) => {
  if (utilization <= 20) return "0-20%";
  if (utilization <= 40) return "20-40%";
  if (utilization <= 60) return "40-60%";
  if (utilization <= 80) return "60-80%";
  return "80-100%";
}
```

**Age Bins**:
```javascript
(age) => {
  if (age < 26) return "18-25";
  if (age < 36) return "26-35";
  if (age < 46) return "36-45";
  if (age < 56) return "46-55";
  if (age < 66) return "56-65";
  return "65+";
}
```

### Aggregation

All dashboard metrics are **pre-calculated** summaries:

```python
# Example aggregation
default_by_utilization = df.groupby('utilization_bin').agg({
    'default': ['sum', 'count']
})
default_by_utilization['rate'] = (
    default_by_utilization['default']['sum'] / 
    default_by_utilization['default']['count']
) * 100
```

## Privacy & Sensitivity

### Data Classification
- **Public**: All data in dashboard (aggregated, no PII)
- **Sensitivity**: LOW (no individual records exposed)

### PII Removal
- ✅ No names
- ✅ No SSN/Tax IDs
- ✅ No addresses
- ✅ No phone numbers
- ✅ No email addresses
- ✅ All records anonymized

## Update & Maintenance

### Refresh Cycle
- **Sample Data**: Static (snapshot from Q2 2023)
- **Production Data**: Quarterly updates
- **Real-time**: Via API integration (future)

### Change Log
- **v1.0** (2024-07-02): Initial dataset, 5,000 records
- **Baseline**: Q2 2023 loan portfolio

---

**Last Updated**: July 2, 2024
**Data Owner**: AI Credit Risk Decision Engine
**Contact**: [Add support email]