## MODIFIED Requirements

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

## ADDED Requirements

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
- **WHEN** a visitor who has already accepted or rejected activates the "Cookies" control below the main content
- **THEN** the consent banner is shown again with both accept and reject actions available

#### Scenario: Withdrawal removes analytics cookies
- **WHEN** a visitor who previously accepted reopens the banner and rejects
- **THEN** the stored choice becomes `rejected`, all `_ga`-prefixed cookies are deleted, and the page reloads so the injected `gtag.js` no longer runs

#### Scenario: Revocation control is unobtrusive
- **WHEN** the page is rendered
- **THEN** the control occupies a single line of small, low-contrast text below `<main>` and does not push the embedded spreadsheet below the fold
