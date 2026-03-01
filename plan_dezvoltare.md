# Plan de Dezvoltare — Replicare Platformă BursaTransport
> **2 Developeri | Claude Code (Sonnet 4.6 + Opus 4.6)**  
> **Obiectiv:** Platformă production-ready, securizată, funcțională 1:1 cu BursaTransport

---

## Stack Tehnologic

| Layer | Tehnologie | Justificare |
|---|---|---|
| **Frontend** | Next.js 14 (App Router) | SSR/SSG, SEO, routing avansat |
| **Styling** | Tailwind CSS + shadcn/ui | Rapid, consistent, accesibil |
| **Backend/API** | Next.js API Routes + tRPC | Type-safe end-to-end, fără overhead |
| **Bază de date** | PostgreSQL (Supabase) | Relațional, scalabil, hosted |
| **ORM** | Prisma | Schema as code, migrații, type safety |
| **Auth** | NextAuth.js v5 | Session, 2FA, JWT, OAuth |
| **Storage fișiere** | Supabase Storage / S3 | Documente, avatare, PDF-uri |
| **Email** | Resend + React Email | Tranzacțional, modern |
| **SMS** | Twilio | Notificări SMS, 2FA |
| **Plăți** | Stripe | Card, subscriptions, invoicing |
| **Hărți** | Mapbox GL JS | Rute, pins, geocoding |
| **Real-time** | Supabase Realtime / Pusher | Licitații live, notificări instant |
| **Job queue** | pg-boss (pe Postgres) | SMS schedulated, expirary jobs |
| **Deployment** | Vercel (FE) + Supabase (DB) | Zero-config, scalabil |
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

---

## Convenții de Lucru cu Claude Code

### Dev 1 — Prompturi recomandate (Opus 4.6)
```
Folosește schema din erd_baza_de_date.md și schema_date.md.
Implementează [entitate/feature] în Prisma cu toate relațiile,
validare Zod, tRPC router type-safe și middleware de autorizare.
Asigură-te că există rate limiting, sanitizare input și logs de audit.
```

### Dev 2 — Prompturi recomandate (Sonnet 4.6)
```
Folosește design system-ul existent (Tailwind + shadcn).
Implementează pagina [X] conform documentației din BursaTransport_Exhaustiv.md,
secțiunea [Y]. Conectează la tRPC hooks. Asigură responsivitate și stări de loading/error.
```

### Reguli generale
- **Commit după fiecare feature complet** (nu la jumătate)
- **Fiecare feature nou = branch separat → PR → review → merge**
- **Checkpoint săptămânal** — demo live al features livrate
- **Documentele de referință** (în `/docs` din repo): `BursaTransport_Exhaustiv.md`, `schema_date.md`, `lista_features.md`, `erd_baza_de_date.md`

---

## FAZA 1 — Setup & Arhitectură de Bază
**Durată estimată: 3-4 zile**  
**Obiectiv:** Repo funcțional, DB up, Auth funcțional, structură de proiect stabilită.

---

### ✅ CHECKPOINT 1 — „Schelet Verde"
> *La finalul Fazei 1, ambii devs pot rula proiectul local, pot face login și văd un dashboard gol.*

---

#### Dev 1 — Backend Setup

**[1.1] Inițializare proiect**
```
- npx create-next-app@latest . --typescript --tailwind --app
- Instalare: prisma, @prisma/client, trpc, zod, next-auth, bcryptjs
- Configurare .env cu toate variabilele (DB, Auth, Stripe, Twilio, Resend)
- Push repo în Sophisticated-Logistics-Architects/platform (branch: main)
```

**[1.2] Schema Prisma completă**
```
- Creare schema.prisma cu TOATE cele 20 entități din erd_baza_de_date.md
- Toate relațiile, indexurile, enum-urile definite
- Prima migrație: npx prisma migrate dev --name init
- Seed de date de test (2 companii, 5 useri, 10 anunțuri demo)
- Claude Opus: "Implementează schema Prisma completă conform erd_baza_de_date.md,
  cu toate indexurile de performanță și enum-urile globale"
```

