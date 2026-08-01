# ForensiQ Installation Guide

This guide covers the installation, upgrade, verification, and removal of **ForensiQ 1.1.3** on Windows 10 and Windows 11.

ForensiQ must be used only with websites, servers, hosting accounts, and log files that you own or are authorised to monitor.

## System requirements

- Windows 10 or Windows 11
- A 64-bit Windows installation
- Permission to install desktop software
- Access to at least one supported web-server log source

ForensiQ does not require a ForensiQ cloud account, subscription, paid API, AI service, inbound Windows port, or paid hosting service.

## Release files

Download these files from the same official ForensiQ release:

```text
ForensiQ-1.1.3-Setup.exe
SHA256SUMS.json
LICENSE.txt
```

Do not download the installer from an unofficial mirror or third-party upload.

## Verify the installer

Before installation, open PowerShell in the folder containing the installer and run:

```powershell
Get-FileHash .\ForensiQ-1.1.3-Setup.exe -Algorithm SHA256
```

Compare the displayed SHA-256 value with the installer entry inside `SHA256SUMS.json` from the same release.

Continue only when the values match exactly.

## Normal installation

1. Double-click `ForensiQ-1.1.3-Setup.exe`.
2. Follow the installation wizard.
3. Keep the Start menu shortcut.
4. Select the optional desktop shortcut only when useful.
5. Complete the installation and launch ForensiQ.

The current portfolio release may be unsigned. Windows may display a security or reputation warning. Proceed only when the installer came from the official ForensiQ release and its SHA-256 value has been verified.

## First launch

ForensiQ creates user-specific application data outside the installation directory.

The local database, logs, cache, backups, reports, and application settings are stored separately from the installed program files. Saved connection secrets are protected through Windows Credential Manager.

Open **Websites** to configure one of the four supported automatic methods:

- Local Apache, XAMPP, or Nginx log
- Read-only SFTP/SSH
- FTPS/cPanel Raw Access
- Cron Micro-Collector

ForensiQ performs a real read-only connection test before a website connection can be saved.

Manual Import remains available as a fallback for supported Apache or Nginx log files.

## Local XAMPP example

For a standard XAMPP installation, the Apache access log is commonly located at:

```text
C:\xampp\apache\logs\access.log
```

Example website details:

```text
Website name: Local XAMPP Test Site
Website address: http://localhost
Web server: Apache / XAMPP
Access-log format: Auto-detect
```

Choose the local log-file connection method and select the access-log path.

## Upgrade

To upgrade ForensiQ:

1. Pause monitoring.
2. Fully quit ForensiQ from the Windows system tray.
3. Run the newer official installer.
4. Install it over the existing version.
5. Launch ForensiQ and confirm the new version number.

The installer replaces the application files but preserves the current Windows user's local database, reports, settings, and Windows Credential Manager records.

ForensiQ creates a verified pre-upgrade database backup before a database migration when one is required.

## Reinstallation

Uninstalling and reinstalling ForensiQ normally does not remove the existing user data.

After reinstallation, previous websites, requests, incidents, reports, and settings may reappear. This is expected because the user data is stored separately from the application installation folder.

## Uninstall

Use:

```text
Settings > Apps > Installed apps > ForensiQ > Uninstall
```

The uninstaller removes the program files and shortcuts.

It deliberately does not remove user evidence, reports, backups, settings, or stored credentials, which prevents accidental data loss during an upgrade, repair, or reinstall.

## Remove all local data

Only do this when you deliberately want a complete reset.

1. Export any reports or backups you need.
2. Pause monitoring.
3. Fully quit ForensiQ from the system tray.
4. Uninstall ForensiQ.
5. Remove the remaining ForensiQ user-data folder from the current Windows user's Local AppData area.
6. Remove any related ForensiQ entries from Windows Credential Manager.

Deleting the user-data folder permanently removes the local database and saved application history.

## Maintenance commands

The installed application supports these local maintenance commands:

```powershell
ForensiQ.exe --check
ForensiQ.exe --backup
ForensiQ.exe --diagnostics
```

- `--check` performs local database integrity checks.
- `--backup` creates a verified local database backup.
- `--diagnostics` creates a privacy-safe diagnostic package.

Inspect diagnostic files before sharing them.

## Troubleshooting

### ForensiQ remains open after closing the window

ForensiQ can continue running in the Windows system tray.

To close it completely:

1. Open the hidden-icons area beside the Windows clock.
2. Right-click the ForensiQ tray icon.
3. Select **Quit ForensiQ**.

### Previous data appears after reinstalling

This is normal. The application data is stored separately from the program files.

### The website connection cannot be saved

Check that:

- The selected log file or remote source exists.
- The account has read-only access to the source.
- The server type and log format are correct.
- Required credentials are available.
- The connection test completes successfully.

### Windows warns about the installer

Verify that the installer came from the official ForensiQ release and confirm its SHA-256 value before continuing.

## Licence

ForensiQ is proprietary freeware.

Users may download, install, and use the unmodified official release free of charge for lawful and authorised purposes. Modification, redistribution, resale, reverse engineering, branding removal, and claiming authorship are prohibited except where applicable law provides otherwise.

Read `LICENSE.txt` before installation.
