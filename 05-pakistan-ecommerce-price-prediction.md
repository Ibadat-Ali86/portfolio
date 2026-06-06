# 🛒 Pakistan E-commerce Price Intelligence Engine

[![Kaggle Notebook](https://img.shields.io/badge/Kaggle-View_Full_Notebook-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/code/ibadatali/pakistan-ecommerce-price-reggression)
[![R² Score](https://img.shields.io/badge/R²_Score-0.92-22c55e?style=for-the-badge)]()
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

---

## 🎯 Business Problem

**Pakistani e-commerce sellers are systematically underpricing premium products and inconsistently pricing across categories.**

Pakistan's e-commerce market is growing at 30%+ annually but pricing is largely intuition-driven. The specific problems:

1. **New product pricing:** When a seller lists a new SKU, they have no data-driven benchmark. They either copy a competitor (ignoring their own costs and market position) or price by gut feel.
2. **Anomaly detection:** Pricing errors (PKR 12,000 listed as PKR 1,200 due to a typo) go undetected until orders expose the mistake.
3. **Category-level insights:** Sellers don't know which product attributes drive price in their category — so they can't optimize their listings accordingly.

**Scale of the problem:** Pakistan's largest e-commerce platform (Daraz) has 60M+ active listings. Even a 5% improvement in pricing accuracy across premium products translates to hundreds of millions in recovered revenue.

---

## 💡 Solution Approach

A **Random Forest regression model** trained on 579,254 actual transactions from Pakistan's largest e-commerce dataset — providing:

1. **Price prediction:** Input product category, quantity, order context → get predicted unit price
2. **Confidence signal:** R² tells the seller how reliable the prediction is for this product type
3. **Feature insight:** Which factors (category, discount structure, order total) most drive the predicted price

---

## 📊 Model Performance

### Overall Metrics

| Metric | Value | Interpretation |
|--------|-------|----------------|
| **R² Score** | **0.92** | Model explains 92% of price variance |
| MAE (log scale) | 0.255 | Proportional error of ~29% |
| RMSE (log scale) | 0.500 | Comparable to industry benchmarks |

*Note: Log-scale metrics are appropriate here. Price spans PKR 10 to PKR 500,000 — absolute MAE would be dominated by a few outlier luxury items. Log-scale measures proportional accuracy.*

### Performance by Price Tier (The Insight That Matters)

| Price Range | R² | Sample Count | Business Recommendation |
|------------|:---:|:---:|:---:|
| **Premium (PKR 10,000+)** | **0.90** | 18,107 | ✅ Deploy confidently |
| Medium (PKR 2,000–10,000) | 0.18 | 14,196 | ⚠️ Supplement with category medians |
| Budget (PKR 0–2,000) | -3.31 | 69,936 | ❌ Use category-level averages |

**Why budget products underperform:** A PKR 200 item can be a phone case, a USB cable, or a stationery item. The model can't distinguish these without more granular category features. The variance is driven by product *type* more than any transactional feature.

**Why premium products excel:** High-value items have more consistent pricing logic. Electronics, appliances, and fashion accessories above PKR 10,000 follow predictable patterns around category, discount structure, and order totals.

### Model Comparison

| Model | R² | MAE (log) | Status |
|-------|:---:|:---:|:---:|
| **Random Forest** | **0.923** | **0.255** | ✅ Production |
| Gradient Boosting | 0.865 | 0.447 | Reference |

---

## 🔬 Feature Importance Analysis

| Rank | Feature | Importance | Business Interpretation |
|------|---------|:---:|:---:|
| 1 | `order_total_amount` | High | Total order value is the strongest proxy for unit value |
| 2 | `product_category` | High | Category sets the price range ceiling/floor |
| 3 | `discount_rate` | Medium | Higher discounts signal high original list price |
| 4 | `quantity_ordered` | Medium | Bulk pricing differs from single-unit pricing |
| 5 | `customer_tenure_days` | Medium | Loyal customers access different pricing tiers |
| 6 | `is_weekend` | Low | Weekend behavior slightly different |
| 7 | `payment_type` | Low | COD vs. card signals buyer segment |

---

## 🗂️ Dataset

| Attribute | Value |
|-----------|-------|
| Source | [Pakistan's Largest E-commerce Dataset](https://www.kaggle.com/datasets/zusmani/pakistans-largest-ecommerce-dataset) |
| Provider | Zusmani (Kaggle) |
| Records | 1,048,575 raw → 584,524 active → 579,254 after cleaning |
| Period | 2016–2018 |
| Size | 208+ MB |
| Target | `unit_price` (PKR per item) |
| Price Range | PKR 1 – PKR 500,000 |
| Avg Order Value | PKR 8,530.62 |

---

## 🌐 Web Application

The model is deployed as a Flask web app with a real-time prediction interface:

**Input fields:**
- Product Category (dropdown: Electronics, Appliances, Fashion, etc.)
- Quantity Ordered
- Order Total Amount (PKR)
- Discount Value (PKR)
- Payment Type (COD, Card, Bank Transfer)
- Order Status (Complete, Pending, Cancelled)
- Order Date (temporal features extracted automatically)

**Output:**
- Predicted Unit Price (PKR)
- Confidence indicator based on price tier

---

## 💼 Business Applications

| Use Case | Mechanism | Target User |
|----------|-----------|-------------|
| **New product pricing** | Input category + expected discount → get price benchmark | Sellers onboarding new SKUs |
| **Anomaly detection** | Flag listings where actual/predicted diverges >50% | Marketplace operations teams |
| **Revenue optimization** | Identify premium products priced below predicted optimum | Account managers / sellers |
| **Competitive analysis** | Understand which categories have highest pricing variance | Category managers |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| ML | scikit-learn 1.3.2 (Random Forest, Gradient Boosting) |
| Data | pandas 2.0.3, numpy 1.24.3 |
| Serialization | joblib 1.3.2 |
| Web | Flask 3.0.0 |
| Visualization | matplotlib, seaborn |
| Notebook | Kaggle Kernels (public) |

---

## 🏭 Industry Context

**E-commerce pricing intelligence** is a growing sub-market:
- **Enterprise tools:** Prisync, Wiser, Intelligence Node — all priced $500–$5,000/month
- **Pakistan-specific gap:** No ML-native pricing tool exists for Pakistan's market structure (COD dominance, Daraz marketplace dynamics, PKR volatility)

This project demonstrates the feasibility of a Pakistan-specific pricing engine using publicly available transaction data — a proof of concept for a deployable SaaS product.

---

*MIT License · Stack: scikit-learn · Flask · pandas · Kaggle · Python 3.8+*

[← Back to Portfolio](../README.md)
