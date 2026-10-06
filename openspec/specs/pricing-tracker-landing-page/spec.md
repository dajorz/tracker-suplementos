# pricing-tracker-landing-page Specification

## Purpose
TBD - created by archiving change add-pricing-tracker-landing-page. Update Purpose after archive.
## Requirements
### Requirement: Static single-file landing page
The system SHALL provide a landing page whose entire markup, styling and behaviour live in a single `index.html` file at the repository root, using semantic HTML5 and Tailwind CSS loaded from a CDN, with no build step required for deployment via GitHub Pages. The repository MAY contain additional static sibling assets at the root that are served verbatim and referenced from `index.html` (such as a social sharing image, `robots.txt` and `sitemap.xml`), provided none of them requires compilation, generation or any other pre-deployment processing.

#### Scenario: Page loads with no build tooling
- **WHEN** `index.html` is served directly (e.g., via GitHub Pages) without any compilation step
- **THEN** the page renders fully styled using the Tailwind CDN script and displays all sections (header, iframe, suggestion CTA, methodology, footer)

#### Scenario: Sibling assets require no processing
- **WHEN** the repository root is inspected
- **THEN** every file other than `index.html` that the deployed site depends on is a static asset committed in its final served form, and no build, bundling or generation script exists

#### Scenario: All markup and behaviour stay in one file
- **WHEN** `index.html` is inspected
- **THEN** it contains no reference to an external stylesheet or script file hosted in this repository, its only external script being the Tailwind CDN

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `<title>`, a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`, `og:image`, `og:image:width`, `og:image:height`, `og:image:alt`) and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`).

The `<title>` SHALL lead with the search intent the page targets rather than the project's own name, and SHALL fit within 60 characters. The `meta description` SHALL be written as search-result copy: it SHALL name the tracked shops and the two comparison figures the tracker provides, and SHALL fit within 160 characters. `og:title` and `twitter:title` SHALL carry the same string as the `<title>`; `og:description` and `twitter:description` SHALL carry the same string as the `meta description`.

These metadata strings SHALL NOT be required to match the project description sentence rendered in the header; the header sentence states what the project is, while the metadata strings address a visitor who has not yet arrived.

No metadata value SHALL contain a figure that changes over time, such as a product count, a shop count, a price or an update date, because the page has no build step able to refresh it.

`twitter:card` SHALL be `summary_large_image`. `og:image` SHALL be an absolute URL pointing to a raster image asset served from this site's own origin, at least 1200 pixels wide, with an aspect ratio between 1.85:1 and 2:1, and whose filename carries a version suffix so that a future redesign can be published under a new URL rather than relying on third-party scrapers invalidating a cached one. The declared `og:image:width` and `og:image:height` SHALL match the asset's real dimensions exactly.

The image SHALL use a single uniform background colour edge to edge. It SHALL NOT render its safe margin as a visible frame or border of a different shade, because scrapers crop the image to 2:1 and an inset frame becomes visibly thinner on two sides than on the other two.

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a `<title>`, a description meta tag, a canonical link, all the listed Open Graph tags and all the listed Twitter card tags

#### Scenario: Title targets a search query
- **WHEN** the `<title>` is read
- **THEN** it leads with the product category and the market the page covers, it is at most 60 characters long, and it does not begin with the project's own name

#### Scenario: Description reads as search-result copy
- **WHEN** the `meta description` is read
- **THEN** it is at most 160 characters long, names the tracked shops, and mentions both the recorded historical minimum and the price per 100 g of protein

#### Scenario: External-facing strings stay consistent with each other
- **WHEN** `<title>`, `og:title` and `twitter:title` are compared, and separately `meta description`, `og:description` and `twitter:description` are compared
- **THEN** each group carries one identical string across all of its tags

#### Scenario: Metadata is decoupled from the header sentence
- **WHEN** the metadata strings are compared with the header's project description sentence
- **THEN** they are permitted to differ, and the header sentence is unchanged by any metadata edit

#### Scenario: No volatile figures in metadata
- **WHEN** every metadata value is inspected
- **THEN** none of them states a product count, a shop count, a price or a date

#### Scenario: Social preview renders a large image card
- **WHEN** the page URL is submitted to a social link scraper
- **THEN** `twitter:card` is `summary_large_image`, `og:image` resolves over HTTPS to a raster image under 200 kB served from this site's origin, at least 1200 pixels wide and with an aspect ratio between 1.85:1 and 2:1, and the declared `og:image:width` and `og:image:height` match the asset's real dimensions

