## ADDED Requirements

### Requirement: Brand palette
The page SHALL use the project's brand colours, navy `#051322` and lime `#98F13E`, as its only accent colours, with neutral greys reserved for body copy, borders and backgrounds. Navy SHALL be the colour of the `<h1>`, of the section headings, of the fill of every brand button and action indicator, and of the footer's consent revocation control. Lime SHALL appear only as text or icon colour on a navy fill, or as a non-text decorative line; it SHALL NOT be used as a text colour on a light background. The page background and the area behind the embedded spreadsheet SHALL remain light, so that the iframe's multiply blending keeps the published table legible. Colours from the default Tailwind `sky` palette SHALL NOT appear on the page.

#### Scenario: Lime never sits as text on a light background
- **WHEN** every text node and icon whose computed colour is `#98F13E` is inspected
- **THEN** its rendered background is the navy `#051322` or its hover variant, never white or a light grey

#### Scenario: Brand buttons share one treatment
- **WHEN** the "Proponer producto" button, the cookie policy's "Cerrar" button and the Telegram action indicator are inspected
- **THEN** each has a navy fill, lime text and fully rounded ends

#### Scenario: Headings use the brand ink
- **WHEN** the computed colour of the `<h1>` and of each section `<h2>` is inspected
- **THEN** it is the navy `#051322`

#### Scenario: Spreadsheet area stays light
- **WHEN** the computed background behind the embedded iframe is inspected
- **THEN** it is the light page background, and the iframe still has `mix-blend-mode: multiply`

#### Scenario: No off-brand blue remains
- **WHEN** the page HTML is inspected
- **THEN** no class from the Tailwind `sky` palette is present

## MODIFIED Requirements

### Requirement: Compact header bar
The page SHALL display a compact, left-aligned header bar at the top of the page containing the project's circular brand mark to the left of the title "Tracker de Precios de Suplementación" as the `<h1>` and, below the title, the project description "Proyecto independiente de seguimiento de precios reales y disponibilidad de stock de creatina y proteína en España" rendered in a lighter tone. The brand mark SHALL be decorative (empty `alt`), SHALL reuse the existing home screen icon file rather than a new asset, and SHALL be clipped to a circle. On desktop viewports the brand mark SHALL span the height of both text rows; on mobile viewports the description SHALL span the full header width beneath the brand mark and title. The header SHALL NOT contain the community credit, which belongs to the footer. The header SHALL be visually separated from the content below by a bottom border, SHALL use reduced vertical padding and reduced type sizes compared to a hero section, and SHALL NOT display a status badge.

#### Scenario: Header renders as a compact bar
- **WHEN** a visitor opens the page
- **THEN** the brand mark, the title and the description line are visible at the top of the page, left-aligned, with a bottom border separating the header from the content below

#### Scenario: Brand mark reuses the home screen icon
- **WHEN** the header's image is inspected
- **THEN** its `src` is the same file referenced by `<link rel="apple-touch-icon">`, its `alt` is empty, and its rendered shape is a circle

#### Scenario: Description occupies one line on desktop
- **WHEN** the page is viewed with a header content width of 864px or wider
- **THEN** the project description renders on a single line without wrapping

#### Scenario: Header is at most two text rows on desktop
- **WHEN** the page is viewed at a desktop viewport
- **THEN** the header contains exactly two text rows, the title and the description line, with the brand mark beside both rather than adding a row

#### Scenario: Header carries no community credit
- **WHEN** the header's HTML is inspected
- **THEN** it contains no reference to the Reddit community, and the description sentence ends without a trailing middot or credit clause

#### Scenario: Description wraps to at most two rows on mobile
- **WHEN** the page is viewed at a 375px-wide viewport
- **THEN** the description paragraph spans the header's full content width and occupies no more than two text rows

#### Scenario: No status badge
- **WHEN** the page HTML is inspected
- **THEN** no "Actualizado en tiempo real" badge or equivalent status indicator is present