**[1.3] tRPC setup + routers de bază**
```
- Configurare tRPC cu context (sesiune user, companie)
- Router: auth (login, register, logout, session)
- Router: company (getById, getPublicProfile, update)
- Router: user (getMe, update)
- Middleware: isAuthenticated, isCompanyAdmin, hasCredits
```

**[1.4] NextAuth.js v5**
```
- Provider: Credentials (username + password)
- JWT cu userId + companyId + subscriptionTier în token
- Middleware Next.js pentru protejare rute /account/*
- Tabel Session în Prisma (pentru Login Securizat / 2FA ulterior)
```

**[1.5] Sistem CRB (credite) — fundație**
```
- Funcție deductCredits(companyId, service, quantity) cu transaction Postgres
- Verificare sold înainte de orice operație cu cost
- Log automat în CreditTransaction la fiecare deducere
```

---

#### Dev 2 — Frontend Setup

**[1.6] Design System**
```
- Instalare shadcn/ui + configurare tema (culori BursaTransport: albastru #1a3a5c, portocaliu #f47920)
- Fonturi: Inter (Google Fonts)
- Componente de bază: Button, Input, Card, Badge, Modal, Toast, Spinner
- Layout-uri: RootLayout, AuthLayout, DashboardLayout
```

**[1.7] Pagini Auth**
```
- /login — formular user + parolă, validare client, redirect după login
- /register — formular companie nouă (CUI, date legale, user principal)
- /forgot-password — email recovery flow
- Conectare la tRPC auth router
- Claude Sonnet: "Implementează paginile de auth conform design system-ului,
  cu validare Zod client-side și stări de error/loading"
```

**[1.8] Header + Navigare principală**
```
- Header cu logo 123cargo, meniu principal (Mărfuri, Camioane, Licitații, Companii, Servicii)
- Top bar: link-uri secundare (Despre Noi, Comunitate, Instrumente)
- Dropdown user (nume companie, credite, linkuri rapide, Ieșire)
- Alertă roșie incidente în dropdown (condiționată de date)
- Responsive (hamburger pe mobile)
```

**[1.9] Dashboard (Panou de Administrare) — shell**
```
- /account/dashboard — layout cu sidebar + widgeturi goale
- Widget: Credite disponibile
- Widget: Statistici anunțuri (mărfuri/camioane publicate)
- Widget: Date companie (read-only, buton Modifică)
- Widget: Date utilizator (read-only, buton Modifică)
- Sidebar cu toate link-urile din secțiunea 7.3 (Companii → Documente → Parc Auto etc.)
```

---

## FAZA 2 — Core Business Logic: Mărfuri & Camioane
**Durată estimată: 7-10 zile**  
**Obiectiv:** Publicare și căutare mărfuri/camioane funcționale end-to-end.

---

### ✅ CHECKPOINT 2 — „Bursa de Transport Funcțională"
> *Un user poate publica o marfă, altul o poate găsi prin search și poate vedea toate detaliile.*

---

#### Dev 1 — Backend Mărfuri & Camioane

**[2.1] tRPC Router: cargo**
```
- cargo.create — validare Zod completă (toate câmpurile din CargoListing)
- cargo.search — filtrare complexă (locație, dată, tip camion, tonaj, ADR, preț/km)
- cargo.getById — cu date companie publicatoare
- cargo.update — doar owner-ul poate edita
- cargo.delete / cargo.expire — soft delete + job de expirare automată
- cargo.getMine — lista mărfurilor proprii
- cargo.highlight — deducere 1 CRB + marcare is_highlighted=true
```

**[2.2] tRPC Router: truck**
```
- Identic cu cargo router dar pentru TruckListing
- truck.create, search, getById, update, delete, getMine, highlight
- Câmp specific: price_per_km (vs price_offered la cargo)
- Câmp specific: sms_notification_enabled
```

