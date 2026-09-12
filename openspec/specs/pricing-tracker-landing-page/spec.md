# pricing-tracker-landing-page Specification

## Purpose
TBD - created by archiving change add-pricing-tracker-landing-page. Update Purpose after archive.
## Requirements
### Requirement: Static single-file landing page
The system SHALL provide a single self-contained `index.html` file at the repository root, using semantic HTML5 and Tailwind CSS loaded from a CDN, with no build step required for deployment via GitHub Pages.

#### Scenario: Page loads with no build tooling
- **WHEN** `index.html` is served directly (e.g., via GitHub Pages) without any compilation step
- **THEN** the page renders fully styled using the Tailwind CDN script and displays all sections (header, suggestion CTA, iframe, footer)

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`), and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`). The `meta description`, `og:description` and `twitter:description` values SHALL match the project description sentence rendered in the header, excluding the community credit.

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a description meta tag, a canonical link, the listed Open Graph tags, and the listed Twitter card tags

#### Scenario: Descriptions match the visible project description
- **WHEN** the `meta description`, `og:description` and `twitter:description` values are compared with the header's project description sentence
- **THEN** all three tags carry the same sentence as the header, without the community credit

### Requirement: Product suggestion call to action
The page SHALL display a call to action positioned below the embedded spreadsheet, headed "¿Echas en falta algún producto?", inviting visitors to suggest missing products or send any other feedback, with a link styled as a button labeled "Proponer producto". The link SHALL open the visitor's mail client at the site owner's address with the prefilled subject "Sugerencia para el tracker". The page SHALL NOT contain an email input field or submit to any third-party form service, and SHALL NOT offer price-drop alerts.

#### Scenario: Call to action follows the spreadsheet
- **WHEN** the page HTML is inspected
- **THEN** the suggestion section appears after the iframe section in document order

#### Scenario: Visitor suggests a product
- **WHEN** a visitor activates the "Proponer producto" button
- **THEN** the browser opens the visitor's mail client addressed to the site owner with the subject "Sugerencia para el tracker"

#### Scenario: Address hidden from scrapers
- **WHEN** the served HTML source is inspected
- **THEN** the contact address does not appear as a literal string, because the `mailto:` href is assembled by JavaScript at runtime

#### Scenario: No form service dependency
- **WHEN** the page HTML is inspected
- **THEN** there is no `<form>` posting to an external endpoint and no `<input type="email">` in the suggestion section

### Requirement: Embedded Google Sheet iframe
The page SHALL embed the published Google Sheet in a responsive `<iframe>` that spans the full available width, uses an adaptive height of at least 600px (or `80vh`), has rounded corners, a shadow, no visible border, and lazy loading enabled.

#### Scenario: Iframe renders the published sheet
- **WHEN** the page loads
- **THEN** the iframe's `src` points to the published Google Sheets URL and the sheet content is visible within a rounded, shadowed, borderless container spanning the container width

#### Scenario: Lazy loading
- **WHEN** the page HTML is inspected
- **THEN** the iframe element includes the `loading="lazy"` attribute

### Requirement: Consent-gated Google Analytics 4 integration
The page SHALL display a cookie-consent banner on first visit offering accept and reject actions, persist the visitor's choice, and load the GA4 `gtag.js` snippet with measurement ID `G-Y49TC503Y9` only after consent is accepted.

#### Scenario: No analytics before consent
- **WHEN** a visitor loads the page and has not yet accepted the consent banner
- **THEN** no `gtag.js` request is made and no analytics cookie is set

#### Scenario: Analytics loads after acceptance
- **WHEN** the visitor accepts the consent banner
- **THEN** the `gtag.js` script is injected and initialized with measurement ID `G-Y49TC503Y9`

#### Scenario: Choice is remembered
- **WHEN** the visitor returns to the page after having accepted or rejected
- **THEN** the banner is not shown again and the previous choice is honoured

#### Scenario: Analytics is not loaded twice
- **WHEN** the visitor accepts consent more than once in the same session
- **THEN** only one `gtag.js` script element is injected

### Requirement: Compact header bar
The page SHALL display a compact, left-aligned header bar at the top of the page containing the title "Tracker de Precios de Suplementación" as the `<h1>` and, on a single line below it, the project description "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España" followed by the community credit "Con feedback de la comunidad de Reddit", separated by a middot and rendered in a lighter tone. The header SHALL be visually separated from the content below by a bottom border, SHALL use reduced vertical padding and reduced type sizes compared to a hero section, and SHALL NOT display a status badge.

