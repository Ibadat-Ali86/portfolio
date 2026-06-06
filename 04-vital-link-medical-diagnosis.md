# 🏥 VITAL-LINK — AI-Powered Multimodal Clinical Reasoning Platform

![Gemini](https://img.shields.io/badge/Gemini_2.0_Flash-4285F4?style=flat-square&logo=google&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js_4.4-FF6384?style=flat-square&logo=chartdotjs&logoColor=white)
![jsPDF](https://img.shields.io/badge/jsPDF-Reports-red?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)

> A multimodal diagnostic platform that correlates chest X-rays, lung audio patterns, and vital signs through Google Gemini 2.0 Flash to deliver ranked differential diagnoses with explainable clinical reasoning.

⚠️ **Disclaimer**: This is an AI research prototype for educational purposes only. NOT FDA-cleared or intended for real clinical decision-making.

---

## What Makes It Different

Most medical AI systems analyze a single data type. VITAL-LINK correlates three distinct modalities simultaneously:

| Data Source | Analysis Type | Detects |
|-------------|---------------|---------|
| 🫁 Chest X-Ray | Deep radiological analysis | Pneumonia, consolidations, effusions |
| 🎧 Lung Sounds | Audio pattern recognition | Crackles, wheezes, rhonchi |
| 📊 Vital Signs | Clinical context integration | Severity, risk stratification |

The fusion of all three gives a more complete clinical picture than any single modality alone.

---

## Pre-Built Clinical Cases

One-click sample cases for instant demonstration:

| Condition | Description |
|-----------|-------------|
| 🦠 Pneumonia | Community-acquired pneumonia |
| 🌬️ COPD | Chronic obstructive pulmonary disease |
| 😮‍💨 Asthma | Bronchospasm with wheezing |
| ✅ Healthy | Normal respiratory baseline |
| 🤧 URTI | Upper respiratory tract infection |
| 🫁 LRTI | Lower respiratory tract infection |
| 🌿 Bronchiectasis | Chronic bronchial dilation |
| 🔥 Bronchiolitis | Small airway inflammation |

---

## Key Features

**Multi-Modal Fusion Intelligence** — Correlates visual, audio, and numerical data in a single inference call.

**Explainable AI Reasoning** — Every prediction comes with a transparent reasoning trace showing which data inputs drove the diagnosis.

**Differential Diagnosis** — Ranked conditions with probability scores, not just a single prediction.

**Risk Stratification** — Automatic severity assessment (mild / moderate / severe / critical).

**Interactive Visualizations**:
- Gauge charts for real-time risk score display
- Feature importance bar charts showing input contributions
- Polar area charts for differential diagnosis comparison

**Professional PDF Reports** — Generate comprehensive medical reports with disease causes, precautions, and clinical actions.

---

## Architecture

```
User Input
    → Chest X-Ray (image)
    → Lung Audio (file)
    → Vital Signs (structured)
                ↓
    Flask Backend (server.py)
                ↓
    Gemini 2.0 Flash (multimodal fusion)
                ↓
    Structured Clinical JSON Response
        - Differential diagnoses + probabilities
        - Risk stratification score
        - Reasoning trace
        - Recommended actions
                ↓
    JavaScript Frontend (Chart.js visualizations)
                ↓
    PDF Report (jsPDF + html2canvas)
```

---

## Technical Implementation

**Gemini Integration** — The platform uses Gemini's multimodal capability to send image, audio (as encoded data), and structured vital signs in a single prompt, requesting a structured JSON clinical response.

**Reasoning Trace** — The system prompt explicitly asks Gemini to explain its diagnostic reasoning step-by-step, making the output auditable.

**Zero-Friction Demo** — Pre-loaded sample cases use real pathological X-ray images and synthetic audio representations so the platform can be evaluated without uploading private medical data.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| AI Engine | Google Gemini 2.0 Flash (multimodal) |
| Backend | Flask + Python 3.10+ |
| Frontend | HTML5, CSS3 (Glassmorphism + Dark Theme), JavaScript |
| Charts | Chart.js 4.4 |
| PDF Reports | jsPDF 2.5 + html2canvas |
| Styling | Custom CSS (1800+ lines) |

---

*Built for Google Gemini Hackathon 2026*

[← Back to Portfolio](../README.md)
