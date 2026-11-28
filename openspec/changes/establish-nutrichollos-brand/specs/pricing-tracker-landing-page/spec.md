## MODIFIED Requirements

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `<title>`, a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`, `og:image`, `og:image:width`, `og:image:height`, `og:image:alt`) and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`).

The `<title>` SHALL lead with the search intent the page targets rather than the project's own name, SHALL end with the brand suffix " | NutriChollos", and SHALL fit within 60 characters. The `meta description` SHALL be written as search-result copy: it SHALL name the brand "NutriChollos", the tracked shops and the two comparison figures the tracker provides, and SHALL fit within 160 characters. `og:title` and `twitter:title` SHALL carry the same string as the `<title>`; `og:description` and `twitter:description` SHALL carry the same string as the `meta description`.

These metadata strings SHALL NOT be required to match the project description sentence rendered in the header; the header sentence states what the project is, while the metadata strings address a visitor who has not yet arrived.

No metadata value SHALL contain a figure that changes over time, such as a product count, a shop count, a price or an update date, because the page has no build step able to refresh it.

`twitter:card` SHALL be `summary_large_image`. `og:image` SHALL be an absolute URL pointing to a raster image asset served from this site's own origin, at least 1200 pixels wide, with an aspect ratio between 1.85:1 and 2:1, and whose filename carries a version suffix so that a future redesign can be published under a new URL rather than relying on third-party scrapers invalidating a cached one. The declared `og:image:width` and `og:image:height` SHALL match the asset's real dimensions exactly.

The image SHALL use a single uniform background colour edge to edge. It SHALL NOT render its safe margin as a visible frame or border of a different shade, because scrapers crop the image to 2:1 and an inset frame becomes visibly thinner on two sides than on the other two.

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a `<title>`, a description meta tag, a canonical link, all the listed Open Graph tags and all the listed Twitter card tags

#### Scenario: Title targets a search query
- **WHEN** the `<title>` is read
- **THEN** it leads with the product category and the market the page covers, ends with " | NutriChollos", is at most 60 characters long, and does not begin with the project's own name

#### Scenario: Description reads as search-result copy
- **WHEN** the `meta description` is read
- **THEN** it is at most 160 characters long, names the brand "NutriChollos" and the tracked shops, and mentions both the recorded historical minimum and the price per 100 g of protein

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

### Requirement: Compact header bar
The page SHALL display a compact, left-aligned header bar at the top of the page containing the project's circular brand mark to the left of the title "NutriChollos" as the `<h1>` and, below the title, the project description "Tracker independiente de precios reales de proteína y creatina en España" rendered in a lighter tone. The brand mark SHALL be decorative (empty `alt`), SHALL reuse the existing home screen icon file rather than a new asset, and SHALL be clipped to a circle. On desktop viewports the brand mark SHALL span the height of both text rows; on mobile viewports the description SHALL span the full header width beneath the brand mark and title. The header SHALL NOT contain the community credit, which belongs to the footer. The header SHALL be visually separated from the content below by a bottom border, SHALL use reduced vertical padding and reduced type sizes compared to a hero section, and SHALL NOT display a status badge.

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

### Requirement: Footer carries the project's secondary information
The `<footer>` SHALL carry, in this order, the full price accuracy disclaimer, the official accounts line, the community credit "Con feedback de la comunidad de Reddit", and the "Cookies" consent revocation control. The footer SHALL remain a landmark element and SHALL NOT contain any control that competes with the Telegram call to action.

#### Scenario: Footer content and order
- **WHEN** the footer's HTML is inspected
- **THEN** the price accuracy disclaimer appears first, followed by the official accounts line, the community credit and the "Cookies" control

#### Scenario: Community credit relocated
- **WHEN** the page is searched for the text "Con feedback de la comunidad de Reddit"
- **THEN** it appears exactly once, inside the `<footer>`, and nowhere in the header

#### Scenario: Footer stays out of the way
- **WHEN** the page is viewed at a 375×667 viewport
- **THEN** the footer renders below the suggestion section and does not overlap the embedded spreadsheet or the consent banner

#### Scenario: Footer introduces no competing call to action
- **WHEN** the footer's interactive elements are inspected
- **THEN** the only controls present are the three official account links and the "Cookies" button, all styled as inline text rather than as button-like calls to action

### Requirement: Structured data
The page SHALL embed structured data as a single inline `application/ld+json` script declaring a `Dataset` describing the price record, a `Person` identifying its maintainer, an `Organization` named "NutriChollos" representing the brand, and a `WebSite` for this site. The `Organization` SHALL declare the canonical URL, an absolute `logo` URL served from this origin, `founder` referencing the `Person`, and `sameAs` listing exactly the official accounts defined by the official accounts requirement. The `WebSite` and the `Dataset` SHALL each declare the `Organization` as `publisher`. The `Dataset` SHALL declare an open-ended `temporalCoverage` and SHALL NOT declare `dateModified`, since no build step can refresh it and a frozen date is a worse signal than none.

