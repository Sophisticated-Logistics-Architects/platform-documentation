# Schema de Date — Platformă Tip BursaTransport

> Document derivat din analiza exhaustivă a platformei BursaTransport (123cargo).  
> Fiecare entitate reflectă câmpurile observate în FE (formulare, profile, tabele de rezultate).

---

## Diagrama Relațiilor (ERD simplificat)

```
COMPANY (1) ──── (N) USER
COMPANY (1) ──── (N) LOCATION
COMPANY (1) ──── (N) VEHICLE
COMPANY (1) ──── (N) WAREHOUSE
COMPANY (1) ──── (N) DOCUMENT
COMPANY (1) ──── (N) SUBSCRIPTION
COMPANY (1) ──── (N) PAYMENT_INCIDENT (declarant)
COMPANY (1) ──── (N) TRANSPORT_INCIDENT (declarant)
COMPANY (1) ──── (N) BLACKLIST_ENTRY
COMPANY (1) ──── (N) CREDITREFORM_REPORT (commanded)
COMPANY (N) ──── (N) COMPANY_SCORE (peer scoring)

USER (1) ──── (N) CARGO_LISTING
USER (1) ──── (N) TRUCK_LISTING
USER (1) ──── (N) AUCTION_REQUEST
USER (1) ──── (N) AUCTION_OFFER
USER (1) ──── (N) FORUM_POST
USER (1) ──── (N) CLASSIFIEDS_POST
USER (1) ──── (N) NOTIFICATION

CARGO_LISTING (1) ──── (0..1) AUCTION_REQUEST
AUCTION_REQUEST (1) ──── (N) AUCTION_OFFER
```

---

## 1. Entitatea `Company` (Companie)

Nucleul platformei. Orice actor (expeditor sau transportator) este o companie.

| Câmp | Tip | Note / Valori posibile |
|---|---|---|
| `id` | UUID / INT | ID intern platformă |
| `platform_id` | STRING | ID afișat public (ex: `ILT751`) |
| `name` | STRING | Denumire comercială |
| `cui` | STRING | Cod Unic de Identificare (RO CIF) |
| `reg_commerce` | STRING | Nr. Registrul Comerțului |
| `administrator` | STRING | Numele administratorului legal |
| `address` | STRING | Sediul social complet |
| `country` | STRING | Țara (ISO 3166) |
| `phone` | STRING | Telefon principal |
| `email` | STRING | Email principal |
| `website` | STRING | Optional |
| `type` | ENUM | `expeditor`, `transportator`, `expeditor_international`, `ambele` |
| `anaf_status` | ENUM | `activ`, `inactiv`, `suspendat` — verificat via ANAF |
| `has_transport_license` | BOOLEAN | Licență ARR transport marfă |
| `has_intermediary_license` | BOOLEAN | Licență ARR intermediere |
| `logo_url` | STRING | URL logo companie |
| `cover_image_url` | STRING | URL imaginea de cover |
| `joined_at` | DATETIME | Data înscrierii pe platformă |
| `subscription_type` | ENUM | `flexibil`, `sprinter`, `caraus`, `premium` |
| `subscription_expires_at` | DATETIME | Data expirare abonament |
| `crb_balance` | DECIMAL | Credite BursaTransport disponibile |
| `crb_incidents_remaining` | INT | Incidente de plată rămase în pachet |
| `crb_reports_remaining` | INT | Rapoarte CreditReform rămase în pachet |
| `creditreform_risk` | ENUM | `scazut`, `mediu`, `ridicat` — din raport |
| `creditreform_credit_limit` | DECIMAL | Limita de credit (RON) din raport |
| `insolvency_declared` | BOOLEAN | Flag insolvență |
| `insolvency_date` | DATE | Data declarației de insolvență |
| `score_as_shipper` | DECIMAL | Scor mediu primit ca expeditor |
| `score_as_carrier` | DECIMAL | Scor mediu primit ca transportator |
| `score_as_buyer` | DECIMAL | Scor mediu primit ca beneficiar (Contracte) |
| `score_as_supplier` | DECIMAL | Scor mediu primit ca furnizor (Contracte) |
| `is_active` | BOOLEAN | Cont activ pe platformă |
| `secure_login_enabled` | BOOLEAN | Login 2FA activat |

