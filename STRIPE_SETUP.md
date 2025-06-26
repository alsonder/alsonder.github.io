# Østerbro Jagttegn - Stripe Integration Setup

Dette projekt indeholder en komplet Stripe betalingsintegration for jagttegnskurser.

## 🚀 Hurtig Setup

### 1. Opret Stripe Konto
1. Gå til [stripe.com](https://stripe.com) og opret en konto
2. Verificer din konto og tilføj virksomhedsoplysninger
3. Aktiver danske betalingsmetoder (Dankort, Visa, Mastercard)

### 2. Hent Stripe API Keys
1. Gå til [Stripe Dashboard > API Keys](https://dashboard.stripe.com/apikeys)
2. Kopiér din **Publishable key** (pk_test_... eller pk_live_...)
3. Kopiér din **Secret key** (sk_test_... eller sk_live_...)

### 3. Opdater Frontend
I `tilmelding.html`, linje ~128:
```javascript
const stripe = Stripe('pk_test_REPLACE_WITH_YOUR_PUBLISHABLE_KEY');
```
Erstat `pk_test_REPLACE_WITH_YOUR_PUBLISHABLE_KEY` med din rigtige publishable key.

### 4. Deploy Backend (Vælg én af følgende)

#### Option A: Vercel (Anbefalet)
1. Installer Vercel CLI: `npm i -g vercel`
2. Login: `vercel login`
3. Deploy: `vercel --prod`
4. Tilføj environment variabler i Vercel dashboard:
   - `STRIPE_SECRET_KEY`: Din secret key
   - `STRIPE_WEBHOOK_SECRET`: (sættes op senere)

#### Option B: Netlify
1. Flyt filerne fra `api/` til `netlify/functions/`
2. Deploy til Netlify
3. Tilføj environment variabler i Netlify settings

### 5. Opsæt Webhook
1. Gå til [Stripe Dashboard > Webhooks](https://dashboard.stripe.com/webhooks)
2. Klik "Add endpoint"
3. URL: `https://din-domain.vercel.app/api/stripe-webhook`
4. Events: Vælg `checkout.session.completed`
5. Kopiér webhook secret og tilføj som `STRIPE_WEBHOOK_SECRET`

### 6. Test Betaling
1. Brug Stripe test kort: `4242 4242 4242 4242`
2. Enhver fremtidig dato for udløb
3. Enhver 3-cifret CVC

## 🇩🇰 Danske Betalinger

Stripe understøtter følgende danske betalingsmetoder:
- **Dankort** (via Stripe)
- **Visa/Mastercard**
- **Apple Pay / Google Pay**
- **Bankkort fra alle danske banker**

## 💰 Priser og Gebyrer

**Stripe gebyrer (2024):**
- EU-kort: 1,4% + 1,80 kr
- Ikke-EU kort: 2,9% + 1,80 kr
- Ingen månedlige gebyrer
- Ingen setup gebyrer

**Eksempel for 3.500 kr kursus:**
- Gebyr: ~51 kr (1,4% + 1,80 kr)
- Du modtager: ~3.449 kr

## 🔒 Sikkerhed

- Stripe er PCI DSS Level 1 certificeret
- Ingen kortoplysninger gemmes på din server
- SSL kryptering på alle transaktioner
- 3D Secure support for danske kort

## 📧 Email Integration

For at sende bekræftelsesmails, tilføj en email service:

**SendGrid (Gratis tier):**
```bash
npm install @sendgrid/mail
```

**Mailgun:**
```bash
npm install mailgun.js
```

Se `api/stripe-webhook.js` for email implementering.

## 🛠️ Udvikling Lokalt

```bash
# Installer dependencies
npm install

# Start udviklings server
vercel dev

# Test webhook lokalt
stripe listen --forward-to localhost:3000/api/stripe-webhook
```

## 📱 Mobile Betaling

Stripe Checkout er fuldt responsivt og understøtter:
- Mobile Safari (iOS)
- Chrome Mobile (Android)
- Apple Pay
- Google Pay

## 🆘 Support

**Stripe Dokumentation:**
- [Stripe Checkout](https://stripe.com/docs/checkout)
- [Webhook Guide](https://stripe.com/docs/webhooks)
- [Danish Payments](https://stripe.com/docs/payments/payment-methods/overview#denmark)

**Fejlfinding:**
1. Tjek browser console for JavaScript fejl
2. Verificer API keys er korrekte
3. Tjek webhook endpoint returnerer 200 status
4. Se Stripe Dashboard > Logs for detaljerede fejlmeddelelser

## 📞 Kontakt

Aleksander Sønder  
Email: aleksander.soender@gmail.com  
Tlf: +45 26160630
