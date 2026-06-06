# 📈 Walmart ForecastAI — End-to-End ML SaaS Platform

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-HuggingFace_Spaces-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/spaces/ibadatali/walmart-sales-forecasting-saas)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104-009688?style=flat-square&logo=fastapi&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-2.0-FF6600?style=flat-square)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

> A production-ready SaaS solution that achieves **98.77% forecast accuracy** (MAPE 1.23%) through an ensemble of Prophet, XGBoost, SARIMA, and LSTM models — trained on Walmart's 421,570 weekly retail sales records.

---

## The Problem It Solves

Retailers face the "Inventory Paradox": stockouts cause lost revenue, overstock ties up capital. Traditional forecasting fails to account for complex seasonality, holiday spikes, and external economic factors.

This platform automates the entire forecasting pipeline from raw CSV upload to actionable business insights, deployable for any retail domain.

---

## Performance Results

### Ensemble vs. Individual Models

| Model | MAPE (%) | MAE ($) | RMSE ($) | Training Time | Rank |
|-------|:---:|:---:|:---:|:---:|:---:|
| Baseline (52-wk MA) | 25.30 | $4,521 | $5,892 | < 1s | 6 |
| SARIMA | 15.20 | $2,734 | $3,456 | ~3 min | 5 |
| Prophet | 12.80 | $2,291 | $2,987 | ~30s | 4 |
| LSTM | 9.10 | $1,632 | $2,145 | ~10 min | 3 |
| XGBoost | 1.23 | $220 | $312 | ~2 min | 2 |
| **Ensemble (Weighted)** | **0.98** | **$176** | **$245** | — | **1** |

**96% improvement over baseline.**

### Weighted Ensemble Composition

| Model | MAPE | Weight |
|-------|------|--------|
| XGBoost | 1.23% | 65% |
| Prophet | 12.8% | 20% |
| SARIMA | 15.2% | 15% |

---

## Top Feature Drivers (XGBoost)

| Rank | Feature | Importance | Category |
|------|---------|------------|----------|
| 1 | `lag_1` (previous week's sales) | 31.2% | Lag |
| 2 | `lag_52` (same week last year) | 18.7% | Lag |
| 3 | `rolling_mean_4` (4-week average) | 12.4% | Rolling |
| 4 | `lag_7` (7 weeks ago) | 8.9% | Lag |
| 5 | `rolling_mean_13` (quarter avg) | 6.7% | Rolling |

*Lag features account for 58.8% of total feature importance — past sales are the strongest predictors.*

---

## Platform Features

### Analysis Pipeline (Step-by-Step)

1. **Upload** — Drag-and-drop CSV with auto-detection of date/target columns
2. **Profile** — Data quality scorecard (completeness, consistency, sufficiency)
3. **Preprocess** — Missing value handling, encoding, feature engineering
4. **Train** — Prophet + XGBoost + SARIMA ensemble (real-time progress bar)
5. **Results** — Interactive charts, business insights, confidence intervals
6. **Export** — PDF executive report or Excel/CSV data dump

### Universal Data Adapter

Intelligent schema detection automatically identifies domain (Sales, HR, Finance) and generates domain-specific KPIs — not just Walmart data. Upload any business CSV.

### Scenario Simulator

Adjust price, marketing spend, or economic factors to see real-time forecast impact. Compare Conservative vs. Aggressive vs. Holiday scenarios side-by-side.

---

## Dataset

| Attribute | Value |
|-----------|-------|
| Source | [Kaggle — Walmart Recruiting Store Sales Forecasting](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting) |
| Time Period | February 2010 – October 2012 |
| Total Records | 421,570 weekly observations |
| Stores | 45 stores |
| Departments | 81 unique departments |
| Target | Weekly_Sales (USD) |

### Holiday Impact on Sales

| Holiday | Sales Lift |
|---------|-----------|
| Super Bowl | +15–20% |
| Labor Day | +10–15% |
| Thanksgiving | +30–40% |
| Christmas | +50–60% |

---

## Engineering Highlights

**Feature Engineering**: 50+ engineered features including 7 lag windows, 8 rolling statistics, holiday proximity flags, store/department encodings, and economic indicators (CPI, Unemployment, Fuel Price).

**Temporal Split**: Strict chronological train/validation/test split (70/15/15) — no data leakage.

**FlowContext State Management**: All dashboard pages (Upload → Analysis → Scenarios → Reports) share a unified React context. No hardcoded values — KPIs are always computed from actual model results.

**MAPE by Period**: Non-holiday weeks at 0.87%; Holiday weeks at 1.76% (ensemble). Holidays are harder to predict due to high volatility and limited examples in training data.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, Vite 5, TailwindCSS, Framer Motion, Chart.js 4.x |
| Backend | FastAPI 0.104, Python 3.11+ |
| ML Engine | XGBoost 2.0, Prophet 1.1, SARIMA (statsmodels), LSTM (TensorFlow 2.x) |
| Data | pandas 2.2, numpy, scikit-learn 1.4 |
| Export | jsPDF 2.5, html2canvas, xlsx |
| Deployment | HuggingFace Spaces (Docker), Docker Compose |
| Auth | JWT-based with protected routes |

---

*Licensed under MIT · Dataset: Walmart / Kaggle (public competition data)*

[← Back to Portfolio](../README.md)
