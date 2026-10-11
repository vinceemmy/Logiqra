# Logiqra 1.2.0: Installation and Upgrade Guide

## Availability

**Logiqra 1.2.0 is [publicly available on GitHub Releases](https://github.com/vinceemmy/Logiqra/releases/tag/v1.2.0).** The older v1.1.3 download is historical and does not install the current Logiqra product.

Obtain [the official installer](https://github.com/vinceemmy/Logiqra/releases/download/v1.2.0/Logiqra-1.2.0-Setup.exe), [its SHA-256 manifest](https://github.com/vinceemmy/Logiqra/releases/download/v1.2.0/SHA256SUMS.json), and [the licence](https://github.com/vinceemmy/Logiqra/releases/download/v1.2.0/LICENSE.txt) from the same release. Avoid unofficial mirrors.

## System requirements

- 64-bit Windows 10 or Windows 11
- Permission to install a desktop application
- Access to at least one authorised web-server log source
- An internet connection for remote or cloud log sources; local monitoring may work offline

The recorded release validation was completed on Windows 11 with Python 3.12.10 in the build environment.

## Verify the release

1. Download the official `Logiqra-1.2.0-Setup.exe` and its corresponding published checksum.
2. Open PowerShell in the download folder.
3. Run:

~~~powershell
Get-FileHash .\Logiqra-1.2.0-Setup.exe -Algorithm SHA256
~~~

4. Compare the SHA-256 value with the checksum for the **exact same released installer**. Do not assume that independently rebuilt installers share the same hash.
5. Read the Logiqra licence provided with the release before installing.

A Windows SmartScreen reputation warning can appear for an unsigned or newly distributed installer. Verify the source and checksum before deciding whether to proceed.

## Install or upgrade

1. If already running, fully quit Logiqra from its Windows system-tray menu.
2. Run the verified installer.
3. Follow the setup wizard.
4. Launch Logiqra from the Start menu.
5. Check that your websites, settings, incidents, and existing data are still available.

Application data is stored separately from the program installation. **Do not delete your database, app-data folders, or saved credentials as part of a normal upgrade.** Back up sensitive data before changing versions.

## First monitoring source

Open the website/source connections section and choose an appropriate method:

- Local Apache, XAMPP, or Nginx log file
- SFTP/SSH using authorised read-only access
- FTPS/cPanel
- HTTPS Cron Micro-Collector for supported hosting
- Compatible cloud log source (for example, Google Cloud Logging where access is configured)
- Manual import when live collection is unsuitable

**New to connecting servers?** Follow the [step-by-step Website and Server Connection Guide](LOGIQRA_CONNECTION_SETUP_GUIDE.md) for all eight automatic methods, Google Cloud/Firebase setup, cPanel, SSH, Cron Collector, cloud providers, manual import and troubleshooting.

Logiqra analyses only the fields present in the source logs. Connection availability and accepted log formats vary.

## Closing and reopening

Closing the window may leave monitoring running in the system tray. Reopen Logiqra from the Start menu to restore the existing app window; to stop it completely, use the tray's Quit option.

## Uninstall or troubleshoot

A normal uninstall should not be treated as a complete data wipe. Avoid manual deletion of user-data folders unless you explicitly intend to erase the database, reports, and history after making backups.

For problems, record the app version, the affected connection type, and a synthetic or safely redacted example. Never share private credentials or customer log data in a public issue.
