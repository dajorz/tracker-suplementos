## MODIFIED Requirements

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
