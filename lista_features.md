# Lista de Features — Platformă Tip BursaTransport
> Structurată pe module, ierarhizată după prioritate.  
> **P1** = Must-have MVP | **P2** = Important (v2) | **P3** = Nice-to-have / diferențiator

---

## Legenda Priorităților

| Prioritate | Descriere |
|---|---|
| 🔴 **P1** | Fără acest feature platforma nu funcționează. Necesar în MVP. |
| 🟡 **P2** | Feature important care adaugă valoare semnificativă. Planificat în v2. |
| 🟢 **P3** | Diferențiator premium, experiență avansată. Planificat în v3+. |

---

## Modul 1: Autentificare & Înregistrare

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 1.1 | Înregistrare companie nouă (CUI, date legale) | 🔴 P1 | |
| 1.2 | Login cu user + parolă | 🔴 P1 | |
| 1.3 | Recuperare parolă via email | 🔴 P1 | |
| 1.4 | Gestionare sesiuni active | 🔴 P1 | |
| 1.5 | Autentificare 2FA / „Login Securizat" | 🟡 P2 | Sistemul BursaTransport numește asta „Login Securizat" |
| 1.6 | Invitare extra-utilizatori în companie | 🟡 P2 | Un utilizator principal invită subordonați |
| 1.7 | SSO (Google, Microsoft) | 🟢 P3 | Confort pentru companii mari |

---

## Modul 2: Profilul Companiei & Administrare

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 2.1 | Panou de administrare (dashboard) cu widgeturi KPI | 🔴 P1 | Credite, statistici publicări, scoruri |
| 2.2 | Editare date firmă (CUI, sediu, administrator) | 🔴 P1 | |
| 2.3 | Editare date utilizator (avatar, rol, limbi, telefon) | 🔴 P1 | |
| 2.4 | Gestionare locații/birouri ale companiei | 🔴 P1 | Birou Sibiu, Birou București etc. |
| 2.5 | Gestionare persoane de contact (tabel angajați) | 🔴 P1 | |
| 2.6 | Profil public al companiei (vizibil pentru alți membri) | 🔴 P1 | Scoruri, parc auto, depozite, contacte |
| 2.7 | Sistem de scoruri / rating (expeditor, transportator, beneficiar, furnizor) | 🔴 P1 | 4 scoruri individuale obținute din tranzacții |
| 2.8 | Upload documente legale (CUI, licențe, CMR, ADR) | 🟡 P2 | |
| 2.9 | Lista neagră a companiei (blocare parteneri) | 🟡 P2 | |
| 2.10 | Gestionare parc auto (flota vehiculelor) | 🟡 P2 | Legat de anunțuri camioane |
| 2.11 | Gestionare depozite | 🟡 P2 | Tip, suprafață, locație |
| 2.12 | Vizualizare facturi platformă (PDF download) | 🟡 P2 | Facturi emise de platformă → companie |
| 2.13 | Sistem de medalii/badge-uri (reputație vizuală) | 🟢 P3 | Gamification reputație |
| 2.14 | Ticker live statistici platformă (în dashboard) | 🟢 P3 | Ex: „31.392 mărfuri disponibile" |

---

## Modul 3: Mărfuri (Freight Exchange)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 3.1 | Publicare anunț marfă spot (formular complet) | 🔴 P1 | Traseu, tip camion, tonaj, extras ADR/FRIGO etc. |
| 3.2 | Câmpuri multiple locații de încărcare/descărcare (max 5) | 🔴 P1 | |
| 3.3 | Căutare + filtrare mărfuri (traseu, dată, tip camion, tonaj) | 🔴 P1 | |
| 3.4 | Vizualizare rezultate ca listă (extinsă și comprimată) | 🔴 P1 | |
| 3.5 | Vizualizare rezultate pe hartă interactivă | 🟡 P2 | OpenStreetMap / Google Maps |
| 3.6 | Tab-uri multiple de căutare simultană | 🟡 P2 | |
| 3.7 | Mărfurile mele (gestionare anunțuri proprii) | 🔴 P1 | |
| 3.8 | Mărfuri favorite (salvare anunțuri cu steluță) | 🟡 P2 | |
| 3.9 | Rute salvate (preset-uri căutare/publicare) | 🟡 P2 | |
| 3.10 | Publicare mărfuri pe termen lung (cereri recurente) | 🟡 P2 | |
| 3.11 | Căutare mărfuri pe termen lung | 🟡 P2 | |
| 3.12 | Anunț evidențiat / VIP (cost CRB) | 🟡 P2 | |
| 3.13 | Notificare SMS la marfă potrivită pentru camion | 🟡 P2 | |
| 3.14 | Reîmprospătare automată rezultate (auto-refresh) | 🟡 P2 | |
| 3.15 | Import bulk mărfuri (fișier CSV/Excel) | 🟢 P3 | Buton „Încarcă fișier bulk" |
| 3.16 | Filtru „Doar cele cu licitație" | 🟡 P2 | |
| 3.17 | Filtru „Doar cele cu preț" | 🟡 P2 | |
| 3.18 | Indicator preț per km acceptabil | 🟡 P2 | Câmp numeric în filtre |
| 3.19 | Verificare identitate contact (anti-fraud) | 🔴 P1 | Modal „Paziti-va de identitate furata!" |

