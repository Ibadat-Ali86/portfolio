# 🛒 Pakistan E-commerce Unit Price Prediction

[![Kaggle Notebook](https://img.shields.io/badge/📊_Kaggle-View_Notebook-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/code/ibadatali/pakistan-ecommerce-price-reggression)
[![R² Score](https://img.shields.io/badge/R²_Score-0.92_(92%25)-success?style=for-the-badge)]()
![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3.2-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0.0-000000?style=flat-square&logo=flask&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

> A supervised Random Forest Regression model achieving **R² = 0.92** on Pakistan's largest e-commerce dataset (579,254 transactions), deployed as a production-ready Flask web application.

---

## The Problem

E-commerce businesses need accurate price predictions to:
- Automate pricing strategies for new products
- Detect pricing anomalies and data entry errors
- Optimize revenue through data-driven pricing
- Understand what actually drives price across product categories

---

## Model Performance

### Overall

| Metric | Value |
|--------|-------|
| **R² Score** | **0.92 (92%)** |
| MAE (log scale) | 0.255 |
| RMSE (log scale) | 0.500 |
| MAPE | 66.7% |

### Performance by Price Range

| Price Range | R² Score | Sample Count | Rating |
|------------|:---:|:---:|:---:|
| **High (PKR 10,000+)** | **0.90** | 18,107 | ⭐⭐⭐⭐⭐ Excellent |
| Medium (PKR 2,000–10,000) | 0.18 | 14,196 | ⭐⭐ Moderate |
| Low (PKR 0–2,000) | -3.31 | 69,936 | ⭐ Limited |

**Key finding**: The model excels for premium products (PKR 10,000+) with 90% accuracy — making it most valuable for high-value product pricing decisions.

### Model Comparison

| Model | R² Score | MAE (log) | RMSE (log) |
|-------|:---:|:---:|:---:|
| **Random Forest** | **0.923** | **0.255** | **0.500** |
| Gradient Boosting | 0.865 | 0.447 | 0.662 |

---

## Top Feature Drivers

| Rank | Feature | Insight |
|------|---------|---------|
| 1 | `order_total_amount` | Strongest predictor — price drives total |
| 2 | `product_category` | Category significantly influences unit price |
| 3 | `discount_rate` | Higher discounts signal premium original price |
| 4 | `quantity_ordered` | Bulk orders often have different unit pricing |
| 5 | `customer_tenure_days` | Loyal customer pricing patterns |

---

## Dataset

| Attribute | Value |
|-----------|-------|
| Source | [Pakistan's Largest E-commerce Dataset (Kaggle)](https://www.kaggle.com/datasets/zusmani/pakistans-largest-ecommerce-dataset) |
| Records | 1,048,575 transactions (584,524 active) |
| Time Period | 2016–2018 |
| Size | 208+ MB |
| Target Variable | `unit_price` (price per item in PKR) |
| Unique Customers | 584,513 |
| Average Order Value | PKR 8,530.62 |

---

## Engineering Decisions

**Why log-scale metrics?** Price distributions are highly right-skewed (some items cost PKR 100, others PKR 500,000). Log-scale MAE/RMSE measures proportional accuracy rather than absolute error — more meaningful for pricing.

**Why low-price items underperform?** Low-price items (PKR 0–2,000) have enormous variance — a phone case and a cheap USB cable can both be PKR 200 but have completely different pricing logic. The model correctly learns premium pricing much better than commodity pricing.

**Production recommendation**: Deploy for high-value products (PKR 10,000+) where the model is most reliable. For budget items, supplement with category-average pricing.

---

## Web Application Features

- **Real-time Predictions**: Instant price output based on input parameters
- **Interactive Form**: Product category, quantity, order total, discount, payment type, order date
- **Responsive Design**: Works on desktop and mobile
- **Animated Results**: Visual breakdown of prediction confidence

---

## Business Applications

| Application | Benefit |
|-------------|---------|
| Automated Pricing | Set prices for new products automatically |
| Anomaly Detection | Flag products with unusual pricing for review |
| Revenue Optimization | Identify underpriced premium products |
| Category Management | Understand price drivers across categories |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| ML | scikit-learn 1.3.2 (Random Forest, Gradient Boosting) |
| Data | pandas 2.0.3, numpy 1.24.3 |
| Web | Flask 3.0.0 |
| Serialization | joblib 1.3.2 |
| Visualization | matplotlib, seaborn |

---

*Licensed under MIT · Dataset: Pakistan E-commerce by Zusmani (Kaggle)*

[← Back to Portfolio](../README.md)
