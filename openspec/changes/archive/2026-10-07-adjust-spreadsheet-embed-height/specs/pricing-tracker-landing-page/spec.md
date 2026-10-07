## MODIFIED Requirements

### Requirement: Embedded Google Sheet iframe
The page SHALL embed the published Google Sheet in a responsive `<iframe>` that spans the full width of its own section, has no visible border, and has lazy loading enabled.

The iframe SHALL have a fixed height equal to the published table's height plus room for a horizontal scrollbar, so that the embed never shows an inner vertical scrollbar and vertical scrolling over the table moves the page. Because the height cannot be read from a cross-origin document, it SHALL be a hand-maintained value, updated in the same change whenever products are added to or removed from the sheet.

The iframe SHALL NOT be framed as a card: it SHALL have no rounded corners and no shadow. It SHALL be composited with `mix-blend-mode: multiply`, so that the opaque white background painted by the published document, including any width left unused to the right of the table, renders in the page background colour.

The iframe section SHALL use a centred container wider than the reading width used by the header text, capped so that the iframe is at most 1296 CSS pixels wide, which is the published table's width plus 16 CSS pixels of room for a scrollbar. The header, the Telegram strip, the suggestion section, the methodology section and the footer SHALL keep the reading width, and widening the iframe section SHALL NOT cause the page itself to overflow horizontally.

The iframe `src` SHALL point to the published document pinned to the prices tab (`gid=0` with `single=true`), SHALL suppress the publication title bar (`chrome=false`) and the sheet tab bar (`widget=false`), SHALL hide row and column headers (`headers=false`), and SHALL NOT restrict the published range.

#### Scenario: Iframe renders the published sheet
- **WHEN** the page loads
- **THEN** the iframe's `src` points to the published Google Sheets URL and the sheet content is visible in a borderless frame spanning its section's width

#### Scenario: Embed is not framed as a card
- **WHEN** the iframe's computed style is inspected
- **THEN** its `border-radius` is 0, its `box-shadow` is `none` and its `mix-blend-mode` is `multiply`

#### Scenario: Embed has no inner vertical scroll
- **WHEN** the page is viewed at any viewport width and the iframe has loaded
- **THEN** the published table's scroll container has no vertical overflow, and a vertical scroll gesture over the table scrolls the page

#### Scenario: Recalibrated height accommodates the current table on desktop
- **WHEN** the iframe height has been recalibrated for the current published table and the page is viewed at a 1920px-wide viewport after the sheet has loaded
- **THEN** the published table's scroll container has `scrollHeight` less than or equal to `clientHeight`, and a vertical scroll gesture over the table scrolls the page rather than the inner container

#### Scenario: Recalibrated height leaves room for mobile horizontal scrolling
- **WHEN** the iframe height has been recalibrated for the current published table and the page is viewed at a 375px-wide viewport after the sheet has loaded
- **THEN** the published table's scroll container has `scrollHeight` less than or equal to `clientHeight`, all columns remain reachable by horizontal scrolling inside the embed, and a vertical scroll gesture over the table scrolls the page

#### Scenario: Unused width blends into the page
- **WHEN** the page is viewed at a 1920px-wide viewport and the published table is narrower than the iframe
- **THEN** the area between the table's right edge and the iframe's right edge renders in the page background colour rather than white

#### Scenario: Lazy loading
- **WHEN** the page HTML is inspected
- **THEN** the iframe element includes the `loading="lazy"` attribute

#### Scenario: Whole table visible on a wide desktop
- **WHEN** the page is viewed at a 1920px-wide viewport and the published table is at most 1280 CSS pixels wide
- **THEN** the iframe is 1296 CSS pixels wide, every column of the table is visible without horizontal scrolling, and no more than 16 CSS pixels separate the table's right edge from the iframe's right edge

#### Scenario: Iframe uses the available width on a laptop
- **WHEN** the page is viewed at a 1280px-wide viewport
- **THEN** the iframe spans the document's client width minus 32 CSS pixels of horizontal padding (1248 CSS pixels with overlay scrollbars), wider than the reading width of the header text

#### Scenario: Text keeps the reading width
- **WHEN** the page is viewed at a viewport 1024px wide or wider
- **THEN** the header text, the suggestion section and the methodology section share the same left edge, and neither section is wider than 864 CSS pixels

#### Scenario: Mobile width is unchanged
- **WHEN** the page is viewed at a 375px-wide viewport
- **THEN** the iframe is as wide as the header's content box

#### Scenario: Page does not overflow horizontally
- **WHEN** the page is viewed at 375px, 1280px or 1920px wide
- **THEN** the document's scroll width equals its client width, so no page-level horizontal scrollbar appears

#### Scenario: Embed carries no Google chrome
- **WHEN** the iframe has loaded
- **THEN** no publication title bar and no sheet tab bar are rendered inside it, and the first visible content is the first row of the prices tab

#### Scenario: Embed URL is pinned and unrestricted
- **WHEN** the iframe `src` is inspected
- **THEN** its query string contains `gid=0`, `single=true`, `widget=false`, `headers=false` and `chrome=false`, and contains no `range` parameter