#### Scenario: Background is uniform edge to edge
- **WHEN** the pixels along each of the image's four edges are compared with the pixels at its centre
- **THEN** they share the same background colour, with no inset frame or border of a different shade

#### Scenario: Social image filename is versioned
- **WHEN** the `og:image` URL is inspected
- **THEN** its filename carries a version suffix, so replacing the artwork means publishing a new URL rather than overwriting a URL already cached by scrapers

#### Scenario: Social image survives cropping and downscaling
- **WHEN** the image is cropped to a 2:1 aspect ratio and then reduced to a quarter of its size, as link scrapers do on mobile
- **THEN** no text or logo element is clipped, and the page title remains legible

#### Scenario: Social image states nothing that expires
- **WHEN** the image content is inspected
- **THEN** it shows no price, no product count, no shop count and no date, so it cannot contradict the site while cached by a scraper

### Requirement: Product suggestion call to action
The page SHALL display a call to action positioned below the embedded spreadsheet, headed "¿Echas en falta algún producto?", inviting visitors to suggest missing products or send any other feedback, with a link styled as a button labeled "Proponer producto". The link SHALL open the visitor's mail client at the site owner's address with the prefilled subject "Sugerencia para el tracker". This section SHALL NOT contain an email input field, SHALL NOT submit to any third-party form service, and SHALL NOT offer price-drop alerts.

The site owner's address SHALL NOT appear as a literal string anywhere in the repository, not only in the served HTML. The repository is public and is itself served by GitHub Pages, so its source files are plain-text surfaces indexed by search engines and queryable through code search — the same harvesting vector the runtime assembly exists to defeat. Specification artefacts SHALL therefore refer to the address descriptively rather than reproducing it.

#### Scenario: Call to action follows the spreadsheet
- **WHEN** the page HTML is inspected
- **THEN** the suggestion section appears after the iframe section in document order

#### Scenario: Visitor suggests a product
- **WHEN** a visitor activates the "Proponer producto" button
- **THEN** the browser opens the visitor's mail client addressed to the site owner with the subject "Sugerencia para el tracker"

#### Scenario: Address hidden from scrapers
- **WHEN** the served HTML source is inspected
- **THEN** the contact address does not appear as a literal string, because the `mailto:` href is assembled by JavaScript at runtime

#### Scenario: Address absent from the whole public repository
- **WHEN** every tracked file in the repository is searched for the address, including `openspec/` proposals, designs, tasks and specifications, both active and archived
- **THEN** it appears nowhere as a literal string, and the only place its parts exist is the runtime assembly in `index.html`

#### Scenario: No form service dependency
- **WHEN** the page HTML is inspected
- **THEN** there is no `<form>` posting to an external endpoint and no `<input type="email">` in the suggestion section

#### Scenario: Suggestion section offers no alerts
- **WHEN** the suggestion section's copy is read
- **THEN** it invites product suggestions and feedback only, and does not offer or link to price-drop alerts

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

