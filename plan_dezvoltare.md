# Plan de Dezvoltare — Platformă Logistică (BursaTransport clone)
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
| **Auth** | Lucia Auth | Compatibil Astro, session-based, 2FA |
| **Storage fișiere** | Supabase Storage | Documente, avatare, PDF-uri |
| **Email** | Resend + React Email | Tranzacțional + notificări (înlocuiește SMS) |
| **Plăți** | Stripe | Card, subscriptions, RON/EUR |
| **Hărți** | MapLibre GL JS + OpenStreetMap | 100% open-source, gratuit |
| **Real-time** | Supabase Realtime | Licitații live, notificări instant |
| **Job queue** | pg-boss (pe Postgres) | Expirare automată anunțuri |
| **Deployment** | Netlify / Vercel + Supabase | Zero-config, scalabil |
| **CI/CD** | GitHub Actions | Auto-deploy, teste automate |
| **Monitoring** | Sentry | Erori FE + BE |

### Decizii finale confirmate
- ✅ **Fără sistem de credite CRB** — plăți directe RON/EUR prin Stripe
- ✅ **Fără SMS** — notificările se trimit prin email (Resend)
- ✅ **Hărți open-source** — MapLibre GL JS + tile-uri OpenStreetMap (Gratuit)
- ✅ **ANAF simulat în MVP** — mock response, integrare reală în v2
- ✅ **Limbă**: RO + EN (i18n)
- ✅ **Prețuri abonamente**: identice cu BursaTransport (în RON/EUR)
- ⏳ **Branding / Nume platformă**: TBD

---

## Roluri Developeri

| | **Dev 1 — Backend Lead** | **Dev 2 — Frontend Lead** |
|---|---|---|
| **Responsabilitate primară** | Schema DB, API, Auth, Business Logic, Securitate | UI/UX, Componente, Pagini, Integrări FE |
| **Model Claude Code** | **Opus 4.6** (logică complexă, securitate) | **Sonnet 4.6** (componente, styling rapid) |
| **Branch git** | `dev/backend` | `dev/frontend` |
| **Review** | Reviewuiește PR-urile FE pentru logică API | Reviewuiește PR-urile BE pentru UX impact |