**[2.3] Geocoding & Locații**
```
- Integrare Mapbox Geocoding API pentru autocomplete locații
- Salvare lat/lng în JSON alături de numele locației
- Funcție distanceKm(pointA, pointB) pentru afișare distanță în rezultate
```

**[2.4] Job queue — expirare automată anunțuri**
```
- pg-boss job la publicare: expireCargoListing(id) la expires_at
- Job la crearea TruckListing similar
- Status → 'expirat' automat
```

**[2.5] SMS Notificări (Truck → Cargo match)**
```
- La publicarea unui cargo: query camioane active cu rută potrivită și sms_notification_enabled=true
- Trimite SMS via Twilio: "Marfă nouă potrivită pentru ruta ta: [detalii]"
- Cost 0.15 CRB per SMS, dedus automat
```

---

#### Dev 2 — Frontend Mărfuri & Camioane

**[2.6] Pagina „Am marfă de transportat!"**
```
- /marfuri/adauga — formular complet conform secțiunii 3.1 din Exhaustiv.md
- Secțiuni: Detalii Traseu, Detalii Marfă/Camion, Informații Preț, Licitație (opțional)
- Autocomplete locații via Mapbox (input cu dropdown sugestii)
- Tag-uri verzi pentru locații multiple adăugate
- Checkbox-uri pentru dotări (ADR, FRIGO, Walking Floor etc.)
- Buton „Publică Marfă" → tRPC cargo.create → redirect la lista proprie
```

**[2.7] Pagina „Caută marfă"**
```
- /marfuri/cauta — layout split (filtre stânga, rezultate dreapta)
- Filtre: traseu, dată, tip camion, tonaj, dotări, preț/km, cu licitație only
- Tab-uri multiple de căutare simultană
- Toggle: Listă extinsă / Listă comprimată / Hartă
- Card rezultat: cod BM-XXXXXX, NEW badge, traseu, distanță, capacitate, preț
- Fundal galben pentru anunțuri evidențiate
- Auto-refresh toggle
- Paginare (20/pagină)
```

**[2.8] Vizualizare pe Hartă**
```
- Mapbox GL JS cu traseele mărfurilor ca linii colorate
- Cluster markers pentru rezultate dense
- Click pe marker → popup cu detalii rapide + buton „Vezi detalii"
```

**[2.9] Paginile de Camioane (mirror)**
```
- /camioane/adauga — similar cu marfuri/adauga, cu specificul FTL/LTL
- /camioane/cauta — similar cu marfuri/cauta
- Diferențe UI: buton albastru-gri vs portocaliu la mărfuri
```

**[2.10] Mărfurile/Camioanele mele + Favorite**
```
- /account/marfurile-mele — tabel cu anunțuri proprii (status, acțiuni: Editează, Șterge, Evidențiază)
- /account/favorite-marfuri — lista salvată cu steluță
- Rute salvate (SavedRoute): dropdown în form de publicare/căutare
```

---

## FAZA 3 — Licitații, Profiluri & Companii
**Durată estimată: 7-10 zile**  
**Obiectiv:** Sistemul de licitații funcțional, profiluri publice și un due diligence de bază.

---

### ✅ CHECKPOINT 3 — „Ecosistem viu"
> *Companiile se văd între ele, licitațiile funcționează în timp real, profilul public e complet.*

---

#### Dev 1 — Backend Licitații & Risk

**[3.1] tRPC Router: auction**
```
- auction.create — lansare cerere de ofertă (legată de CargoListing)
- auction.getActive — toate licitațiile active, ordonate după expires_at
- auction.getById — cu toate ofertele primite (doar pentru owner)
- auction.submitOffer — depunere ofertă (transportator)
- auction.adjudicate — owner selectează câștigătorul → status ADJUDECAT
- Real-time: Supabase Realtime channel per auction_request_id
- Notificare: la fiecare ofertă nouă → update winning/losing status pentru toți ofertanții
```

