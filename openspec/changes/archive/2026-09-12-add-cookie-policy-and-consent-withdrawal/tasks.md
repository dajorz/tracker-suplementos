## 1. GA4 measurement ID

- [x] 1.1 Replace the `G-XXXXXXXXXX` placeholder in `index.html` with the real measurement ID `G-Y49TC503Y9`
- [x] 1.2 Update the GA4 note in `README.md` to state the real ID and that it loads only after consent
- [x] 1.3 Remove the code comment instructing the reader to replace the placeholder

## 2. Cookie policy dialog

- [x] 2.1 Add a `<dialog id="cookie-policy-dialog">` after `<main>` with a scrollable, max-height inner container so it fits small viewports
- [x] 2.2 Write the policy body: no cookies before consent, the `_ga` / `_ga_Y49TC503Y9` cookies with purpose and ~2-year retention, Google Ireland Limited with links to its privacy policy and opt-out add-on, the `cookie-consent` localStorage key, the embedded Google Sheets iframe, and how to change the choice
- [x] 2.3 Add a "Cerrar" button inside a `<form method="dialog">` so the dialog closes without JavaScript
- [x] 2.4 Add a "Más información" button to the consent banner text and wire it to `showModal()`

## 3. Consent revocation

- [x] 3.1 Add a one-line bar below `<main>` containing a "Cookies" button styled as small, low-contrast text
- [x] 3.2 Wire the button to unhide the consent banner
- [x] 3.3 Add `clearAnalyticsCookies()` that expires every `_ga`-prefixed cookie against both the root path and the current hostname
- [x] 3.4 Call it from the reject handler, and reload the page when `gtagLoaded` is true
- [x] 3.5 Guard `loadGtag()` with the `gtagLoaded` flag so repeated acceptance injects only one script

## 4. Verification

- [x] 4.1 Confirm the banner appears on first visit and "Más información" opens the policy above it
- [x] 4.2 Confirm "Cerrar" dismisses the dialog and "Aceptar" hides the banner
- [x] 4.3 Confirm the "Cookies" bar reopens the banner after a choice has been stored
- [x] 4.4 Confirm the policy adds no vertical space to the page while closed
- [x] 4.5 On the deployed page, reject after accepting and confirm the `_ga` cookies are gone and no further `google-analytics.com/g/collect` requests are made
- [x] 4.6 On the deployed page, accept consent and confirm the visit appears in GA4 Realtime
