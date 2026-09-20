## MODIFIED Requirements

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

## ADDED Requirements

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
