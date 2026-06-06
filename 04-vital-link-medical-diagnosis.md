# 🔬 VITAL-LINK — Multimodal Clinical Reasoning Platform

![Gemini](https://img.shields.io/badge/Gemini_2.0_Flash-4285F4?style=for-the-badge&logo=google&logoColor=white)
![Hackathon](https://img.shields.io/badge/Google_Gemini_Hackathon-2026-FFD21E?style=for-the-badge)

> ⚠️ **Disclaimer:** Research prototype for educational purposes only. NOT FDA-cleared.

---

## 🎯 Business Problem

**Single-modality diagnostics miss the full clinical picture.**

A physician looking only at a chest X-ray might miss the correlation with crackles in lung audio that confirm pneumonia. Vital signs alone don't tell you *why* SpO2 is dropping. The human diagnostic process is inherently multimodal — clinicians correlate what they see, hear, and measure simultaneously.

Existing AI diagnostic tools analyze **one modality at a time.** VITAL-LINK was built to change that.

---

## 💡 Solution: 3-Modality Fusion via Gemini

VITAL-LINK sends all three data types in a **single Gemini 2.0 Flash inference call**:

| Modality | Input Format | Clinical Value |
|----------|-------------|----------------|
| 🫁 Chest X-Ray | Image (base64) | Consolidation, effusion, cardiomegaly |
| 🎧 Lung Audio | Encoded waveform features | Crackles, wheezes, rhonchi, bronchial sounds |
| 📊 Vital Signs | Structured JSON | SpO2, temperature, respiratory rate, BP, age |

The fusion produces a **ranked differential diagnosis** with probability scores and a transparent reasoning trace — not a black-box label.

---

## 📊 Clinical Conditions Covered

| Condition | ICD-10 Equivalent | Typical Presentation |
|-----------|------------------|---------------------|
| Pneumonia | J18 | Consolidation + crackles + fever |
| COPD | J44 | Hyperinflation + decreased breath sounds |
| Asthma | J45 | Wheeze + normal X-ray + reversible obstruction |
| URTI | J06 | Normal X-ray + mild vitals changes |
| LRTI | J22 | Infiltrates + productive cough |
| Bronchiectasis | J47 | Tram-track opacities + chronic cough |
| Bronchiolitis | J21 | Hyperinflation + viral trigger (pediatric) |
| Healthy | — | Normal X-ray + normal audio + stable vitals |

---

## 🏗️ Architecture

```
User Input (3 modalities)
  → Chest X-ray image
  → Lung audio (waveform features)
  → Vital signs (structured form)
          ↓
Flask Backend (server.py)
  → Construct multimodal Gemini prompt
          ↓
Gemini 2.0 Flash API
  → System prompt: structured clinical JSON output
  → Returns: differential_dx[], risk_score, reasoning_trace, recommended_actions
          ↓
JavaScript Frontend
  → Interactive gauge chart (risk score)
  → Feature importance bars (which modality contributed most)
  → Polar area chart (differential comparison)
  → Reasoning trace (explainability panel)
          ↓
PDF Report (jsPDF + html2canvas)
  → Professional clinical summary
  → Disease-specific precautions
  → Recommended follow-up actions
```

---

## 🔑 Key Engineering Choices

**Structured JSON output from Gemini:** The system prompt explicitly specifies the JSON schema expected, including field names and value types. This eliminates parsing ambiguity and makes the output deterministic enough for downstream visualization.

**Reasoning trace as first-class output:** Most diagnostic AI hides its reasoning. VITAL-LINK treats the reasoning trace as a primary product — the clinician must be able to audit the logic before acting on the diagnosis.

**Pre-built sample cases:** Without real patient data, the platform would be impossible to demonstrate. Pre-loading pathological X-rays and matched vital signs enables zero-friction evaluation.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| AI | Google Gemini 2.0 Flash (multimodal) |
| Backend | Flask 2.x + Python 3.10+ |
| Frontend | HTML5, CSS3 (Glassmorphism), JavaScript |
| Charts | Chart.js 4.4 (gauge, bar, polar area) |
| Reports | jsPDF 2.5 + html2canvas |

---

## 🏭 Industry Context

**Medical AI / Clinical Decision Support** market: projected $45B by 2030 (Grand View Research).

Key differentiators vs. existing tools:
- **IBM Watson Health** (now Merative) — requires EHR integration, enterprise pricing
- **Aidoc** — radiologist-focused, single modality (imaging only)
- **Viz.ai** — stroke/PE focused, not general respiratory

VITAL-LINK's approach: lightweight, three-modality, explainable, deployable without EHR integration.

---

*Google Gemini Hackathon 2026 submission*

[← Back to Portfolio](../README.md)
