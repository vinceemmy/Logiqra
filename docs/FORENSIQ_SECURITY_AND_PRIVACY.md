# ForensiQ Security and Privacy

This document describes the security and privacy design of **ForensiQ 1.1.3**.

ForensiQ must be used only with websites, servers, hosting accounts, and log files that you own or are expressly authorised to monitor.

## Local-first design

ForensiQ is designed as a local Windows security-analysis application.

The following information is stored on the current Windows computer:

- Parsed web-server requests
- Detection findings
- Attack Stories and incidents
- Analyst notes and workflow history
- Monitoring settings and source checkpoints
- Generated reports
- Backups
- Runtime logs
- Application configuration

ForensiQ has no mandatory cloud backend, analytics service, telemetry account, AI API, subscription, or paid usage service.

Local log monitoring can operate without an internet connection. Remote SFTP, FTPS, and Cron Micro-Collector sources use the user's normal hosting connection but do not require a ForensiQ cloud service.

## Local data storage

ForensiQ stores user-specific application data separately from the installation directory, inside the current Windows user's Local AppData area.

A standard installation uses a location similar to:

```text
%LOCALAPPDATA%\ForensiQ\ForensiQ
```

This data can include the local SQLite database, logs, reports, backups, cache files, and settings.

Uninstalling ForensiQ removes the application files but does not automatically remove this user data. This prevents accidental loss during upgrades or repairs.

Users who require a complete reset must remove the remaining ForensiQ user-data folder and any related Windows Credential Manager entries after exporting anything they need.

## Credentials and secrets

Passwords, SSH key passphrases, and collector secrets are stored through Windows Credential Manager.

The ForensiQ SQLite database stores only a non-secret credential reference. It does not store the plaintext password or collector secret.

There is no plaintext credential fallback.

Users must not place credentials inside:

- Public repositories
- Screenshots
- Diagnostic reports
- Issue reports
- Example configuration files
- Shared access logs
- Generated portfolio material

## Connection security

### SFTP/SSH

ForensiQ:

- Pins the exact SHA-256 SSH host-key fingerprint
- Verifies the server identity before authentication
- Supports protected passwords, SSH agent or default keys, and an optional local identity file
- Uses read-only file operations
- Does not run remote shell commands
- Closes each short-lived connection after catching up

### FTPS/cPanel

ForensiQ:

- Uses explicit FTPS
- Requires trusted certificate validation
- Requires hostname validation
- Requires TLS 1.2 or newer
- Uses a protected data channel
- Uses passive, read-only metadata and retrieval operations
- Does not fall back to plain FTP

### Cron Micro-Collector

ForensiQ:

- Requires HTTPS
- Verifies endpoint binding
- Verifies batch identity and byte ranges
- Verifies SHA-256 content digests
- Verifies HMAC-SHA256 signatures
- Rejects altered, incomplete, oversized, overlapping, gapped, or misdirected batches
- Keeps queued batches private on the hosting account until they are acknowledged

The real collector `config.php` file and collector secret must never be committed to a public repository.

## Evidence privacy

Before permanent storage, ForensiQ redacts or protects commonly sensitive request data, including:

- Password fields
- Authorization values
- Cookies
- Session identifiers
- JWTs
- API keys
- Common token and secret parameters
- Sensitive paths and query values

Users may choose to retain a complete client IP address or store only a masked network identity:

- IPv4 `/24`
- IPv6 `/64`

Retention settings can remove old request and review evidence automatically.

## Source-log responsibility

ForensiQ protects the evidence it stores, but the original web-server log may still contain sensitive information before ForensiQ reads it.

Users remain responsible for:

- Securing the original access logs
- Restricting operating-system and hosting access
- Using read-only credentials where possible
- Applying suitable log-retention policies
- Avoiding unnecessary logging of passwords, tokens, or personal data
- Following applicable privacy, employment, and data-protection requirements

## Reports and exports

Generated PDF, CSV, and JSON reports redact:

- Credentials
- Session values
- Authorization material
- Host-level client IP details

Reports are written atomically, protected against accidental overwrite, assigned a SHA-256 checksum, and recorded in the local export history.

Even after redaction, reports may contain security-sensitive operational information. Store and share them carefully.

## Runtime logs and diagnostics

Runtime logs use credential redaction and network-mask client IP addresses.

Privacy-safe diagnostic ZIPs contain only:

- A small system-health manifest
- A bounded, re-redacted runtime-log tail
- Non-secret application and environment details needed for troubleshooting

Diagnostic ZIPs do not contain:

- The SQLite database
- Database backups
- Windows Credential Manager secrets
- Full access logs
- Unredacted host-level client IP addresses

Inspect every diagnostic package before sharing it.

## Backups

ForensiQ backups are complete local SQLite copies and therefore contain the same protected evidence stored in the application.

Each backup:

- Is created only after database integrity checks
- Receives a SHA-256 checksum
- Is stored locally
- Is subject to bounded retention

Treat backup files as sensitive. Do not upload them to GitHub, attach them to public issues, or share them through an untrusted channel.

## Release integrity

Official Windows releases include a SHA-256 manifest.

Before installation, verify the installer with:

```powershell
Get-FileHash .\ForensiQ-1.1.3-Setup.exe -Algorithm SHA256
```

Compare the result with the corresponding value in `SHA256SUMS.json` from the same official release.

The current portfolio release may be unsigned. Users should install it only when it was obtained from the official ForensiQ project page and its checksum has been verified.

## Access control

Anyone with sufficient access to the Windows user account or local computer may be able to access ForensiQ data.

Users should:

- Protect the Windows account with a strong sign-in method
- Restrict access to authorised users
- Use full-disk encryption where appropriate
- Keep Windows and security software updated
- Protect exported reports and backups
- Avoid sharing the ForensiQ data folder

ForensiQ does not replace Windows account security or disk encryption.

## Product limitations

ForensiQ is a detection and analysis tool.

It does not:

- Automatically block attacks
- Patch vulnerable applications
- Guarantee detection of every malicious request
- Guarantee that every alert is a real attack
- Replace secure coding, parameterised queries, patching, access control, backups, or network security controls
- Replace professional legal, compliance, or incident-response judgement

Detection results should be reviewed in context before action is taken.

## Responsible deployment

Connect only websites, hosting accounts, servers, and log files that you own or are authorised to monitor.

Use read-only hosting credentials whenever possible.

Do not use ForensiQ to access, probe, disrupt, monitor, or analyse third-party systems without permission.

## Public repository safety

The public ForensiQ showcase must not contain:

- The private source code
- SQLite databases
- Access logs
- Reports containing real activity
- Real collector `config.php`
- Collector state files
- Credentials
- Private keys
- Hosting paths
- Personal server addresses
- Generated build environments
- Private diagnostic packages

The complete source repository should remain private unless the owner explicitly decides otherwise.

## Licence

ForensiQ is proprietary freeware.

Users may download, install, and use the unmodified official release free of charge for lawful and authorised purposes. Modification, redistribution, resale, reverse engineering, branding removal, and claiming authorship are prohibited except where applicable law provides otherwise.

See `LICENSE.txt` for the complete terms.
