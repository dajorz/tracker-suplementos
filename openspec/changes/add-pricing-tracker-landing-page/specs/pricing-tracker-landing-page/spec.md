## ADDED Requirements

### Requirement: Static single-file landing page
The system SHALL provide a single self-contained `index.html` file at the repository root, using semantic HTML5 and Tailwind CSS loaded from a CDN, with no build step required for deployment via GitHub Pages.

#### Scenario: Page loads with no build tooling
- **WHEN** `index.html` is served directly (e.g., via GitHub Pages) without any compilation step
- **THEN** the page renders fully styled using the Tailwind CDN script and displays all sections (header, suggestion CTA, iframe, footer)

### Requirement: Hero header with status badge
The page SHALL display a header containing the title "Tracker de Precios de Suplementación", the subtitle "Monitorización e histórico independiente de precios de creatina y proteína en España.", and a badge with a green indicator reading "Actualizado en tiempo real".

#### Scenario: Header renders on load
- **WHEN** a visitor opens the page
- **THEN** the title, subtitle, and "Actualizado en tiempo real" badge with a green indicator are visible at the top of the page

### Requirement: SEO and social sharing metadata
The page SHALL declare `lang="es"` on the `<html>` element and include a `meta description`, a canonical link, Open Graph tags (`og:title`, `og:description`, `og:type`, `og:url`, `og:locale`), and Twitter card tags (`twitter:card`, `twitter:title`, `twitter:description`).

#### Scenario: Metadata present for crawlers and social previews
- **WHEN** the page HTML is inspected
- **THEN** the `<html>` element declares `lang="es"` and the `<head>` contains a description meta tag, a canonical link, the listed Open Graph tags, and the listed Twitter card tags

### Requirement: Product suggestion call to action
The page SHALL display a call to action positioned above the embedded spreadsheet, headed "¿Echas en falta algún producto?", inviting visitors to suggest missing products or send any other feedback, with a link styled as a button labeled "Proponer producto". The link SHALL open the visitor's mail client at the site owner's address with the prefilled subject "Sugerencia para el tracker". The page SHALL NOT contain an email input field or submit to any third-party form service, and SHALL NOT offer price-drop alerts.

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
The page SHALL display a cookie-consent banner on first visit offering accept and reject actions, persist the visitor's choice, and load the GA4 `gtag.js` snippet with measurement ID placeholder `G-XXXXXXXXXX` only after consent is accepted.

#### Scenario: No analytics before consent
- **WHEN** a visitor loads the page and has not yet accepted the consent banner
- **THEN** no `gtag.js` request is made and no analytics cookie is set

#### Scenario: Analytics loads after acceptance
- **WHEN** the visitor accepts the consent banner
- **THEN** the `gtag.js` script is injected and initialized with the `G-` prefixed measurement ID

#### Scenario: Choice is remembered
- **WHEN** the visitor returns to the page after having accepted or rejected
- **THEN** the banner is not shown again and the previous choice is honoured

### Requirement: Footer with author credit
The page SHALL display a footer with the text "Desarrollado por @dajorz · Herramienta independiente de comparación." and a link to `https://github.com/dajorz`.

#### Scenario: Footer renders with working link
- **WHEN** a visitor scrolls to the bottom of the page
- **THEN** the footer text is visible and the GitHub profile link points to `https://github.com/dajorz`
