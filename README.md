# 🛒 Customer Shopping Behavior Analysis

> **An end-to-end data analytics project** that explores customer shopping patterns using **Python**, **SQL (PostgreSQL)**, and **Power BI** — from raw data cleaning to interactive dashboards and actionable business insights.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Tech Stack](#-tech-stack)
- [Dataset Description](#-dataset-description)
- [Project Architecture](#-project-architecture)
- [Analysis Pipeline](#-analysis-pipeline)
  - [Data Cleaning & Feature Engineering (Python)](#1-data-cleaning--feature-engineering-python)
  - [Exploratory SQL Analysis (PostgreSQL)](#2-exploratory-sql-analysis-postgresql)
  - [Interactive Dashboard (Power BI)](#3-interactive-dashboard-power-bi)
- [Key SQL Queries & Business Questions](#-key-sql-queries--business-questions)
- [Key Insights & Findings](#-key-insights--findings)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Future Scope](#-future-scope)
- [Author](#-author)

---

## 🔍 Project Overview

This project performs a **comprehensive analysis of customer shopping behavior** across 3,900 transactions to uncover purchasing patterns, demographic trends, and actionable insights for retail strategy. The analysis spans the complete data analytics lifecycle:

1. **Data Wrangling** — Cleaning, transforming, and enriching raw transactional data using Python (Pandas).
2. **Database Integration** — Loading the cleaned dataset into PostgreSQL (with support for MySQL and MS SQL Server) for structured querying.
3. **SQL-Based Analysis** — Answering 10 business-critical questions using advanced SQL (CTEs, window functions, aggregations).
4. **Dashboard Visualization** — Building an interactive Power BI dashboard for stakeholder-ready visual storytelling.

---

## 💼 Business Problem

A retail company wants to understand its customer base better in order to:

- Identify **high-value customer segments** and their purchasing behaviors
- Evaluate the **impact of discounts and promotions** on spending
- Understand **seasonal and demographic trends** in product categories
- Assess **subscription and loyalty program effectiveness**
- Optimize **shipping, payment, and marketing strategies**

The full business problem statement is documented in [`Business Problem Document.pdf`](Business%20Problem%20%20Document.pdf).

---

## 🛠 Tech Stack

| Tool / Technology | Purpose |
|---|---|
| **Python 3.9+** | Data cleaning, transformation, feature engineering |
| **Pandas** | DataFrame manipulation and analysis |
| **SQLAlchemy** | ORM-based database connection (Python ↔ SQL) |
| **PostgreSQL** | Primary relational database for SQL analysis |
| **SQL** | Business queries (aggregations, CTEs, window functions) |
| **Power BI** | Interactive dashboard and data visualization |
| **Jupyter Notebook** | Development environment for Python analysis |

---

## 📊 Dataset Description

**Source**: `customer_shopping_behavior.csv`  
**Records**: 3,900 transactions  
**Columns**: 18 features  

| # | Column | Type | Description |
|---|---|---|---|
| 1 | `Customer ID` | Integer | Unique customer identifier (1–3,900) |
| 2 | `Age` | Integer | Customer age (18–70 years) |
| 3 | `Gender` | Categorical | Male / Female |
| 4 | `Item Purchased` | Categorical | Product name (25 unique items) |
| 5 | `Category` | Categorical | Product category — Clothing, Footwear, Accessories, Outerwear |
| 6 | `Purchase Amount (USD)` | Integer | Transaction value ($20–$100) |
| 7 | `Location` | Categorical | U.S. state (50 states) |
| 8 | `Size` | Categorical | S, M, L, XL |
| 9 | `Color` | Categorical | Product color (25 colors) |
| 10 | `Season` | Categorical | Spring, Summer, Fall, Winter |
| 11 | `Review Rating` | Float | Customer rating (2.5–5.0) |
| 12 | `Subscription Status` | Categorical | Yes / No |
| 13 | `Shipping Type` | Categorical | Free Shipping, Standard, Express, Next Day Air, 2-Day Shipping, Store Pickup |
| 14 | `Discount Applied` | Categorical | Yes / No |
| 15 | `Promo Code Used` | Categorical | Yes / No |
| 16 | `Previous Purchases` | Integer | Count of past purchases (1–50) |
| 17 | `Payment Method` | Categorical | Credit Card, PayPal, Cash, Debit Card, Bank Transfer, Venmo |
| 18 | `Frequency of Purchases` | Categorical | Weekly, Fortnightly, Monthly, Quarterly, Bi-Weekly, Every 3 Months, Annually |

### Key Statistics

| Metric | Value |
|---|---|
| Average Age | 44.1 years |
| Average Purchase Amount | $59.76 |
| Average Review Rating | 3.75 / 5.0 |
| Average Previous Purchases | 25.4 |
| Male-to-Female Ratio | ~68% Male |
| Missing Values | 37 (in Review Rating only) |

---

## 🏗 Project Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│                 │     │                  │     │                  │
│   Raw CSV Data  │────▶│  Python (Pandas) │────▶│   PostgreSQL DB  │
│   (3,900 rows)  │     │  Clean + Enrich  │     │  SQL Queries     │
│                 │     │                  │     │                  │
└─────────────────┘     └──────────────────┘     └────────┬─────────┘
                                                          │
                                                          ▼
                                                ┌──────────────────┐
                                                │                  │
                                                │   Power BI       │
                                                │   Dashboard      │
                                                │                  │
                                                └──────────────────┘
```

---

## 🔬 Analysis Pipeline

### 1. Data Cleaning & Feature Engineering (Python)

Performed in [`Customer_Shopping_Behavior_Analysis.ipynb`](Customer_Shopping_Behavior_Analysis.ipynb):

| Step | Action | Details |
|---|---|---|
| **Load Data** | Read CSV into Pandas DataFrame | 3,900 rows × 18 columns |
| **Inspect Data** | `.info()`, `.describe()`, null checks | Identified 37 missing values in `Review Rating` |
| **Handle Missing Values** | Median imputation by category | `groupby('Category').transform(lambda x: x.fillna(x.median()))` |
| **Rename Columns** | Convert to snake_case | E.g., `Purchase Amount (USD)` → `purchase_amount` |
| **Feature: Age Group** | Quartile-based segmentation | Young Adult, Adult, Middle-aged, Senior |
| **Feature: Purchase Frequency Days** | Map text to numeric days | Weekly→7, Fortnightly→14, Monthly→30, Quarterly→90, Annually→365 |
| **Drop Redundant Column** | Remove `promo_code_used` | 100% identical to `discount_applied` |
| **Database Export** | Load into PostgreSQL via SQLAlchemy | Also includes MySQL and MS SQL Server connection code |

### 2. Exploratory SQL Analysis (PostgreSQL)

Performed in [`customer_behavior_sql_queries.sql`](customer_behavior_sql_queries.sql) — 10 business-driven queries covering:

- Revenue analysis by gender
- Discount effectiveness
- Product ratings
- Shipping type comparison
- Subscription impact
- Customer segmentation (New / Returning / Loyal)
- Category-level product ranking (window functions)
- Repeat buyer subscription correlation
- Age group revenue contribution

### 3. Interactive Dashboard (Power BI)

Built in [`customer_behavior_dashboard.pbix`](customer_behavior_dashboard.pbix):

- Revenue breakdowns by demographics, category, and season
- Customer segmentation visuals
- Discount & subscription impact analysis
- Geographic distribution of purchases
- KPI cards for key business metrics

The full presentation with dashboard screenshots and insights is available in [`Customer-Shopping-Behavior-Analysis.pptx`](Customer-Shopping-Behavior-Analysis.pptx).

---

## 📝 Key SQL Queries & Business Questions

| # | Business Question | SQL Technique |
|---|---|---|
| Q1 | Total revenue by gender | `GROUP BY` + `SUM()` |
| Q2 | Discount users spending above average | Subquery with `AVG()` |
| Q3 | Top 5 products by average review rating | `GROUP BY` + `ORDER BY DESC` + `LIMIT` |
| Q4 | Average purchase: Standard vs Express shipping | Filtered `GROUP BY` |
| Q5 | Subscriber vs non-subscriber spend comparison | Multi-aggregate `GROUP BY` |
| Q6 | Top 5 products by discount usage rate | `CASE WHEN` + percentage calc |
| Q7 | Customer segmentation (New/Returning/Loyal) | `CTE` + `CASE WHEN` |
| Q8 | Top 3 products per category | `CTE` + `ROW_NUMBER() OVER (PARTITION BY ...)` |
| Q9 | Repeat buyers vs subscription correlation | Filtered `GROUP BY` |
| Q10 | Revenue contribution by age group | `GROUP BY` + `ORDER BY` |

<details>
<summary>📄 Click to view a sample query (Q8 — Top 3 Products per Category)</summary>

```sql
WITH item_counts AS (
    SELECT category,
           item_purchased,
           COUNT(customer_id) AS total_orders,
           ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

</details>

---

## 💡 Key Insights & Findings

- **Gender Split**: Male customers represent ~68% of the customer base and contribute the majority of revenue.
- **Discounts**: Customers who used discounts still spend above the average purchase amount, suggesting discounts drive volume without eroding value.
- **Subscriptions**: Non-subscribers significantly outnumber subscribers (~73% vs ~27%), but the average spend between both groups is comparable — indicating untapped upsell potential.
- **Loyalty**: Customers are segmented into New (1 purchase), Returning (2–10), and Loyal (11+) — with loyal customers forming the largest segment.
- **Seasonal Trends**: Spring leads in transaction volume, suggesting seasonal campaign opportunities.
- **Shipping Preference**: Free Shipping is the most popular shipping method, followed by Store Pickup.
- **Payment Methods**: PayPal is the most used payment method across all segments.
- **Product Categories**: Clothing dominates with ~45% of all transactions, followed by Accessories.

---

## 📁 Project Structure

```
customer-trends-data-analysis-SQL-Python-PowerBI/
│
├── 📄 README.md                                    # Project documentation (this file)
├── 📊 customer_shopping_behavior.csv               # Raw dataset (3,900 records × 18 columns)
├── 📓 Customer_Shopping_Behavior_Analysis.ipynb     # Python notebook — cleaning & feature engineering
├── 🗃️ customer_behavior_sql_queries.sql             # 10 SQL business queries (PostgreSQL)
├── 📈 customer_behavior_dashboard.pbix              # Power BI interactive dashboard
├── 📑 Customer Shopping Behavior Analysis.pdf       # Analysis report (PDF)
├── 📑 Customer-Shopping-Behavior-Analysis.pptx      # Presentation with insights & dashboard screenshots
└── 📄 Business Problem Document.pdf                 # Business problem statement
```

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.9+** with `pandas`, `sqlalchemy`
- **PostgreSQL** (or MySQL / MS SQL Server)
- **Power BI Desktop** (for `.pbix` dashboard)
- **Jupyter Notebook** (or VS Code with Jupyter extension)

### Installation & Setup

```bash
# 1. Clone the repository
git clone https://github.com/your-username/customer-trends-data-analysis-SQL-Python-PowerBI.git
cd customer-trends-data-analysis-SQL-Python-PowerBI

# 2. Install Python dependencies
pip install pandas sqlalchemy psycopg2-binary

# 3. Create the PostgreSQL database
# In pgAdmin or psql:
CREATE DATABASE customer_behavior;

# 4. Open and run the Jupyter Notebook
jupyter notebook Customer_Shopping_Behavior_Analysis.ipynb
# → This will clean the data and load it into PostgreSQL

# 5. Run SQL queries
# Open customer_behavior_sql_queries.sql in pgAdmin or any SQL client

# 6. Open the Power BI dashboard
# Open customer_behavior_dashboard.pbix in Power BI Desktop
```

### Database Alternatives

The notebook includes ready-to-use connection code for:

| Database | Driver | Default Port |
|---|---|---|
| **PostgreSQL** | `psycopg2` | 5432 |
| **MySQL** | `pymysql` | 3306 |
| **MS SQL Server** | `pyodbc` | 1433 |

> ⚠️ **Note**: Update the `username`, `password`, `host`, `port`, and `database` variables in the notebook before running.

---

## 🔮 Future Scope

- **Predictive Modeling**: Build ML models (e.g., customer churn prediction, purchase amount forecasting) using scikit-learn
- **RFM Analysis**: Implement Recency-Frequency-Monetary segmentation for targeted marketing
- **A/B Testing Framework**: Evaluate discount strategies with statistical significance testing
- **Real-Time Dashboard**: Migrate to a cloud-based solution (e.g., Azure SQL + Power BI Service) for live updates
- **Sentiment Analysis**: Incorporate NLP on review text data for deeper customer feedback insights
- **Geographic Deep Dive**: State-level purchasing pattern analysis with map visualizations

---

## 👤 Author

**Tunir**

If you found this project helpful, please ⭐ star this repository!

---

<p align="center">
  <strong>Built with</strong> 🐍 Python &nbsp;•&nbsp; 🐘 PostgreSQL &nbsp;•&nbsp; 📊 Power BI
</p>
