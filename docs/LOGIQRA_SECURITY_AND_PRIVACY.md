# Logiqra 1.2.0: Security and Privacy

## Local-first approach

Logiqra is designed to analyse and retain security evidence on the user's Windows computer. Live cloud and remote-log sources require a network connection and appropriate authorisation; the desktop application does not require a mandatory Logiqra cloud account.

The local database and related user-data directories are separate from installed program files. Uninstalling or reinstalling the program should not be confused with deleting evidence. Keep backups before upgrades.

## Credentials and connectors

For sources requiring authentication, use the application's supported protected credential handling. Windows Credential Manager is used for protected secret storage, while non-secret settings and references are stored separately.

- Use read-only SFTP/SSH and FTPS/cPanel permissions where possible.
- Verify SSH host identities and TLS certificates instead of bypassing warnings.
- Keep cloud service-account credentials and API tokens private.
- Deploy the Cron Micro-Collector only on authorised hosting infrastructure and use HTTPS.

Never publish collector secrets, credential files, access tokens, or private keys.

## Evidence minimisation

Web logs can include sensitive request paths, tokens, addresses, and other user activity. Logiqra applies privacy-aware handling to stored evidence and reporting. Log-source administrators must still secure the raw source logs and apply appropriate data-retention policies.

An alert is an indication for review, not proof of a successful intrusion. The absence of a finding is not proof that the monitored system is safe.

## Reports, diagnostics and backups

PDF, CSV, and JSON exports may contain operationally sensitive evidence even after redaction. Store them securely.

Database backups contain stored evidence and must never be committed to a public repository. Inspect diagnostic packages before sharing, and remove any accidentally included credentials or identifying data.

## Public repository boundary

The public Logiqra showcase must not include application source code, real databases, customer logs, incident reports containing personal data, private hosting details, keys, or build environments containing secrets.

The latest official installer should be accompanied by a matching SHA-256 digest. Validate the exact published release before installing; a checksum from an earlier build is not interchangeable.

## Responsible use

Only monitor websites, systems, cloud accounts, and log sources you own or are expressly permitted to assess. Logiqra does not patch, block, or replace application security controls.
