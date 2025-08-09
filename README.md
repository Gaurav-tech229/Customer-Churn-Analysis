# Customer-Churn-Analysis

This repository contains a Jupyter Notebook for analyzing a customer churn dataset. The analysis aims to identify key factors influencing customer churn, perform data cleaning, feature engineering, statistical tests, visualizations, and derive actionable insights and recommendations. The dataset used is `customer_churn_dataset-training-master.csv`, which includes customer attributes such as age, tenure, support calls, and churn status.

## Problem Statement

Customer churn (the rate at which customers stop doing business with a company) is a critical issue for businesses, especially in subscription-based services. The goal of this analysis is to:

- Explore and clean the dataset to ensure data quality.
- Engineer new features to better capture churn drivers.
- Perform statistical tests (e.g., t-tests, chi-square) to identify significant predictors of churn.
- Visualize relationships between features and churn.
- Segment customers and analyze churn rates across segments.
- Derive insights into why customers churn and provide recommendations to reduce churn.

Key questions addressed:
- What is the overall churn rate?
- Which features (e.g., support calls, payment delays, engagement) strongly correlate with churn?
- How do customer segments (based on spending, tenure, etc.) differ in churn behavior?
- Are there interactions between features (e.g., support calls and payment delays) that amplify churn risk?

## Dataset

- **File**: `customer_churn_dataset-training-master.csv`
- **Source**: Training dataset for customer churn prediction (likely from a Kaggle-like source; original context not specified).
- **Description**: Contains 440,832 rows (after cleaning) and 12 columns:
  - `CustomerID`: Unique identifier (float64, but treated as int where possible).
  - `Age`: Customer age (int).
  - `Gender`: Male/Female (object).
  - `Tenure`: Months with the service (int).
  - `Usage Frequency`: Frequency of usage (int).
  - `Support Calls`: Number of support calls made (int).
  - `Payment Delay`: Days delayed in payment (int).
  - `Subscription Type`: Basic/Standard/Premium (object).
  - `Contract Length`: Monthly/Quarterly/Annual (object).
  - `Total Spend`: Total amount spent (float).
  - `Last Interaction`: Days since last interaction (int).
  - `Churn`: 1 (churned) or 0 (retained) (int).
- **Issues Identified**:
  - 1 missing value per column (dropped).
  - No duplicates.
  - No outliers (via IQR method).
  - No negative values in non-negative columns.
  - Categorical consistency checked (e.g., Gender: Male=250,252, Female=190,580).

## Analysis Steps

The notebook follows these key steps:

1. **Import Libraries**: Loads pandas, numpy, matplotlib, seaborn, sklearn components, scipy stats, and StringIO.

2. **Set Visualization Style**: Uses seaborn's 'deep' palette.

3. **Load Data**: Reads the CSV file.

4. **Data Cleaning**:
- Checks and drops missing values (1 row dropped).
- Checks and removes duplicates (none found).
- Verifies data types and converts where needed (e.g., Age to int).
- Checks categorical consistency.
- Detects outliers using IQR (none found).
- Ensures no negative values in relevant columns.

5. **Feature Engineering**:
- Creates `Tenure Band` (e.g., '0-3 months', '4-12 months').
- Creates `Support Calls Bucket` (Low: 0-2, Medium: 3-5, High: 6-10).
- Creates `Payment Delay Bucket` (Low: 0-7, Medium: 8-14, High: 15-30).
- Interaction term: `Support_Payment_Interaction` = Support Calls * Payment Delay.
- Polynomial features: Adds `Tenure^2`, `Tenure*Total Spend`, `Total Spend^2`.
- Scales numerical features using StandardScaler.
- Customer segmentation via KMeans clustering (3 clusters based on Tenure, Total Spend, Engagement Score).
- Derived features like `Engagement_Score` = Usage Frequency / (Last Interaction + 1).

6. **Exploratory Data Analysis (EDA)**:
- Computes overall churn rate (~56.71%).
- Correlation matrix with churn.
- Statistical tests:
  - T-tests for numerical features (e.g., Engagement Score, Support_Payment_Interaction).
  - Chi-square for categorical features (e.g., Customer Segment).
- Cross-tabulations (e.g., churn by contract length and payment delay).

7. **Visualizations**:
- Churn distribution pie chart.
- Churn by gender, subscription type, contract length (bar plots).
- Age distribution by churn (histogram).
- Churn by tenure band, support calls bucket, payment delay bucket (bar plots).
- Correlation heatmap.
- Churn by customer segment (bar plot).
- Pair plot of key features (Age, Tenure, Total Spend, Engagement_Score, Churn).

8. **Insights and Recommendations**: Summarized in the final cell (printed output).

## Key Findings

From the notebook:

- **Overall Churn Rate**: 56.71%, with variations across segments.
- **Strong Predictors**:
- Engagement Score (p=0.0000) and Support_Payment_Interaction (p=0.0000) are highly significant.
- Support Calls (correlation: 0.57) and Support_Payment_Interaction (0.48) are top drivers.
- **Segment Analysis**:
- Customer segments (via KMeans) show association with churn (chi2=94,757.12, p=0.0000).
- Low-spend segments (e.g., Segment 2) have higher churn (94.92%) vs. high-spend (40.42%).
- **High-Risk Groups**:
- Monthly contract customers with high support calls (6-10): 100% churn.
- New customers (<3 months) in low-spend segments: 100% churn.
- **Other Observations**:
- Higher support calls and payment delays amplify churn.
- Shorter contracts (monthly) correlate with higher churn.

## Recommendations

Based on the analysis:

1. Implement loyalty programs for low-spend segments to boost retention.
2. Enhance onboarding for new customers (<3 months) with high support calls to reduce early churn.
3. Streamline support and billing processes to mitigate the combined effect of support issues and payment delays.
4. Monitor Engagement Score as a leading indicator, triggering personalized campaigns for low scores.
5. Promote longer-term contracts (e.g., annual) with incentives to lower churn, especially for monthly subscribers.

---