### Requirement: Telegram channel call to action
The page SHALL display a full-width slim strip, no taller than two text lines on desktop viewports, positioned immediately below the header and above the embedded spreadsheet, linking to the project's Telegram channel at `https://t.me/NutriChollos`. The entire strip SHALL be a single link, and it SHALL display a pill-shaped, non-interactive action indicator to the right of the copy at every viewport, never stacked beneath it. The strip SHALL be separated from the spreadsheet by a lime bottom line. The strip SHALL identify the channel as belonging to this tracker, SHALL describe the alerts as triggered when a product reaches its recorded minimum rather than an all-time historical minimum, SHALL word the detection conditionally, and SHALL NOT promise any message frequency. The strip link SHALL be the only actionable control in the region above the spreadsheet and SHALL present a hit area of at least 44 CSS pixels tall; on viewports narrower than 640 CSS pixels the action indicator SHALL itself render at least 44 CSS pixels tall. The link SHALL open in a new browsing context with `rel="noopener noreferrer"`, SHALL set no cookie, and SHALL remain fully usable without consent. Any click measurement SHALL reuse the existing consent gate.

#### Scenario: Channel is discoverable without scrolling
- **WHEN** a visitor opens the page at a viewport 900px tall
- **THEN** the Telegram strip is visible without scrolling, and the top edge of the embedded spreadsheet remains visible

#### Scenario: Strip follows the header directly
- **WHEN** the page HTML is inspected
- **THEN** the Telegram strip is the first element after `<header>` in document order

#### Scenario: Whole strip is one link
- **WHEN** the strip's HTML is inspected
- **THEN** it contains exactly one `<a>` element, that element spans the strip's full width, and the action indicator inside it is not a link or button

#### Scenario: Link meets the primary target size
- **WHEN** the bounding box of the strip link is measured at any viewport
- **THEN** it is at least 44 CSS pixels tall

#### Scenario: Indicator sits beside the copy
- **WHEN** the strip is rendered at 375px and at 1280px wide
- **THEN** the action indicator's left edge is to the right of the copy's right edge, and the two boxes overlap vertically

#### Scenario: Indicator reads as a button on mobile
- **WHEN** the action indicator is measured at a 375px-wide viewport
- **THEN** it is at least 44 CSS pixels tall

#### Scenario: Channel is attributed to this tracker
- **WHEN** the strip's copy is read
- **THEN** it presents the channel as the Telegram channel of this tracker, so a visitor who has never heard of "NutriChollos" can tell it belongs to the same project

#### Scenario: Alert promise is bounded
- **WHEN** the strip's copy is read
- **THEN** it refers to a recorded minimum rather than an unqualified "mínimo histórico", attributes the detection to the bot in a conditional clause so no exhaustiveness is promised, and names no message frequency or cadence

#### Scenario: Only the Telegram strip is actionable above the spreadsheet
- **WHEN** the page region above the embedded spreadsheet is inspected
- **THEN** the Telegram strip link is the only link or button in that region

#### Scenario: Link opens safely in a new tab
- **WHEN** a visitor activates the strip
- **THEN** `https://t.me/NutriChollos` opens in a new browsing context and the originating page retains `window.opener === null`

#### Scenario: No consent impact
- **WHEN** a visitor who has rejected or not yet answered the consent banner loads the page
- **THEN** the strip renders, no cookie is set by it, no analytics request is made, and the cookie policy content is unchanged

#### Scenario: Click measurement is consent-gated
- **WHEN** a visitor who has rejected or not yet answered the consent banner activates the strip
- **THEN** the channel still opens and no analytics request is made

#### Scenario: Click measurement records accepted visits
- **WHEN** a visitor who has accepted the consent banner activates the strip
- **THEN** a `join_telegram` event is sent to the existing GA4 property

#### Scenario: Mobile layout preserves the spreadsheet
- **WHEN** the page is viewed at a 375×667 viewport
- **THEN** the header and Telegram strip stack without overlapping and the top edge of the embedded spreadsheet is still on screen
