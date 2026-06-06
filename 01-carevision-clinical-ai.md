# 🏥 CareVision — Multimodal AI Clinical Decision-Support PWA

[![Live Demo](https://img.shields.io/badge/Live_Demo-carevision--chw.vercel.app-22c55e?style=for-the-badge&logo=vercel&logoColor=white)](https://carevision-chw.vercel.app/landing)
![Status](https://img.shields.io/badge/Status-Production_Live-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-Apache_2.0-blue?style=for-the-badge)

---

## 🎯 Business Problem

**Healthcare delivery gap in resource-constrained environments.**

Over 1 billion people globally receive healthcare from Community Health Workers (CHWs) who lack real-time access to diagnostic tools or specialist consultation. Misdiagnosis rates at last-mile clinics are estimated at 30–50% for common conditions like malaria, wound infections, and medication errors.

The existing digital health toolkit fails these workers because:
- Tools require reliable internet (absent in rural areas)
- Most require formal medical training to interpret outputs
- Multilingual support is an afterthought, not a design principle
- Security standards designed for enterprise, not field use

**The result:** Preventable deaths and treatment errors in the populations that need AI most.

---

## 💡 Solution Approach

CareVision delivers a **mobile-first, offline-capable AI clinical decision support system** that a community health worker can install on a phone, use without internet, and trust in their local language.

The architecture was designed around three non-negotiable constraints:
1. **Must work offline** — Dexie (IndexedDB) queues analyses when connectivity drops, syncs when restored
2. **Must be fast enough for field use** — Gemini 2.0 Flash chosen specifically for sub-2-second inference vs. Pro
3. **Must support the user's language** — i18next syncs UI language AND Gemini prompt language so AI responses arrive in the worker's native tongue

---

## 🤖 AI Modules

| Module | Clinical Task | AI Technique |
|--------|-------------|--------------|
| **TestStrip Reader** | Interprets malaria RDTs, pregnancy tests, urine dipsticks | Vision → structured JSON |
| **MedScan** | Drug name, dosage, indications, contraindications from packaging | Vision → clinical summary |
| **Wound Assessment** | Structured severity scoring (1–5) + referral recommendations | Vision → severity score |
| **DocReader** | Extracts and interprets lab reports and medical documents | Vision + text → summary |
| **Protocol Assistant** | Conversational AI retaining image context for follow-up queries | Multimodal conversation |

---

## 🏗️ Technical Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      CLIENT LAYER                           │
│  React 18 · Vite 5 · TypeScript 5.2 · Zustand 4.5          │
│  Radix UI · TailwindCSS 3.4 · i18next 23 (15+ locales)      │
│  PWA: vite-plugin-pwa · Workbox · Dexie (IndexedDB)         │
│              ▶ Hosted: Vercel CDN Edge                       │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTPS / REST JSON
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                      API LAYER                              │
│  FastAPI 0.111 · Python 3.12 · Uvicorn (ASGI)               │
│  Auth: PyJWT (HS256) · Argon2id (argon2-cffi 23.1)          │
│  Validation: Pydantic v2 · Rate limiting · CORS             │
│  Logging: Pydantic Logfire (structured audit trail)         │
│              ▶ Hosted: Railway (Dockerized)                  │
└──────────────────────────┬──────────────────────────────────┘
                           │ OpenAI-compatible REST
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                   AI INFERENCE LAYER                        │
│  Google Gemini 2.0 Flash                                    │
│  generativelanguage.googleapis.com/v1beta/openai/           │
│  image + text input → structured clinical JSON              │
└──────────────────────────┬──────────────────────────────────┘
                           │ asyncpg
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                      DATA LAYER                             │
│  PostgreSQL (Neon serverless) · SSL enforced                 │
│  SQLite (aiosqlite) · local development                      │
│  ORM: SQLAlchemy 2.0 async · Migrations: Alembic 1.13       │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔐 Security Model

| Control | Implementation | Why It Matters |
|---------|---------------|----------------|
| Password hashing | Argon2id (argon2-cffi) | GPU-resistant; protects patient-adjacent data |
| Token strategy | HS256 JWT, short TTL, refresh rotation | Access revocation without shared secrets |
| Input validation | Pydantic v2 at every API boundary | Prevents injection and malformed payloads |
| SQL queries | Fully parameterized via SQLAlchemy ORM | Zero raw string interpolation |
| Secrets | Environment variables only; `.env` git-excluded | No credentials in source history |
| Transport | TLS enforced; Neon requires `sslmode=require` | End-to-end encryption |
| CORS | Regex-based allowlist for `*.vercel.app` | Preview deployments without manual config |

---

## 🌍 Internationalization Strategy

15+ supported locales: `English · French · Spanish · Arabic · Hindi · Swahili · Amharic · Bengali · Indonesian · Vietnamese · Burmese · Khmer · Portuguese · Tagalog · Hausa`

**Critical design decision:** Language synchronization extends beyond the UI. When a user selects Swahili, the system prompts sent to Gemini are also in Swahili — so AI clinical responses arrive in the worker's native language rather than requiring secondary translation.

---

## 📦 Deployment Stack

| Layer | Platform | Notes |
|-------|---------|-------|
| Frontend | Vercel | Auto-deploys from `main`; SPA routing via `vercel.json` |
| Backend | Railway | Dockerized FastAPI; auto-deploys from `main` |
| Database | Neon | Serverless PostgreSQL with connection pooling |
| AI | Google AI Studio | Gemini 2.0 Flash via OpenAI-compatible endpoint |

---

## 🧠 Key Engineering Decisions & Tradeoffs

**Gemini Flash vs. Pro:** Flash delivers ~1–2s inference at 10x lower cost. For field workers making real-time decisions, latency matters more than marginal accuracy gain.

**PWA vs. native app:** CHWs use heterogeneous Android devices. A PWA is installable, works offline, requires no app store, and can be deployed via a URL — critical for NGO distribution channels.

**Argon2id vs. bcrypt:** bcrypt is vulnerable to GPU-accelerated attacks because it doesn't consume significant memory. Argon2id's memory-hardness makes parallel brute-force economically unfeasible even with dedicated hardware.

**Neon serverless PostgreSQL:** Standard PostgreSQL on Railway hibernates; Neon's serverless architecture means connection pooling handles traffic spikes without cold-start failures.

---

## 📁 Repository Structure

```
carevision/
├── backend/
│   ├── app/
│   │   ├── config.py         # Pydantic settings (env var parsing + validation)
│   │   ├── main.py           # FastAPI app factory, CORS, routers
│   │   ├── routes/           # auth, analyze, protocols, log
│   │   ├── schemas/          # Pydantic request/response schemas
│   │   ├── services/         # Gemini client, image processor, storage
│   │   └── prompts/          # Clinical system prompts per analysis type
│   └── Dockerfile
└── frontend/
    └── src/
        ├── api/              # Axios client + endpoint functions
        ├── components/       # Reusable UI (Radix + Tailwind)
        ├── features/         # TestStrip, MedScan, WoundAssess, DocReader
        ├── i18n/locales/     # 15+ translation JSON files
        ├── pages/            # Landing, Dashboard, Login
        └── store/            # Zustand: auth, settings, offline queue
```

---

## 🏭 Industry Context

CareVision sits in the **Digital Health / HealthTech** sector, directly competing with (or complementing):
- **mHealth platforms** like CommCare, KoBoToolbox (data collection, not AI diagnosis)
- **Telemedicine apps** like Sehat Kahani — require connectivity and trained clinicians
- **Clinical DSS systems** like Isabel DDx — enterprise-priced, not field-deployable

CareVision's differentiation: the **only** offline-first, multilingual, vision-AI clinical support tool designed specifically for CHWs in low-resource settings.

---

*Apache 2.0 License · Stack: React 18 · FastAPI · Gemini 2.0 Flash · PostgreSQL (Neon) · Docker · Vercel · Railway*

[← Back to Portfolio](../README.md)
