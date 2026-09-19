## MODIFIED Requirements

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`), and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`). The `meta description`, `og:description` and `twitter:description` values SHALL match the project description sentence rendered in the header.

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a description meta tag, a canonical link, the listed Open Graph tags, and the listed Twitter card tags

#### Scenario: Descriptions match the visible project description
- **WHEN** the `meta description`, `og:description` and `twitter:description` values are compared with the header's project description sentence
- **THEN** all three tags carry the same sentence as the header, character for character

### Requirement: Compact header bar
The page SHALL display a compact, left-aligned header bar at the top of the page containing the title "Tracker de Precios de Suplementación" as the `<h1>` and, below it, the project description "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España" rendered in a lighter tone. The header SHALL NOT contain the community credit, which belongs to the footer. The header SHALL be visually separated from the content below by a bottom border, SHALL use reduced vertical padding and reduced type sizes compared to a hero section, and SHALL NOT display a status badge.

#### Scenario: Header renders as a compact bar
- **WHEN** a visitor opens the page
- **THEN** the title and the description line are visible at the top of the page, left-aligned, with a bottom border separating the header from the content below

#### Scenario: Description occupies one line on desktop
- **WHEN** the page is viewed with a header content width of 864px or wider
- **THEN** the project description renders on a single line without wrapping

#### Scenario: Header is at most two text rows on desktop
- **WHEN** the page is viewed at a desktop viewport
- **THEN** the header contains exactly two text rows: the title, and the description line

#### Scenario: Header carries no community credit
- **WHEN** the header's HTML is inspected
- **THEN** it contains no reference to the Reddit community, and the description sentence ends without a trailing middot or credit clause

#### Scenario: Description wraps to at most two rows on mobile
- **WHEN** the page is viewed at a 375px-wide viewport
- **THEN** the description paragraph occupies no more than two text rows

#### Scenario: No status badge
- **WHEN** the page HTML is inspected
- **THEN** no "Actualizado en tiempo real" badge or equivalent status indicator is present

### Requirement: Price accuracy disclaimer
The page SHALL display a persistent notice inside the `<footer>`, stating that the price record is unofficial and unaffiliated with the shops, that the figures may be out of date, and that the price shown on the shop's own page always prevails. The notice SHALL be the first content of the footer, SHALL be rendered at a smaller type size than body copy, and SHALL NOT be dismissible, collapsible, abbreviated, or hidden behind an interaction. No notice strip SHALL remain between the header and the embedded spreadsheet.

#### Scenario: Disclaimer lives in the footer
- **WHEN** the page HTML is inspected
- **THEN** the disclaimer text is the first content inside the `<footer>` element, and no `<aside>` carrying it exists between the header and the embedded spreadsheet

#### Scenario: Disclaimer cannot be hidden
- **WHEN** the page HTML is inspected
- **THEN** the notice has no dismiss control, is not inside a `<details>` element, and no script removes or hides it

#### Scenario: Disclaimer states all three caveats
- **WHEN** the disclaimer text is read
- **THEN** it communicates that the record is unofficial and unaffiliated with the shops, that figures may be outdated, and that the shop page's price prevails

#### Scenario: Disclaimer text is preserved in full
- **WHEN** the footer disclaimer is compared with the wording previously rendered above the spreadsheet
- **THEN** the sentence is unchanged, neither shortened nor summarised

#### Scenario: Disclaimer remains legible
- **WHEN** the computed colour of the disclaimer text is compared with the footer background
- **THEN** the contrast ratio is at least 4.5:1

### Requirement: Spreadsheet leads the main content
The embedded spreadsheet SHALL be the first element of the main content, immediately below the header and the Telegram call-to-action strip, with no call-to-action card, notice strip or section preceding it. A single full-width slim strip, no taller than two text lines on desktop viewports, linking to the project's Telegram channel is the only block permitted between the header and the spreadsheet.

#### Scenario: Spreadsheet precedes all other main content
- **WHEN** the page HTML is inspected
- **THEN** the iframe section is the first child section of `<main>`, and the suggestion section appears after it in document order

#### Scenario: Spreadsheet visible on a laptop viewport
- **WHEN** the page is viewed at a viewport 900px tall
- **THEN** the top of the embedded spreadsheet is visible without scrolling

#### Scenario: Telegram strip is the only block above the spreadsheet
- **WHEN** the region between the header and the embedded spreadsheet is inspected
- **THEN** it contains the Telegram call-to-action strip and nothing else, with no notice strip and no CTA card carrying a heading, body copy and a centred button

#### Scenario: Preamble stays within budget
- **WHEN** the distance between the top of the viewport and the top edge of the embedded spreadsheet is measured at a 375px-wide viewport
- **THEN** it is no greater than 220 CSS pixels

### Requirement: Accessible text and controls
All page text SHALL meet a contrast ratio of at least 4.5:1 against its background. Every interactive control SHALL present a hit area of at least 24×24 CSS pixels. All page content SHALL sit inside a landmark region. Links that open a new browsing context SHALL announce it to assistive technology.

#### Scenario: Text meets AA contrast
- **WHEN** the computed colour of any text node is compared with its rendered background
- **THEN** the contrast ratio is at least 4.5:1, including the community credit in the footer, the price accuracy disclaimer and the consent revocation control

#### Scenario: Controls are large enough to hit
- **WHEN** the bounding box of every link and button is measured
- **THEN** each is at least 24 CSS pixels tall and 24 wide

#### Scenario: No orphan content
- **WHEN** the direct children of `<body>` carrying text are inspected
- **THEN** each one is a landmark element or carries an explicit landmark role with an accessible name

