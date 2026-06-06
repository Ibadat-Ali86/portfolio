# 📈 ForecastAI — Retail Demand Forecasting SaaS Platform

[![Live Demo](https://img.shields.io/badge/Live_Demo-HuggingFace_Spaces-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/spaces/ibadatali/walmart-sales-forecasting-saas)
![Accuracy](https://img.shields.io/badge/Forecast_Accuracy-98.77%25_MAPE_0.98%25-22c55e?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

---

## 🎯 Business Problem

**Inaccurate demand forecasting costs retailers 3–4% of annual revenue.**

For a retailer doing $500M in annual sales, that's $15–20M in preventable losses — split between:

- **Stockouts:** Lost sales + customer churn when popular items run out (especially during holidays)
- **Overstock:** Capital tied up in slow-moving inventory + markdown losses to clear it
- **Supply chain inefficiency:** Last-minute expedited shipping when forecasts miss

Traditional methods fail because they use simple moving averages that can't capture:
- **Multi-level seasonality:** Weekly patterns nested inside yearly cycles
- **Holiday spikes:** Thanksgiving causes a 30–60% sales surge that averages don't anticipate
- **External signals:** CPI changes, unemployment shifts, and fuel prices all affect demand
- **Store heterogeneity:** A 200,000 sq ft Type A store and a 40,000 sq ft Type C store don't follow the same patterns

---

## 💡 Solution Approach

A **production-grade SaaS platform** that trains a 4-model weighted ensemble on any retail CSV — not just Walmart data — and surfaces forecasts, confidence intervals, business narratives, and scenario simulations through an interactive dashboard.

The key insight: **no single model wins across all patterns.** XGBoost excels at tabular feature interactions; Prophet handles seasonality and holidays natively; SARIMA captures autocorrelation; LSTM learns long-range temporal dependencies. The ensemble captures what each misses.

---

## 📊 Performance Results

### Model Comparison: 421,570 Weekly Walmart Sales Records

| Model | MAPE ↓ | MAE ($) ↓ | RMSE ($) ↓ | Baseline Improvement |
|-------|:---:|:---:|:---:|:---:|
| Baseline (52-week MA) | 25.30% | $4,521 | $5,892 | — |
| SARIMA | 15.20% | $2,734 | $3,456 | 40% better |
| Prophet | 12.80% | $2,291 | $2,987 | 49% better |
| LSTM | 9.10% | $1,632 | $2,145 | 64% better |
| XGBoost | 1.23% | $220 | $312 | 95% better |
| **Weighted Ensemble** | **0.98%** | **$176** | **$245** | **96% better** |

### Ensemble Weighting (Inverse-MAPE)

| Model | MAPE | Weight |
|-------|------|--------|
| XGBoost | 1.23% | **65%** |
| Prophet | 12.8% | **20%** |
| SARIMA | 15.2% | **15%** |

### Error by Period (Ensemble)

| Period | MAPE |
|--------|------|
| Standard weeks | 0.87% |
| Holiday weeks | 1.76% |
| Type A stores (large) | 1.12% |
| Type B stores (medium) | 0.92% |
| Type C stores (small) | 0.84% |

---

## 🔬 Feature Engineering (50+ Engineered Features)

### Temporal Features
`week_of_year` · `month` · `quarter` · `is_month_start/end` · `is_quarter_start/end` · `is_year_start/end`

### Lag Features (Autoregressive)
| Feature | Window | Purpose |
|---------|--------|---------|
| `lag_1` | t-1 | Last week's sales |
| `lag_52` | t-52 | Same week last year — **#2 most important feature** |
| `lag_7`, `lag_13`, `lag_26` | Various | Quarterly/semi-annual patterns |

### Rolling Window Features
| Feature | Window | Purpose |
|---------|--------|---------|
| `rolling_mean_4` | 4 weeks | Short-term trend |
| `rolling_mean_52` | 52 weeks | Yearly moving average |
| `rolling_std_4`, `rolling_std_13` | Volatility | Risk signals |

### External Economic Features
- `CPI_change` — Inflation rate (forward fill for gaps)
- `Unemployment_change` — Employment trend
- `Fuel_Price_normalized` — Regional price sensitivity

### Top Feature Importance (XGBoost)

| Rank | Feature | Importance | Category |
|------|---------|:---:|:---:|
| 1 | `lag_1` (previous week) | 31.2% | Lag |
| 2 | `lag_52` (same week last year) | 18.7% | Lag |
| 3 | `rolling_mean_4` | 12.4% | Rolling |
| 4 | `lag_7` | 8.9% | Lag |
| 5 | `rolling_mean_13` | 6.7% | Rolling |

**Lag features account for 58.8% of total importance** — past sales are overwhelmingly the best predictor of future sales.

---

## 🏗️ Platform Architecture

```
User
  ↓ Upload CSV (any retail domain)
Frontend (React 19 + Vite 5)
  - Auto-detection of date/target/feature columns
  - Column mapping modal
  ↓ POST /api/analysis/upload
FastAPI Backend
  - Session management (UUID-based)
  - Data validation (Pydantic v2)
  ↓ Pipeline Steps
  1. Data Profiling
     - Quality scorecard: completeness, consistency, sufficiency
     - Missing value heatmap
  2. Preprocessing
     - Missing value imputation strategy by column type
     - Categorical encoding
     - Feature engineering (50+ features auto-generated)
  3. Model Training (background task)
     ┌──────────────┐  ┌──────────────┐  ┌────────────┐  ┌────────┐
     │    Prophet   │  │   XGBoost    │  │   SARIMA   │  │  LSTM  │
     └──────┬───────┘  └──────┬───────┘  └─────┬──────┘  └───┬────┘
            └─────────────────┴────────────┬────┘             │
                                           ↓
                              Weighted Ensemble
                              (inverse-MAPE weights)
  4. Results
     - Forecast vs. actual chart (Recharts)
     - Confidence intervals (95% CI)
     - Business narrative (auto-generated)
     - Action recommendations
  5. Scenario Simulator
     - Adjust markdown, promotions, holidays
     - Compare Conservative / Aggressive / Holiday presets
  6. Export
     - PDF executive report
     - Excel multi-sheet workbook
     - Raw CSV forecast data
```

---

## 🔄 FlowContext State Architecture

All platform pages share a unified `FlowContext` — upload data flows through profiling, preprocessing, training, and into every downstream page (scenarios, reports, dashboard) without re-querying.

**This is by design:** No hardcoded demo values anywhere in the production platform. Every KPI, every chart, every recommendation is computed from the user's actual uploaded data.

---

## 📦 Dataset

| Attribute | Value |
|-----------|-------|
| Competition | [Walmart Recruiting — Store Sales Forecasting (Kaggle)](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting) |
| Period | Feb 2010 – Oct 2012 |
| Records | 421,570 weekly observations |
| Stores | 45 stores (Types A, B, C) |
| Departments | 81 unique departments |
| Split | 70% train / 15% val / 15% test (chronological — no data leakage) |

---

## 🛠️ Full Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, Vite 5, TailwindCSS 3.4, Framer Motion 12, Chart.js 4.x |
| State | FlowContext (React Context + useReducer) |
| Backend | FastAPI 0.104, Python 3.11+ |
| ML | XGBoost 2.0, Prophet 1.1, statsmodels SARIMAX, TensorFlow 2.x LSTM |
| Data | pandas 2.2, numpy, scikit-learn 1.4 |
| Export | jsPDF 2.5, html2canvas, xlsx |
| Auth | JWT-based with protected routes |
| Deployment | HuggingFace Spaces (Docker), Docker Compose |

---

## 🏭 Industry Context

ForecastAI targets the **Retail Analytics** and **Supply Chain Intelligence** market:

- **Enterprise buyers:** SAP IBP, Oracle Demand Management, Blue Yonder are the incumbents — priced at $200K–$2M/year for enterprise licenses
- **Mid-market gap:** Growing retailers can't afford SAP but need better than Excel. ForecastAI fills this gap with a deployable SaaS model
- **Differentiators:** Ensemble transparency, scenario simulation, domain-agnostic CSV upload, open-source deployability

Competing tools: Relex Solutions, Anaplan, Forecast Pro.

---

*MIT License · Stack: React 19 · FastAPI · XGBoost · Prophet · LSTM · Docker · HuggingFace Spaces*

[← Back to Portfolio](../README.md)