#### Scenario: Header renders as a compact bar
- **WHEN** a visitor opens the page
- **THEN** the title and the description line are visible at the top of the page, left-aligned, with a bottom border separating the header from the content below

#### Scenario: Description and credit share one line on desktop
- **WHEN** the page is viewed with a header content width of 864px or wider
- **THEN** the project description and the community credit render on a single line without wrapping

#### Scenario: Header is at most two text rows on desktop
- **WHEN** the page is viewed at a desktop viewport
- **THEN** the header contains exactly two text rows: the title, and the combined description-and-credit line

#### Scenario: No status badge
- **WHEN** the page HTML is inspected
- **THEN** no "Actualizado en tiempo real" badge or equivalent status indicator is present

### Requirement: Price accuracy disclaimer
The page SHALL display a persistent notice positioned between the header and the embedded spreadsheet, stating that the price record is unofficial and unaffiliated with the shops, that the figures may be out of date, and that the price shown on the shop's own page always prevails. The notice SHALL be rendered as a slim, visually distinct full-width strip at a smaller type size than body copy, and SHALL NOT be dismissible, collapsible, or hidden behind an interaction.

#### Scenario: Disclaimer is read before the prices
- **WHEN** a visitor opens the page
- **THEN** the disclaimer is visible above the embedded spreadsheet without scrolling

#### Scenario: Disclaimer cannot be hidden
- **WHEN** the page HTML is inspected
- **THEN** the notice has no dismiss control, is not inside a `<details>` element, and no script removes or hides it

#### Scenario: Disclaimer states all three caveats
- **WHEN** the disclaimer text is read
- **THEN** it communicates that the record is unofficial and unaffiliated with the shops, that figures may be outdated, and that the shop page's price prevails

### Requirement: Spreadsheet leads the main content
The embedded spreadsheet SHALL be the first element of the main content, immediately below the header and the price accuracy disclaimer, with no call-to-action section preceding it.

#### Scenario: Spreadsheet precedes all other main content
- **WHEN** the page HTML is inspected
- **THEN** the iframe section is the first child section of `<main>`, and the suggestion section appears after it in document order

#### Scenario: Spreadsheet visible on a laptop viewport
- **WHEN** the page is viewed at a viewport 900px tall
- **THEN** the top of the embedded spreadsheet is visible without scrolling

### Requirement: Accessible cookie policy
The page SHALL provide a cookie policy, reachable before consent is given, that states which cookies are set, their purpose and approximate retention, the third-party data controller with links to its privacy policy and opt-out mechanism, the browser storage used to persist the consent choice, and the presence of the embedded Google Sheets iframe. The policy SHALL be rendered in a modal dialog so that it occupies no vertical space in the page flow.

#### Scenario: Policy reachable from the consent banner
- **WHEN** the visitor activates the "Más información" control in the consent banner
- **THEN** the cookie policy opens in a modal dialog above the banner, without the visitor having consented

#### Scenario: Policy content is complete
- **WHEN** the cookie policy is open
- **THEN** it names the `_ga` and `_ga_Y49TC503Y9` cookies with their purpose and approximate two-year retention, identifies Google Ireland Limited as controller with links to its privacy policy and opt-out add-on, describes the `cookie-consent` localStorage key, and mentions the embedded Google Sheets document

#### Scenario: Policy adds no vertical space
- **WHEN** the page is loaded and the policy has not been opened
- **THEN** the policy content is not rendered in the page flow and does not affect the position of the embedded spreadsheet

#### Scenario: Policy can be dismissed
- **WHEN** the visitor activates the "Cerrar" control in the open policy dialog
- **THEN** the dialog closes and the underlying page state is unchanged

### Requirement: Consent can be changed or withdrawn
The page SHALL display a persistent control that reopens the consent banner so the visitor can change or withdraw a previously stored choice. On rejection, the page SHALL delete any `_ga`-prefixed cookies, and SHALL reload if `gtag.js` was already injected during the session.

#### Scenario: Stored choice can be revisited
- **WHEN** a visitor who has already accepted or rejected activates the "Cookies" control below the main content
- **THEN** the consent banner is shown again with both accept and reject actions available

#### Scenario: Withdrawal removes analytics cookies
- **WHEN** a visitor who previously accepted reopens the banner and rejects
- **THEN** the stored choice becomes `rejected`, all `_ga`-prefixed cookies are deleted, and the page reloads so the injected `gtag.js` no longer runs

#### Scenario: Revocation control is unobtrusive
- **WHEN** the page is rendered
- **THEN** the control occupies a single line of small, low-contrast text below `<main>` and does not push the embedded spreadsheet below the fold

