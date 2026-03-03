# 🐄 MediFarm Connect

B2B SaaS CRM for veterinary pharmaceutical companies. Digitalizes the **vet → farm → animal → medicine** chain with a purchase-threshold loyalty engine and real-time sales analytics.

---

## Stack

| Layer | Tech |
|---|---|
| Frontend | React 18 + TypeScript (Vite) |
| State | TanStack Query v5 + Zustand |
| UI | TailwindCSS + shadcn/ui |
| Backend | Node.js 20 + Express 5 |
| ORM | Prisma 5 + PostgreSQL 15 |
| Cache | Redis 7 |
| Auth | JWT + Refresh Token (httpOnly cookie) |
| Maps | Mapbox GL JS |
| Mail / SMS | Resend + Netgsm |
| CI/CD | GitHub Actions → Vercel + Railway |

---

## Structure

```
medifarm-connect/
│
├── frontend/
│   └── src/
│       ├── api/              # Axios instances & query functions
│       ├── components/
│       │   ├── ui/           # shadcn/ui primitives
│       │   └── shared/       # ScoreBar, ThresholdBar, Badge…
│       ├── features/
│       │   ├── auth/
│       │   ├── vets/
│       │   ├── farms/
│       │   ├── usage/
│       │   ├── products/
│       │   ├── purchases/
│       │   └── campaigns/
│       ├── hooks/            # Shared custom hooks
│       ├── lib/              # Formatters, utils
│       ├── store/            # Zustand global state
│       └── types/            # Shared TypeScript types
│
├── backend/
│   └── src/
│       ├── config/           # DB, Redis, env
│       ├── middleware/       # auth, rbac, rateLimit, errorHandler
│       ├── modules/
│       │   ├── auth/
│       │   ├── users/
│       │   ├── vets/
│       │   ├── farms/
│       │   ├── animals/
│       │   ├── products/
│       │   ├── usage/
│       │   ├── purchases/
│       │   ├── score/        # Loyalty score engine
│       │   ├── campaigns/
│       │   └── notify/       # Mail, SMS, push
│       ├── jobs/             # Cron jobs
│       └── prisma/           # Schema, migrations, seed
│
└── .github/
    ├── workflows/
    │   └── deploy.yml        # test → build → staging → prod
    └── ISSUE_TEMPLATE/
```

---

## Quick Start

```bash
git clone https://github.com/YOUR_ORG/medifarm-connect.git
cd medifarm-connect

# Backend
cd backend && npm install
cp .env.example .env
npx prisma migrate dev
npx prisma db seed
npm run dev          # → localhost:3000

# Frontend
cd ../frontend && npm install
cp .env.example .env
npm run dev          # → localhost:5173
```

---

## Roles

| Code | Name | Access |
|---|---|---|
| `admin` | Company Admin | Full system, campaigns, threshold management |
| `rep` | Sales Rep | Vet management, usage & purchase approval |
| `vet` | Veterinarian | Own farms, animals, usage records |

---

## Business Rules

- **300,000 TL** approved purchases → account active for **12 months**
- Every usage record requires rep approval (`PENDING → APPROVED`)
- Stock below minimum → automatic mail + SMS alert
- Loyalty score = purchase volume (40%) + usage activity (35%) + farm count (25%)
- 6 months no purchase → read-only; passive after 30-day warning

---

## Roadmap

| Phase | Weeks | Focus |
|---|---|---|
| **1 – MVP** | 1–12 | Auth, vets, farms, usage flow, 300K threshold, dashboard |
| **2 – Growth** | 13–24 | Loyalty engine, map analytics, vaccination, campaigns |
| **3 – Global** | 25–48 | AI recommendations, i18n (TR/EN/AR), multi-tenant, scaling |

---

## Git Workflow

```
feature/US-01-vet-crud  →  develop  →  main
fix/US-12-score-bug     →  develop  →  main
```

Commits follow **Conventional Commits**: `feat:` `fix:` `chore:` `docs:` `test:`
