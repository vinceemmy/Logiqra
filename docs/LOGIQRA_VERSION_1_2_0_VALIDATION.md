# Logiqra 1.2.0: Windows Validation Report

## Scope

This report summarises validation of the **Logiqra 1.2.0 Batch 3** Windows build. It records the verification that was carried out, not a claim of complete security coverage.

## Windows environment and automated checks

| Check | Recorded result |
|---|---|
| Build operating system | Windows 11 |
| Python build interpreter | 3.12.10 |
| Foundation/source check | Passed |
| Ruff lint | Passed |
| mypy type checking | Passed; **116** source files |
| pytest automated suite | **1,059 passed** |
| Total measured automated test coverage | **86%** |
| PyInstaller packaging | Completed |
| Windows installer build and validation | Passed |

The release-validation process was rerun after correcting one Ruff finding and one mypy unpacking finding in the new single-instance restoration helper. A later Windows rebuild from the re-extracted source again passed the complete test suite and installer build.

## Post-install manual checks

| Check | Result |
|---|---|
| Start-menu reopening restores existing system-tray instance without duplicate-instance warning | PASS |
| PDF report filename uses `logiqra-` | PASS |
| CSV report filename uses `logiqra-` | PASS |
| JSON report filename uses `logiqra-` | PASS |
| Warning-dialog text and controls are readable | PASS |
| Ice-blue sidebar Live Activity glow is sufficiently visible | **NEEDS POLISH: Too faint** |

**Manual result: five checks passed, one visual improvement pending.** The glow issue does not imply that monitoring or the existing breathing animation failed.

## Release status

The Windows installer was built and installed for local testing. It was subsequently [published on GitHub Releases](https://github.com/vinceemmy/Logiqra/releases/tag/v1.2.0) on 11 October 2026. The published installer SHA-256 matched the locally verified release file.

The release includes the Windows installer, an approved licence file, and a SHA-256 manifest. Each published asset digest matched its prepared counterpart. This confirms file integrity, not malware safety or the behaviour of the installed application.

## Limitations

- Passing automated tests does not establish that every application workflow works on all computers.
- Test coverage is a measure of exercised code paths, not threat-detection accuracy.
- Detection depends on log format, recorded fields, configuration, and attack characteristics.
- Real-world connections, false positives, and UI appearance require ongoing validation.
- No claim is made that all requests, attacks, or malicious behaviours will be detected.

Historical v1.1.3 evidence remains accessible in the repository's earlier Git commits.