---

## 2. Entitatea `User` (Utilizator / Persoana de Contact)

Un cont individual angajat al companiei. O companie poate avea mai mulți utilizatori.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `location_id` | FK → Location | Biroul/sediul de care aparține |
| `username` | STRING | User de login |
| `password_hash` | STRING | Parolă hashată |
| `email` | STRING | |
| `phone` | STRING | |
| `skype` | STRING | Optional |
| `first_name` | STRING | |
| `last_name` | STRING | |
| `role_in_company` | STRING | Ex: `Expeditor`, `Dispecer`, `Administrator` |
| `avatar_url` | STRING | Poza de profil |
| `languages` | ARRAY[STRING] | Limbi vorbite (ISO 639), flaguri de țări |
| `user_type` | ENUM | `principal`, `extra` |
| `is_active` | BOOLEAN | |
| `score_personal` | DECIMAL | Rating personal (tranzacții) |
| `cargo_posted_count` | INT | Nr. mărfuri publicate |
| `truck_posted_count` | INT | Nr. camioane publicate |
| `last_login_at` | DATETIME | |
| `created_at` | DATETIME | |

---

## 3. Entitatea `Location` (Locație / Birou al Companiei)

O companie poate avea mai multe sedii cu echipe separate.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `name` | STRING | Ex: `Birou Sibiu`, `Birou RUSE` |
| `address` | STRING | Adresa completă |
| `city` | STRING | |
| `county` | STRING | |
| `country` | STRING | ISO 3166 |
| `postal_code` | STRING | |
| `phone` | STRING | |
| `fax` | STRING | Optional |
| `email` | STRING | |
| `contact_persons_count` | INT | Persoane de contact alocate |
| `users_count` | INT | Conturi utilizator alocate |
| `vehicles_count` | INT | Vehicule declarate la locație |

---

## 4. Entitatea `Vehicle` (Vehicul / Camion din Parc Auto)

Fiecare vehicul declarat de o companie în flota sa.

| Câmp | Tip | Note / Valori posibile |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `location_id` | FK → Location | Optional |
| `plate_number` | STRING | Număr înmatriculare |
| `type` | ENUM | `duba`, `prelata`, `platforma`, `agabaritic`, `cisterna`, `cap_tractor`, `basculanta`, `container`, `transport_autoturisme`, `cisterna_alimentara` |
| `capacity_tons` | DECIMAL | Capacitate tonaj (t) |
| `capacity_m3` | DECIMAL | Capacitate volum (m³) |
| `floor_length_m` | DECIMAL | Lungime podea (m) |
| `has_adr` | BOOLEAN | Certificat ADR (mărfuri periculoase) |
| `has_frigo` | BOOLEAN | Echipament frigorific |
| `has_walking_floor` | BOOLEAN | Walking Floor |
| `has_lift` | BOOLEAN | Lift hidraulic |
| `is_mega_trailer` | BOOLEAN | Mega Trailer |
| `has_stendere` | BOOLEAN | Stendere (prelată extensibilă) |
| `car_transport_slots` | INT | Nr. locuri transport autoturisme |
| `container_type` | ENUM | `20ft`, `40ft`, `45ft`, null |
| `mode` | ENUM | `ftl`, `ltl` — Complet sau grupaj |
| `year` | INT | Anul fabricației |
| `brand` | STRING | Ex: `Volvo`, `MAN`, `DAF` |
| `is_active` | BOOLEAN | Disponibil pe platformă |

---

## 5. Entitatea `Warehouse` (Depozit)

Spații de depozitare declarate de companie.

| Câmp | Tip | Note / Valori posibile |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `location_id` | FK → Location | Optional — biroul aferent |
| `type` | ENUM | `tranzit`, `gestiune_stoc`, `frigorific` |
| `surface_m2` | DECIMAL | Suprafață (m²) |
| `volume_m3` | DECIMAL | Volum (m³) |
| `address` | STRING | Adresa depozitului |
| `city` | STRING | |
| `country` | STRING | |
| `notes` | TEXT | |

---

## 6. Entitatea `CargoListing` (Marfă Spot)

