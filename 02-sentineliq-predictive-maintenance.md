# ⚙️ SentinelIQ — Predictive Maintenance Intelligence for Turbofan Engines

[![Live Demo](https://img.shields.io/badge/Live_Demo-sentinel--iq--nasa.vercel.app-22c55e?style=for-the-badge&logo=vercel&logoColor=white)](https://sentinel-iq-nasa.vercel.app)
![Status](https://img.shields.io/badge/Status-Production_Live-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

---

## 🎯 Business Problem

**Unplanned equipment failure is one of the most expensive operational risks in aviation and manufacturing.**

The aviation MRO (Maintenance, Repair & Overhaul) market is valued at over **$90B globally**. A single unplanned engine removal costs $500K–$2M in emergency parts, labor, and AOG (Aircraft on Ground) delays. Industrial manufacturers face similar dynamics — unplanned downtime costs an average of **$260,000 per hour** in discrete manufacturing.

The status quo has two failure modes:
- **Over-maintenance:** Time-based schedules replace components that still have life. A typical engine is overhauled every 2,000–3,000 cycles regardless of actual health. Wasteful.
- **Under-maintenance:** Run-to-failure strategies that rely on operator intuition miss degradation signals in sensor data. Catastrophic.

**Neither approach uses the wealth of real-time sensor telemetry available on every modern engine.**

---

## 💡 Solution Approach

SentinelIQ transforms the maintenance decision from a calendar event into a **data-driven, individually-prescribed intervention.**

Three components work together:

1. **RUL Forecasting** — Predicts the exact number of remaining operational cycles before failure. Maintenance is scheduled when the engine genuinely needs it.

2. **SHAP Explainability** — Every alert is backed by a per-sensor importance score. Maintenance engineers don't trust black boxes. They need to know *why* an alert fired before grounding an aircraft.

3. **MILP Scheduler** — Given a fleet with overlapping maintenance windows, finds the cost-minimum schedule that balances failure risk against downtime cost across all engines simultaneously.

---

## 📊 Measurable Business Outcome

| Scenario | Cost |
|---------|------|
| Unplanned fleet maintenance (naive reactive model) | $124,800 |
| **SentinelIQ prescriptive model** | **$86,600** |
| **Savings per fleet cycle** | **$38,200 (30.6%)** |

Scaled to a 200-engine fleet over 10 years: **>$7.6M in preventable costs.**

---

## 🤖 Model Performance

### Benchmark: NASA C-MAPSS FD001 Test Set

| Model | RMSE (cycles) ↓ | MAE (cycles) ↓ | NASA Score ↓ | Training Time |
|-------|:---:|:---:|:---:|:---:|
| Baseline (Linear regression) | 31.4 | 24.8 | 3,481 | < 1s |
| TCN Ensemble (dilated causal conv) | 16.48 | 12.44 | 517.4 | ~4 min |
| **BiLSTM (Production model)** | **14.37** | **11.25** | **355.3** | ~8 min |

**Lower is better.** NASA Score penalizes late predictions more heavily than early ones (asymmetric loss function matching operational reality — missing a failure is worse than premature maintenance).

### Why BiLSTM Won

Standard LSTMs process sequences forward only. A Bidirectional LSTM processes the sequence in both directions before making a prediction — capturing both "what has degraded so far" and "how this pattern typically ends." For RUL, this bidirectional context improved RMSE by 2.1 cycles over unidirectional LSTM.

### RUL Capping Rationale

Raw RUL labels spike at the start of an engine's life (engine starts at cycle 1 with, say, 300 cycles of life). Training on uncapped labels causes the model to waste capacity predicting on healthy engines where prediction precision doesn't matter. Capping at 125 cycles (piecewise linear) focuses the model on the degradation zone where predictions drive decisions — this alone improved NASA Score by **54%** vs uncapped training.

---

## 🔬 SHAP Explainability: Top Failure Predictors

| Rank | Sensor | SHAP Direction | Physical Meaning |
|------|--------|---------------|-----------------|
| 1 | `sensor_11` | ↑ risk | HPC outlet temperature — rises sharply in late degradation |
| 2 | `sensor_14` | ↑ risk | LPT coolant bleed — loss of cooling efficiency |
| 3 | `sensor_12` | ↑ risk | HPC outlet static pressure — compressor degradation |
| 4 | `sensor_7` | ↑ risk | Total pressure at HPC exit — compression efficiency loss |
| 5 | `sensor_4` | ↓ risk | Total temperature — stable = healthy thermal management |

These findings align with known turbofan degradation physics — validating that the model learned genuine engineering relationships, not statistical artifacts.

---

## 🏗️ Technical Architecture

```
NASA C-MAPSS Raw Data
        ↓
Data Loader + Validation (pandas 2.2 + dtype optimization)
        ↓
Feature Engineering
  - Min-max normalization per sensor
  - Operational cluster assignment (K-Means on op settings)
  - Piecewise linear RUL label computation (cap=125)
        ↓
Model Training
  ┌─────────────────────────────────────────┐
  │  TCN (dilated causal convolutions)      │
  │  BiLSTM (bidirectional sequence model)  │
  │  Isolation Forest (anomaly scorer)      │
  │  Autoencoder (reconstruction error)     │
  └─────────────────────────────────────────┘
        ↓
SHAP DeepExplainer (per-sensor attribution)
        ↓
PuLP MILP Scheduler (fleet-level cost optimization)
        ↓
FastAPI Inference Server (port 7860)
  - Async endpoints
  - SQLAlchemy 2.0 → PostgreSQL
  - Sub-140ms P95 inference latency
        ↓
NGINX Reverse Proxy (port 80)
        ↓
Next.js 15 Dashboard (port 3000)
  - Real-time sensor streams (Recharts)
  - Anomaly scoring visualization
  - Fleet maintenance calendar
  - SHAP waterfall charts per engine
```

---

## ☁️ Infrastructure

Fully containerized microservices via Docker Compose:

```yaml
services:
  nginx:      # Reverse proxy + SSL termination
  ml_server:  # FastAPI inference (port 7860)
  frontend:   # Next.js 15 dashboard (port 3000)
  db:         # PostgreSQL 15 (results + history)
```

Single command to deploy the full stack: `make docker-up`

---

## 📦 Dataset

| Attribute | Details |
|-----------|---------|
| Source | NASA Ames Prognostics Center of Excellence |
| Dataset | C-MAPSS Turbofan Engine Degradation Simulation |
| Subsets | FD001 (1 op condition, 1 fault mode) → FD004 (6 conditions, 2 fault modes) |
| Training | 20,631 cycles / 100 engines |
| Features | 21 sensor channels + 3 operational settings |
| Target | RUL in engine cycles |

---

## 🛠️ Full Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Core ML | PyTorch | 2.2 |
| Classical ML | scikit-learn | 1.4 |
| Explainability | SHAP DeepExplainer | 0.45 |
| Optimization | PuLP (MILP) | 2.8 |
| Data | pandas, numpy | 2.2, 1.26 |
| API | FastAPI, SQLAlchemy | 0.111, 2.0 |
| Frontend | Next.js, Recharts, Framer Motion | 15 |
| Infrastructure | Docker Compose, NGINX, PostgreSQL | 15 |
| Testing | pytest, pytest-cov | 8 |
| CI/CD | GitHub Actions | — |

---

## 🏭 Industry Context

SentinelIQ targets the **Predictive Maintenance (PdM)** software market, projected to reach **$28.2B by 2026** (MarketsandMarkets). Key industrial buyers:

- **Tier-1 aerospace MRO providers** (ST Engineering, Lufthansa Technik, Air France Industries)
- **Industrial equipment manufacturers** (GE, Siemens, Honeywell)
- **Power generation utilities** with large turbine fleets
- **Contract manufacturers** running CNC machinery and compressors

The differentiator vs. existing PdM platforms (IBM Maximo, SAP PM, C3.ai): most enterprise platforms are expensive, cloud-dependent, and don't provide per-sensor SHAP attribution. SentinelIQ demonstrates that a Docker-deployable, explainable-AI maintenance system can be built on open-source tools.

---

*MIT License · Stack: PyTorch · SHAP · PuLP · FastAPI · Next.js 15 · Docker Compose · PostgreSQL · GitHub Actions CI*

[← Back to Portfolio](../README.md)
