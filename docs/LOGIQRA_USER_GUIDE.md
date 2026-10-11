# Logiqra 1.2.0: User Guide

Logiqra is a Windows application for analysing authorised web and cloud-log activity. Detection depends on the log source and fields available; it does not establish that an attack succeeded.

## Typical workflow

1. Add an authorised website or log source in the connection setup.
2. Test and save the connection using the required read-only or cloud permissions.
3. Start or resume monitoring and check source health.
4. Review Dashboard totals and **Live Activity**.
5. Examine findings under Detection Rules, alerts, and the investigation views.
6. Correlate related requests through **Attack Stories** and **Incidents**.
7. Record investigation notes and export reports when required.

## Connections and source types

Depending on source configuration, Logiqra can work with:

- Local Apache/XAMPP/Nginx access logs
- Supported structured IIS logs
- SFTP/SSH and FTPS/cPanel log files
- HTTPS Cron Micro-Collector batches
- Supported Google Cloud Logging / Firebase-related and other cloud log sources
- Manual import

Enable only connections you own or are authorised to inspect. Do not put cloud credentials, password files, SSH private keys, or collector secrets into public repositories.

## Monitoring and system tray

Monitoring can run while the main window is hidden in the Windows system tray. The **Running** state uses a pulsing green indicator and elapsed-time display. Pausing preserves existing stored findings and monitoring checkpoints.

Use **Quit** from the tray menu to close the application completely. If it is still running in the tray, clicking Logiqra in the Windows Start menu restores the original window rather than opening a second monitor against the same database.

## Live Activity

Live Activity displays recently collected requests and their classifications. Where present, controls allow selection of data source, status, severity, and record limit.

The **event-time filter** supports All Time and the last **5, 10, 30, 60, or 120 minutes**, using the request's recorded event timestamp rather than when the request was imported. Recent windows update over time; older requests age out of the displayed window.

The sidebar activity indicator animates while monitoring is Running and stops animating while Paused. A future UI polish will make its ice-blue glow more visible.

## Detection Rules and findings

Logiqra supports explainable detection for SQL injection, XSS, path traversal, command injection, suspicious automation/scanning, rapid authentication attempts, restricted-page probing, and possible data-exfiltration indicators where applicable log evidence exists.

Rules can be configured using tick-style selection controls. A finding may include matched evidence, severity, confidence, or scoring information. A high severity does not by itself prove a successful compromise.

## Incidents and Attack Stories

Attack Stories group potentially related suspicious requests so an analyst can inspect the sequence of activity instead of individual rows alone. Incidents provide an investigation workflow, review status, and notes. Confirm the underlying requests and the relevant website context before drawing conclusions.

## Reports

Supported export formats are **PDF**, **CSV**, and **JSON**. In Logiqra 1.2.0, exported filenames use the `logiqra-` prefix. Treat downloaded reports as potentially sensitive even when privacy redaction is applied.

## Limitations

- Logiqra does not block attacks or fix vulnerable sites.
- It cannot analyse request-body content that the source log never recorded.
- Source-specific fields determine which detections are meaningful.
- Alerts can be false positives or incomplete.
- Testing must be limited to authorised systems.

For installation instructions, see [Installation and Upgrade](LOGIQRA_INSTALLATION_GUIDE.md).
