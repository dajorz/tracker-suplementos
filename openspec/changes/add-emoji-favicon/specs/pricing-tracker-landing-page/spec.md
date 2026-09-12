## ADDED Requirements

### Requirement: Favicon
The page SHALL declare a favicon in `<head>` via a `<link rel="icon">` element using an inline data URI (no additional binary asset file), so browser tabs show a distinct icon instead of the browser's default document icon. The favicon SHALL NOT use any third-party trademarked logo or icon (e.g. Google's Sheets/Drive iconography).

#### Scenario: Favicon declared without an extra asset file
- **WHEN** the page HTML `<head>` is inspected
- **THEN** it contains a `<link rel="icon">` element whose `href` is a `data:` URI, and no new binary favicon file exists in the repository

#### Scenario: No third-party trademarked icon used
- **WHEN** the favicon's content is inspected
- **THEN** it is a generic emoji glyph rendered via inline SVG, not a copy of Google's (or any other company's) logo or product icon
