## Context

The repo has no site yet. GitHub Pages will serve directly from the repository root on the default branch (no Jekyll build, no `_config.yml`), so the deliverable must be a single self-contained `index.html` with all styling and scripts inline or CDN-loaded. The data source is a Google Sheet already published to the web (`pubhtml` URL), which Google serves inside an `<iframe>`-friendly page. There is no backend and no third-party form service: visitor feedback arrives as `mailto:` messages in the author's inbox, and analytics goes to Google's `gtag.js` only after the visitor consents.

## Goals / Non-Goals

**Goals:**
- Single static HTML file, zero build step, deployable by pushing to GitHub Pages.
- Responsive on mobile and desktop using Tailwind CSS (CDN build).
- Embed the published Google Sheet with lazy loading and sensible fallback sizing.
- Give visitors a zero-dependency way to suggest missing products, brands or shops (`mailto:`).
- Be shareable: correct language, description, canonical URL, and social card metadata.
- Wire up GA4 behind a consent gate, with a placeholder measurement ID and documented follow-up steps.

**Non-Goals:**
- No custom backend, database, serverless function, or third-party form service.
- No price-drop alerts and no mailing list — inbound suggestions are handled manually by the author.
- No build tooling (bundlers, npm scripts, frameworks) — plain HTML/CSS/JS only.
- No authentication, personalization, or dynamic server-rendered content.
- Not responsible for the content/formatting of the Google Sheet itself.

## Decisions

- **Tailwind via CDN script (`https://cdn.tailwindcss.com`) instead of a build pipeline**: keeps the project a single file with no `npm install`/build step, matching the "one HTML file" deliverable. Trade-off: slightly larger runtime CSS generation cost in-browser vs. a compiled stylesheet; acceptable for a low-traffic landing page.
- **A product-suggestion CTA instead of a price-alert signup**: the author's actual goal is coverage feedback ("which product is missing?"), and alerts would be a promise the page cannot keep without a mailing list or automation. A concrete question ("¿Echas en falta algún producto?") also converts better than a generic "Contacto". The prefilled subject `Sugerencia para el tracker` makes inbox filtering trivial.
- **`mailto:` link instead of a hosted form (Formspree)**: removes a third-party dependency, a data processor, and a submission quota. A plain link is chosen over an email input wired to `mailto:` via JS, because an input field implies server-side capture that does not exist — the visitor's mail client supplies their address anyway, so the input would be misleading. Trade-off: no list is built and conversion is materially lower, especially on mobile webmail.
- **Email address assembled in JavaScript rather than written as a literal `mailto:` href**: a plain address on a public GitHub Pages site is harvested by scrapers within weeks. The page already ships JS for the consent banner, so building the `href` at runtime costs nothing structurally. Trade-off: the CTA is inert with JS disabled; acceptable given the consent banner has the same dependency.
- **`<iframe loading="lazy">` pointing at the Google Sheets `pubhtml` URL** rather than fetching sheet data via an API: avoids needing API keys/CORS handling; Google's published-to-web view is already an embeddable HTML page.
- **GA4 loaded only after explicit consent**, rather than on page load: the audience is in Spain, so GDPR/LSSI-CE require opt-in before analytics cookies are set. The `gtag.js` tag is injected by a small vanilla-JS handler when the visitor accepts; the choice is persisted in `localStorage`. This keeps the page a single file with no consent-management SaaS.
- **GA4 placeholder ID** (`G-XXXXXXXXXX`): requires a property the user owns; the code ships wired up and inert until substituted. The contact address (`virtualtoolsapps@gmail.com`) and canonical URL (`https://dajorz.github.io/tracker-suplementos/`) are known and hardcoded.
- **Static SEO/OG meta tags** rather than a generated sitemap or structured data: the site is one page, so hand-written `description`, `canonical`, `og:*`, and `twitter:card` tags cover sharing on WhatsApp/X/LinkedIn at no complexity cost.

## Risks / Trade-offs

- [Google may stop serving the published sheet or change its `pubhtml` markup] → Mitigation: iframe is isolated and easily replaced; no other part of the page depends on sheet internals.
- [`mailto:` converts poorly] → Accepted: this is the deliberate trade-off for having no backend and no data processor. If suggestion volume matters later, revisit with a hosted form.
- [The CTA does not work with JavaScript disabled, since the address is assembled at runtime] → Accepted: anti-scraping is worth more than the sliver of no-JS traffic; the consent banner already requires JS.
- [Tailwind CDN script adds an external network dependency and a console warning about production use] → Mitigation: acceptable for a small personal project; documented as a known trade-off, not a blocker.
- [Consent banner suppresses analytics for visitors who decline or ignore it] → Accepted: under-reporting is the correct outcome versus setting cookies without consent.
- [Placeholder GA4 ID or email left unreplaced] → Mitigation: proposal calls out the follow-up steps; task list includes reminders.

## Migration Plan

- New file only; no existing functionality to migrate.
- Deploy by enabling GitHub Pages on the repo (Settings → Pages → serve from root of default branch) and pushing `index.html`.
- Rollback: remove/revert `index.html` or disable GitHub Pages.

## Open Questions

- `og:image` is omitted for now (a single-file page ships no image asset). Adding a preview image is a follow-up if social cards look too bare.
- The canonical URL assumes the default GitHub Pages domain; it must be updated if a custom domain is added later.
- None blocking; only the GA4 property creation remains as a user follow-up, covered in tasks.md.
