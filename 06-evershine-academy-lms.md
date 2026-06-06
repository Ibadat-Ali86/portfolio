# 🎓 Evershine Academy LMS — Full-Stack Education Management Platform

![Next.js](https://img.shields.io/badge/Next.js-16.x-000000?style=flat-square&logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-5.x-2D3748?style=flat-square&logo=prisma&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![License](https://img.shields.io/badge/License-ISC-green?style=flat-square)

> A professional, production-oriented academy management platform covering admissions, academics, finance, attendance, reporting, and multi-role dashboards — built with Next.js App Router and Prisma ORM.

---

## What It Manages

A complete educational institution lifecycle — from the moment a student applies to graduation:

| Module | Capabilities |
|--------|-------------|
| 📋 **Admissions** | Student application workflow, enrollment, lifecycle management |
| 📚 **Academics** | Academic years, class sections, timetable scheduling, grading workflows |
| 💰 **Finance** | Fee collection, expense tracking, financial reporting |
| ✅ **Attendance** | Daily/weekly attendance with reporting and alerts |
| 👥 **Multi-Role Dashboards** | Student portal, parent portal, teacher dashboard, accountant view, admin panel |
| 📊 **Reporting** | Academic reports, financial summaries, attendance exports, PDF workflows |

---

## Architecture

```
User Interface → Next.js App Routes / Dashboard Pages
                 ↓
          API Layer and Validation (TypeScript)
                 ↓
        Prisma ORM + PostgreSQL Database
                 ↓
        Reports, Exports, and PDF Workflows
```

---

## Technical Highlights

**App Router Architecture** — Built on Next.js App Router (not Pages Router) with server components, server actions, and route handlers for a modern, type-safe full-stack experience.

**Prisma Data Model** — The database schema covers all educational entities: students, teachers, classes, subjects, attendance records, fee structures, payment history, academic years, and exam results — with proper foreign key relationships and cascading rules.

**Type-Safe End-to-End** — TypeScript throughout frontend, API layer, and Prisma schema ensures compile-time safety across the entire stack.

**Multi-Role Access Control** — Separate dashboards and API routes for each role (student, parent, teacher, accountant, administrator) with appropriate data access restrictions.

**PDF Export Workflows** — Generate report cards, fee receipts, and attendance reports directly from the application.

---

## Repository Structure

```
app/              # Next.js App Router pages and API routes
  ├── (auth)/     # Authentication routes
  ├── (admin)/    # Administrator dashboard
  ├── (teacher)/  # Teacher portal
  ├── (student)/  # Student portal
  ├── (parent)/   # Parent portal
  └── api/        # REST API endpoints
components/       # Shared UI components
lib/              # Utilities, validation, and integration logic
prisma/           # Schema and seed data
scripts/          # Setup and migration helpers
tests/            # Automated validation suites
docs/             # Deployment and operational documentation
```

---

## Development Workflow

```bash
git clone https://github.com/Ibadat-Ali86/evershine_lms.git
cd evershine_lms
npm install
cp .env.example .env.local
npx prisma generate
npx prisma migrate dev
npm run dev
```

Available at `http://localhost:5000`.

---

## Why This Project Matters

LMS platforms are typically expensive, cloud-dependent SaaS products with recurring fees. Evershine is self-hosted, customizable, and designed specifically for Pakistani/regional educational institutions that need features like:

- Local curriculum and grading patterns
- PKR-denominated fee management
- Flexible timetabling for small institutions
- Low-bandwidth-friendly interface

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js (App Router) |
| Language | TypeScript 5.x |
| ORM | Prisma 5.x |
| Database | PostgreSQL (compatible) |
| Styling | Tailwind CSS / component library |
| Auth | Next.js Auth patterns |
| Deployment | Node.js 20+ hosting |

---

*Licensed under ISC*

[← Back to Portfolio](../README.md)
