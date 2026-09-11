## ADDED Requirements

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

## MODIFIED Requirements

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

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`), and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`). The `meta description`, `og:description` and `twitter:description` values SHALL match the project description sentence rendered in the header, excluding the community credit.

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a description meta tag, a canonical link, the listed Open Graph tags, and the listed Twitter card tags

#### Scenario: Descriptions match the visible project description
- **WHEN** the `meta description`, `og:description` and `twitter:description` values are compared with the header's project description sentence
- **THEN** all three tags carry the same sentence as the header, without the community credit

## REMOVED Requirements

### Requirement: Hero header with status badge
**Reason**: The centred hero and the green "Actualizado en tiempo real" badge pushed the embedded spreadsheet below the fold and made the page read as a marketing landing rather than a tool. The badge also made a claim that cannot be verified from the page.
**Migration**: Replaced by the "Compact header bar" requirement, which keeps the same title in a dense, left-aligned bar without a badge.

### Requirement: Footer with author credit
**Reason**: The footer's independence claim is now stated more precisely by the header description and the price accuracy disclaimer, and after the section reorder it sits below an 80vh iframe where it is effectively never read.
**Migration**: None required. The author's contact channel remains available through the "Proponer producto" `mailto:` link; the `https://github.com/dajorz` profile link is no longer surfaced on the page.
