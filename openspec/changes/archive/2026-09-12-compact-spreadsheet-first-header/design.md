## Context

`index.html` is a single static file styled with the Tailwind CDN. Its current vertical stack is: `header` (centred hero, ~130px tall with the badge) → CTA card (~180px with margin) → iframe (`80vh`, min 600px) → footer. On a 900px-tall viewport the spreadsheet therefore begins roughly 310px down, leaving only a sliver of it above the fold, and the first thing a visitor reads is promotional copy instead of data.

The user's framing — "menos web, más spreadsheet" — was clarified to mean: give the embedded sheet more prominence, not add spreadsheet-like decoration (no fake column letters, no monospace styling).

Three new pieces of copy must also be placed: the project positioning line (independent tracking of real prices *and stock availability*), the community credit (built with Reddit feedback), and a price-accuracy disclaimer. These pull in the opposite direction from the compaction goal, so their placement is the core design problem of this change.

## Goals / Non-Goals

**Goals:**
- The spreadsheet iframe starts within the first screen on a typical laptop viewport.
- The header reads as compact app chrome: left-aligned, dense, visually separated from the content by a thin rule.
- The disclaimer is read *before* the prices, and cannot be dismissed.
- The suggestion CTA remains fully intact, just relocated after the sheet.

**Non-Goals:**
- Changing the iframe's height, source, or container width (explicitly kept at `80vh` / `min-height: 600px` inside `max-w-4xl`).
- Full-bleed or full-viewport-height layout.
- Skinning the header to imitate spreadsheet cells, toolbars, or menu bars.
- Touching the consent banner, GA4, or the canonical URL.
- Adding a link to the specific subreddit (none was provided).

## Decisions

**Compact top bar instead of a centred hero.** Drop `text-center`, reduce padding (`pt-6 pb-4` → `py-3`), demote the title from `text-xl sm:text-2xl` to `text-base sm:text-lg`, and put the subtitle in `text-xs text-slate-500`. Add `border-b border-slate-200` on a full-width wrapper while keeping the inner content constrained to `max-w-4xl mx-auto`, so the rule spans the viewport like an app bar but the text still aligns with the sheet below. *Alternative considered:* keeping the hero and only shrinking font sizes — rejected, centred large text still reads as a landing page. *Alternative considered:* a Sheets-style toolbar with icons and a menu row — rejected as decoration that adds markup without serving the stated goal.

**Remove the status badge rather than restyle it.** The user chose removal. It is also the single tallest non-essential element in the header and its claim ("Actualizado en tiempo real") is not verifiable from the page.

**Reorder sections in `<main>` rather than duplicate the CTA.** The CTA `<section>` moves verbatim below the iframe `<section>`; only the margin utilities are swapped (`mb-8` → `mt-8` on the CTA, `mb-12` → `mb-8` on the iframe section) so spacing stays balanced. The element keeps `id="suggest-product-link"`, so the `mailto:`-assembly script at the bottom of the file needs no change. *Alternative considered:* collapsing the CTA into a header link — rejected by the user; it would also bury the only feedback channel.

**Merge the positioning line with the existing subtitle instead of stacking both.** "Monitorización e histórico independiente de precios de creatina y proteína en España." and "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock." overlap almost entirely. They become one sentence — "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España." — which keeps the SEO keywords, adds the stock dimension, and costs one line instead of two. The `meta`/`og`/`twitter` descriptions are updated to the same string so the metadata does not drift from the visible copy.

**Reddit credit appended to the description line, not on the title row.** The credit is the page's strongest credibility signal, so it belongs above the fold, but it must not compete with the `<h1>` and must not cost a third text row. It is appended to the description sentence after a middot, inside a `text-slate-400` span so it reads as secondary metadata. Wording is shortened to "Con feedback de la comunidad de Reddit" so description + credit measure one line at the 864px header content width (verified in-browser). *Alternative considered:* right-aligning the credit on the title row with `justify-between` — implemented first and rejected: it splits the two prose lines across separate rows and reads as two competing headers. *Alternative considered:* placing it inside the CTA card or the footer — after this change both sit below an 80vh iframe, so the credit would rarely be seen.

**Disclaimer as a slim strip between header and sheet, not in the footer.** A disclaimer about price accuracy is only useful if read before the prices. It is rendered as a full-width `bg-amber-50 border-b border-amber-200 text-amber-900 text-xs py-2` strip with a warning glyph, one line on desktop and two on mobile (~28–48px). *Alternative considered:* footer placement — rejected, it would sit below an 80vh iframe and effectively never be read. *Alternative considered:* a dismissible banner or a `<details>` toggle — rejected, a liability notice must not be hideable. The amber tint is deliberate: it is the one element allowed to draw attention away from the sheet, and it mirrors the notification bars real spreadsheet apps use, so it does not reintroduce a "landing page" feel.

**Disclaimer wording is condensed from the user's text.** "Registro de precios no oficial y ajeno a las tiendas; las cifras pueden estar desactualizadas y siempre prevalece el precio de la página de la tienda." is kept semantically intact but tightened to fit one desktop line, preserving all three claims (unofficial, unaffiliated, shop price prevails).

**Drop the footer rather than shrink it.** Its two claims are now redundant: "Herramienta independiente de comparación" is stated more precisely by the header description and the disclaimer strip. What remains — the `@dajorz` GitHub link — does not justify a page-wide block sitting below an 80vh iframe where it is effectively never seen. Removing it also keeps the page to exactly three zones (chrome, notice, content), which reinforces the "tool, not landing page" goal. *Alternative considered:* folding the author credit into the header — rejected, it would compete with the Reddit credit already on that line.

## Risks / Trade-offs

- **The disclaimer strip gives back some of the vertical space the compaction won** → Accepted; capped at `text-xs` with `py-2` so the header + strip stay under ~120px on desktop, still well above the previous ~310px offset.
- **CTA discoverability drops now that it sits after an 80vh iframe** → Accepted trade-off; the iframe scrolls internally, so the CTA is reachable by page scroll. With the footer gone it is the only contact channel on the page. Can be revisited with a sticky header action if suggestions dry up.
- **Scroll trapping: pointer wheel events inside the Google Sheets iframe may not propagate to the page**, making the CTA harder to reach on some setups → Mitigated by keeping the iframe at `80vh` rather than full height, so page-level scrollbar and surrounding whitespace remain reachable.
- **Smaller title weakens the visual hierarchy for first-time visitors** → Mitigated by keeping the `<h1>` semantics and the subtitle text; the canonical URL and title tag are untouched.
- **The amber strip could be misread as an error state** → Mitigated by the warning glyph and neutral, informational wording; no red tones are used.
- **Regression risk in the consent/mailto scripts** → Low: the change is markup-order, copy and class-only, with no renamed or removed IDs. Verified by loading the page and clicking "Proponer producto".