### Requirement: Favicon
The page SHALL declare a favicon in `<head>` via a `<link rel="icon" type="image/png" sizes="32x32">` element pointing to a 32×32 PNG file served as a static sibling asset at the repository root, so browser tabs show a distinct icon instead of the browser's default document icon. The page SHALL also declare a `<link rel="apple-touch-icon">` element pointing to a 180×180 PNG sibling asset, so saving the page to an iOS home screen shows the same brand mark. Both files SHALL be derived from the project's own circular brand mark, and their file names SHALL carry a version suffix so a future redesign is published under a new URL instead of overwriting a cached one. The tab favicon SHALL be transparent outside the circular mark; the home screen icon SHALL be fully opaque. The source artwork the icons are derived from SHALL NOT be committed to the repository. The favicon SHALL NOT use any third-party trademarked logo or icon (e.g. Google's Sheets/Drive iconography).

#### Scenario: Tab favicon declared as a versioned sibling file
- **WHEN** the page HTML `<head>` is inspected
- **THEN** it contains exactly one `<link rel="icon">` element, its `href` is not a `data:` URI, it declares `type="image/png"` and `sizes="32x32"`, and its `href` resolves to a file at the repository root whose name contains a version suffix

#### Scenario: Tab favicon file matches its declaration
- **WHEN** the file referenced by `<link rel="icon">` is inspected
- **THEN** it is a PNG of exactly 32×32 pixels whose four corner pixels are fully transparent

#### Scenario: Home screen icon declared and opaque
- **WHEN** the page HTML `<head>` and the file referenced by `<link rel="apple-touch-icon">` are inspected
- **THEN** the element exists, its `href` resolves to a PNG at the repository root of exactly 180×180 pixels whose name contains a version suffix, and none of its pixels is transparent

#### Scenario: Icons are reachable when served
- **WHEN** both icon files are requested over HTTPS at the site's served root path
- **THEN** each responds with status 200 and an image content type

#### Scenario: Source artwork not published
- **WHEN** the repository's tracked files are listed
- **THEN** no square image file larger than 180×180 pixels is tracked anywhere in the repository

#### Scenario: No third-party trademarked icon used
- **WHEN** the favicon's content is inspected
- **THEN** it is the project's own brand mark, not a copy of Google's (or any other company's) logo or product icon

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

### Requirement: Methodology and provenance section
The page SHALL render a methodology section as the last section of `<main>`, positioned after the embedded spreadsheet and after the product suggestion call to action. It SHALL be introduced by an `<h2>` and subdivided by `<h3>` headings, and SHALL explain, in prose aimed at a first-time reader of the table: which shops are tracked, which product categories are covered, what the price per 100 g of protein means and why it allows comparing different pack sizes, what the "mínimo registrado" figure is and how it differs from a struck-through list price, how often the data is refreshed, and who is behind the project.

The section SHALL disclose the project's commercial relationship with the tracked shops as a statement of **present fact**, covering both this page and the Telegram channel this page links to, and SHALL NOT phrase that disclosure as a perpetual commitment. Where the two surfaces differ, the disclosure SHALL name the difference, since the page funnels visitors into the channel.

Independently of that disclosure, the section SHALL state that the ordering of the table and the recorded minimum derive from observed prices alone and are never influenced by any commercial arrangement. That statement is structural rather than conditional: it SHALL remain true regardless of how either surface is monetised.

It SHALL NOT state any figure that changes over time, such as a product count, a shop count, a specific price or a specific date, because no build step exists to refresh it. It SHALL NOT be written to accumulate search keywords: every subsection SHALL answer a question a reader of the table would genuinely have, and keyword coverage SHALL be a consequence of that rather than its purpose.

#### Scenario: Section is the last content of main
- **WHEN** the page HTML is inspected
- **THEN** the methodology section is the final `<section>` inside `<main>`, appearing after both the iframe section and the suggestion section in document order

#### Scenario: Suggestion CTA is not buried
- **WHEN** the distance between the bottom of the iframe section and the top of the suggestion section is inspected
- **THEN** no other section separates them, so the call to action remains the first content a visitor meets after the table

#### Scenario: Tracked shops appear in the page's own HTML
- **WHEN** the served HTML is searched outside the iframe
- **THEN** the names of all tracked shops appear in the methodology section's prose

#### Scenario: Product vocabulary appears in the page's own HTML
- **WHEN** the served HTML is searched outside the iframe
- **THEN** the covered product categories are named in prose, including creatine monohydrate, Creapure, whey concentrate, whey isolate and casein

#### Scenario: Comparison figures are explained
- **WHEN** the methodology section is read
- **THEN** it explains what the price per 100 g of protein normalises and why, and it explains that the recorded minimum is the lowest price observed since tracking began rather than a shop's reference price

#### Scenario: Commercial relationship disclosed as present fact
- **WHEN** the methodology section is read
- **THEN** it states in the present tense whether affiliate links or any compensation from the tracked shops currently exist, and it makes no claim that this will always be so

#### Scenario: Disclosure covers the linked Telegram channel
- **WHEN** the methodology section's commercial disclosure is read
- **THEN** it covers both this page and the Telegram channel the page links to, naming any difference between the two

#### Scenario: Data independence is asserted unconditionally
- **WHEN** the methodology section is read
- **THEN** it states that the table's ordering and the recorded minimum derive only from observed prices and are not influenced by any commercial arrangement, phrased so that it stays true even if either surface is later monetised

#### Scenario: Disclosure is updated in the same change that monetises
- **WHEN** an affiliate link or any compensation from a tracked shop is introduced on either the page or the linked Telegram channel
- **THEN** the methodology section's commercial disclosure is updated within that same change, so the page never claims a commercial status it no longer has

#### Scenario: Authorship is identifiable
- **WHEN** the methodology section is read
- **THEN** it identifies `dajorz` as the maintainer of the tracker, using the same name declared in the page's structured data

#### Scenario: Heading hierarchy describes the subject
- **WHEN** the page's headings are listed in document order
- **THEN** the `<h1>` is the page title and at least one `<h2>` names the subject matter of the tracker, so the outline no longer consists solely of a contact call to action and a legal notice

#### Scenario: No volatile figures in the prose
- **WHEN** the methodology section is inspected
- **THEN** it contains no product count, shop count, specific price or specific date that would require manual updating

### Requirement: Structured data
The page SHALL embed structured data as a single inline `application/ld+json` script declaring a `Dataset` describing the price record and a `Person` identifying its maintainer. The `Dataset` SHALL declare an open-ended `temporalCoverage` and SHALL NOT declare `dateModified`, since no build step can refresh it and a frozen date is a worse signal than none.

The page SHALL NOT declare `Product` or `Offer` markup for the tracked items, because those items are rendered inside a third-party iframe and are therefore not content of this page, and because `Offer` would assert that this site sells them, which is false. The page SHALL NOT declare `FAQPage` or a `SearchAction`, whose corresponding result types are no longer available to a site of this kind.

#### Scenario: Structured data is present and valid
- **WHEN** the page HTML is inspected
- **THEN** `<head>` or `<body>` contains exactly one `application/ld+json` script, it parses as valid JSON, and it validates without errors against the declared schema.org types

#### Scenario: Dataset describes the price record
- **WHEN** the `Dataset` node is inspected
- **THEN** it declares a name, a description, `inLanguage` of `es-ES`, `isAccessibleForFree` of true, the canonical URL, and a `creator` referencing the `Person` node

#### Scenario: No frozen modification date
- **WHEN** the `Dataset` node is inspected
- **THEN** it declares `temporalCoverage` as an open-ended interval and declares no `dateModified`

#### Scenario: No product or offer markup
- **WHEN** the structured data is inspected
- **THEN** it contains no `Product`, `Offer`, `AggregateOffer` or `ItemList` node describing the tracked items

#### Scenario: No markup for unavailable result types
- **WHEN** the structured data is inspected
- **THEN** it contains no `FAQPage` node and no `SearchAction`

#### Scenario: Markup adds no third-party requests
- **WHEN** the page loads with analytics consent rejected
- **THEN** the structured data script issues no network request and sets no storage, leaving the consent flow unaffected

### Requirement: Crawler directives and sitemap
The site SHALL serve a `robots.txt` and a `sitemap.xml` from its served root. The `robots.txt` SHALL permit all user agents and SHALL declare the absolute URL of the sitemap. The `sitemap.xml` SHALL list the canonical URL of the landing page and SHALL NOT declare a `lastmod`, since no build step can refresh it.

#### Scenario: Robots file permits crawling and points to the sitemap
- **WHEN** `robots.txt` is fetched from the served root
- **THEN** it allows all user agents and contains a `Sitemap:` line with the sitemap's absolute HTTPS URL

#### Scenario: Sitemap lists the canonical URL
- **WHEN** `sitemap.xml` is fetched
- **THEN** it is valid sitemap XML listing the same URL declared in the page's canonical link, with no `lastmod` element

#### Scenario: Files are reachable at the served root
- **WHEN** both files are requested over HTTPS at the site's served root path
- **THEN** each returns HTTP 200 with its content, not the site's HTML

### Requirement: Search performance instrumentation
The page SHALL carry a Google Search Console verification meta tag in `<head>`, so that indexing coverage and search performance for this site can be observed. The tag SHALL be accompanied by a comment stating what breaks if it is removed. The verification tag SHALL NOT load any resource, set any storage or observe the visitor, and SHALL therefore sit outside the analytics consent gate.

#### Scenario: Verification tag present and annotated
- **WHEN** the page `<head>` is inspected
- **THEN** it contains a `google-site-verification` meta tag preceded by a comment explaining that removing it unverifies the Search Console property

#### Scenario: Verification is inert
- **WHEN** the page loads with analytics consent rejected
- **THEN** the verification tag issues no network request, sets no cookie and writes no storage

#### Scenario: Property reports the project subfolder
- **WHEN** the Search Console property is inspected
- **THEN** it is a URL-prefix property scoped to the project's subfolder, since the site has no DNS control and therefore cannot use a domain property

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

