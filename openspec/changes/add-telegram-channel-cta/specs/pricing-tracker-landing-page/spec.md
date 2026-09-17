## ADDED Requirements

### Requirement: Accessible text and controls
All page text SHALL meet a contrast ratio of at least 4.5:1 against its background. Every interactive control SHALL present a hit area of at least 24×24 CSS pixels. All page content SHALL sit inside a landmark region. Links that open a new browsing context SHALL announce it to assistive technology.

#### Scenario: Text meets AA contrast
- **WHEN** the computed colour of any text node is compared with its rendered background
- **THEN** the contrast ratio is at least 4.5:1, including the community credit in the header and the consent revocation control

#### Scenario: Controls are large enough to hit
- **WHEN** the bounding box of every link and button is measured
- **THEN** each is at least 24 CSS pixels tall and 24 wide

#### Scenario: No orphan content
- **WHEN** the direct children of `<body>` carrying text are inspected
- **THEN** each one is a landmark element or carries an explicit landmark role with an accessible name

#### Scenario: New tab is announced
- **WHEN** a link opens in a new browsing context
- **THEN** its accessible name states that a new window will open

### Requirement: Telegram channel call to action
The page SHALL display a full-width slim strip, no taller than two text lines, positioned between the price accuracy disclaimer and the embedded spreadsheet, linking to the project's Telegram channel at `https://t.me/NutriChollos`. The strip SHALL identify the channel as belonging to this tracker, SHALL describe the alerts as triggered when a product reaches its recorded minimum rather than an all-time historical minimum, SHALL word the detection conditionally, and SHALL NOT promise any message frequency. The strip SHALL carry the only actionable control in the region above the spreadsheet, so that it is distinguishable from the disclaimer by affordance and not only by colour. The link SHALL open in a new browsing context with `rel="noopener noreferrer"`, SHALL set no cookie, and SHALL remain fully usable without consent. Any click measurement SHALL reuse the existing consent gate.

#### Scenario: Channel is discoverable without scrolling
- **WHEN** a visitor opens the page at a viewport 900px tall
- **THEN** the Telegram strip is visible without scrolling, and the top edge of the embedded spreadsheet remains visible

#### Scenario: Channel is attributed to this tracker
- **WHEN** the strip's copy is read
- **THEN** it presents the channel as the Telegram channel of this tracker, so a visitor who has never heard of "NutriChollos" can tell it belongs to the same project

#### Scenario: Alert promise is bounded
- **WHEN** the strip's copy is read
- **THEN** it refers to a recorded minimum rather than an unqualified "mínimo histórico", states the detection conditionally (e.g. "cuando detectamos"), and names no message frequency or cadence

#### Scenario: Only the Telegram strip is actionable above the spreadsheet
- **WHEN** the page region above the embedded spreadsheet is inspected
- **THEN** the Telegram strip contains the only link or button in that region, and the price accuracy disclaimer remains passive text

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

#### Scenario: Legal notice keeps precedence
- **WHEN** the page HTML is inspected
- **THEN** the price accuracy disclaimer appears before the Telegram strip in document order, immediately below the header

#### Scenario: Mobile layout preserves the spreadsheet
- **WHEN** the page is viewed at a 375×667 viewport
- **THEN** the header, disclaimer and Telegram strip stack without overlapping and the top edge of the embedded spreadsheet is still on screen

## MODIFIED Requirements

### Requirement: Consent can be changed or withdrawn
The page SHALL display a persistent control that reopens the consent banner so the visitor can change or withdraw a previously stored choice. On rejection, the page SHALL delete any `_ga`-prefixed cookies, and SHALL reload if `gtag.js` was already injected during the session.

#### Scenario: Stored choice can be revisited
- **WHEN** a visitor who has already accepted or rejected activates the "Cookies" control below the main content
- **THEN** the consent banner is shown again with both accept and reject actions available

#### Scenario: Withdrawal removes analytics cookies
- **WHEN** a visitor who previously accepted reopens the banner and rejects
- **THEN** the stored choice becomes `rejected`, all `_ga`-prefixed cookies are deleted, and the page reloads so the injected `gtag.js` no longer runs

#### Scenario: Revocation control is unobtrusive but legible
- **WHEN** the page is rendered
- **THEN** the control occupies a single line of small, muted text below `<main>`, meets 4.5:1 contrast, offers a hit area of at least 24×24 CSS pixels, and does not push the embedded spreadsheet below the fold

### Requirement: Spreadsheet leads the main content
The embedded spreadsheet SHALL be the first element of the main content, immediately below the header and the page-level notice strips, with no call-to-action card or section preceding it. A single full-width slim strip, no taller than two text lines, linking to the project's Telegram channel is the only call to action permitted above the spreadsheet.

#### Scenario: Spreadsheet precedes all other main content
- **WHEN** the page HTML is inspected
- **THEN** the iframe section is the first child section of `<main>`, and the suggestion section appears after it in document order

#### Scenario: Spreadsheet visible on a laptop viewport
- **WHEN** the page is viewed at a viewport 900px tall
- **THEN** the top of the embedded spreadsheet is visible without scrolling

#### Scenario: No call-to-action card above the spreadsheet
- **WHEN** the region between the header and the embedded spreadsheet is inspected
- **THEN** it contains only slim notice strips and the Telegram call-to-action strip, with no CTA card carrying a heading, body copy and a centred button

### Requirement: Price accuracy disclaimer
The page SHALL display a persistent notice positioned between the header and the embedded spreadsheet, stating that the price record is unofficial and unaffiliated with the shops, that the figures may be out of date, and that the price shown on the shop's own page always prevails. The notice SHALL be rendered as a slim, visually distinct full-width strip at a smaller type size than body copy, and SHALL NOT be dismissible, collapsible, or hidden behind an interaction. The notice SHALL use a neutral tint and carry no warning icon, so that the Telegram call-to-action strip remains the only coloured element between the header and the spreadsheet.

#### Scenario: Disclaimer is read before the prices
- **WHEN** a visitor opens the page
- **THEN** the disclaimer is visible above the embedded spreadsheet without scrolling

#### Scenario: Disclaimer cannot be hidden
- **WHEN** the page HTML is inspected
- **THEN** the notice has no dismiss control, is not inside a `<details>` element, and no script removes or hides it

#### Scenario: Disclaimer states all three caveats
- **WHEN** the disclaimer text is read
- **THEN** it communicates that the record is unofficial and unaffiliated with the shops, that figures may be outdated, and that the shop page's price prevails

#### Scenario: Disclaimer does not compete with the call to action
- **WHEN** the region between the header and the embedded spreadsheet is inspected
- **THEN** the disclaimer carries no alert tint and no warning icon, and the Telegram strip is the only coloured block in that region

### Requirement: Product suggestion call to action
The page SHALL display a call to action positioned below the embedded spreadsheet, headed "¿Echas en falta algún producto?", inviting visitors to suggest missing products or send any other feedback, with a link styled as a button labeled "Proponer producto". The link SHALL open the visitor's mail client at the site owner's address with the prefilled subject "Sugerencia para el tracker". This section SHALL NOT contain an email input field, SHALL NOT submit to any third-party form service, and SHALL NOT offer price-drop alerts.

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

#### Scenario: Suggestion section offers no alerts
- **WHEN** the suggestion section's copy is read
- **THEN** it invites product suggestions and feedback only, and does not offer or link to price-drop alerts
