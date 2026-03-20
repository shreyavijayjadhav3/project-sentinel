# 🔍 Project Sentinel — E-Commerce Data Leak Investigation

> *A forensic data analysis project uncovering ghost orders, payment bypass exploits, and revenue leakage across a Brazilian e-commerce platform (2016–2018).*

---

## 📌 Overview

This project investigates a real-world financial anomaly in e-commerce transaction data — where orders were marked as **delivered** without proper payment authorization, resulting in significant revenue loss. The analysis spans the full data pipeline: from raw SQL queries to Python-based EDA and a final Power BI dashboard.

| Metric | Value |
|---|---|
| 🧾 Total Orders Analyzed | 99,000+ |
| 👻 Ghost Orders Detected | 586 (Power BI) / 1,037 (Python EDA) |
| 💸 Total Revenue Lost | R$ 80.86K – $165K |
| 📊 Fraud Rate | 0.59% |
| 🔇 Silent Victims | 61.26% |
| 📅 Breach Period | Jan 2017 – Aug 2018 |

---

## 🗂️ Project Structure

```
project-sentinel/
│
├── SQL_Query.sql                          # Fraud detection SQL queries
├── E-Commerce_Data_Leak_Project_DA.ipynb  # Python EDA & data cleaning notebook
├── Final_Dashboard.pdf                    # Exported Power BI dashboard
├── Sentinel_Master_Data.csv              # Cleaned master dataset (119K rows, 36 cols)
├── Sentinel_Ghost_Orders.csv             # Isolated ghost order records
└── README.md
```

---

## 🛠️ Tools & Technologies

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-EDA-green?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Analysis-blue?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-orange)
![Seaborn](https://img.shields.io/badge/Seaborn-Charts-teal)
![SQL](https://img.shields.io/badge/SQL-Fraud%20Queries-yellow?logo=postgresql)
![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-F2C811?logo=powerbi)

---

## 🔎 Phase 1 — SQL Fraud Detection

Four targeted queries were written to surface anomalies directly from the relational database.

**Query 1 — Identity Theft Detection**
Flags customers placing orders from multiple distinct ZIP code locations, a key indicator of account takeover or identity fraud.

**Query 2 — Ghost Delivery Detection**
Isolates orders with `order_status = 'delivered'` but a `NULL` value in `order_approved_at`, meaning goods were shipped and received without financial gateway approval.

**Query 3 — Value Leakage Analysis**
Detects orders where the actual payment was less than 80% of the listed product price, identifying systematic underpayment or coupon/voucher exploitation.

**Query 4 — Master Fraud Query**
A consolidated query joining customers, orders, items, and payments to surface high-risk customers showing both multi-location activity and suspected revenue loss exceeding R$100.

---

## 🐍 Phase 2 — Python EDA (Jupyter Notebook)

**Libraries used:** `pandas`, `numpy`, `matplotlib`, `seaborn`

**Datasets loaded:**

- `orders.csv` — order lifecycle & timestamps
- `order_payments.csv` — payment method & values
- `customers.csv` — customer geography
- `order_items.csv` — product pricing
- `products.csv` + `product_category_name_translation.csv` — product metadata
- `sellers.csv` — seller info
- `order_reviews.csv` — customer feedback & star ratings

**Key steps performed:**

1. Data loading and schema inspection across 8 CSVs
2. Merging datasets into a unified `master_df` (119,143 rows × 36 columns)
3. Null handling, datetime parsing, and type correction
4. Feature engineering — `revenue_leak`, `is_customer_silent`, `process_gap`, `month_year`
5. Ghost order isolation into a separate `ghost_orders` DataFrame
6. EDA visualizations including breach timeline, category breakdown, and fraud success rate
7. Export to `Sentinel_Master_Data.csv` and `Sentinel_Ghost_Orders.csv` for Power BI ingestion

**Final Python Dashboard — "Project Sentinel: The $165,000 Ghost Order Audit"**

A 4-panel matplotlib figure summarizing:
- Breach Heartbeat (14-month exploitation timeline)
- Top 10 Victim Categories by volume
- Fraud Success Rate via delivery-confirmed review scores
- Executive Summary text block with key findings

---

## 📊 Phase 3 — Power BI Dashboard

Two report pages built from the exported CSVs.

### Page 1 — E-Commerce Data Leak Overview

| Visual | Insight |
|---|---|
| Ghost Order Breach Timeline | Peak in mid-2017, second wave in early 2018 |
| Top 10 Victim Categories | `bed_bath_table` most targeted (80 orders) |
| Payment Method Exploitation | 61.77% via credit card, 38.23% via voucher |
| Silent Rate Donut | 61.26% of victims never complained |
| Revenue Leak by Category & Payment | `watches_gifts` and `bed_bath_table` top revenue losses |

### Page 2 — Full Platform Analysis (Project Sentinel)

| Visual | Insight |
|---|---|
| Platform Order Growth Trend | Monthly orders across 2016–2018 |
| Product-wise Average Payment Value | Computers & fixed telephony highest avg. spend |
| Payment Type Distribution | 75% credit card, 19% boleto |
| Top 10 Categories by Total Revenue | `bed_bath_table` and `health_beauty` lead at R$1.7M each |

---

## 💡 Key Findings

- **Ghost orders bypass the financial gateway** — goods are delivered without `order_approved_at` being recorded, exploiting a gap in the standard fulfillment process.
- **Credit cards and vouchers are the primary attack vectors**, accounting for 100% of ghost order payment types.
- **61.26% of affected customers never raised a complaint**, making this a largely silent exploit that went undetected without data-level investigation.
- **`bed_bath_table` is the most targeted category** by volume; `watches_gifts` leads in revenue loss.
- The breach ran for approximately **14 months**, with the highest spike occurring around **May 2017**.

---

## 📁 Dataset

This project uses the [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) available on Kaggle.

> The dataset contains anonymized information on 100k orders from 2016 to 2018 across multiple Brazilian marketplaces.

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/your-username/project-sentinel.git
cd project-sentinel

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Launch the notebook
jupyter notebook "E-Commerce_Data_Leak_Project_DA.ipynb"

# 4. Run SQL queries against your local DB or a tool like DB Browser for SQLite / PostgreSQL
```

---

## 👤 Author

**Shreya Jadhav**
Data Analyst | Passionate about uncovering stories hidden in messy data.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/jadhavshreya03pune/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?logo=github)](https://github.com/your-username)

---

*Project Sentinel — Because the most dangerous fraud is the kind nobody notices.*
