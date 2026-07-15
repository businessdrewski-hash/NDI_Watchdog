# Changelog

## v0.3.0

- Moved stall detection, reset scheduling, pulse restoration, verification, and automatic recovery to a background watchdog independent of Qt hover/focus events.
- Changed the visible drift and rate refresh cadence to 1000 ms while retaining 250 ms internal sampling.
- Expanded the offset median to 10 seconds and added robust 60-second display and 120-second controller rate estimates.
- Added plain-English drift direction text.
- Increased dark-theme contrast for secondary text and tooltips.
- Reset pulses now capture, restore, and verify the complete original DistroAV configuration, including FrameSync off on audio sources.
- Known-good snapshot restoration now uses the watchdog rather than `QTimer::singleShot`.
- Added experimental Adaptive Soft Sync, disabled by default and fully removable, with desktop-only or linked-mic modes.
- Added pre-filter timestamp tracking so Soft Sync correction is not counted twice in drift measurement.
- Kept reset fallback tied to raw transport drift and recentered accumulated Soft Sync trim after successful receiver resets.
- Added a Windows installer workflow producing `SyncGuardian-Setup.exe`, a portable ZIP, and checksums.