Cerere de transport publicată de un expeditor.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `platform_code` | STRING | Codul afișat (ex: `BM-176763779`) |
| `user_id` | FK → User | Expeditorul |
| `company_id` | FK → Company | |
| `loading_locations` | ARRAY[GeoPoint] | Puncte de încărcare (max 5) |
| `unloading_locations` | ARRAY[GeoPoint] | Puncte de descărcare (max 5) |
| `available_from` | DATE | Data de la care marfa e disponibilă |
| `available_duration_days` | INT | Zile disponibilitate |
| `vehicle_types` | ARRAY[ENUM] | Tipuri camion acceptate |
| `capacity_tons` | DECIMAL | Tonaj marfă |
| `capacity_m3` | DECIMAL | Volum marfă (m³) |
| `floor_length_m` | DECIMAL | Lungime podea necesară (m) |
| `car_transport_slots` | INT | Nr. autoturisme |
| `container_type` | ENUM | Optional |
| `requires_adr` | BOOLEAN | |
| `requires_frigo` | BOOLEAN | |
| `requires_walking_floor` | BOOLEAN | |
| `requires_lift` | BOOLEAN | |
| `requires_mega_trailer` | BOOLEAN | |
| `requires_stendere` | BOOLEAN | |
| `is_groupage` | BOOLEAN | Grupaj (LTL) |
| `cargo_description` | TEXT | Descriere publică marfă |
| `internal_notes` | TEXT | Comentarii nepublicabile |
| `price_offered` | DECIMAL | Optional |
| `price_currency` | ENUM | `RON`, `EUR`, `USD` etc. |
| `price_includes_vat` | BOOLEAN | |
| `is_highlighted` | BOOLEAN | Anunț evidențiat (+1 CRB) |
| `has_auction` | BOOLEAN | Dacă a lansat și licitație |
| `type` | ENUM | `spot`, `termen_lung` |
| `status` | ENUM | `activ`, `expirat`, `anulat`, `adjudecat` |
| `created_at` | DATETIME | |
| `expires_at` | DATETIME | |

---

## 7. Entitatea `TruckListing` (Camion Liber Spot)

Ofertă de transport publicată de un transportator.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `platform_code` | STRING | Ex: `BC-36788937` |
| `user_id` | FK → User | |
| `company_id` | FK → Company | |
| `vehicle_id` | FK → Vehicle | Optional — legare la parc auto |
| `departure_location` | GeoPoint | |
| `arrival_location` | GeoPoint | Poate fi null = „oriunde" |
| `available_from` | DATE | |
| `available_duration_days` | INT | |
| `vehicle_type` | ENUM | Tip camion (dropdown single) |
| `capacity_tons` | DECIMAL | |
| `capacity_m3` | DECIMAL | |
| `floor_length_m` | DECIMAL | |
| `car_transport_slots` | INT | |
| `container_type` | ENUM | |
| `mode` | ENUM | `ftl`, `ltl` |
| `has_adr` | BOOLEAN | |
| `has_frigo` | BOOLEAN | |
| `has_walking_floor` | BOOLEAN | |
| `has_lift` | BOOLEAN | |
| `is_mega_trailer` | BOOLEAN | |
| `has_stendere` | BOOLEAN | |
| `truck_description` | TEXT | |
| `internal_notes` | TEXT | |
| `price_per_km` | DECIMAL | Preț/km solicitat |
| `price_currency` | ENUM | |
| `price_includes_vat` | BOOLEAN | |
| `is_highlighted` | BOOLEAN | |
| `sms_notification_enabled` | BOOLEAN | Notificare SMS la marfă potrivită |
| `type` | ENUM | `spot`, `termen_lung` |
| `status` | ENUM | `activ`, `expirat`, `anulat` |
| `created_at` | DATETIME | |
| `expires_at` | DATETIME | |

---

## 8. Entitatea `AuctionRequest` (Cerere de Ofertă / Licitație)

