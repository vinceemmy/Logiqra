# ForensiQ User Guide

This guide explains how to use **ForensiQ 1.1.3** to collect supported web-server access logs, review SQL-injection detections, investigate Attack Stories, and export privacy-aware reports.

Use ForensiQ only with websites, servers, hosting accounts, and log files that you own or are authorised to monitor.

## 1. Main workflow

A typical ForensiQ workflow is:

1. Open **Websites** and add a web-server log source.
2. Complete the read-only connection test.
3. Save the website.
4. Start or resume monitoring.
5. Review source health and totals on **Dashboard**.
6. Use **Live Activity** to inspect incoming requests.
7. Use **Incidents** to investigate reconstructed Attack Stories.
8. Add notes and update the incident status.
9. Use **Reports** to export privacy-redacted investigation material.
10. Use **Settings** for monitoring preferences, backups, diagnostics, and maintenance.

## 2. Supported log sources

ForensiQ supports four automatic source types:

- **Local log file** for Apache, XAMPP, or Nginx
- **SFTP/SSH** using read-only remote file access
- **FTPS/cPanel Raw Access**
- **Cron Micro-Collector** for supported shared-hosting environments

A separate **Manual Import** option is available for historical logs, emergency analysis, or hosting environments that do not support one of the automatic methods.

## 3. Add a website

Open **Websites**, then select the option to add a new website.

### Website details

Enter:

- Website name
- Website address
- Web-server type
- Access-log format

Use **Auto-detect** when the exact supported format is not known.

### Connection method

Choose one of the supported automatic methods.

For every method, ForensiQ validates the required fields before the connection can be tested.

### Connection test

ForensiQ performs a real read-only test.

The website cannot be saved until the test succeeds.

### Local XAMPP example

A standard XAMPP Apache access log is commonly located at:

```text
C:\xampp\apache\logs\access.log
```

Example details:

```text
Website name: Local XAMPP Test Site
Website address: http://localhost
Web server: Apache / XAMPP
Access-log format: Auto-detect
```

Choose the local log-file method and select the access-log path.

## 4. Monitoring controls

The main monitoring controls are:

- **Start** starts every enabled automatic source.
- **Pause** temporarily stops collection while preserving checkpoints.
- **Resume** continues from the saved checkpoints.
- **Quit ForensiQ** fully closes the application from the Windows system tray.

Pausing does not delete requests, incidents, reports, or checkpoints.

Each website has its own:

- Monitor
- Health state
- Checkpoint
- Retry path
- Recent monitoring history

A failure on one website does not stop unrelated websites.

## 5. System-tray behaviour

Closing the main ForensiQ window can leave the application running in the Windows system tray when that option is enabled.

The tray menu provides:

- **Open ForensiQ**
- **Pause monitoring** or **Resume monitoring**
- **Quit ForensiQ**

To close the application completely, use **Quit ForensiQ** from the tray menu.

## 6. Dashboard

The Dashboard provides a high-level view of the current local data.

It can show:

- Connected websites
- Processed requests
- Suspicious requests
- Active incidents or Attack Stories
- Severity totals
- Source health
- Last synchronisation information
- Recent alerts and actionable findings

Use the Dashboard to confirm that monitoring is healthy before moving into detailed investigation.

## 7. Live Activity

Live Activity shows the most recent stored requests.

Available controls include:

- Search
- Website filter
- Classification filter
- Severity filter
- Source filter
- Latest 100, 250, or 500 rows
- Needs Review tab
- Full request target viewer

### Request classifications

A request may appear as:

- **Normal**
- **Suspicious**
- **Needs Review**

Suspicious rows display their severity, such as Low, Medium, High, or Critical.

### Full request target

Select a row to inspect the complete request target in the full-request viewer beneath the table.

This is useful when the table shortens a long request.

### Needs Review

Malformed, incomplete, or unsupported entries can be placed in the privacy-safe Needs Review queue rather than being discarded.

## 8. Detection Rules

ForensiQ Version 1 focuses on explainable SQL-injection detection.

The Detection Rules page allows individual rules to be enabled or disabled.

Each enabled rule can contribute:

- Matched evidence
- Risk
- Confidence
- Scoring factors
- Rule explanation
- Severity

Use the checkboxes to enable or disable a rule.

Disabling a rule changes future analysis. It does not erase existing stored findings.

## 9. Incidents and Attack Stories

ForensiQ groups related actionable activity into persistent **Attack Stories** and incidents.

The Incidents page supports:

- Search
- Website filtering
- Status filtering
- Severity filtering
- Attack Story chronology
- Request evidence
- Matched rules
- Relationship reasons
- Analyst notes
- Status changes

### Incident statuses

An incident can be:

- **Open**
- **Investigating**
- **Resolved**
- **Dismissed**

A resolved or dismissed incident can reopen automatically when new actionable activity enters the same Attack Story.

### Investigation workflow

A simple investigation process is:

