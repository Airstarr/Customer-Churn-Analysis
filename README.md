# Customer Churn Analysis — E-commerce Retail

## Business Problem
An online retailer is acquiring new customers but revenue has plateaued. This analysis investigates why customers are not returning after their first purchase, identifies the highest-risk segments, and quantifies the revenue opportunity from a targeted win-back campaign.

---

## Key Finding

> **One-time buyers churn at 56.7% — nearly 3× the rate of repeat buyers (21.2%).**
> Getting a customer to place a second order is the single highest-leverage retention action available to this business.

330 high-value customers in the At Risk and High Value Lapsed segments have already churned, representing £1,191,259 in historical revenue. A targeted win-back campaign recovering just 15% of this group would generate an estimated **£53,410 in recovered revenue.**

---

## Dashboard Preview

> 📊 [View Live Power BI Dashboard](https://app.powerbi.com/links/ukiPWNwphw?ctid=54198da2-892d-44f3-b505-cd2cd2dbafcb&pbi_source=linkShare)

![Dashboard](dashboard_screenshot.png)

---

## Dataset

| Detail | Value |
|---|---|
| Source | [UCI Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online+retail) |
| Period | December 2010 – December 2011 |
| Raw rows | 541,909 |
| Customers analysed | 4,338 |
| Total revenue | £8,887,209 |

---

## Methodology

### 1. Data Cleaning
- Removed 135,080 rows with no CustomerID (guest checkouts)
- Removed cancellation invoices (InvoiceNo starting with 'C')
- Removed rows with zero or negative unit prices
- Dropped exact duplicate rows
- **Rows after cleaning:** ~390,000

### 2. Churn Definition
Churn was defined as a customer who had not placed any order in the **90 days prior to the reference date (2011-12-10).** This definition was chosen because the dataset covers a 12-month period and 90 days represents a meaningful period of inactivity for a retail business with monthly purchase cycles.

### 3. Feature Engineering
Built a customer-level table with one row per customer containing:
- `first_purchase` / `last_purchase` dates
- `total_orders` — distinct invoice count (not row count)
- `total_revenue` — sum of Quantity × UnitPrice
- `days_since_last` — days between last purchase and reference date
- `is_churned` — 1 if days_since_last > 90, else 0
- `avg_order_value` — total revenue ÷ total orders

### 4. RFM Segmentation
Scored each customer 1–4 on Recency, Frequency, and Monetary value using quartile-based scoring. Combined scores into six business segments: Champions, Loyal Customers, Promising, At Risk, High Value Lapsed, and Lost.

---

## Results

### Headline KPIs

| Metric | Value |
|---|---|
| Total customers | 4,338 |
| Overall churn rate | 33.4% |
| Repeat purchase rate | 65.6% |
| Median customer LTV | £669 |
| Mean customer LTV | £2,049 |

> Note: Mean LTV is 3× higher than median, indicating a small group of high-spending wholesale buyers skews the average. Median is the more representative figure for the typical customer.

### One-time vs Repeat Buyers

| Segment | Churn Rate | Median LTV |
|---|---|---|
| One-time buyers | 56.7% | £256 |
| Repeat buyers | 21.2% | £1,154 |

A repeat buyer has a median LTV **4.5× higher** than a one-time buyer. Every unconverted first-time customer represents approximately £898 in unrealised lifetime value.

### RFM Segment Summary

| Segment | Customers | Churn Rate | Median LTV | Total Revenue |
|---|---|---|---|---|
| Champions | 482 | 0.0% | £3,970 | £4,406,718 |
| Loyal customers | 1,037 | 0.0% | £1,258 | £2,170,313 |
| At risk | 469 | 36.2% | £1,220 | £774,876 |
| Lost | 1,426 | 78.5% | £296 | £624,188 |
| Promising | 657 | 0.0% | £353 | £494,731 |
| High value lapsed | 267 | 59.9% | £978 | £416,383 |

### Win-back Opportunity

| Recovery Rate | Customers Recovered | Estimated Revenue |
|---|---|---|
| 10% | 33 | £35,970 |
| 15% | 49 | £53,410 |
| 20% | 66 | £71,940 |

---

## Recommendation

**Launch a 30-day post-purchase win-back email campaign targeting first-time buyers before they go cold.**

The data shows the critical window is within 30 days of a first purchase. The campaign should be automated, triggered by first-purchase completion, and include a personalised product recommendation with a time-limited incentive.

**Priority audience for immediate recovery:** The 330 churned customers in the At Risk and High Value Lapsed segments. These customers had a median LTV of £1,090, averaged 2–4 orders, and had a genuine purchase history before going quiet.

---

## Tools Used

| Tool | Purpose |
|---|---|
| Python / pandas | Data cleaning, feature engineering, RFM segmentation |
| Matplotlib / Seaborn | Exploratory visualisations and distribution analysis |
| Power BI | Interactive dashboard and business presentation |
| Excel | Sanity checks on aggregated figures |

---

## Files in this Repository

| File | Description |
|---|---|
| `customer_churn_analysis.ipynb` | Full Python notebook — cleaning, analysis, segmentation |
| `customer_churn.csv` | Cleaned customer-level dataset |
| `segment_summary.csv` | RFM segment aggregations for Power BI |
| `winback_targets.csv` | 330 priority win-back customers |
| `README.md` | This file |

---

## How to Run

```bash
# Clone the repo
git clone https://github.com/Airstarr/customer-churn-analysis

# Install dependencies
pip install pandas numpy matplotlib seaborn openpyxl

# Download the dataset
# https://archive.ics.uci.edu/dataset/352/online+retail
# Place 'Online Retail.xlsx' in the project folder

# Open the notebook
jupyter notebook customer_churn_analysis.ipynb
```

---

*Analysis by [Ojo Esther](https://www.linkedin.com/in/estherojo-data)*