---

## Modul 4: Camioane (Truck Exchange)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 4.1 | Publicare anunț camion liber spot (formular complet) | 🔴 P1 | Traseu, tip, tonaj, dotări |
| 4.2 | Câmp „Destinație = oriunde" (opțional) | 🔴 P1 | Pâna la 5 alternative |
| 4.3 | Câmp mod transport FTL / LTL | 🔴 P1 | Radio button complet vs grupaj |
| 4.4 | Căutare + filtrare camioane | 🔴 P1 | |
| 4.5 | Filtru „Doar trafic internațional" | 🟡 P2 | |
| 4.6 | Camioanele mele (gestionare anunțuri proprii) | 🔴 P1 | |
| 4.7 | Camioane favorite | 🟡 P2 | |
| 4.8 | Camioane pe termen lung | 🟡 P2 | |
| 4.9 | Legare anunț camion la vehicul din parc auto | 🟡 P2 | |
| 4.10 | Preț solicitat per km | 🔴 P1 | Diferit față de marfă care are preț total |

---

## Modul 5: Licitații (Spot Bidding / Auctions)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 5.1 | Lansare cerere de ofertă (simultan cu marfă spot) | 🔴 P1 | Preț fără TVA, termen plată, ore încărcare/descărcare |
| 5.2 | Durată configurabilă licitație (în minute) | 🔴 P1 | |
| 5.3 | Vizualizare licitații active (ca transportator) | 🔴 P1 | |
| 5.4 | Depunere ofertă la licitație | 🔴 P1 | |
| 5.5 | Status ofertă în timp real (Winning / Losing) | 🔴 P1 | |
| 5.6 | Arhivă cereri de ofertă (istorice) | 🟡 P2 | |
| 5.7 | Arhivă oferte depuse (istorice) | 🟡 P2 | |
| 5.8 | Sistem de rating post-tranzacție (licitații adjudecate) | 🔴 P1 | |
| 5.9 | FAQ / Ajutor licitații (secțiune dedicată) | 🟡 P2 | Cu acordeon Q&A |
| 5.10 | Notificare la ofertă câștigătoare / perdantă | 🔴 P1 | |

---

## Modul 6: Companii & Due Diligence (Risk Management)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 6.1 | Căutare companii membre (după nume, CUI, localitate, activitate) | 🔴 P1 | |
| 6.2 | Filtru „Doar companii active" și „Cu licență validă" | 🔴 P1 | |
| 6.3 | Căutare flote (camioane ale altor companii după dotări) | 🟡 P2 | |
| 6.4 | Căutare depozite (ale altor companii după tip) | 🟡 P2 | |
| 6.5 | Verificare stare ANAF (după CUI) | 🔴 P1 | Integrare API ANAF |
| 6.6 | Verificare licențe ARR (transport, intermediere) | 🔴 P1 | Integrare ARR |
| 6.7 | Verificare Ministerul Finanțelor (bilanțuri, accize) | 🟡 P2 | |
| 6.8 | Secțiune insolvențe declarate pe platformă | 🟡 P2 | |
| 6.9 | Comandă raport CreditReform (simplu + premium) | 🟡 P2 | Cost în credite |
| 6.10 | Vizualizare risc CreditReform în profil public | 🟡 P2 | |
| 6.11 | Declarare incident de plată (sub/peste 1000 Lei) | 🔴 P1 | Cu documente justificative |
| 6.12 | Vizualizare incidente declarate împotriva companiei | 🔴 P1 | |
| 6.13 | Declarare incident de transport (avarie, întârziere) | 🟡 P2 | |
| 6.14 | Serviciu factoring (123Încasare rapidă — partener extern) | 🟢 P3 | Integrare Invoitix / OmniCredit |
| 6.15 | Serviciu garantare factură (partener extern Coface) | 🟢 P3 | |

---

## Modul 7: Monetizare & Credite (CRB System)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 7.1 | Sistem de credite interne (monedă virtuală platformă) | 🔴 P1 | 1 CRB = preț configurabil |
| 7.2 | Abonamente cu niveluri diferite de acces | 🔴 P1 | Flexibil / Sprinter / Cărăuș / Premium |
| 7.3 | Acces flexibil (plată per zi de utilizare) | 🔴 P1 | Alternativă la abonament |
| 7.4 | Calculator credite (wizard cumpărare CRB) | 🔴 P1 | 3 pași: alege acces → calculează → plătește |
| 7.5 | Plată online cu cardul | 🔴 P1 | |
| 7.6 | Plată prin ordin de plată bancar (1a accesare) | 🔴 P1 | |
| 7.7 | Plată prin SMS (sume mici) | 🟡 P2 | |
| 7.8 | Reînnoire automată abonament cu cardul | 🟡 P2 | |
| 7.9 | Wizard activare extra-utilizatori (cost CRB/slot) | 🟡 P2 | |
| 7.10 | Credite neutilizate valabile 6 luni | 🔴 P1 | Regulă de business |
| 7.11 | Dashboard credite disponibile (cu contor în header) | 🔴 P1 | |
| 7.12 | Tabel comparativ abonamente (pricing page) | 🔴 P1 | |
| 7.13 | Matrice servicii accesorii cu prețuri în CRB și LEI | 🔴 P1 | |
| 7.14 | Emitere factură proformă automată | 🔴 P1 | |
| 7.15 | Emitere factură fiscală finală + descărcare PDF | 🔴 P1 | |
| 7.16 | Prețuri diferențiate pe țări (non-rezidenți România) | 🟡 P2 | |

