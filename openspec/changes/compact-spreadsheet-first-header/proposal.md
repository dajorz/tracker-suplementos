## Why

The page currently opens like a marketing landing: a centred hero (large title, subtitle, green "Actualizado en tiempo real" pill) followed by a full-width CTA card, so the embedded spreadsheet — the actual product — starts below the fold on most laptop screens. The page should read as a tool wrapped around a spreadsheet, not as a landing page that happens to contain one. At the same time the page omits two things visitors need in order to trust the data: where it comes from (community feedback), and the fact that it is an unofficial record that can lag behind the shops' own prices.

## What Changes

- Replace the centred hero header with a compact, left-aligned top bar (thin bottom border), so it behaves like the chrome of a spreadsheet app rather than a hero section.
- Remove the "Actualizado en tiempo real" status badge entirely.
- Merge the existing subtitle with the new positioning line into a single sentence: "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España." — this keeps the SEO keywords while adding stock coverage and avoiding two near-duplicate sentences.
- Add the community credit "Desarrollado con el feedback de la comunidad de Reddit" to the header, on the title row (right-aligned on desktop, stacked below on mobile).
- Add a persistent, non-dismissible disclaimer strip between the header and the spreadsheet: unofficial record, unaffiliated with the shops, figures may be stale, the shop's own page always prevails.
- Move the "¿Echas en falta algún producto?" CTA card from above the iframe to below it, so the spreadsheet is the first content under the header and disclaimer.
- Remove the footer entirely. Its independence claim is now covered by the header description and the disclaimer strip, and the author credit it carried was the only reason for the extra vertical block.
- Update `meta description`, `og:description` and `twitter:description` to match the new subtitle so the metadata does not drift from the visible copy.
- Keep the iframe sizing unchanged (`80vh`, `min-height: 600px`, rounded, shadowed, lazy-loaded) and keep the CTA card's content, `mailto:` behaviour and scraper-safe address assembly unchanged.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`: the header requirement drops the status badge and changes from a centred hero to a compact top bar carrying the merged subtitle and the Reddit community credit; a new price-accuracy disclaimer requirement is added; the product-suggestion CTA requirement changes its required position from above to below the embedded spreadsheet; the footer requirement is removed.

## Impact

- Affected files: `index.html` only (head description tags, header markup/classes, new disclaimer strip, section order inside `<main>`, footer removal).
- No change to analytics/consent, canonical URL, or iframe source or sizing, nor to the `mailto:` link-building script. The CTA's `id="suggest-product-link"` is preserved so the existing JavaScript keeps working.
- The GitHub profile link to `https://github.com/dajorz` disappears from the page along with the footer; the `mailto:` CTA becomes the only contact channel.
- No new dependencies; still a single static file with the Tailwind CDN.
- Legal posture: the disclaimer makes the non-affiliation and staleness of the data explicit, which is the main reason it must sit above the data rather than in the footer.
- Note: `pricing-tracker-landing-page` is not yet in `openspec/specs/` — it is introduced by the pending `add-pricing-tracker-landing-page` change. This change's delta applies on top of it and must be archived after it.
