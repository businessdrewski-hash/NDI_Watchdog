# Sync Guardian v0.3.2 controlled-test release

This release changes how persistent drift is judged while Adaptive Soft Sync is active.

The regular persistent drift threshold now follows the **estimated corrected output drift**, not the uncorrected transport drift. For example, raw `-50 ms` corrected to `-2 ms` will not trip a `50 ms` persistent drift threshold. Disabling Adaptive Soft Sync immediately returns threshold behavior to raw transport drift.

The dock has also been simplified:

- Drift is the prominent color-coded diagnostic.
- Adaptive correction appears on the same line.
- Trend and baseline are shown in a compact secondary line.
- Technical details can be hidden.
- Every major section can be collapsed.

Source-property preservation from v0.3.1 remains in place for targeted resets, full-group rebuilds, and saved-state restoration.

The repository includes a GitHub Actions workflow that produces:

- `SyncGuardian-Setup.exe`
- `SyncGuardian-Portable.zip`
- `SHA256SUMS.txt`

A Windows binary is not precompiled in this source package.
