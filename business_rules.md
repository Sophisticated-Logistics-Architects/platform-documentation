# Business Rules — Platformă Logistică

Regulile invizibile care nu apar în FE dar guvernează logica de backend.

---

## 1. Abonamente & Acces

### Tipuri de abonament
| Tip | Durata | Preț (RON + TVA) | Utilizatori incluși |
|---|---|---|---|
| Acces Flexibil | Per zi | 45 RON / zi | 1 |
| Sprinter | 3 / 6 / 12 luni | 675 / 1200 / 1950 RON | 1 |
| Cărăuș | 1 / 3 / 6 / 12 luni | — / 825 / 1500 / 2400 RON | 1 |
| Premium | 3 / 12 luni | 1050 / 3000 RON | 1 |

> ⚠️ Prețurile sunt identice cu BursaTransport (sursa: `BursaTransport_Exhaustiv.md` secțiunea 8).

### Reguli de acces
- Un abonament include **1 utilizator**. Utilizatori extra = cost suplimentar (495 RON / slot).
- **Acces Flexibil**: se activează zilnic la prima autentificare. Se debitează automat.
- La expirarea abonamentului: **downgrade automat la „fără acces"** (nu la Flexibil).
- Companiile noi au **3 zile trial gratuit** (de implementat în v2).
- **Companiile non-rezidente în România** pot avea prețuri diferențiate (MVP: același preț).

### Ce include fiecare tier
| Feature | Flexibil | Sprinter | Cărăuș | Premium |
|---|---|---|---|---|
| Publicare marfă | ✅ | ❌ | ❌ | ✅ |
| Căutare marfă | ✅ | ✅ (< 3.5t) | ✅ | ✅ |
| Publicare camion | ✅ | ✅ (< 3.5t) | ✅ | ✅ |
| Căutare camion | ✅ | ✅ | ✅ | ✅ |
| Incidente plată | ❌ | incluse / lună: 1/2/4/8 | incluse / lună: 1/2/4/8 | incluse: 1/2/8 |
| Rapoarte companie | ❌ | incluse: 25/50/100 | incluse: 25/50/100 | incluse: 25/100 |
| Notificări email marfă | ❌ | ✅ | ✅ | ✅ |

---

## 2. Anunțuri (Mărfuri & Camioane)

### Publicare
- Un anunț **spot** are o durată de valabilitate: `available_from` + `available_duration_days`.
- Un anunț **termen lung** are o perioadă fixă: ex. 28-02-2026 → 28-03-2026.
- **Anunț evidențiat** (highlighted): afișat cu fundal galben, cost 15 RON + TVA.
- Numărul maxim de locații de încărcare/descărcare per anunț: **5**.
- La expirarea unui anunț → `status = 'expirat'` (soft delete, nu ștergere fizică).
- Job automat rulează la `expires_at` și schimbă statusul.

### Căutare
- Rezultatele sunt sortate implicit **invers cronologic** (cel mai nou primul).
- Anunțurile evidențiate apar **în topul listei**, indiferent de dată.
- Auto-refresh opțional: la fiecare 30 secunde.
- Paginare: **20 rezultate pe pagină**.
- Distanța traseu este **orientativă** și nu constituie obligație contractuală.

### Notificări email (înlocuiește SMS)
- La publicarea unui **cargo nou**: se caută camioanele active cu rută potrivită și `email_notification_enabled = true`.
- Se trimite email: „Marfă nouă potrivită pentru ruta ta".
- Cost: gratuit (inclus în abonament pentru Sprinter+).

---

## 3. Licitații (Spot Bidding)

- O licitație se lansează **simultan** cu publicarea unui anunț de marfă.
- Durata licitației: configurabilă de expeditor (ex: 180 minute). La expirare → `status = 'expirat'`.
- **Orice transportator autentificat** cu abonament activ poate depune ofertă.
- Un transportator poate depune **o singură ofertă** per licitație (dar o poate modifica).
- Ofertantul vede în timp real dacă e „CÂȘTIGĂTOR" sau „PERDANT" față de cea mai bună ofertă curentă.
- Expeditorul **adjudecă manual** câștigătorul (nu e automat la cel mai mic preț).
- La adjudecare: toți ceilalți ofertanți primesc email de notificare.
- Post-adjudecare: ambele companii pot acorda **rating reciproc** (1-5 stele).