#### Scenario: New tab is announced
- **WHEN** a link opens in a new browsing context
- **THEN** its accessible name states that a new window will open

### Requirement: Consent can be changed or withdrawn
The page SHALL display a persistent control that reopens the consent banner so the visitor can change or withdraw a previously stored choice. On rejection, the page SHALL delete any `_ga`-prefixed cookies, and SHALL reload if `gtag.js` was already injected during the session.

#### Scenario: Stored choice can be revisited
- **WHEN** a visitor who has already accepted or rejected activates the "Cookies" control in the footer
- **THEN** the consent banner is shown again with both accept and reject actions available

#### Scenario: Withdrawal removes analytics cookies
- **WHEN** a visitor who previously accepted reopens the banner and rejects
- **THEN** the stored choice becomes `rejected`, all `_ga`-prefixed cookies are deleted, and the page reloads so the injected `gtag.js` no longer runs

#### Scenario: Revocation control is unobtrusive but legible
- **WHEN** the page is rendered
- **THEN** the control sits in the footer below `<main>` in small, muted text, meets 4.5:1 contrast, offers a hit area of at least 24×24 CSS pixels, and does not push the embedded spreadsheet below the fold

#### Scenario: Control remains distinguishable among footer text
- **WHEN** the footer is rendered alongside the disclaimer and the community credit
- **THEN** the "Cookies" control is still identifiable as an interactive control by its underline and its accessible role

### Requirement: Telegram channel call to action
The page SHALL display a full-width slim strip, no taller than two text lines on desktop viewports, positioned immediately below the header and above the embedded spreadsheet, linking to the project's Telegram channel at `https://t.me/NutriChollos`. The strip SHALL identify the channel as belonging to this tracker, SHALL describe the alerts as triggered when a product reaches its recorded minimum rather than an all-time historical minimum, SHALL word the detection conditionally, and SHALL NOT promise any message frequency. The strip SHALL carry the only actionable control in the region above the spreadsheet, and its button SHALL present a hit area of at least 44 CSS pixels tall. The link SHALL open in a new browsing context with `rel="noopener noreferrer"`, SHALL set no cookie, and SHALL remain fully usable without consent. Any click measurement SHALL reuse the existing consent gate.

#### Scenario: Channel is discoverable without scrolling
- **WHEN** a visitor opens the page at a viewport 900px tall
- **THEN** the Telegram strip is visible without scrolling, and the top edge of the embedded spreadsheet remains visible

#### Scenario: Strip follows the header directly
- **WHEN** the page HTML is inspected
- **THEN** the Telegram strip is the first element after `<header>` in document order

#### Scenario: Button meets the primary target size
- **WHEN** the bounding box of the Telegram button is measured at any viewport
- **THEN** it is at least 44 CSS pixels tall

#### Scenario: Channel is attributed to this tracker
- **WHEN** the strip's copy is read
- **THEN** it presents the channel as the Telegram channel of this tracker, so a visitor who has never heard of "NutriChollos" can tell it belongs to the same project

#### Scenario: Alert promise is bounded
- **WHEN** the strip's copy is read
- **THEN** it refers to a recorded minimum rather than an unqualified "mínimo histórico", attributes the detection to the bot so no exhaustiveness is promised, and names no message frequency or cadence

#### Scenario: Only the Telegram strip is actionable above the spreadsheet
- **WHEN** the page region above the embedded spreadsheet is inspected
- **THEN** the Telegram strip contains the only link or button in that region

#### Scenario: Link opens safely in a new tab
- **WHEN** a visitor activates the strip's button
- **THEN** `https://t.me/NutriChollos` opens in a new browsing context and the originating page retains `window.opener === null`

#### Scenario: No consent impact
- **WHEN** a visitor who has rejected or not yet answered the consent banner loads the page
- **THEN** the strip renders, no cookie is set by it, no analytics request is made, and the cookie policy content is unchanged

#### Scenario: Click measurement is consent-gated
- **WHEN** a visitor who has rejected or not yet answered the consent banner activates the strip's button
- **THEN** the channel still opens and no analytics request is made

#### Scenario: Click measurement records accepted visits
- **WHEN** a visitor who has accepted the consent banner activates the strip's button
- **THEN** a `join_telegram` event is sent to the existing GA4 property

#### Scenario: Mobile layout preserves the spreadsheet
- **WHEN** the page is viewed at a 375×667 viewport
- **THEN** the header and Telegram strip stack without overlapping and the top edge of the embedded spreadsheet is still on screen

## ADDED Requirements

### Requirement: Footer carries the project's secondary information
The `<footer>` SHALL carry, in this order, the full price accuracy disclaimer, the community credit "Con feedback de la comunidad de Reddit", and the "Cookies" consent revocation control. The footer SHALL remain a landmark element and SHALL NOT contain any control that competes with the Telegram call to action.

#### Scenario: Footer content and order
- **WHEN** the footer's HTML is inspected
- **THEN** the price accuracy disclaimer appears first, followed by the community credit and the "Cookies" control

#### Scenario: Community credit relocated
- **WHEN** the page is searched for the text "Con feedback de la comunidad de Reddit"
- **THEN** it appears exactly once, inside the `<footer>`, and nowhere in the header

#### Scenario: Footer stays out of the way
- **WHEN** the page is viewed at a 375×667 viewport
- **THEN** the footer renders below the suggestion section and does not overlap the embedded spreadsheet or the consent banner

#### Scenario: Footer introduces no competing call to action
- **WHEN** the footer's interactive elements are inspected
- **THEN** the only control present is the "Cookies" button, styled as inline text rather than as a button-like call to action
