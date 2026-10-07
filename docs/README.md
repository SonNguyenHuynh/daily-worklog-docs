# Daily Worklog for Jira

Daily Worklog helps Jira teams review worklogs by project, contributor, and day. It is a read-only Jira Cloud app: it does not create or edit Jira issues or worklogs.

## Install and open the app

When Daily Worklog is available on the Atlassian Marketplace, ask your Jira site administrator to install it on your Jira Cloud site. The administrator must approve the `read:jira-work` permission. After installation, open Jira and select **Apps > Daily Worklog**.

## Run a report

1. Select a project you can access.
2. Choose the start and end dates. The date range is inclusive and can be up to 93 calendar days.
3. Choose a reporting time zone.
4. Select **Run report**.

The report includes worklogs in the selected project and date range that are available to your Jira account. Results are grouped by worklog author and calendar day, with daily totals and issue-level time details. Use **Hide weekends** to filter weekend rows from the displayed report.

## Date and time-zone behavior

Worklog dates are based on when each entry started. An entry that spans midnight is not split across two days. Jira evaluates the search using the Jira user's time zone, so using the same time zone as your Jira profile helps avoid missing entries around date boundaries.

Jira data can change while a report is loading. If results look stale or unexpected, run the report again and verify important totals in Jira.

## Access and privacy

Daily Worklog requests the `read:jira-work` permission and accesses Jira as the signed-in user. It cannot bypass project permissions, issue security, worklog visibility, or site app-access rules. If a project or worklog is not visible to your Jira account, it will not be available in the report.

The app processes project and issue identifiers, worklog dates and durations, and author account IDs, display names, and Atlassian-hosted avatar URLs to create the report. Report data is held in page memory while you view it; the app does not keep a separate persistent copy in developer-controlled storage. Atlassian's Jira and Forge services process data under their own terms and privacy notices.

See the [Privacy Policy](https://sonnguyenhuynh.github.io/daily-worklog-docs/privacy-policy.html) and [Terms of Service](https://sonnguyenhuynh.github.io/daily-worklog-docs/terms-of-service.html).

## Troubleshooting

- **No projects appear:** Check that your Jira account has access to at least one project, then reload the app.
- **A project or worklog is missing:** Confirm your Jira permissions and worklog visibility. Match the report time zone to your Jira profile time zone and rerun the report.
- **Access denied:** Ask your Jira site administrator to verify the app installation, `read:jira-work` consent, and your project permissions.
- **The report fails or is rate-limited:** Wait briefly and retry. For recurring problems, contact support with the steps to reproduce the issue and any error text. Remove sensitive Jira information from screenshots.

## Support

Email [huynhson140198@gmail.com](mailto:huynhson140198@gmail.com) with bug reports or feature requests. Support is available Monday through Friday, and we aim to acknowledge inquiries within 48 hours. This is a response target, not a guaranteed resolution time.

See the [Support page](https://sonnguyenhuynh.github.io/daily-worklog-docs/support.html).
