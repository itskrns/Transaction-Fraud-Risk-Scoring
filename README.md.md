# Transaction Fraud Risk Analysis – Rule-Based Scoring Project

## Project Overview

This project focuses on transaction-level fraud detection using an explainable, rule-based fraud risk scoring system. Instead of relying on black-box machine learning models, the objective is to build a transparent and business-friendly scoring logic that clearly explains why a transaction is considered risky.

Python is used for data cleaning, exploratory analysis, and feature engineering, while Power BI is used to visualize fraud patterns and risk bands across multiple dimensions.

---

## Key Metrics

- Total transactions: ~14,000 (displayed as 14K on the dashboard)
- Total fraud transactions: 1,781
- Overall fraud rate: 12.38%
- Average fraud amount: ₹518.35

---

## Dataset Description

Each row in the dataset represents a single card transaction with a binary fraud label.

Main fields include:
- Transaction fields: trans_date, trans_time, amt, category, merchant
- Customer fields: city, state, lat, long, city_pop, job, dob
- Merchant fields: merch_lat, merch_long
- Target field: is_fraud (0 = legitimate, 1 = fraud)

Basic data quality checks such as null value handling, duplicate checks, and data type validation are performed during preprocessing using Python.

---

## Feature Engineering

### Time-Based Features

- trans_datetime created by combining transaction date and time
- transaction_hour (0–23)
- day_of_week (Monday–Sunday)
- transaction_month (1–12)

### Customer Feature

- Customer age calculated at the time of transaction using date of birth and transaction date

### Amount-Based Features

- Amount buckets used consistently across analysis and reporting:
  - Low range: 0–500
  - Medium range: 501–1500
  - High range: above 1500
- high_amount_flag to quickly identify higher-ticket transactions

### Risk Signals for Scoring

- Category-level fraud rates used to label categories as high-risk or medium-risk
- Merchant-level fraud rates used to identify high-risk merchants for watchlisting

---

## Exploratory Insights

### Fraud by Amount Range

Fraud is heavily concentrated in medium-range transaction amounts.

- Medium range (501–1500): fraud rate ~88.29%, representing the primary fraud hotspot
- Low range (0–500): fraud rate ~7.03%, high volume but relatively safer
- High range: present in the dataset but not a major contributor to fraud

### Fraud by Risk Band

Transactions are grouped into three risk bands using the rule-based score, showing strong separation in fraud behavior.

- High risk band: fraud rate ~68.92%
- Medium risk band: fraud rate ~18.42%
- Low risk band: fraud rate ~2.69%

Fraud probability increases sharply as the risk band moves from low to high, validating the effectiveness of the scoring logic.

### Fraud by Category

Certain categories exhibit significantly higher fraud rates:

- shopping_net: ~27.35%
- grocery_pos: ~27.22%
- misc_net: ~26.63%
- shopping_pos: ~13.88%
- gas_transport: ~10.74%

Fraud is primarily concentrated in online and card-not-present style categories, while essential and routine spend categories are comparatively safer.

### Fraud by Day of Week

Clear weekly patterns are observed in fraud activity:

- Wednesday: 18.05%
- Saturday: 16.35%
- Friday: 15.33%
- Thursday: 12.17%
- Monday: 11.05%
- Tuesday: 9.70%
- Sunday: 9.09%

Mid-week and parts of the weekend show elevated fraud risk compared to early weekdays and Sunday.

---

## Rule-Based Risk Scoring Design

The project uses a transparent rule engine rather than a black-box model. The final risk score is derived by combining multiple business-driven signals:

- Transaction amount range
- Transaction time window (late night, shoulder hours, daytime)
- Day-of-week risk grouping
- Category risk level (high, medium, low)
- Merchant risk level (watchlisted merchants vs others)

Each signal is mapped to a predefined number of points, and all points are summed to generate a single risk_score. The score is then mapped into Low, Medium, and High risk bands.

The resulting band-wise fraud rates of approximately 2.69%, 18.42%, and 68.92% demonstrate strong and meaningful risk separation.

---

## Power BI Dashboard

The primary deliverable of this project is an interactive Power BI dashboard that summarizes fraud risk across key dimensions.

The dashboard includes:
- KPI cards for total transactions, total frauds, overall fraud rate, and average fraud amount
- Fraud rate by risk band (Low, Medium, High)
- Fraud rate by category
- Fraud rate by amount range
- Fraud rate by day of week
- Category slicer to filter the entire dashboard and analyze category-specific risk patterns

The layout is designed to support quick decision-making and clear communication of fraud risk.

---

## Business Takeaways

- Fraud is highly concentrated in medium-range transaction amounts and a limited set of online and card-not-present categories
- The rule-based scoring system successfully concentrates risk into Medium and High bands, with fraud rates jumping from ~2.7% in the Low band to nearly 69% in the High band
- The explainable nature of the score makes it suitable for real-time alerts, manual review prioritization, and as an engineered feature for future machine learning models

