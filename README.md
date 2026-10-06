# tracker-suplementos

## Deployment

The landing page (`index.html`) is a static file with no build step. To publish it via GitHub Pages: **Settings → Pages → Source: Deploy from a branch → select the default branch and `/ (root)` folder.**

Analytics uses GA4 measurement ID `G-Y49TC503Y9`, loaded by `index.html` only after the visitor accepts the cookie banner.

## Embedded price table

The table is the published Google Sheet (`pubhtml`), embedded in an `<iframe>` whose size is fitted by hand to the published table, because a cross-origin document cannot be measured from the page:

- **Width**: the iframe section's `max-w-[83rem]` gives a 1296 px iframe = 1280 px table + 16 px scrollbar. Update it whenever column widths change in the sheet.
- **Height**: `height: 1190px` = 1174 px table + 16 px horizontal scrollbar, so the iframe has no inner vertical scroll. Update it whenever products are added or removed (about 37 px per product whose name wraps to two lines).

The table's size does not depend on the viewport: column widths are fixed in the sheet and «Producto» (160 px) wraps text. To re-measure, open the `pubhtml` URL from `index.html` and read the width and height of `table.waffle`.