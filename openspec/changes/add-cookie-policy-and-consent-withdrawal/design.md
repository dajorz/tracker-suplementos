## Context

`index.html` is a single static file on GitHub Pages, styled with the Tailwind CDN, with no backend and no consent-management SaaS. The `compact-spreadsheet-first-header` change deliberately deleted the footer and reordered `<main>` so the spreadsheet iframe starts as close to the top of the viewport as possible; any new bottom-of-page element works against that goal and must justify its vertical cost.

The consent gate already exists: a fixed banner with accept/reject, the choice persisted in `localStorage` under `cookie-consent`, and `gtag.js` injected only on acceptance. Two gaps remain. First, the visitor is asked to consent without being told what is stored, by whom, or for how long. Second, a stored choice is final — the banner never reappears, so consent cannot be withdrawn without clearing site data manually. Both are required by GDPR (Art. 7.3: withdrawal as easy as giving) and LSSI-CE. The placeholder measurement ID has now been replaced by a real GA4 property (`G-Y49TC503Y9`), so these gaps affect real third-party cookies rather than an inert snippet.

## Goals / Non-Goals

**Goals:**
- Make the cookie policy readable *before* the visitor consents.
- Give a permanent, always-available way to change or withdraw the stored choice.
- Ensure rejection after acceptance actually stops the tracking and removes the cookies it set.
- Keep the added vertical footprint close to zero so the spreadsheet stays above the fold.
- Keep the page a single self-contained file with no new dependencies.

**Non-Goals:**
- A general privacy policy or legal notice covering anything beyond cookies.
- Granular per-purpose consent categories — there is exactly one non-essential purpose (analytics), so a single accept/reject pair is sufficient.
- Server-side consent logging or proof-of-consent records.
- Restoring the author credit that the footer used to carry.

## Decisions

**Cookie policy in a native `<dialog>`, not inline page content.** An inline section (even collapsed in a `<details>`) adds a permanent block below `<main>` and pushes the page taller, which is exactly what the previous change worked to avoid. A modal renders in the browser's top layer, so it costs zero layout space until opened and appears above the fixed consent banner without any `z-index` bookkeeping. *Alternatives considered:* a separate `politica-cookies.html` page — rejected because it breaks the single-file deployment model and navigating away from the page loses the visitor's place in the spreadsheet; a `<details>` disclosure in a footer — rejected on vertical cost, and it was the first attempt at this change before the layout conflict was noticed.

**Native `<dialog>` with no polyfill.** Supported by all current evergreen browsers. In an unsupported browser the element degrades to inline rendering: the policy text is still present and readable in the document, which is the legally relevant outcome, and `showModal()` throwing would leave the page otherwise functional. Adding a polyfill would mean a second CDN dependency for a shrinking tail of browsers.

**Policy entry point inside the consent banner.** The information has to be available before consent is given, so the "Más información" link lives in the banner itself rather than only at the bottom of the page. *Alternative considered:* a second link in the bottom bar next to "Cookies" — rejected as redundant, since the bottom bar reopens the banner and the banner links to the policy; two clicks is an acceptable path for a returning visitor.

**A one-line bar rather than a restored footer.** The removed footer was a content block with promotional copy; this is a single line of `text-xs` in `text-slate-400` carrying one control, roughly 30px. The `compact-spreadsheet-first-header` rationale — "the footer's claims are redundant and it sits where nobody reads it" — does not transfer, because a consent-revocation control is a legal requirement rather than redundant copy, and convention puts it exactly where visitors look for it. *Alternative considered:* placing "Cookies" in the amber disclaimer strip above the sheet — rejected because it would dilute a warning that must be read before the prices, and above-the-fold space is the scarcest resource on the page.

**Reload on withdrawal.** Once `gtag.js` is injected it cannot be unloaded; removing the `<script>` element does not stop the already-initialised tag. Setting `window['ga-disable-<ID>'] = true` suppresses future hits but leaves the tag resident and is easy to get wrong. Reloading is unambiguous: the stored choice is now `rejected`, so the gate simply never injects the script. The reload is conditional on `gtagLoaded`, so a visitor who rejects without ever having accepted is not disturbed.

**Delete `_ga` cookies by prefix on rejection.** Rejecting must not leave the identifiers that acceptance created. Cookies are cleared by prefix (`_ga`, covering both `_ga` and `_ga_Y49TC503Y9`) with `Max-Age=0` against both the bare path and the current hostname, since GA sets them on the registrable domain.

## Risks / Trade-offs

- [The bottom bar re-adds vertical space to a layout that was just compacted] → Mitigation: capped at one line of `text-xs`; the iframe sits at `80vh` and the bar lands below it, so the above-the-fold composition is unchanged.
- [Reloading on withdrawal loses scroll position inside the spreadsheet iframe] → Accepted: withdrawal is a rare, deliberate action, and correctness of the consent state outweighs the interruption.
- [Cookie deletion assumes cookies are scoped to the current hostname and root path] → Holds for `dajorz.github.io`; would need revisiting if the site moves to a custom domain with subdomains. Failure mode is a stale cookie, not a functional break, and the tag is no longer running to read it.
- [`<dialog>` unsupported in an old browser] → Degrades to inline rendering of the policy text; `showModal()` may throw but the consent gate itself is unaffected.
- [Policy text hardcodes the measurement ID `_ga_Y49TC503Y9`] → It will drift if the GA4 property is ever replaced; the ID appears in exactly two places in `index.html`, both greppable.
- [Visitors who withdraw consent reduce analytics volume] → Accepted; under-reporting is the correct outcome versus retaining consent that has been revoked.