**[3.2] tRPC Router: company (extins)**
```
- company.search — filtrare după tip, locatie, licenta valida, activ
- company.getPublicProfile — toate datele publice + scoruri + incidente
- company.searchFleets — camioanele altor companii
- company.searchWarehouses — depozitele altor companii
- company.updateScore — recalculare scor după adjudecare licitație
```

**[3.3] tRPC Router: incident**
```
- incident.declarePayment — creare PaymentIncident cu validare
- incident.getMine — incidentele declarate de compania mea
- incident.getAgainstMe — incidentele primite (vizibil în profil public)
- incident.resolve — marcare ca rezolvat (de acuzat + declarant)
- Deducere automată CRB din crb_incidents_remaining
```

**[3.4] Verificări externe (ANAF, ARR)**
```
- Integrare API ANAF (webservicesp.anaf.ro/PlatitorTvaRest)
- Funcție checkAnafStatus(cui) → activ/inactiv
- Integrare ARR scraping sau API (dacă există endpoint public)
- Salvare snapshot în ExternalVerification + deducere credite
- Cache 24h per CUI (nu se verifică de N ori aceeași firmă)
```

---

#### Dev 2 — Frontend Licitații & Profiluri

**[3.5] Pagina Licitații — Cereri active**
```
- /licitatii — lista cereri active cu timer countdown live
- Evidențiere specială față de search normal (badge LICITAȚIE, timer roșu)
- Buton „Depune ofertă" → modal cu câmp preț + confirmare
- Update live (Supabase Realtime) — oferte noi apar fără refresh
- Status personal: „Ești CÂȘTIGĂTOR / PERDANT la moment"
```

**[3.6] Paginile de arhivă + „Ofertele mele"**
```
- /licitatii/ofertele-mele — table cu toate ofertele depuse + status final
- /licitatii/arhiva — cereri expirate/adjudecate
- Sistem de rating post-adjudecare (1-5 stele + comentariu)
```

**[3.7] Profil Public Companie**
```
- /companie/[id] — pagina publică conform secțiunii 7.4 din Exhaustiv.md
- Header companie cu logo, cover, scoring widget
- Tab-uri: „Ca Expeditor" / „Ca Transportator" (medalii, cifre)
- Indicatori: Risc CreditReform, Limita credit
- Tab-uri resurse: LOCATII & CONTACTE | PARC AUTO | DEPOZITE (cu numere)
- Alertă roșie vizibilă dacă compania are incidente de plată active
```

**[3.8] Căutare Companii**
```
- /companii — search cu filtre (nume/CUI, localitate, tip activitate, activ, cu licență)
- Card companie în rezultate: logo, nume, tip, scor, nr. incidente
- Tab suplimentar: Căutare Flote / Căutare Depozite
```

**[3.9] Incidente de Plată — UI**
```
- /account/incidente — tabs: „Declarate de mine" / „Împotriva mea"
- Formular declarare incident cu upload documente
- Tabel incident: DATA | DECLARANT | VALOARE | STARE + buton Rezolvă
```

---

## FAZA 4 — Monetizare, Admin & Features Avansate
**Durată estimată: 8-12 zile**  
**Obiectiv:** Sistemul de plăți funcțional, back-office admin, instrumente logistice.

---

### ✅ CHECKPOINT 4 — „Platforma se autofinanțează"
> *Un user nou se poate înregistra, alege un abonament, plăti cu cardul și accesa platforma.*

---

#### Dev 1 — Backend Monetizare & Admin

