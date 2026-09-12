## Why

The page has no favicon; browser tabs show a generic document icon. We considered reusing the Google Sheets icon (the page embeds a published Sheet) but ruled it out — it's Google's trademark and using it as a favicon would misleadingly imply affiliation or endorsement with Google, which their brand guidelines prohibit. An emoji favicon gives the tab an identity now, with zero licensing risk, no extra file, and no build step.

## What Changes

- Add a favicon to `index.html` using a data-URI `<link rel="icon">` that renders an emoji (a bar-chart/spreadsheet-flavoured glyph, e.g. 📊) via inline SVG, so no binary asset is added to the repo.
- No visual/behavioural change to the page body, iframe, consent flow, or metadata.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`: add a requirement that the page declares a favicon in `<head>`.

## Impact

- Affected files: `index.html` only (one `<link rel="icon">` tag added to `<head>`).
- No new dependencies, no binary assets, no build step change.
- No legal/trademark exposure: the emoji glyph is not Google's icon, avoiding the brand-affiliation risk discussed for the Sheets icon.
