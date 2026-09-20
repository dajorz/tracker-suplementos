## 1. Page Scaffold

- [x] 1.1 Create `index.html` at repo root with HTML5 boilerplate, `<html lang="es">`, and Tailwind CDN `<script src="https://cdn.tailwindcss.com">` in `<head>`
- [x] 1.2 Set page `<title>` and meta viewport tag for responsive rendering
- [x] 1.3 Add SEO meta tags: `description`, and `<link rel="canonical" href="https://dajorz.github.io/tracker-suplementos/">`
- [x] 1.4 Add Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`) and `twitter:card` / `twitter:title` / `twitter:description`

## 2. Header / Hero

- [x] 2.1 Add title "Tracker de Precios de Suplementación" and subtitle text
- [x] 2.2 Add "Actualizado en tiempo real" badge with a green status dot

## 3. Product Suggestion CTA

- [x] 3.1 Add CTA section with heading "¿Echas en falta algún producto?" and subtext "Escríbeme y lo añado al tracker — o cuéntame cualquier sugerencia sobre la herramienta."
- [x] 3.2 Add an `<a>` styled as a button labeled "Proponer producto" — no form element and no email input
- [x] 3.3 Assemble the `mailto:` href in JavaScript on load, storing the address as separate user and domain fragments (never written out as a literal anywhere in this repository) with subject `Sugerencia para el tracker`, so it never appears literally in the served HTML
- [x] 3.4 Add a code comment explaining that the fragments are split deliberately for anti-scraping and must stay split

## 4. Google Sheet Embed

- [x] 4.1 Add responsive container (`w-full`) with `<iframe>` using the provided pubhtml URL: `https://docs.google.com/spreadsheets/u/3/d/e/2PACX-1vS4kFZXNEAWRMb34W3yZiF3HItRKxUTmHtrLlq0wyfGXUSEwm7NrtQgKoQPmC2zhyKAbnz5Li6JGSY3/pubhtml`
- [x] 4.2 Set `width="100%"`, height via `80vh` (min 600px), `loading="lazy"`, `rounded-xl`, `shadow-lg`, `border-0`

## 5. Analytics & Consent

- [x] 5.1 Add a cookie-consent banner (fixed at the bottom) with a short notice, an "Aceptar" button and a "Rechazar" button
- [x] 5.2 Persist the visitor's choice in `localStorage` and keep the banner hidden on subsequent visits
- [x] 5.3 Inject the GA4 `gtag.js` script with placeholder measurement ID `G-XXXXXXXXXX` **only** after consent is accepted — no analytics script or cookie before that
- [x] 5.4 Document (in a code comment) that the user must replace `G-XXXXXXXXXX` with their real GA4 measurement ID

## 6. Footer

- [x] 6.1 Add footer with text "Desarrollado por @dajorz · Herramienta independiente de comparación." and link to `https://github.com/dajorz`

## 7. Verification & Deployment Guidance

- [x] 7.1 Manually check page in a mobile-width and desktop-width viewport for layout correctness
- [x] 7.2 Confirm iframe loads the published sheet correctly in a browser
- [x] 7.3 Verify no `_ga` cookie and no request to `googletagmanager.com` occurs before accepting consent, and that both appear after accepting
- [x] 7.4 Verify the "Proponer producto" button opens a mail client addressed to the site owner with the prefilled subject, and that the address does not appear in the page source
- [x] 7.5 Add brief GitHub Pages enablement note (Settings → Pages → deploy from root of default branch) to repo README or PR description
- [x] 7.6 Guide the user through creating a real GA4 property and replacing the placeholder measurement ID (follow-up, tracked separately from this change's completion)