**[4.1] Stripe Integration — Subscriptions**
```
- Creare Stripe Products + Prices pentru fiecare tier (Flexibil, Sprinter 3L/6L/12L, Caraus, Premium)
- Webhook handler: checkout.session.completed → activare subscription în DB
- Webhook: invoice.paid → reînnoire automată
- Webhook: customer.subscription.deleted → downgrade la Flexibil
- tRPC: billing.createCheckoutSession, billing.getPortalUrl, billing.getInvoices
```

**[4.2] Stripe Integration — One-time Credits**
```
- Cumpărare CRB adiționale (calculator credite din secțiunea 8.4)
- Checkout one-time payment → credit_amount adăugat în company.crb_balance
- Generare factură PDF (Stripe Invoice sau custom cu @react-pdf/renderer)
```

**[4.3] tRPC Router: admin**
```
- admin.getStats — dashboard global (companii active, anunțuri publicate, venituri)
- admin.listCompanies — cu filtre și acțiuni (suspendă, activează)
- admin.manageSubscriptions — override manual abonamente
- admin.listIncidents — moderare incidente raportate
- admin.listInvoices — facturile emise de platformă
- Middleware: isAdmin (rol separat în User)
```

**[4.4] Email transacțional (Resend)**
```
- Template React Email pentru: Welcome, Confirm Email, Reset Password,
  Incident declarat, Subscripție activată, Factură emisă, Licitație adjudecată
```

**[4.5] Rate Limiting & Securitate**
```
- Upstash Redis rate limiter pe toate endpoint-urile publice
- Limite stricte: /api/auth/login (5 req/min), /api/cargo/search (30 req/min)
- Input sanitization cu DOMPurify pe toate câmpurile text
- CORS strict, CSRF tokens via NextAuth
- Helmet.js headers (CSP, HSTS, X-Frame-Options)
- Audit log pentru acțiuni sensibile (login, modificare date firmă, plăți)
```

---

#### Dev 2 — Frontend Monetizare & Instrumente

**[4.6] Pagina „Servicii și Costuri"**
```
- /servicii — landing page complet conform secțiunii 8.1 din Exhaustiv.md
- Tabel comparativ 4 abonamente (Acces Flexibil / Sprinter / Cărăuș / Premium)
- Matrice servicii accesorii cu prețuri CRB + LEI
- Butoane CTA: „Cumpără" → Stripe checkout
- Widget current: abonamentul activ al userului logat
```

**[4.7] Wizard „Cumpără CRB"**
```
- /servicii/cumpara-credite — 3 pași conform secțiunii 8.4
- Pas 1: Alege acces (abonament sau acces flexibil)
- Pas 2: Calculator credite (tabel interactiv cu cantitate → cost live)
- Pas 3: Review + buton plată → Stripe checkout
```

**[4.8] Instrumente Logistice**
```
- /instrumente/distante — Mapbox Directions API + panel input plecare/sosire
- /instrumente/curs-valutar — fetch BNR API + grafic Chart.js evoluție
- /instrumente/pret-motorina — tabel static sau scrapat săptămânal
- /instrumente/calculator-tarif — hartă cu rută + estimare preț bazată pe distanță și tonaj
```

**[4.9] Admin Back-office**
```
- /admin/* — layout separat, protejat cu rol admin
- Dashboard: grafice Recharts (utilizatori noi/lună, venituri, anunțuri active)
- Tabele: Companii, Utilizatori, Subscripții, Incidente, Facturi
- Acțiuni: Suspendă companie, Override abonament, Rezolvă incident
```

**[4.10] Comunitate**
```
- /comunitate/forum — list subiecte + paginare
- /comunitate/forum/[id] — thread cu răspunsuri nested
- /comunitate/mica-publicitate — grid anunțuri cu poze
- Rich text editor minimal (TipTap) pentru postare mesaje
```

---

## FAZA 5 — Securitate, Testing & Optimizare
**Durată estimată: 5-7 zile**  
**Obiectiv:** Pregătire pentru producție — teste, securitate avansată, performanță.

---

