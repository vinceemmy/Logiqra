# Changelog

All notable changes to ForensiQ are documented in this file.

## [1.1.3] - 2026-07-31

### Detection tuning

- Added detection for controlled `CAST(... AS SIGNED)` and `CAST(... AS UNSIGNED)` conversion probes.
- Added detection for numeric `GROUP BY` enumeration patterns such as `GROUP BY 1,2,3`.
- Added reconstruction and detection of `SELECT ... FROM` statements split across duplicate query-parameter values.
- Reduced false positives for narrow educational searches containing phrases such as `SHOW TABLES documentation` and `CASE WHEN examples`, while preserving the original rule match for audit purposes.
- Added the six previously missed malicious requests and four previous false-positive controls to the permanent regression suite.

### Validation

- Passed the final clean localhost regression with 40 of 40 controlled requests classified correctly.
- Detected 32 of 32 malicious requests.
- Classified 8 of 8 benign controls as Normal.
- Recorded zero misses and zero false positives in the final controlled regression.
- Updated package, executable, installer, verification, and release-manifest metadata to Version 1.1.3.

## [1.1.2] - 2026-07-31

### Live Activity contrast

- Added dedicated styling for the model-based Live Activity table so it uses the ForensiQ dark theme instead of the Windows default table appearance.
- Restored high-contrast row text, alternating dark rows, clear headers, visible grid lines, hover feedback, and selected-row highlighting.
- Preserved the responsiveness improvements and row limits introduced in Version 1.1.1.
- Updated package, executable, installer, verification, and release-manifest metadata to Version 1.1.2.

## [1.1.1] - 2026-07-31

### Performance and responsiveness

- Replaced the eight-column `QTableWidget` with a lightweight `QAbstractTableModel` and `QTableView`.
- Added Latest 100, Latest 250, and Latest 500 row limits for Live Activity.
- Removed expensive resize-to-content scans and assigned stable interactive column widths.
- Stopped Live Activity refresh work when the user navigates away, while preserving a pending refresh for the next visit.
- Coalesced monitoring-driven refreshes through a short timer instead of rebuilding the table after every coordinator snapshot.
- Kept the Needs Review tab independent so it refreshes only when selected.
- Stopped hidden Dashboard, Websites, Incidents, and Reports pages from performing heavy refresh work.
- Updated visible pages normally when opened.
- Changed the Live Activity query to return one highest-priority active finding per request, preventing duplicate request rows.
- Added LocalAppData discovery for Inno Setup in the Windows build process.

### Build corrections

- Corrected `QAbstractTableModel` method annotations so `rowCount`, `columnCount`, and `data` match the PySide6 type definitions used by strict Mypy validation.
- Updated package, executable, installer, verification, and release-manifest metadata to Version 1.1.1.

## [1.1.0] - 2026-07-30

### Detection and false-positive protection

- Expanded the transparent SQL-injection catalogue to 14 Version 1.1 rules.
- Added bounded block-comment-tolerant matching for keywords split inside words, including `un/**/ion`, `se/**/lect`, `o/**/r`, and `sl/**/eep`.
- Added coverage for function-based Boolean probes, error-based functions, administrative commands, query-structure probes, isolated terminal quotes, data statements, and subqueries.
- Added bounded analysis of repeated query parameters so split values can be detected while evidence still points to the original request characters.
- Added narrow, auditable suppression for successful read-only educational searches such as `union select tutorial`, while keeping stronger SQL syntax and additional attack evidence actionable.
- Added a 160-case controlled regression corpus containing 155 malicious category cases and five benign controls.

### Interface and usability

- Replaced the generic system-tray computer icon with the packaged ForensiQ icon.
- Added high-contrast styling for the tray menu, tooltips, fields, disabled states, selected text, table selections, and evidence or report text areas.
- Replaced solid-fill checkbox states with square controls containing a clear tick.
- Added a full-request viewer beneath Live Activity so long request targets can be inspected without table truncation.

### Release preparation

- Updated active package, executable, installer, verification, and release-manifest metadata to Version 1.1.0.
- Preserved the completed Version 1.0.0 source separately.

## [1.0.0] - 2026-07-29

### Initial release

- Added automatic monitoring for Local, SFTP/SSH, FTPS/cPanel, and Cron Micro-Collector sources.
- Added Apache and Nginx parsing, normalization, privacy filtering, SQL-injection rules, risk scoring, false-positive protection, evidence highlighting, and Attack Story Reconstruction.
- Added checkpoint recovery, rotation protection, multi-website background health, manual fallback import, Dashboard, Live Activity, Incidents, Notifications, Reports, and Settings.
- Added single-instance protection, database preflight checks, verified backups, privacy-safe diagnostics, branded assets, Windows build and installer definitions, release verification, and SHA-256 checksums.
- Completed final Windows validation for dashboard severity rendering, deterministic rule-match ordering, database backup flushing, path canonicalization, startup lifecycle behaviour, and Windows newline byte-size handling.
