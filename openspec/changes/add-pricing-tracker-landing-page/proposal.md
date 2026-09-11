## Why

The "tracker-suplementos" repo currently has no public-facing page. A Google Sheet already tracks creatine/protein prices in Spain, but sharing a raw Google Sheets URL looks unprofessional and gives visitors no way to reach the author. The most valuable input from visitors is coverage feedback — which products, brands or shops are missing from the sheet. A lightweight GitHub Pages landing page can present the sheet, build trust, and open a direct channel for those suggestions.

## What Changes

- Add a single static `index.html` at the repo root, deployable via GitHub Pages, with no build step or dependencies beyond CDN-hosted Tailwind CSS.
- Add SEO and social-sharing meta tags (`lang="es"`, description, canonical, Open Graph, Twitter card).
- Add a hero header with title, subtitle, and a "live update" status badge.
- Add a product-suggestion call to action positioned above the embedded sheet: copy asking "¿Echas en falta algún producto?" and a "Proponer producto" `mailto:` link styled as a button, with a prefilled subject. No third-party form service, and the address is assembled in JavaScript so it never appears in the served HTML.
- Embed the published Google Sheet in a responsive, lazy-loaded `<iframe>`.
- Add Google Analytics 4 (`gtag.js`) tracking with a placeholder measurement ID (`G-XXXXXXXXXX`), gated behind a cookie-consent banner so no analytics cookies are set before the visitor accepts.
- Add a footer with author credit and a link to the GitHub profile.

## Capabilities

### New Capabilities
- `pricing-tracker-landing-page`: A static, single-file HTML landing page (SEO/OG meta, header, `mailto:` product-suggestion CTA, embedded Google Sheet iframe, consent-gated GA4 analytics, footer) deployed via GitHub Pages.

### Modified Capabilities
(none)

## Impact

- Affected files: new `index.html` at repo root. GitHub Pages is served from the root of the default branch.
- External dependencies: Tailwind CSS CDN script, Google Analytics `gtag.js` CDN script (loaded only after consent), published Google Sheets URL (provided by user).
- No backend, build tooling, form service, or server-side code introduced.
- Accepted trade-off: `mailto:` builds no mailing list and converts worse than a hosted form. Suggestions arrive as inbound emails the author triages manually. Price-drop alerts are explicitly out of scope for this change.
- Follow-up action required from the user: create a GA4 property and replace the `G-XXXXXXXXXX` placeholder. The contact address (`virtualtoolsapps@gmail.com`) and canonical URL (`https://dajorz.github.io/tracker-suplementos/`) are confirmed.