Lansat de expeditor simultan cu o MarfăSpot.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `cargo_listing_id` | FK → CargoListing | Legată de marfă |
| `user_id` | FK → User | |
| `company_id` | FK → Company | |
| `price_without_vat` | DECIMAL | Prețul de pornire / bugetul maxim |
| `price_currency` | ENUM | |
| `km_dislocation_included` | INT | Km dislocare incluși |
| `payment_term_days` | INT | Termen de plată de la facturare (ex: 30) |
| `loading_at` | DATETIME | |
| `unloading_at` | DATETIME | |
| `valid_duration_minutes` | INT | Durata licitației (ex: 180 minute) |
| `status` | ENUM | `activ`, `expirat`, `adjudecat`, `anulat` |
| `created_at` | DATETIME | |
| `expires_at` | DATETIME | |

---

## 9. Entitatea `AuctionOffer` (Ofertă Licitație)

Depusă de un transportator la o licitație deschisă.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `auction_request_id` | FK → AuctionRequest | |
| `user_id` | FK → User | Transportatorul ofertant |
| `company_id` | FK → Company | |
| `price_offered` | DECIMAL | Prețul oferit |
| `price_currency` | ENUM | |
| `status` | ENUM | `winning`, `losing`, `adjudecat`, `retras` |
| `created_at` | DATETIME | |

---

## 10. Entitatea `PaymentIncident` (Incident de Plată)

Instrument de management al riscului financiar.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `declarant_company_id` | FK → Company | Cine declară incidentul |
| `declarant_user_id` | FK → User | |
| `accused_company_id` | FK → Company | Compania acuzată |
| `invoice_amount` | DECIMAL | Valoarea facturii neîncasate |
| `currency` | ENUM | |
| `incident_type` | ENUM | `sub_1000_lei`, `peste_1000_lei` |
| `incident_date` | DATE | Data scadenței |
| `due_date` | DATE | Data limită de plată |
| `payment_instrument` | STRING | Ex: OP, CEC, bilet la ordin |
| `description` | TEXT | |
| `attachments` | ARRAY[URL] | Documente justificative |
| `status` | ENUM | `in_asteptare`, `rezolvat`, `contestat`, `confirmat` |
| `crb_cost` | DECIMAL | Costul declarării în credite |
| `created_at` | DATETIME | |

---

## 11. Entitatea `TransportIncident` (Incident de Transport)

Situații neconforme operaționale (avarii, întârzieri).

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `declarant_company_id` | FK → Company | |
| `accused_company_id` | FK → Company | |
| `incident_type` | ENUM | `intarziere_descarcare`, `avarie_marfa`, `alta` |
| `description` | TEXT | |
| `attachments` | ARRAY[URL] | |
| `status` | ENUM | `in_asteptare`, `rezolvat`, `contestat` |
| `created_at` | DATETIME | |

---

## 12. Entitatea `Subscription` / `CreditTransaction` (Abonament și Credite)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `type` | ENUM | `flexibil`, `sprinter_3l`, `sprinter_6l`, `caraus_1l`, `caraus_3l`, `caraus_6l`, `caraus_12l`, `premium_3l`, `premium_12l` |
| `starts_at` | DATETIME | |
| `expires_at` | DATETIME | |
| `crb_included` | DECIMAL | Credite incluse în pachet |
| `incidents_included` | INT | Incidente incluse |
| `reports_included` | INT | Rapoarte incluse |
| `max_users` | INT | Utilizatori incluși |
| `price_crb` | DECIMAL | Costul în credite |
| `price_ron` | DECIMAL | Costul în LEI + TVA |
| `auto_renew` | BOOLEAN | Reînnoire automată cu cardul |
| `payment_method` | ENUM | `card`, `op_bancar`, `sms` |

### `CreditTransaction` (log tranzacții CRB)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `user_id` | FK → User | |
| `service` | ENUM | `extra_utilizator`, `zi_premium`, `incident_plata`, `report_companie`, `anunt_evidentiat`, `notificare_sms`, `consultatie_juridica`, `garantare_factura`, `alimentare_portofoliu` |
| `quantity` | INT | |
| `crb_amount` | DECIMAL | Sumă în CRB (negativ = debit) |
| `ron_amount` | DECIMAL | Echivalent RON |
| `created_at` | DATETIME | |

---

## 13. Entitatea `CompanyDocument` (Document Legal)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `type` | ENUM | `cui`, `licenta_transport`, `licenta_intermediere`, `asigurare_cmr`, `certificat_adr`, `alt` |
| `file_url` | STRING | |
| `issued_at` | DATE | |
| `expires_at` | DATE | |
| `uploaded_at` | DATETIME | |

