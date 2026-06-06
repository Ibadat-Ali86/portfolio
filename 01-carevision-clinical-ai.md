# 🏥 CareVision — Multimodal AI Clinical Decision-Support PWA

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-carevision--chw.vercel.app-7c3aed?style=for-the-badge)](https://carevision-chw.vercel.app/landing)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat-square&logo=python&logoColor=white)
![React](https://img.shields.io/badge/React-18.3-61DAFB?style=flat-square&logo=react&logoColor=black)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-009688?style=flat-square&logo=fastapi&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini_2.0_Flash-4285F4?style=flat-square&logo=google&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-Enabled-5A0FC8?style=flat-square&logo=pwa)
![Status](https://img.shields.io/badge/Status-Live_in_Production-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-Apache_2.0-green?style=flat-square)

> An enterprise-grade Progressive Web Application that empowers Community Health Workers (CHWs) and clinicians with AI-assisted diagnostic capabilities in resource-constrained environments.

---

## The Problem It Solves

Community health workers in last-mile clinics face complex clinical scenarios without immediate access to specialist advice. Existing digital health tools require high-bandwidth internet — structurally absent in these regions.

CareVision bridges this gap with a mobile-first, offline-capable clinical decision support system powered by Google Gemini's multimodal vision capabilities.

---

## Live Demo

| Component | URL | Platform |
|---|---|---|
| **Frontend Application** | [carevision-chw.vercel.app](https://carevision-chw.vercel.app/landing) | Vercel |
| **Backend API (Swagger)** | `/docs` on the Railway service | Railway |

---

## AI Modules

| Module | Capability |
|---|---|
| 🔬 **TestStrip Reader** | Automated interpretation of malaria RDTs, pregnancy tests, urine dipsticks |
| 💊 **MedScan** | Identifies drug name, dosage, indications, contraindications from packaging |
| 🩹 **Wound Assessment** | Structured severity scoring (1–5 scale) with referral recommendations |
| 🧠 **DocReader** | Extracts and interprets clinical information from medical documents and lab reports |
| 🤖 **Protocol Assistant** | Conversational AI retaining context from prior diagnostic images |

---

## System Architecture

```
┌─────────────────────────────────────────────────────┐
│              React 18 + Vite 5 + TypeScript          │
│   Zustand · Radix UI · TailwindCSS · i18next 23      │
│   Offline DB: Dexie (IndexedDB) · PWA: Workbox       │
│              Hosted: Vercel (CDN edge)                │
└───────────────────────┬─────────────────────────────┘
                        │ HTTPS / REST JSON
                        ▼
┌─────────────────────────────────────────────────────┐
│         FastAPI 0.111 · Python 3.12 · Uvicorn        │
│   Auth: PyJWT (HS256) + Argon2id password hashing    │
│   Validation: Pydantic 2.9 · Rate Limiting · CORS    │
│              Hosted: Railway (Dockerized)             │
└───────────────────────┬─────────────────────────────┘
                        │ OpenAI-compatible REST
                        ▼
┌─────────────────────────────────────────────────────┐
│          Google AI Studio — gemini-2.0-flash         │
│     image + text input → structured clinical JSON    │
└───────────────────────┬─────────────────────────────┘
                        │ asyncpg
                        ▼
┌─────────────────────────────────────────────────────┐
│  PostgreSQL (Neon serverless) — production           │
│  SQLite (aiosqlite) — local development              │
│  ORM: SQLAlchemy 2.0 · Migrations: Alembic 1.13      │
└─────────────────────────────────────────────────────┘
```

---

## Key Technical Decisions

**Why PWA over native app?** Community health workers use a wide variety of Android devices. A PWA is installable, works offline, and requires no app store — critical for deployment in low-resource settings.

**Why Argon2id for passwords?** Standard bcrypt is vulnerable to GPU-accelerated cracking. Argon2id's memory-hardness makes brute-force attacks computationally prohibitive even with specialized hardware.

**Why Gemini 2.0 Flash?** Speed and cost matter in clinical settings. Flash delivers vision-language inference in ~1-2 seconds at a fraction of Pro cost, enabling real-time diagnostic feedback.

---

## Security Model

| Control | Implementation |
|---|---|
| Password hashing | Argon2id (argon2-cffi) |
| JWT tokens | HS256-signed, short-lived, with refresh rotation |
| Input validation | Pydantic 2.9 at all API boundaries |
| SQL injection | Fully parameterized via SQLAlchemy ORM |
| Secrets management | Environment variables only — zero secrets in source code |
| Transport | TLS enforced; Neon PostgreSQL requires sslmode=require |

---

## Internationalization

Supports 15+ languages with AI prompt synchronization:

`English · French · Spanish · Arabic · Hindi · Swahili · Amharic · Bengali · Indonesian · Vietnamese · Burmese · Khmer · Portuguese · Tagalog · Hausa`

AI responses are generated in the user's selected language — not just the UI.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, Vite 5, TypeScript 5.2, Zustand, Radix UI, TailwindCSS |
| Backend | FastAPI 0.111, Python 3.12, Uvicorn (ASGI) |
| Auth | PyJWT (HS256), Argon2id (argon2-cffi 23.1) |
| AI | Google Gemini 2.0 Flash via generativelanguage.googleapis.com |
| Database | PostgreSQL (Neon serverless), SQLite (dev), SQLAlchemy 2.0, Alembic |
| PWA | vite-plugin-pwa 0.19, Workbox, Dexie (IndexedDB) |
| Deployment | Vercel (frontend), Railway (backend), Neon (database) |
| Observability | Pydantic Logfire audit logging |

---

## Deployment Stack

| Layer | Platform |
|---|---|
| Frontend | Vercel — auto-deploys from main; SPA routing via vercel.json |
| Backend | Railway — Dockerized; auto-deploys from main |
| Database | Neon — Serverless PostgreSQL with SSL connection pooling |
| AI Inference | Google AI Studio — gemini-2.0-flash |

---

*Built for frontline healthcare workers · Licensed under Apache 2.0*

[← Back to Portfolio](../README.md)
