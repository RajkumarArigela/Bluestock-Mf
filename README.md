# 📊 Bluestock Mutual Fund Analytics Platform

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

> **Data Analyst Internship — Cohort 2026 | Bluestock Fintech**
> End-to-end mutual fund data engineering and analytics platform built on real Fintech data.

---

## 🚀 Project Overview

This project is a complete **Mutual Fund Analytics Pipeline** built during the Bluestock Fintech Data Analyst Internship. It covers the full data lifecycle — from raw CSV ingestion to a cleaned SQLite database, performance analytics, risk metrics, and an interactive Power BI dashboard.

The dataset covers **40 mutual fund schemes** across major Indian AMCs (SBI, HDFC, ICICI, Mirae, Kotak, Axis) with **3+ years of daily NAV history**, **32,778 investor transactions**, and detailed performance metrics.

---

## 📁 Project Structure

```
Bluestock-Mf/
│
├── 📂 data/
│   ├── raw/                          # Original CSV files from AMFI
│   │   ├── 02_nav_history.csv
│   │   ├── 07_scheme_performance.csv
│   │   └── 08_investor_transactions.csv
│   │
│   └── processed/                    # Cleaned & engineered datasets
│       ├── nav_history_clean.csv
│       ├── investor_transactions_clean.csv
│       ├── scheme_performance_clean.csv
│       ├── fund_scorecard.csv
│       ├── alpha_beta.csv
│       └── full_performance_metrics.csv
│
├── 📂 sql/
│   ├── schema.sql                    # SQLite star schema DDL
│   └── queries.sql                   # 10 analytical SQL queries
│
├── 📂 notebooks/
│   ├── Day2_Data_Cleaning.ipynb      # Data cleaning pipeline
│   └── Performance_Analytics.ipynb   # Risk & return analytics
│
├── 📂 docs/
│   └── data_dictionary.md            # Column-level documentation
│
├── 📂 charts/                        # Generated visualizations
│   ├── benchmark_comparison.png
│   ├── chart3_sharpe_sortino.png
│   ├── chart4_alpha_beta.png
│   ├── chart5_max_drawdown.png
│   └── chart6_scorecard_heatmap.png
│
├── 🗄️ bluestock_mf.db               # SQLite database (star schema)
├── 📋 requirements.txt
└── 📖 README.md
```

---

## 📦 Dataset Summary

| Dataset | Rows | Description |
|---------|------|-------------|
| `nav_history` | 64,320 | Daily NAV prices (forward-filled for holidays) |
| `investor_transactions` | 32,778 | SIP, Lumpsum & Redemption records |
| `scheme_performance` | 40 | Returns, risk ratios, AUM, expense ratios |

**Coverage:** 40 schemes · 14 AMCs · Jan 2022 – May 2026 · 12 Indian states

---

## 🗄️ Database Schema (Star Schema)

```
                    ┌─────────────┐
                    │  dim_date   │
                    └──────┬──────┘
                           │ date_id
                    ┌──────┴──────┐
                    │  dim_fund   │
                    └──────┬──────┘
                           │ amfi_code
          ┌────────────────┼─────────────────┐
          │                │                 │
   ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐
   │  fact_nav   │  │fact_transact│  │fact_perform │
   └─────────────┘  └─────────────┘  └─────────────┘
                           │
                    ┌──────┴──────┐
                    │  fact_aum   │
                    └─────────────┘
```

---

## ⚙️ Day-by-Day Work Log

### ✅ Day 2 — Data Cleaning & SQLite Pipeline
- Parsed dates, forward-filled NAV for 18,320 holiday/weekend rows
- Standardised transaction types (SIP / Lumpsum / Redemption)
- Validated expense ratios within SEBI range (0.1%–2.5%)
- Built SQLite star schema with 6 tables, PKs, FKs, and indexes
- Loaded all data using `df.to_sql()` and verified row counts
- Wrote 10 analytical SQL queries
- Created full data dictionary

### ✅ Day 5 — Performance Analytics & Risk Metrics
- Computed daily returns: `daily_return = NAV_t / NAV_t-1 − 1`
- CAGR for 1yr, 3yr, full period using `(NAV_end/NAV_start)^(1/n) − 1`
- Sharpe Ratio: `(Rp − Rf) / σp × √252` with Rf = 6.5%
- Sortino Ratio using downside deviation only
- Alpha & Beta via OLS regression (`scipy.stats.linregress`)
- Maximum Drawdown: `min(NAV / running_max − 1)` with date range
- Fund Scorecard (0–100): composite of 5 weighted metrics
- Benchmark comparison chart + tracking error vs Nifty 100 proxy

---

## 🏆 Top 5 Funds — Composite Scorecard

| Rank | Fund | Score | 3yr CAGR | Sharpe | Alpha |
|------|------|-------|----------|--------|-------|
| 🥇 1 | ICICI Pru Midcap Fund | 85.5 | 31.78% | 1.18 | 30.74% |
| 🥈 2 | HDFC Mid-Cap Opportunities | 82.0 | 32.44% | 1.09 | 29.43% |
| 🥉 3 | Axis Midcap Fund | 81.75 | 35.11% | 1.00 | 24.73% |
| 4 | Kotak Flexicap Fund | 77.75 | 29.58% | 1.31 | 25.94% |
| 5 | Mirae Asset Large Cap | 76.0 | 34.00% | 1.45 | 13.44% |

