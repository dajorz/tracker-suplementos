## Context

`index.html` is a single static file with no build step; assets must be inline or referenced by absolute URL. There is currently no `<link rel="icon">` in `<head>`, so browsers fall back to a generic document icon. The idea of reusing Google Sheets' icon (visible because the page embeds a published Sheet) was rejected: it's part of Google's brand assets, and Google's brand guidelines don't allow using their icons/logos in a way that could imply affiliation or endorsement — a favicon is exactly that kind of branding use, not a functional reference like the iframe embed itself.

## Goals / Non-Goals

**Goals:**
- Give the browser tab a distinct icon with zero trademark/licensing risk.
- Add it without introducing a binary asset file or a build step.

**Non-Goals:**
- Pixel-perfect custom iconography (a real designed icon) — deferred; this is a stop-gap.
- Multiple sizes / `apple-touch-icon` / PWA manifest icons — out of scope for now.

## Decisions

**Emoji rendered via an inline SVG data URI, not a static file.** A `<link rel="icon" href="data:image/svg+xml,...">` embedding `<text>📊</text>` keeps the file self-contained (matches the "single self-contained index.html" constraint already in the spec) and needs no new file in the repo. *Alternative considered:* a `.ico`/`.png` file — rejected, adds a binary asset and a second file to keep in sync for a one-line visual change.

**Glyph: 📊 (bar chart).** Reads as "data/tracking" without borrowing any company's mark. *Alternative considered:* 💰 or 🏷️ (price-themed) — 📊 was picked as more directly tied to "tracker/spreadsheet" than a currency symbol.

## Risks / Trade-offs

- **Emoji rendering differs slightly across OS/browsers (different emoji font sets)** → Accepted; favicons are small and this is a stop-gap, not a brand asset.
- **No `apple-touch-icon` / manifest icon** → Accepted for now; can be added later without touching this decision.