---

## Modul 8: Comunitate & Conținut

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 8.1 | Forum de discuții (subiecte + răspunsuri) | 🟡 P2 | |
| 8.2 | Categorii forum | 🟡 P2 | Discuții generale, juridic etc. |
| 8.3 | Moderare forum (reguli, avertizare) | 🟡 P2 | |
| 8.4 | Căutare în forum | 🟡 P2 | |
| 8.5 | Mică publicitate (anunțuri vânzări: camioane, mărfuri) | 🟡 P2 | |
| 8.6 | Chat intern între utilizatori (în timp real) | 🟢 P3 | Iconița de mesagerie |
| 8.7 | Secțiune știri / blog platformă | 🟡 P2 | Feed cu tag-uri (INFORMARI, UTILIZARE) |
| 8.8 | Tutoriale video integrate | 🟢 P3 | |
| 8.9 | Formular de asistență (support ticket) | 🔴 P1 | |
| 8.10 | Pagini statice: Termeni & Condiții, Politică GDPR, Securitate | 🔴 P1 | |
| 8.11 | Pagini SEO (Despre Noi, Pentru Transportatori, Pentru Expeditori) | 🟡 P2 | |
| 8.12 | Sistem de banere publicitare (parteneri externi) | 🟢 P3 | Zonă reclame laterale |

---

## Modul 9: Instrumente Logistice

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 9.1 | Calculator distanțe rutiere Europa (hartă) | 🟡 P2 | |
| 9.2 | Tarifar / Calculator cost transport (cu hartă rute) | 🟡 P2 | |
| 9.3 | Convertor valutar (cu grafic evoluție, bazat pe BNR) | 🟡 P2 | |
| 9.4 | Prețuri motorină în Europa (tabel per țară) | 🟡 P2 | |
| 9.5 | Restricții de circulație pe șosele din România | 🟢 P3 | |
| 9.6 | Calendar sărbători legale / zile nelucratoare Europa | 🟢 P3 | |
| 9.7 | Taxe de acces în orașe România (hartă interactivă) | 🟢 P3 | |
| 9.8 | Generator formular atestat activitate șofer (PDF) | 🟢 P3 | |
| 9.9 | Integrare API publicare rapidă (pentru ERP-uri externe) | 🟢 P3 | |

---

## Modul 10: Admin Platformă (Back-Office)

| # | Feature | Prioritate | Note |
|---|---|---|---|
| 10.1 | Dashboard admin global (companii, utilizatori, tranzacții) | 🔴 P1 | |
| 10.2 | Gestionare companii (aprobare, suspendare, verificare) | 🔴 P1 | |
| 10.3 | Gestionare abonamente și prețuri CRB | 🔴 P1 | |
| 10.4 | Gestionare incidente (moderare, rezolvare) | 🔴 P1 | |
| 10.5 | Rapoarte interne (statistici activitate platformă) | 🔴 P1 | |
| 10.6 | Gestionare conținut editorial (știri, regulament) | 🟡 P2 | |
| 10.7 | Gestionare bannere publicitare | 🟢 P3 | |
| 10.8 | Sistem de notificări globale (anunțuri platformă) | 🟡 P2 | |
| 10.9 | Logs de audit (cine a verificat ce companie, când) | 🟡 P2 | |

---

## Sumar pe Prioritate

| Prioritate | Nr. Features | Module acoperite |
|---|---|---|
| 🔴 **P1 (MVP)** | ~45 | Auth, Profil, Mărfuri, Camioane, Licitații, Risk, Monetizare, Admin |
| 🟡 **P2 (v2)** | ~30 | Extinderi funcționale, instrumente, comunitate |
| 🟢 **P3 (v3+)** | ~15 | Diferențiatori premium, integrări externe, chat |
| **TOTAL** | **~90** | **10 module** |

---

## Roadmap Sugestiv

```
MVP (P1)           → v2 (P2)               → v3+ (P3)
─────────────────────────────────────────────────────
Auth & Profil       Forum & Blog             Chat intern
Mărfuri Spot        Licitații Arhivă         Factoring extern
Camioane Spot       Instrumente logistice    Generator PDF
Licitații (core)    Rapoarte CreditReform    API integrare ERP
Incidente Plată     Mică publicitate         Bannere publicitare
Monetizare CRB      Extra-utilizatori        SSO
Verificare ANAF     Verificare MFin          Restricții circulație
Admin back-office   Notificări SMS           Diferențiere prețuri geo
```
