## 1. Compact header bar

- [x] 1.1 In `index.html`, make the `<header>` span the full width with `border-b border-slate-200 bg-white`, and wrap its content in an inner `div` with `max-w-4xl mx-auto w-full px-4 py-3`
- [x] 1.2 Remove `text-center` from the header and demote the `<h1>` from `text-xl sm:text-2xl` to `text-base sm:text-lg`
- [x] 1.3 Render the description and the credit as one `mt-1 text-xs text-slate-500` paragraph below the `<h1>`: "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España" followed by a `text-slate-400` span holding " · Con feedback de la comunidad de Reddit"
- [x] 1.4 Verify in the browser that the description line does not wrap at the 864px header content width
- [x] 1.5 Delete the `<span>` containing the green dot and "Actualizado en tiempo real"

## 2. Price accuracy disclaimer

- [x] 2.1 Add a full-width strip immediately after `</header>` with `bg-amber-50 border-b border-amber-200 text-amber-900`, and an inner `div` with `max-w-4xl mx-auto w-full px-4 py-2 text-xs`
- [x] 2.2 Set the strip text to "Registro de precios no oficial y ajeno a las tiendas. Las cifras pueden estar desactualizadas; siempre prevalece el precio de la página de la tienda.", prefixed by a warning glyph marked `aria-hidden="true"`
- [x] 2.3 Confirm the strip has no dismiss button, is not wrapped in `<details>`, and is not referenced by any script

## 3. Spreadsheet-first section order

- [x] 3.1 Move the `<section>` containing "¿Echas en falta algún producto?" so it appears after the iframe `<section>` inside `<main>`
- [x] 3.2 Swap the spacing utilities: `mb-8` → `mt-8` on the suggestion section, `mb-12` → `mb-8` on the iframe section; add `pt-6` to `<main>` so the sheet is not flush against the disclaimer strip
- [x] 3.3 Confirm the iframe `src`, `width`, inline `style` (`80vh` / `min-height: 600px`), `loading="lazy"`, and classes are unchanged
- [x] 3.4 Delete the `<footer>` element and confirm no script or style references it

## 4. Metadata alignment

- [x] 4.1 Update `meta description`, `og:description` and `twitter:description` to the project description sentence (without the community credit) so they match the visible copy
- [x] 4.2 Confirm `<title>`, `og:title`, `twitter:title`, `og:url` and the canonical link are unchanged

## 5. Verification

- [x] 5.1 Open `index.html` in a browser and confirm that at a ~900px-tall viewport the header + disclaimer occupy under ~120px and the top of the spreadsheet is visible without scrolling
- [x] 5.2 Confirm no status badge is present, the header is left-aligned with a visible bottom rule, and the description and credit share a single line
- [x] 5.3 Check the layout at ~375px wide: the description line wraps cleanly and nothing overflows horizontally
- [x] 5.4 Click "Proponer producto" and confirm the `mailto:` link still opens with the subject "Sugerencia para el tracker" (the `id="suggest-product-link"` hook must be intact)
- [x] 5.5 Reload with cleared storage and confirm the cookie-consent banner still appears, its accept/reject buttons still work, and it does not overlap the disclaimer strip
