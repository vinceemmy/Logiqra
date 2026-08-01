# Security Policy

## Supported versions

Security fixes are provided for the latest public ForensiQ release.

| Version | Supported |
|---|---|
| 1.1.3 | Yes |
| 1.1.0 - 1.1.2 | No |
| 1.0.0 | No |

Users should upgrade to the latest release before reporting a problem that may already have been fixed.

## Reporting a vulnerability

Do not disclose suspected vulnerabilities through a public issue, discussion, screenshot, or pull request.

Report the issue privately to the project maintainer. Include only the minimum information required to reproduce and understand the problem:

- The affected ForensiQ version
- The Windows version
- A clear description of the issue
- Minimal reproduction steps
- The expected and actual behaviour
- Relevant error messages
- A small synthetic log sample, when necessary
- A privacy-safe diagnostic ZIP, when useful

ForensiQ can generate a diagnostic ZIP through **Settings** or:

```powershell
ForensiQ.exe --diagnostics
```

Inspect every diagnostic file before sharing it.

## Information that must not be shared publicly

Do not include any of the following in a public report or repository:

- Real access logs
- SQLite databases or database backups
- Generated incident reports containing real activity
- Website credentials
- SFTP, FTPS, or hosting passwords
- SSH private keys or passphrases
- Collector secrets
- The collector `config.php` file
- Windows Credential Manager data
- Session identifiers, tokens, cookies, or authorization headers
- Real server addresses, hosting paths, usernames, or private infrastructure details

Use placeholders and synthetic examples wherever possible.

## Responsible testing

Only test ForensiQ with systems, websites, logs, and infrastructure that you own or are explicitly authorised to assess.

Do not use this project to access, disrupt, probe, or analyse third-party systems without permission.

## Product security scope

ForensiQ is a local Windows security-analysis application. It detects and explains suspicious patterns in supported web-server access logs.

ForensiQ Version 1 does not automatically block attacks, patch vulnerable applications, or replace secure coding and operational controls. Continue to use:

- Secure coding and input validation
- Parameterised database queries
- Timely patching
- Least-privilege database accounts
- Backups
- Server and network monitoring
- Appropriate firewall and access-control measures

## Disclosure handling

Please allow the maintainer reasonable time to investigate and prepare a fix before publishing technical details.

Security reports should remain private until the issue has been resolved or coordinated disclosure has been agreed.