---

## 14. Entitatea `BlacklistEntry` (Lista Neagră)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | Compania care blochează |
| `blocked_company_id` | FK → Company | Compania blocată |
| `blocked_user_id` | FK → User | Optional — utilizator specific |
| `reason` | TEXT | |
| `created_at` | DATETIME | |

---

## 15. Entitatea `ForumPost` (Discuții / Comunitate)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `parent_id` | FK → ForumPost | null = subiect nou, altfel = răspuns |
| `user_id` | FK → User | |
| `company_id` | FK → Company | |
| `category` | ENUM | `discutii_generale`, `alta` |
| `title` | STRING | Doar pentru subiect principal |
| `body` | TEXT | |
| `views_count` | INT | |
| `replies_count` | INT | |
| `created_at` | DATETIME | |
| `updated_at` | DATETIME | |

---

## 16. Entitatea `ClassifiedAd` (Mică Publicitate)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `user_id` | FK → User | |
| `company_id` | FK → Company | |
| `category` | ENUM | `vanzari_camioane`, `vanzari_marfa`, `servicii`, `alta` |
| `title` | STRING | |
| `description` | TEXT | |
| `images` | ARRAY[URL] | |
| `price` | DECIMAL | Optional |
| `currency` | ENUM | |
| `phone_contact` | STRING | |
| `is_active` | BOOLEAN | |
| `created_at` | DATETIME | |
| `expires_at` | DATETIME | |

---

## 17. Entitatea `Notification` (Notificări)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `user_id` | FK → User | |
| `type` | ENUM | `incident_plata_nou`, `marfa_potrivita_sms`, `oferta_licitatie`, `mesaj_forum`, `alta` |
| `title` | STRING | |
| `body` | TEXT | |
| `channel` | ENUM | `in_app`, `sms`, `email` |
| `is_read` | BOOLEAN | |
| `created_at` | DATETIME | |

---

## 18. Entitatea `SavedRoute` (Rute Salvate)

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `user_id` | FK → User | |
| `name` | STRING | Eticheta rutei |
| `loading_locations` | ARRAY[GeoPoint] | |
| `unloading_locations` | ARRAY[GeoPoint] | |
| `route_type` | ENUM | `marfa`, `camion` |
| `created_at` | DATETIME | |

---

## 19. Entitatea `PlatformInvoice` (Facturi Contract Platformă)

Facturi emise de platforma B2B (DacodaSoft/BursaTransport) către companie.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | |
| `invoice_number` | STRING | |
| `description` | STRING | Ex: `servicii in luna Ianuarie 2026` |
| `amount_ron` | DECIMAL | |
| `vat_amount_ron` | DECIMAL | |
| `issued_at` | DATE | |
| `downloaded_by_user_id` | FK → User | |
| `pdf_url` | STRING | |

---

## 20. Entitatea `ExternalVerification` (Verificări Externe)

Log al verificărilor efectuate prin integrările externe.

| Câmp | Tip | Note |
|---|---|---|
| `id` | UUID / INT | |
| `company_id` | FK → Company | Cine a făcut verificarea |
| `user_id` | FK → User | |
| `target_cui` | STRING | CUI-ul verificat |
| `source` | ENUM | `anaf`, `ministerul_finantelor`, `arr`, `insolventa`, `creditreform_simplu`, `creditreform_premium` |
| `result_snapshot` | JSON | Snapshot răspuns extern |
| `cost_crb` | DECIMAL | |
| `created_at` | DATETIME | |

---

## Tipuri de Date Comune (Enums Globale)

```
VEHICLE_TYPE: duba | prelata | platforma | agabaritic | cisterna | cap_tractor | basculanta | container | transport_autoturisme | cisterna_alimentara

CURRENCY: RON | EUR | USD | GBP | HUF | PLN | BGN | CZK

LANGUAGE: ro | en | de | fr | it | pl | bg | hu | cz | (alte ISO 639-1)

COUNTRY: ISO 3166-1 alpha-2 (RO | DE | FR | IT | BG | HU | ...)

SUBSCRIPTION_TIER: flexibil | sprinter | caraus | premium
```
