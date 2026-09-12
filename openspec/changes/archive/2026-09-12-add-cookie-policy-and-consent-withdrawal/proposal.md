## Why

The consent banner lets a visitor accept or reject analytics cookies, but once the choice is stored there is no way to change it, and the page never explains what is actually stored, by whom, or for how long. Under GDPR and LSSI-CE both are required: consent must be as easy to withdraw as it is to give, and the visitor must be informed *before* consenting. Now that a real GA4 property (`G-Y49TC503Y9`) is live rather than a placeholder, the page is setting real third-party cookies and the gap is no longer theoretical.

## What Changes

- Add a cookie policy describing the analytics cookies (`_ga`, `_ga_Y49TC503Y9`), their purpose and retention, the data controller (Google Ireland Limited) with links to its privacy policy and opt-out add-on, the `cookie-consent` localStorage key, and the embedded Google Sheets iframe.
- Present the policy in a modal `<dialog>` rather than inline page content, so it costs no vertical space in a layout that was deliberately compacted to put the spreadsheet above the fold.
- Add a "Más información" link inside the consent banner that opens the policy, so the information is available before the visitor consents.
- Add a slim single-line bar below `<main>` with a "Cookies" control that reopens the consent banner, allowing the choice to be changed or withdrawn at any time.
- On rejection, delete any `_ga`-prefixed cookies and reload the page if `gtag.js` was already injected in the session, since an injected tag cannot otherwise be unloaded.
- Replace the GA4 placeholder measurement ID with the real `G-Y49TC503Y9`.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `pricing-tracker-landing-page`: the consent requirement gains an accessible cookie policy, a persistent control to revoke consent, and removal of analytics cookies on rejection; the GA4 requirement drops the placeholder measurement ID in favour of the real one.

## Impact

- Affected files: `index.html` only (new `<dialog>`, new bottom bar, consent banner copy, consent script), plus the GA4 ID note in `README.md`.
- Reintroduces a small element below `<main>` after `compact-spreadsheet-first-header` removed the footer. That removal was justified by redundant promotional copy; this bar carries a legal control instead and is one line of `text-xs`, not a content block.
- No new dependencies. The modal uses the native `<dialog>` element (no polyfill; unsupported browsers fall back to the dialog rendering inline, which remains readable).
- Analytics volume is unchanged for visitors who accept and may decrease slightly if some now withdraw consent — the correct outcome.
