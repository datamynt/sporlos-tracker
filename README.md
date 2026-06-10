# Sporløs tracker

> Sporingsscriptet til [Sporløs](https://sporlos.no) — norsk, cookieløs, samtykkefri
> webanalyse. Dette er nøyaktig den koden som kjører i besøkernes nettlesere,
> publisert åpent så hvem som helst kan etterprøve personvern-løftene.
>
> *The tracking script for [Sporløs](https://sporlos.no), a Norwegian cookieless,
> consent-free web analytics service. Published openly so anyone can verify the
> privacy claims. English summary at the bottom.*

## Hva scriptet gjør — og ikke gjør

**Sender (per sidevisning):** site-ID, sti (`location.pathname`), referrer,
og hvitlistede `utm_source` / `utm_medium` / `utm_campaign`.

**Aldri:**

- ❌ Setter cookies eller skriver til `localStorage` / `sessionStorage`
- ❌ Leser eller sender IP-adresser (serveren forkaster IP etter en daglig-roterende hash)
- ❌ Fingerprinter (ingen canvas, ingen fonter, ingen skjermoppløsning)
- ❌ Sender query-strenger (kan inneholde personopplysninger) — kun de tre utm-nøklene over
- ❌ Følger besøkende på tvers av dager eller nettsteder

Fordi scriptet aldri lagrer eller leser noe på besøkerens enhet, utløses ikke
samtykkekravet i ekomloven § 3-15 — og uten personopplysninger utløses heller
ikke GDPR-samtykke. Derfor trenger du ikke cookie-banner.

Les selv: [`sporlos.js`](./sporlos.js) er ~45 linjer kommentert JavaScript (~1,5 kB).

## Installasjon

Lim inn i `<head>` i den delte malen din (én gang — gjelder hele nettstedet):

```html
<script defer data-site="DIN_SITE_ID"
        data-api="https://sporlos.no/api/event"
        src="https://sporlos.no/sporlos.js"></script>
```

Site-ID får du i [Sporløs-dashbordet](https://sporlos.no/app) under «Vis sporings-kode».
Self-hoster du Sporløs, peker du `data-api` og `src` på din egen server.

- **WordPress:** offisiell plugin gjør dette automatisk (kommer på wordpress.org)
- **Wix / Squarespace o.l.:** lim inn i nettstedets felles «head»-felt
- **SPA (React/Vue/…):** legg i `index.html` — scriptet håndterer `pushState`-navigasjon

## Egendefinerte hendelser

```js
// fra egen kode:
window.sporlos("signup");

// eller deklarativt:
<button data-sporlos-event="kjop">Kjøp</button>
```

## Spørsmål og svar

**Hvorfor er bare scriptet åpent, ikke hele tjenesten?**
Scriptet er den delen som kjører hos *dine besøkende* — den flaten bør være
etterprøvbar for alle. Resten av Sporløs er planlagt åpnet (AGPL) som et eget,
bevisst steg.

**Kan jeg bruke scriptet mot min egen backend?**
Ja — MIT-lisens. `data-api` kan peke hvor som helst.

## English summary

This is the complete tracking script served from `sporlos.no/sporlos.js`.
It sends site ID, pathname, referrer and whitelisted `utm_*` keys per pageview.
It never sets cookies, never touches `localStorage`, never fingerprints, and the
server discards IPs after computing a daily-rotating hash — which is why sites
using it do not need a consent banner under Norwegian/EU law. MIT licensed;
point `data-api` anywhere you like.

---

Et produkt fra [Datamynt AS](https://datamynt.no) · org.nr 936 017 207 · [sporlos.no](https://sporlos.no)
