# Logiqra Security Policy

## Supported product

**Logiqra 1.2.0** is the current Windows product version and has completed Windows build and automated-test validation. Its official Windows installer is [published on GitHub](https://github.com/vinceemmy/Logiqra/releases/tag/v1.2.0).

The older v1.1.3 download remains a historical release. It should not be confused with the current Logiqra installer.

## Report a vulnerability privately

Please contact the repository maintainer privately to report security weaknesses. Do not publish exploit steps, sensitive logs, passwords, or private infrastructure information in a public GitHub issue.

A useful report includes:

- Affected Logiqra version and Windows version
- Expected and actual behaviour
- Steps to reproduce using synthetic test data
- Sanitised error messages or privacy-safe diagnostics
- Impact and any reasonable mitigation

Allow time for investigation and coordinated disclosure.

## Do not share secrets or live customer evidence

Never publish:

- Web-server logs containing real user activity
- SQLite databases or backups
- Real incident reports and diagnostic files that have not been inspected
- Cloud credentials, service-account JSON files, API tokens, or private keys
- SFTP/FTPS passwords, SSH passphrases, or Cron Micro-Collector secrets
- Session identifiers, cookies, or authorisation values
- Private server addresses, internal paths, or confidential deployment details

Use synthetic samples and redact identifying information.

## Security scope

Logiqra is a log-analysis and investigation tool, not a web application firewall. It does not automatically block requests, patch code, guarantee every alert is malicious, or replace standard security controls.

Only connect systems and data sources you own or are expressly authorised to assess. Limit access to reports, backups, and the local Windows account.

## Release integrity

Before running the 1.2.0 public installer, check its SHA-256 value against the checksum published alongside that **same release**. Do not use a checksum from another build or from the archived v1.1.3 download.

See [Security and Privacy](docs/LOGIQRA_SECURITY_AND_PRIVACY.md) for the product's high-level data-handling design.
