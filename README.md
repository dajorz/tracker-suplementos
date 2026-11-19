# tracker-suplementos

## Deployment

The landing page (`index.html`) is a static file with no build step. To publish it via GitHub Pages: **Settings → Pages → Source: Deploy from a branch → select the default branch and `/ (root)` folder.**

Analytics uses GA4 measurement ID `G-Y49TC503Y9`, loaded by `index.html` only after the visitor accepts the cookie banner.

## Embedded price table

The table is the published Google Sheet (`pubhtml`), embedded in a lazy-loaded `<iframe>`. On load, the page fetches the published HTML without using the browser cache, validates its response and table structure, then measures a sanitized, script-free copy in an off-screen sandbox. The iframe height is recalculated when its available width or display scale changes, so moving a window between monitors triggers a fresh measurement. Product additions and removals are picked up on the next page load without editing a per-product height.

Automatic sizing depends on Google allowing the published document to be fetched with CORS and keeping a compatible HTML structure. If fetching or measuring fails, the page reports an identifiable error in the browser console and keeps the iframe visible at its initial or last valid height; the initial 1420 px height is a reserve for the currently published table, and the sheet remains scrollable if later content exceeds it. JavaScript-disabled browsers also retain this initial height. A page already open does not poll for sheet updates, so reload it to pick up later changes to the publication.

The iframe section is capped at 1296 px (the published table is about 1280 px wide) and uses the available width on smaller viewports. Column widths are defined in the sheet, and «Producto» (160 px) may wrap depending on the browser's fonts. The measurement has been exercised in the available Chromium-based browser; Firefox and Safari have not been independently verified.