1. Open the incident.
2. Review the summary and severity.
3. Inspect the Attack Story timeline.
4. Open the linked requests.
5. Review the matched evidence and rule explanations.
6. Add an analyst note.
7. Change the status when appropriate.
8. Export a report when needed.

## 10. Reports

ForensiQ can export selected incidents as:

- PDF
- CSV
- JSON

Reports can include:

- Attack Story timeline
- Relationship reasons
- Request evidence
- Matched rules
- Scoring factors
- Analyst history
- Conclusion

Exports apply privacy redaction to sensitive information such as credentials, session values, authorization material, and host-level client IP details.

Reports are written atomically, protected against accidental overwrite, assigned a SHA-256 checksum, and recorded in the local export history.

Even after redaction, reports may contain operational security information. Store and share them carefully.

## 11. Manual Import

Use Manual Import for:

- Historical analysis
- Emergency review
- Offline investigation
- Unsupported hosting
- A temporary automatic-connection problem

Supported inputs include plain and Gzip Apache or Nginx access logs.

ZIP containers are rejected.

Manual imports use the same:

- Privacy protection
- Parsing
- SQL-injection rules
- Scoring
- Evidence handling
- Duplicate protection
- Attack Story pipeline

Manual-only sources are treated as fallback sources and are not monitored automatically.

## 12. Notifications

ForensiQ creates durable local notifications for qualifying incident activity.

The notification centre supports:

- Unread
- Read
- Dismissed

A configurable minimum severity determines which activity produces alerts.

Optional Windows notification-area messages use the existing ForensiQ tray icon.

## 13. Settings

Settings controls application behaviour, monitoring defaults, privacy options, alert thresholds, and maintenance.

Typical options include:

- Start automatic monitoring when ForensiQ launches
- Start monitoring after saving a website
- Continue active monitoring in the system tray
- Default polling interval for future connections
- Minimum incident severity
- Minimum confidence
- Alert severity threshold
- Evidence-retention behaviour
- Client-IP privacy level

Changes to default intervals apply to future website connections unless an explicit bulk action is used for existing automatic sources.

## 14. Maintenance and diagnostics

Open **Settings > Release maintenance and diagnostics** to:

- Check SQLite integrity and foreign-key consistency
- Create a verified local database backup
- Create a privacy-safe diagnostic ZIP

Equivalent command-line actions are:

```powershell
ForensiQ.exe --check
ForensiQ.exe --backup
ForensiQ.exe --diagnostics
```

### Database check

The database check confirms SQLite integrity and foreign-key consistency.

### Backup

A backup is created only after the database passes integrity checks.

Each backup receives a SHA-256 checksum.

Backups contain the same protected evidence as the main database and must be kept private.

### Diagnostics

Diagnostic ZIPs include a small health manifest and a bounded, re-redacted runtime-log tail.

They do not include:

- The SQLite database
- Database backups
- Windows Credential Manager secrets
- Full access logs

Inspect diagnostic files before sharing them.

## 15. Upgrades and reinstallations

Installing a newer version replaces the application files but preserves the current Windows user's local database, settings, reports, and saved credential references.

Uninstalling and reinstalling ForensiQ may therefore restore the same websites, requests, incidents, and settings.

This is expected.

A complete reset requires removal of the separate ForensiQ user-data folder and related Windows Credential Manager entries.

## 16. Troubleshooting

### ForensiQ remains open after closing the window

The application may still be running in the system tray.

Right-click the ForensiQ tray icon and select **Quit ForensiQ**.

### A website shows Retrying

Check:

- The source is available
- The file path is correct
- The remote server is reachable
- The credentials are valid
- The account still has read-only access
- The pinned host key or certificate is correct

ForensiQ automatically retries temporary connector failures.

### Live Activity does not show a new request

Check that:

- Monitoring is running
- The website status is Healthy
- The correct access log is connected
- The web server actually wrote the request to the log
- The selected Live Activity filters do not hide the row

### Previous data appears after reinstalling

This is normal because the user data is stored separately from the application program files.

## 17. Important limitations

ForensiQ analyses supported web-server access logs.

Standard access logs normally do not contain HTTP request bodies. Body-only attacks may therefore be invisible.

ForensiQ does not:

- Automatically block requests
- Modify server files
- Patch vulnerable applications
- Guarantee detection of every attack
- Guarantee that every alert is malicious
- Replace secure coding
- Replace parameterised database queries
- Replace patching and least-privilege database permissions
- Replace a properly configured WAF
- Replace professional incident-response judgement

Review findings in context before taking action.

## 18. Responsible use

Use ForensiQ only with systems, websites, servers, hosting accounts, and logs that you own or are explicitly authorised to monitor.

Do not use it to access, probe, disrupt, monitor, or analyse third-party systems without permission.

## 19. Licence

ForensiQ is proprietary freeware.

Users may download, install, and use the unmodified official release free of charge for lawful and authorised purposes.

Modification, redistribution, resale, reverse engineering, branding removal, and claiming authorship are prohibited except where applicable law provides otherwise.

Read `LICENSE.txt` for the complete terms.
