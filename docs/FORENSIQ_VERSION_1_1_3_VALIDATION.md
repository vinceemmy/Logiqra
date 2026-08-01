# ForensiQ Version 1.1.3 Validation Report

This document summarises the validation completed for **ForensiQ 1.1.3** before its portfolio release.

## 1. Validation scope

Validation covered:

- Source compilation
- Project structure and release metadata
- Automated tests
- Static analysis and type checking
- Windows application packaging
- Installer creation
- Release-content verification
- Interface and usability checks
- Performance and responsiveness
- SQL-injection detection
- False-positive controls
- Clean installation from the final corrected source archive

ForensiQ was tested only with authorised local test data and the user's own XAMPP/Apache environment.

## 2. Source-generation validation

The Version 1.1.3 source-generation environment completed these checks successfully:

- Python source compilation
- Foundation structure validation
- Release metadata validation
- 359 selected automated tests
- Existing Version 1.1 detection tests
- Request-normalisation tests
- SQL-injection integration tests
- Release-hardening tests
- Release-metadata tests
- The complete 40-request Version 1.1.2 regression corpus
- The six previously missed malicious patterns
- The four previous educational-search false positives

The six previously missed malicious cases became actionable, while the four educational-search controls were removed from the actionable queue without deleting their audit matches.

## 3. Windows build validation

The final corrected Version 1.1.3 source was built successfully on Windows.

The Windows build process completed:

- Ruff validation
- Mypy validation
- Complete automated test execution
- PyInstaller packaging
- Packaged-content verification
- Inno Setup installer compilation
- SHA-256 release-manifest generation

The completed installer was:

```text
ForensiQ-1.1.3-Setup.exe
```

The release manifest was:

```text
SHA256SUMS.json
```

## 4. Independent source-archive verification

The final corrected source ZIP was extracted into a new folder and built independently.

That clean build successfully produced a new Version 1.1.3 installer without requiring any additional patch.

The existing ForensiQ installation was then removed, and the newly generated installer was used for a fresh installation.

The reinstalled application opened and operated correctly, confirming that the final corrected source archive contained the complete Version 1.1.3 release state.

## 5. Interface validation

The following interface areas were checked successfully:

- Dashboard
- Live Activity
- Incidents and Attack Stories
- Detection Rules
- Reports
- Settings
- System-tray icon
- System-tray menu
- Version display

Version 1.1.3 displayed correctly in the application.

The system-tray icon used the proper ForensiQ branding.

Tray-menu text, application text, disabled controls, table rows, selected rows, and other interface elements used readable colour contrast.

Selection controls displayed clear tick-style checkboxes.

## 6. Performance and responsiveness validation

Earlier testing identified a responsiveness problem after opening Live Activity.

The performance hotfix introduced:

- A lightweight model-based Live Activity table
- Latest 100, 250, and 500 row limits
- Controlled refresh coalescing
- No heavy refreshes for hidden pages
- Independent Needs Review loading
- Reduced duplicate row rendering
- Cancellation of unnecessary Live Activity work after navigation

After the fix, the application switched between Live Activity, Incidents, Reports, Detection Rules, Dashboard, and other pages without showing **Not Responding**.

No further hanging was observed during the final checks.

## 7. Contrast regression validation

The initial model-based Live Activity table used an incorrect light Windows appearance.

Version 1.1.2 restored:

- Dark table background
- Readable text
- Alternating dark rows
- Clear headers
- Visible grid lines
- Hover feedback
- Selected-row highlighting
- Matching dark-theme styling

Version 1.1.3 preserved those fixes.

## 8. Targeted Version 1.1.3 retest

A targeted ten-case retest covered the exact weaknesses identified in Version 1.1.2.

### Previously missed malicious patterns

Six previously missed malicious patterns were retested:

- Signed conversion probe
- Unsigned conversion probe
- Numeric `GROUP BY` variation 1
- Numeric `GROUP BY` variation 2
- Duplicate-parameter `SELECT ... FROM` variation 1
- Duplicate-parameter `SELECT ... FROM` variation 2

Result:

```text
6 of 6 detected
0 remaining misses
```

### Previous false-positive controls

Four harmless educational searches were retested:

- Administrative-command documentation search
- Extended administrative-command documentation search
- Conditional-expression example search
- Extended conditional-expression example search

Result:

```text
4 of 4 classified as Normal
0 remaining false positives
```

### Targeted retest result

```text
10 of 10 passed
```

## 9. Final clean controlled regression

A new clean ForensiQ data profile was created before the final regression.

The final controlled set contained:

- 32 malicious requests
- 8 benign controls
- 40 total requests

### Results

| Category | Result |
|---|---:|
| Malicious requests detected | 32 of 32 |
| Malicious requests missed | 0 |
| Benign controls classified as Normal | 8 of 8 |
| False positives | 0 |
| Overall controlled result | 40 of 40 |
| Controlled detection rate | 100% |
| Controlled benign accuracy | 100% |
| Overall controlled accuracy | 100% |

The Live Activity total showed 32 suspicious requests, matching the 32 malicious requests in the controlled set.

## 10. Detection areas covered

The controlled validation included representative cases involving:

- Isolated quote probes
- Boolean conditions
- `UNION SELECT`
- Time-delay functions
- Comments inserted inside SQL keywords
- Error-based functions
- Signed and unsigned conversion probes
- Function-based Boolean expressions
- `ORDER BY`
- Numeric `GROUP BY`
- `HAVING`
- Administrative commands
- Duplicate query parameters
- Split SQL statements
- SQLite enumeration
- Query-plan reconnaissance
- Stacked statements
- Schema enumeration
- Educational false-positive controls

## 11. Release and data-preservation checks

The installer replaced earlier ForensiQ program files while preserving the separate local user-data folder.

Uninstalling and reinstalling the application did not automatically remove:

- Websites
- Requests
- Incidents
- Reports
- Settings
- Monitoring history

This behaviour was confirmed as expected because application data is stored separately from the installation folder.

A clean data profile could still be created manually when required for controlled testing.

## 12. Release artefacts

The final portfolio release uses:

```text
ForensiQ-1.1.3-Setup.exe
SHA256SUMS.json
LICENSE.txt
```

The complete source code remains private.

The public showcase contains documentation, screenshots, release information, validation results, and the downloadable installer.

## 13. Limitations of the validation

The 40-of-40 result applies only to the documented controlled test set.

It does not prove that ForensiQ will:

- Detect every real-world SQL-injection variation
- Produce no false positives on every possible website
- Identify attacks that never appear in the monitored access log
- Detect body-only payloads when the server log does not contain the request body
- Replace secure coding, parameterised queries, patching, access control, or a properly configured WAF

Real-world results depend on:

- Log format
- Logged request fields
- Encoding
- Server configuration
- Application behaviour
- Traffic context
- Rule configuration
- Privacy and retention settings

ForensiQ detections should be reviewed by a human before operational, legal, disciplinary, or business action is taken.

## 14. Final status

ForensiQ 1.1.3 passed the planned:

- Source validation
- Windows build validation
- Installer validation
- Independent source-archive rebuild
- Reinstallation check
- Interface checks
- System-tray checks
- Performance checks
- Targeted detection retest
- Final clean controlled regression

**Final portfolio-release status: validated and ready for public showcase distribution as proprietary freeware.**
