# Daily Worklog for Jira

Daily Worklog is a Jira Cloud Forge app. It displays worklogs from the current Jira user in a `jira:globalPage` built with Custom UI. Users choose a project, date range and reporting time zone; results are grouped by member and day, with issue-level details.

This repository is an implementation, not a registered, deployed or Marketplace-approved app. The app ID in `manifest.yml` is a placeholder until you register this project with your Atlassian account.

## Authentication and permissions

- The Custom UI calls Forge resolvers with `@forge/bridge` `invoke()`.
- Resolvers use `api.asUser().requestJira()`. Forge supplies the current user's identity and handles Jira consent; the app does not ask users for email addresses, API tokens or passwords.
- No credentials or worklog data are persisted by the app. Report data is held in page memory while the user views it.
- `read:jira-work` is the only declared Jira scope. It covers the read-only project, issue-search and worklog requests used by the resolver. Jira project permissions, issue security, worklog visibility and app access rules still apply; this app does not bypass them.
- Avatar images are loaded by the browser from Atlassian-hosted domains allowed by the manifest's image CSP. No Jira API calls or worklog data are sent to third-party services.
- The UI does not send Jira requests directly. All Jira REST access belongs in the Forge resolver.

## Project layout

```text
manifest.yml                 Jira Global Page, Custom UI resource, resolver and scope
package.json                 Build, development and test commands
package-lock.json            Reproducible dependencies
jest.config.cjs
src/
  index.ts                   Forge resolver and as-user Jira gateway
  contracts.ts               Shared request and response types
  report.ts                  Input validation, date bounds and aggregation
  report.spec.ts             Aggregation and time-zone tests
  service.ts                 Jira pagination, concurrency and error handling
  service.spec.ts            Mocked Jira API tests
frontend/
  vite.config.ts              Custom UI build and development server
  src/App.tsx                 Report filters, progress, issue details and table
  src/preview.tsx             Development-only mock responses
  src/styles.css
  dist/                       Generated Custom UI assets
```

The resolver reads accessible projects, searches matching issues with Jira's enhanced JQL endpoint, and reads paginated worklogs. Worklog requests are processed with bounded concurrency. Dates are inclusive, limited to 93 calendar days, and grouped by the selected IANA time zone. Jira evaluates JQL worklog dates in the user's Jira time zone, so match the report time zone to that setting to avoid candidate issues being missed around date boundaries.

## Prerequisites

- Node.js 22 and npm
- A Jira Cloud site where you can install development apps
- A Forge Developer Space and the Forge CLI

Install the dependencies and verify the local project:

```powershell
npm ci
npm run build
npm test
npm run test:coverage
```

`npm run build` typechecks the project and writes the Custom UI to `frontend/dist`, the resource path in `manifest.yml`. Do not commit generated build output.

## Local preview and Forge tunnel

To preview the UI with mock data outside Jira:

```powershell
$env:VITE_USE_MOCK = "true"
npm run dev
```

Open `http://127.0.0.1:5173/`. This preview does not call Jira and does not verify Forge integration. Clear the flag before using a Forge tunnel or building for deployment:

```powershell
Remove-Item Env:VITE_USE_MOCK -ErrorAction SilentlyContinue
```

For development inside Jira, use two terminals. In the first terminal, make sure the mock flag is unset and start Vite:

```powershell
npm run dev
```

In the second terminal, route the installed development app to the local UI and resolver:

```powershell
forge tunnel --environment development
```

The Vite port `5173` must be available; it is configured in `manifest.yml`. Forge bridge calls work in the Jira iframe, not on the standalone Vite page unless mock mode is enabled.

## Register, deploy and install

Install and authenticate the Forge CLI:

```powershell
npm install -g @forge/cli@latest
forge login
```

Register this existing app once. Do not run `forge create` in this repository; that command is for scaffolding a separate project.

```powershell
forge register "Daily Worklog"
```

Registration replaces the placeholder app ID in `manifest.yml`. Keep that ID for subsequent deployments. Then build, lint, deploy and install on your development site:

```powershell
npm run build
forge lint
forge deploy --environment development
forge install --environment development --site your-site.atlassian.net --product jira
```

Open the installed app from Jira's Apps navigation and approve the requested `read:jira-work` consent. After changing the manifest or scopes, rebuild and deploy, then upgrade the installation:

```powershell
npm run build
forge lint
forge deploy --environment development
forge install --upgrade --environment development --site your-site.atlassian.net --product jira
```

Before production, deploy and test in a staging environment and install it on a test site. Use `forge deploy --environment production` only after acceptance checks pass. Deployment alone does not publish an app to Marketplace.

## Marketplace release checklist

Marketplace publication requires more than a successful Forge deployment. Confirm the current Atlassian requirements before submitting:

- Use an eligible Marketplace Partner account and complete the applicable identity, business and Developer Space setup.
- Provide accurate product details, screenshots, support contact, documentation, end-user terms and a public privacy policy.
- Explain why `read:jira-work` is needed and accurately disclose what worklog and user data the app processes.
- Complete the Developer Console privacy, security and distribution details. Do not claim certifications or data-residency guarantees that have not been independently established.
- Test installation, consent, access restrictions, error handling, keyboard navigation, accessibility and narrow viewports on Jira Cloud.
- Submit the app listing for Atlassian review and respond to reviewer requests. Approval and listing activation are separate from deployment.

This implementation does not enforce paid-app licensing. Keep the listing free unless backend license enforcement and the related test/install workflow are added. Never trust a license state supplied only by the frontend.

## Acceptance checks

Before release, compare results with a small Jira dataset and verify:

- Multiple issues for the same member and date combine into one member/day total; members with identical display names remain distinct.
- Start and end dates are inclusive, time-zone boundaries behave as expected, and weekend filtering updates displayed totals.
- More than one issue-search page and more than one worklog page are included exactly once.
- Users without project access, restricted issues/worklogs, revoked consent and app access restrictions cannot see inaccessible data.
- Empty results, invalid inputs, ranges longer than 93 days, rate limiting, retry failure and cancellation produce the expected UI states.
- The installed Jira iframe works at desktop and narrow viewport sizes, including keyboard navigation and consent/error states.

The report reflects worklog `started` timestamps; entries spanning midnight are not split. Jira search is eventually consistent, so edits made while a report is loading can make results stale. This report is not an immutable payroll or audit ledger. Unit tests cover the report core and service behavior with mocked Jira responses; they do not replace live Jira, Marketplace or UI acceptance testing.

## Official references

- [Forge CLI: register an existing app](https://developer.atlassian.com/platform/forge/cli-reference/register/)
- [Forge tunneling](https://developer.atlassian.com/platform/forge/tunneling/)
- [Forge app distribution](https://developer.atlassian.com/platform/forge/distribute-your-apps/)
- [Publish a Developer Space](https://developer.atlassian.com/platform/forge/developer-space/publish-developer-space/)
- [List a Forge app in Marketplace](https://developer.atlassian.com/platform/marketplace/listing-forge-apps/)
- [Marketplace approval guidelines](https://developer.atlassian.com/platform/marketplace/app-approval-guidelines/)
- [Marketplace security requirements](https://developer.atlassian.com/platform/marketplace/security-requirements/)
- [Forge user privacy guidelines](https://developer.atlassian.com/platform/forge/user-privacy-guidelines/)

Atlassian CLI and Marketplace requirements can change; recheck the official documentation before each release.