### ✅ CHECKPOINT 5 — „Gata de staging"
> *Toate testele trec, platforma suportă 500+ utilizatori concurenți, 0 vulnerabilități critice.*

---

#### Dev 1 — Securitate & Testing Backend

**[5.1] Teste automate backend**
```
- Jest + @trpc/server pentru unit tests pe toate routerele
- Test: auth flow (register, login, logout, 2FA)
- Test: cargo lifecycle (create → search → expire → cleanup)
- Test: auction flow (create → bid → adjudicate → score)
- Test: billing (stripe webhook handling, credit deduction)
- Test: incident flow (declare → view în profil public → resolve)
- Target: >80% code coverage pe business logic
```

**[5.2] Securitate avansată**
```
- 2FA (Login Securizat): TOTP via otplib, QR code generare
- Pentest checklist: SQL injection (Prisma protejat by default), XSS, CSRF
- Row Level Security în Supabase (compania X nu poate citi datele companiei Y)
- Secrets rotation plan (env vars în Vercel, nu în cod)
- Backup automat daily Supabase (plan plătit)
```

**[5.3] Performanță DB**
```
- EXPLAIN ANALYZE pe cele mai lente query-uri (search cargo, search truck)
- Indecși suplimentari dacă e necesar
- Connection pooling via Supabase PgBouncer
- Caching Redis (Upstash) pentru: scoruri companii, statistici live, cursuri valutare
```

---

#### Dev 2 — Testing Frontend & Optimizare

**[5.4] Teste E2E (Playwright)**
```
- Test: înregistrare companie nouă → login → publicare marfă → căutare marfă
- Test: licitație end-to-end (dev1=expeditor, dev2=transportator)
- Test: cumpărare abonament (Stripe test mode)
- Test: declarare + rezolvare incident de plată
- CI: rulare automată la fiecare PR pe GitHub Actions
```

**[5.5] Optimizare performanță FE**
```
- Core Web Vitals audit (LCP < 2.5s, FID < 100ms, CLS < 0.1)
- next/image pentru toate imaginile
- Lazy loading componente grele (hartă, charts)
- Bundle analyzer — eliminare pachete neutilizate
- Static generation pentru paginile publice (homepage, despre noi, servicii)
```

**[5.6] Responsivitate completă**
```
- Audit complet pe: mobile 375px, tablet 768px, desktop 1280px+
- Toate formularele funcționale pe mobile
- Harta — vizibilă și utilizabilă pe mobile
- Tabele — scroll horizontal pe mobile
```

---

## FAZA 6 — Launch & Production
**Durată estimată: 3-5 zile**  
**Obiectiv:** Deploy, domeniu, monitoring, go-live.

---

### ✅ CHECKPOINT 6 — „LIVE 🚀"
> *Platforma este accesibilă publicului, monitorizată, cu SSL, domeniu custom și backup activ.*

---

#### Ambii Developeri

**[6.1] Infrastructure setup**
```
- Vercel: conectare repo, env vars producție configurate
- Supabase: project producție separat de development (nu același DB!)
- Domeniu custom: configurare DNS pe Vercel (ex: platforma-logistica.ro)
- SSL: automat via Vercel
- Supabase: activare Point-in-Time Recovery (backup)
```

**[6.2] Monitoring**
```
- Sentry: integrare Next.js (erori FE + BE capturate automat)
- Vercel Analytics: trafic, Core Web Vitals în timp real
- Uptime robot / Better Uptime: alertă dacă site-ul pică
- Supabase Dashboard: monitorizare query performance, connections
```

**[6.3] Seed date producție**
```
- Date de configurare: tipuri abonamente, prețuri CRB, enum-uri
- Cont admin creat manual
- Testare end-to-end pe producție cu date reale
```

**[6.4] Documentație tehnică**
```
- README.md: cum rulezi local, cum faci deploy, variabile de mediu necesare
- /docs/api.md: documentație tRPC endpoints principale
- /docs/architecture.md: diagrama arhitecturii (link către erd_baza_de_date.md)
- Runbook: ce faci dacă pică DB, dacă Stripe nu răspunde etc.
```