The page SHALL NOT declare `Product` or `Offer` markup for the tracked items, because those items are rendered inside a third-party iframe and are therefore not content of this page, and because `Offer` would assert that this site sells them, which is false. The page SHALL NOT declare `FAQPage` or a `SearchAction`, whose corresponding result types are no longer available to a site of this kind.

#### Scenario: Structured data is present and valid
- **WHEN** the page HTML is inspected
- **THEN** `<head>` or `<body>` contains exactly one `application/ld+json` script, it parses as valid JSON, and it validates without errors against the declared schema.org types

#### Scenario: Dataset describes the price record
- **WHEN** the `Dataset` node is inspected
- **THEN** it declares a name, a description, `inLanguage` of `es-ES`, `isAccessibleForFree` of true, the canonical URL, a `creator` referencing the `Person` node and a `publisher` referencing the `Organization` node

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

#### Scenario: Brand is tied to this site and its official profiles
- **WHEN** the `Organization` node is inspected
- **THEN** its `name` is "NutriChollos", its `url` is the canonical URL, its `logo` is an absolute URL on this origin at least 112×112 pixels, and its `sameAs` contains exactly the X, Telegram and TikTok URLs of the official accounts

#### Scenario: Website is published by the brand
- **WHEN** the `WebSite` node is inspected
- **THEN** its `name` is "NutriChollos", its `url` is the canonical URL, its `publisher` references the `Organization`, and it declares no `potentialAction`

## ADDED Requirements

### Requirement: Official accounts
The page SHALL list, in the footer, the project's official accounts as inline text links, stating that they are the only official accounts of NutriChollos: X at `https://x.com/nutrichollos`, Telegram at `https://t.me/NutriChollos` and TikTok at `https://www.tiktok.com/@nutrichollos_tracker`. Each link SHALL open in a new browsing context with `rel` including `me`, `noopener` and `noreferrer`, and its accessible name SHALL include the platform, the handle and that it opens in a new window. The page SHALL NOT link to, name or mention any third-party account using a similar name, in particular the TikTok profile `@nutrichollos`. The set of URLs in this list SHALL be identical to the `sameAs` set of the `Organization` structured data node.

#### Scenario: Official accounts are listed in the footer
- **WHEN** the footer is read
- **THEN** it names X, Telegram and TikTok with their handles, links each to its official URL, and states that there are no other official accounts

#### Scenario: Third-party profile is never linked
- **WHEN** every `href` and every structured data value in the page is inspected
- **THEN** none points to `https://www.tiktok.com/@nutrichollos` or any other TikTok profile than `@nutrichollos_tracker`

#### Scenario: Visible list matches structured data
- **WHEN** the footer links are compared with the `Organization` node's `sameAs`
- **THEN** both contain the same three URLs

#### Scenario: Links leave the consent flow untouched
- **WHEN** a visitor who has rejected or not answered the consent banner activates an official account link
- **THEN** the profile opens in a new browsing context, `window.opener` is null, and no cookie or analytics request is made

### Requirement: Price table legend
The page SHALL display a legend directly after the embedded spreadsheet and before the suggestion section, explaining the row states and the price range indicator of the published sheet. The legend SHALL NOT be placed between the header and the spreadsheet.

The legend SHALL describe four row states, each with a colour swatch matching the published sheet and a text label, so that meaning does not depend on colour alone:
- dark green (`#93c47d`): the product is at its recorded minimum;
- light green (`#d9ead3`): cheap compared with its own history;
- white: usual price, not enough history, or no price changes;
- grey with struck-through text: not available at the shop, showing the last known data.

The legend SHALL explain that the range column reads `minimum [·●········] maximum` of the prices recorded for that product, that the dot marks where the current price falls within that range, and that a dot on the left means the current price is close to its recorded minimum. It SHALL note that the indicator is relative to that product's own history, so with little history or a narrow range it carries little information.

When the published sheet changes its row colours or the range format, the legend SHALL be updated in the same change.

#### Scenario: Legend follows the spreadsheet
- **WHEN** the page HTML is inspected
- **THEN** the legend appears after the iframe and before the suggestion section, and nothing new appears between `</header>` and the iframe

#### Scenario: Row states are explained without relying on colour
- **WHEN** the legend is read with colours disabled
- **THEN** each of the four row states is still identifiable by its text label, and the not-available state is shown with struck-through sample text

#### Scenario: Swatches match the sheet
- **WHEN** the swatch colours are compared with the background colours used by the published sheet
- **THEN** they match for dark green, light green and the not-available grey

#### Scenario: Range indicator is explained
- **WHEN** the legend's range explanation is read
- **THEN** it states that the bounds are the recorded minimum and maximum, that the dot is the current price, that left means close to the recorded minimum, and that the reading is relative to the product's own history

#### Scenario: Legend text is legible
- **WHEN** the contrast of the legend text against its background is measured
- **THEN** it is at least 4.5:1
