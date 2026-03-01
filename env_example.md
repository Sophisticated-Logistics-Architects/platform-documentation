# .env.example — Variabile de Mediu

Copiază acest fișier ca `.env` și completează valorile reale.  
**Nu comite niciodată `.env` în git.**

```bash
# ─────────────────────────────────────────
# DATABASE (Supabase)
# ─────────────────────────────────────────
DATABASE_URL="postgresql://postgres:[PASSWORD]@db.[PROJECT].supabase.co:5432/postgres"
DIRECT_URL="postgresql://postgres:[PASSWORD]@db.[PROJECT].supabase.co:5432/postgres"

# Supabase SDK (pentru Realtime + Storage)
PUBLIC_SUPABASE_URL="https://[PROJECT].supabase.co"
PUBLIC_SUPABASE_ANON_KEY="eyJ..."
SUPABASE_SERVICE_ROLE_KEY="eyJ..."

# ─────────────────────────────────────────
# AUTH (Lucia Auth)
# ─────────────────────────────────────────
AUTH_SECRET="[random 32 char string - openssl rand -base64 32]"

# ─────────────────────────────────────────
# PAYMENTS (Stripe)
# ─────────────────────────────────────────
STRIPE_SECRET_KEY="sk_live_..."          # sk_test_... în development
STRIPE_WEBHOOK_SECRET="whsec_..."
PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_live_..."

# Stripe Price IDs (creare în Stripe Dashboard)
STRIPE_PRICE_FLEXIBIL_DAY="price_..."
STRIPE_PRICE_SPRINTER_3L="price_..."
STRIPE_PRICE_SPRINTER_6L="price_..."
STRIPE_PRICE_SPRINTER_12L="price_..."
STRIPE_PRICE_CARAUS_1L="price_..."
STRIPE_PRICE_CARAUS_3L="price_..."
STRIPE_PRICE_CARAUS_6L="price_..."
STRIPE_PRICE_CARAUS_12L="price_..."
STRIPE_PRICE_PREMIUM_3L="price_..."
STRIPE_PRICE_PREMIUM_12L="price_..."

# ─────────────────────────────────────────
# EMAIL (Resend)
# ─────────────────────────────────────────
RESEND_API_KEY="re_..."
EMAIL_FROM="noreply@[domeniu].ro"

# ─────────────────────────────────────────
# MAPS (OpenStreetMap — fără API key necesar)
# MapLibre GL JS folosește tile-uri publice OSM
# ─────────────────────────────────────────
# Opțional: Nominatim self-hosted pentru geocoding
# PUBLIC_NOMINATIM_URL="https://nominatim.openstreetmap.org"

# ─────────────────────────────────────────
# VERIFICATIONS (Simulat în MVP)
# ─────────────────────────────────────────
ANAF_API_URL="https://webservicesp.anaf.ro/PlatitorTvaRest/api/v8/ws/tva"
ANAF_MOCK_MODE="true"                    # false în producție cu integrare reală

# ─────────────────────────────────────────
# APP CONFIG
# ─────────────────────────────────────────
PUBLIC_APP_URL="http://localhost:4321"   # https://[domeniu].ro în producție
PUBLIC_APP_NAME="[Nume Platformă]"       # TBD
NODE_ENV="development"                   # production
```
