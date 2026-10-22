# Daily Worklog Security

**Last updated:** October 22, 2026

This page describes the security practices for the Daily Worklog Jira Cloud app published by **Son Nguyen Apps**. The app runs on Atlassian Forge and uses Atlassian services for hosting and Jira access.

## Access control

Daily Worklog requests the Jira `read:jira-work` permission and accesses Jira as the signed-in user. It does not bypass project permissions, issue security, worklog visibility, or site app-access rules. Users should only access information they are authorized to view.

## Data handling

The app processes Jira project and issue identifiers, worklog dates and durations, and author account IDs, display names, and Atlassian-hosted avatar URLs to produce a report. Report data is held in page memory while displayed. Son Nguyen Apps does not operate an external server for the Forge app and does not intentionally store report data in developer-controlled databases or app storage. The app does not send report data to advertising, analytics, or other third-party services.

Atlassian operates the Forge and Jira infrastructure and is responsible for the security controls of those services. Please review Atlassian's applicable security, privacy, and service documentation for details about its infrastructure, encryption, logging, and retention practices. Jira records remain subject to the customer's Jira configuration and retention policies.

## Encryption and infrastructure

The Forge app relies on Atlassian's platform for hosting, authentication, and communications with Jira. We do not make separate claims here about Atlassian's internal encryption implementation or certifications; refer to Atlassian's current documentation for those details. The app does not maintain a separate persistent store of Jira worklog report data.

## Vulnerability reports

To report a suspected security issue, contact **huynhson140198@gmail.com**. Include a description and steps to reproduce, but do not include Jira worklog data, API tokens, passwords, or other secrets. We aim to acknowledge support inquiries within 48 hours; this is a response target, not a guaranteed resolution time.

## Certifications and assessments

Son Nguyen Apps does not claim that Daily Worklog has an independent security certification or third-party security assessment. Atlassian's platform certifications and controls apply to Atlassian's services and should not be interpreted as a certification of this app or its publisher.

This page describes the app's current design and may be updated as the app changes. For privacy practices, see the [Privacy Policy](https://sonnguyenhuynh.github.io/daily-worklog-docs/privacy-policy.html). For support, see the [Support page](https://sonnguyenhuynh.github.io/daily-worklog-docs/support.html).