**[6.5] Go-live checklist**
```
□ Toate variabilele de mediu setate în Vercel (prod)
□ Stripe în modul LIVE (nu test)
□ Twilio număr SMS achiziționat și verificat
□ Email-uri transacționale testate end-to-end
□ ANAF API cheie activată pentru producție
□ DB backup verificat că funcționează
□ Sentry primeşte evenimente
□ Rate limiting activ pe toate endpoint-urile
□ 2FA flow testat de la zero
□ Stripe webhooks configurate cu secret key prod
□ CORS origins setate corect (doar domeniu prod)
```

---

## Estimare Timp Total

| Faza | Durată | Dev 1 | Dev 2 |
|---|---|---|---|
| Faza 1 — Setup | 3-4 zile | Schema DB + Auth + tRPC | Design System + Auth UI + Dashboard |
| Faza 2 — Mărfuri & Camioane | 7-10 zile | Cargo/Truck API + Geocoding + Jobs | Formulare + Search + Hartă |
| Faza 3 — Licitații & Companii | 7-10 zile | Auction real-time + Incidente + ANAF | Licitații UI + Profiluri + Search Companii |
| Faza 4 — Monetizare & Admin | 8-12 zile | Stripe + Admin API + Rate Limiting | Pricing UI + Admin BO + Instrumente + Forum |
| Faza 5 — Testing & Securitate | 5-7 zile | Jest + Securitate + DB optim. | Playwright + Core Web Vitals + Mobile |
| Faza 6 — Launch | 3-5 zile | Infra + Monitoring | Deploy + Seed + Docs |
| **TOTAL** | **33-48 zile** | | |

> **Estimare realistă cu 2 devs full-time folosind Claude Code activ:** ~6-8 săptămâni.

---

## Structura Repo Git

```
platform/
├── src/
│   ├── app/                    # Next.js App Router (pagini)
│   │   ├── (auth)/             # login, register, forgot-password
│   │   ├── (main)/             # marfuri, camioane, licitatii, companii
│   │   ├── account/            # dashboard, setări, facturi
│   │   ├── admin/              # back-office admin
│   │   └── api/                # tRPC handler, Stripe webhooks, NextAuth
│   ├── components/             # Componente UI reutilizabile
│   │   ├── ui/                 # shadcn/ui componente de bază
│   │   ├── forms/              # FormCargo, FormTruck, FormAuction etc.
│   │   ├── cards/              # CargoCard, TruckCard, CompanyCard
│   │   ├── maps/               # MapboxMap, RouteLayer, MarkerCluster
│   │   └── layout/             # Header, Footer, Sidebar, DashboardLayout
│   ├── server/
│   │   ├── api/routers/        # auth, cargo, truck, auction, company, incident, billing, admin
│   │   ├── db/                 # Prisma client singleton
│   │   └── services/           # anaf.ts, twilio.ts, stripe.ts, resend.ts, mapbox.ts
│   ├── lib/
│   │   ├── utils.ts            # helpers
│   │   ├── validators/         # scheme Zod per entitate
│   │   └── constants.ts        # prețuri CRB, enum-uri, config
│   └── types/                  # tipuri TypeScript globale
├── prisma/
│   ├── schema.prisma           # Schema completă (20 entități)
│   ├── migrations/             # Migrații auto-generate
│   └── seed.ts                 # Date de test
├── docs/
│   ├── BursaTransport_Exhaustiv.md
│   ├── schema_date.md
│   ├── lista_features.md
│   └── erd_baza_de_date.md
├── tests/
│   ├── unit/                   # Jest tests (backend)
│   └── e2e/                    # Playwright tests
├── .github/workflows/          # CI/CD GitHub Actions
├── .env.example                # Template variabile de mediu
└── README.md
```
