# Logiqra 1.2.0: Technical Architecture

This is a high-level public architecture description. The complete implementation source remains in a private repository. Details may vary by log-source connector.

## Core design

Logiqra is a **local-first Windows desktop application** built with Python and PySide6, using a local SQLite database for persistent evidence and analysis state. The user interface, monitoring coordinator, connectors, parsers, detection logic, incident reconstruction, and reporting layer are separate responsibilities.

~~~mermaid
flowchart LR
    A["Authorised local/remote/cloud logs"] --> B["Connector, checkpoint and collection"]
    B --> C["Parse and normalise"]
    C --> D["Privacy-aware evidence handling"]
    D --> E["Explainable detection rules"]
    E --> F["Findings and risk scoring"]
    F --> G["Attack Stories / incidents"]
    G --> H["Dashboard, alerts and reports"]
~~~

## Data sources

The collection layer includes supported local files, SFTP/SSH, FTPS/cPanel, Cron Micro-Collector batches, manual imports, and supported cloud log connections such as Google Cloud Logging. IIS structured-log ingestion is available for supported formats.

Connectors should read from user-authorised sources without modifying the monitored website. Source data, timestamp quality, and log fields differ, so not all detections apply equally to every connector.

## Monitoring reliability

Checkpoint and duplicate-handling logic limit unnecessary reprocessing after retries, restarts, or log rotation. Monitoring can continue in the system tray. The Start-menu restore path returns users to the existing running application instead of starting a second monitoring process against the same local database.

## Detection and investigation

The detector produces explainable findings from supported log evidence. Categories include SQL injection, XSS, path traversal, command injection, abnormal scanning, password-guessing indicators, restricted-page probing, and possible data-exfiltration indicators when supported by available fields.

Related requests may be reconstructed into Attack Stories and linked to incidents. The analyst remains responsible for validating context, false positives, and business impact.

## Local persistence and privacy

Local SQLite stores monitored evidence, findings, incident state, and settings. Authentication secrets use supported Windows credential protection rather than being deliberately published in plain text. Reports and diagnostics apply privacy-aware handling. Backups must be handled as sensitive evidence.

## UI and reporting

The PySide6 interface exposes Dashboard, website connections, Live Activity, Detection Rules, Incidents, Reports, and Settings. Live Activity supports event-time-based filtering. Reports are exported in PDF, CSV, and JSON.

## Test and distribution boundary

The Windows validation workflow covers static checks, the automated test suite, packaging, and installer generation. The 1.2.0 Windows test recorded **1,059 passing tests** and **86% overall measured coverage**. This does not guarantee perfect detection against real-world traffic.

Only product documentation, selected synthetic evidence, screenshots, and official release artifacts belong in the public showcase. Source code, live logs, database files, private credentials, and deployment secrets must remain private.