---

## 4. Incidente de Plată

- Orice companie cu abonament activ poate **declara un incident** împotriva altei companii.
- Incidentele sunt **publice** — apar în profilul public al companiei acuzate.
- Compania acuzată primește **email de notificare** și poate răspunde/contesta.
- Incident rezolvat: ambele companii confirmă rezolvarea → `status = 'rezolvat'`.
- Incident contestat: intră în analiză manuală de admin.
- Limita de incidente pe care le poți declara depinde de **tipul abonamentului** (vezi tabelul de mai sus).

---

## 5. Scoring Companie

Fiecare companie are **4 scoruri independente**, calculate ca medie ponderată:

| Scor | Calculat din |
|---|---|
| `score_as_shipper` | Ratinguri primite **ca expeditor** de la transportatori |
| `score_as_carrier` | Ratinguri primite **ca transportator** de la expeditori |
| `score_as_buyer` | Ratinguri primite **ca beneficiar** în contracte pe termen lung |
| `score_as_supplier` | Ratinguri primite **ca furnizor** în contracte pe termen lung |

- Rating: 1-5 stele + comentariu opțional.
- Scorurile se recalculează la fiecare rating nou (medie aritmetică simplă în MVP).
- Un score < 2 → alertă vizuală în profilul public.
- Companiile cu incidente active **nu pot fi ascunse** din rezultatele de căutare.

---

## 6. Profilul Public al Companiei

Ce vede orice utilizator autentificat despre altă companie:
- Datele de bază (denumire, tip activitate, locație, CUI)
- Scorurile (4 tipuri)
- Numărul de incidente active vs. rezolvate
- Stare ANAF (Activ / Inactiv) — simulat în MVP
- Parc auto (număr vehicule declarate, tipuri)
- Depozite (număr, tipuri)
- Locații & Contacte (toate sediile cu telefon și email publice)

Ce **nu** se vede public:
- Datele de facturare
- Comentariile interne de pe anunțuri
- Parola, sesiunile active

---

## 7. Extra Utilizatori

- Pachetul de bază include **1 utilizator per companie**.
- Utilizatori extra: 495 RON + TVA / slot, activat pe toată durata abonamentului curent.
- Excepție: utilizatorii activați în **ultima lună** de abonament au drept de utilizare și pe abonamentul următor.
- Utilizatorul principal poate gestiona și dezactiva userii extra.

---

## 8. Documente Legale

- Companiile pot uploada: CUI, licențe transport/intermediere, CMR, ADR.
- Documentele **nu sunt verificate automat** în MVP (admin le verifică manual).
- Licența declarată valabilă apare ca bifă în căutare și profil public.
- Documentele **nu sunt publice** — doar adminii platformei le văd.

---

## 9. Monetizare (fără CRB)

> **Decizie:** Sistemul de credite virtuale (CRB) este eliminat. Toate plățile se fac direct în RON sau EUR prin Stripe.

### Servicii plătite (à la carte, fără abonament)
| Serviciu | Preț RON + TVA |
|---|---|
| Extra utilizator | 495 RON / slot |
| Anunț evidențiat (1 anunț) | 15 RON |
| Raport companie BT | 15 RON |
| Incident plată < 1000 RON | 60 RON |
| Incident plată > 1000 RON | 120 RON |
| Consultație juridică | 210 RON |

### Facturare
- La activarea abonamentului: **factură proformă** emisă automat.
- La plată: **factură fiscală finală** emisă automat (descărcabilă PDF).
- Facturile sunt legate de companie, vizibile în `/account/facturi`.

---

## 10. i18n (Multilingvism)

- Limbile suportate: **RO** (default) și **EN**.
- URL-uri: `/ro/...` și `/en/...` sau switch prin cookie/localStorage.
- Toate textele UI sunt externalizate în fișiere de traducere (`src/i18n/ro.json`, `src/i18n/en.json`).
- Conținutul generat de utilizatori (descrieri anunțuri, mesaje forum) **nu se traduce** automat.
