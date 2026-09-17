## ADDED Requirements

### Requirement: Telegram channel call to action
The page SHALL display a full-width, single-line strip positioned between the price accuracy disclaimer and the embedded spreadsheet, linking to the project's Telegram channel at `https://t.me/NutriChollos`. The strip SHALL identify the channel as belonging to this tracker, SHALL describe the alerts as triggered when a product reaches its recorded minimum rather than an all-time historical minimum, SHALL word the detection conditionally, and SHALL NOT promise any message frequency. The strip SHALL carry the only actionable control in the region above the spreadsheet, so that it is distinguishable from the disclaimer by affordance and not only by colour. The link SHALL open in a new browsing context with `rel="noopener noreferrer"`, SHALL set no cookie, and SHALL remain fully usable without consent. Any click measurement SHALL reuse the existing consent gate.

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

### Requirement: Spreadsheet leads the main content
The embedded spreadsheet SHALL be the first element of the main content, immediately below the header and the page-level notice strips, with no call-to-action card or section preceding it. A single full-width, single-line strip linking to the project's Telegram channel is the only call to action permitted above the spreadsheet.

#### Scenario: Spreadsheet precedes all other main content
- **WHEN** the page HTML is inspected
- **THEN** the iframe section is the first child section of `<main>`, and the suggestion section appears after it in document order

#### Scenario: Spreadsheet visible on a laptop viewport
- **WHEN** the page is viewed at a viewport 900px tall
- **THEN** the top of the embedded spreadsheet is visible without scrolling

#### Scenario: No call-to-action card above the spreadsheet
- **WHEN** the region between the header and the embedded spreadsheet is inspected
- **THEN** it contains only single-line notice strips and the Telegram call-to-action strip, with no CTA card carrying a heading, body copy and a centred button

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
