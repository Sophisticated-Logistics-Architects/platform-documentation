# Plan de Dezvoltare — Replicare Platformă BursaTransport
> **2 Developeri | Claude Code (Sonnet 4.6 + Opus 4.6)**  
> **Obiectiv:** Platformă production-ready, securizată, funcțională 1:1 cu BursaTransport

---

## Stack Tehnologic

| Layer | Tehnologie | Justificare |
|---|---|---|
| **Frontend** | Astro + React Islands | SSG/SSR, performanță maximă, SEO excelent |
| **Styling** | Tailwind CSS + shadcn/ui | Rapid, consistent, accesibil |
| **Backend/API** | Astro API Routes + tRPC | Type-safe end-to-end, fără overhead |
| **Bază de date** | PostgreSQL (Supabase) | Relațional, scalabil, hosted |
| **ORM** | Prisma | Schema as code, migrații, type safety |
| **Auth** | Lucia Auth / Better Auth | Compatibil Astro, session-based, 2FA |
| **Storage fișiere** | Supabase Storage / S3 | Documente, avatare, PDF-uri |
| **Email** | Resend + React Email | Tranzacțional, modern |
| **SMS** | Twilio | Notificări SMS, 2FA |
| **Plăți** | Stripe | Card, subscriptions, invoicing |
| **Hărți** | Mapbox GL JS | Rute, pins, geocoding |
| **Real-time** | Supabase Realtime / Pusher | Licitații live, notificări instant |
| **Job queue** | pg-boss (pe Postgres) | SMS scheduled, expiry jobs |
| **Deployment** | Netlify / Vercel (FE Astro) + Supabase (DB) | Zero-config, scalabil |
| **CI/CD** | GitHub Actions | Auto-deploy, teste automate |
| **Monitoring** | Sentry + Vercel Analytics | Erori, performanță |

---

## Roluri Developeri

| | **Dev 1 — Backend Lead** | **Dev 2 — Frontend Lead** |
|---|---|---|
| **Responsabilitate primară** | Schema DB, API, Auth, Business Logic, Securitate | UI/UX, Componente, Pagini, Integrări FE |
| **Model Claude Code** | **Opus 4.6** (logică complexă, securitate) | **Sonnet 4.6** (componente, styling rapid) |
| **Branch git** | `dev/backend` | `dev/frontend` |
| **Review** | Reviewuiește PR-urile FE pentru logică API | Reviewuiește PR-urile BE pentru UX impact |