> **Scorecard Formula:** 30% × 3yr Return + 25% × Sharpe + 20% × Alpha + 15% × Expense Ratio (inverse) + 10% × Max Drawdown (inverse)

---

## 📊 10 Analytical SQL Queries

| # | Query | Business Question |
|---|-------|-------------------|
| Q1 | Top 5 funds by AUM | Which funds are largest by assets? |
| Q2 | Avg NAV per month | How does the market NAV trend monthly? |
| Q3 | SIP YoY growth | How much has SIP investing grown year-over-year? |
| Q4 | Transactions by state | Which states have the most investor activity? |
| Q5 | Expense ratio < 1% | Which are the most cost-efficient funds? |
| Q6 | Top 5 by 1yr return | Best performing funds this year? |
| Q7 | Monthly txn volume by type | SIP vs Lumpsum vs Redemption trends? |
| Q8 | Fund vs benchmark | Which funds beat their benchmark? |
| Q9 | Age group demographics | Which age group invests the most? |
| Q10 | NAV volatility ranking | Which funds are most volatile? |

---

## 📈 Key Charts

| Chart | Description |
|-------|-------------|
| `benchmark_comparison.png` | Top 5 funds vs Nifty 100 proxy — 3yr normalised performance |
| `chart3_sharpe_sortino.png` | Sharpe & Sortino ratio rankings for all 40 funds |
| `chart4_alpha_beta.png` | Alpha vs Beta scatter — OLS regression results |
| `chart5_max_drawdown.png` | Max drawdown depth and duration per fund |
| `chart6_scorecard_heatmap.png` | Composite scorecard heatmap across all 40 funds |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Python 3.10+** | Core language |
| **Pandas** | Data cleaning, transformation, EDA |
| **NumPy** | Numerical computations |
| **SciPy** | OLS regression (Alpha/Beta) |
| **Matplotlib & Seaborn** | Data visualisation |
| **SQLite + sqlite3** | Database storage |
| **Jupyter Notebook** | Interactive analysis |
| **Power BI Desktop** | Dashboard & reporting |
| **Git & GitHub** | Version control |

---

## 🚀 How to Run This Project

### 1. Clone the Repository
```bash
git clone https://github.com/RajkumarArigela/Bluestock-Mf.git
cd Bluestock-Mf
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Run Data Cleaning (Day 2)
Open `notebooks/Day2_Data_Cleaning.ipynb` in Jupyter or Google Colab.
Upload the 3 raw CSV files when prompted and run all cells.

### 4. Run Performance Analytics (Day 5)
Open `notebooks/Performance_Analytics.ipynb` and run all cells.
This generates `fund_scorecard.csv`, `alpha_beta.csv`, and all charts.

### 5. Explore the Database
```python
import sqlite3, pandas as pd

conn = sqlite3.connect('bluestock_mf.db')

# Top 5 funds by AUM
df = pd.read_sql("""
    SELECT f.scheme_name, f.category, a.aum_crore
    FROM fact_aum a
    JOIN dim_fund f ON a.amfi_code = f.amfi_code
    ORDER BY a.aum_crore DESC
    LIMIT 5
""", conn)
print(df)
```

---

## 📋 Requirements

```
pandas>=1.5.0
numpy>=1.23.0
scipy>=1.9.0
matplotlib>=3.6.0
seaborn>=0.12.0
jupyter>=1.0.0
```

---

## 📚 Domain Knowledge Applied

| Concept | Applied In |
|---------|-----------|
| **NAV** (Net Asset Value) | Daily price tracking, return calculation |
| **CAGR** | 1yr / 3yr / 5yr return comparison |
| **Sharpe Ratio** | Risk-adjusted return ranking |
| **Sortino Ratio** | Downside risk measurement |
| **Alpha & Beta** | Fund manager skill vs market |
| **Maximum Drawdown** | Worst-case loss analysis |
| **Expense Ratio** | SEBI compliance check (0.1%–2.5%) |
| **SIP** | Systematic Investment Plan trend analysis |
| **AMFI codes** | Unique fund identifiers across all tables |
| **T30/B30 cities** | Geographic investor segmentation |

---

## 👨‍💻 Author

**Raj Kumar Arigela**
Data Analyst Intern — Bluestock Fintech | Cohort 2026
B.Sc Computer Science — Girraj Government College, Nizamabad

[![GitHub](https://img.shields.io/badge/GitHub-RajkumarArigela-181717?style=flat&logo=github)](https://github.com/RajkumarArigela)

---

## 🏢 Internship

**Organization:** Bluestock Fintech
**Program:** Data Analyst Internship — Cohort 2025
**Domain:** Fintech | Financial Analytics | Data Engineering
**Duration:** 2 Weeks (Week 1: Learning + Capstone | Week 2: Evaluation)

---

*Built with ❤️ during the Bluestock Fintech Data Analyst Internship 2026*
