# Sync Guardian v0.3.0 controlled-test release

This build addresses two observed reliability problems:

1. Automatic resets and pulse restoration no longer depend on Qt dock activity. The watchdog runs independently, so hovering or refocusing OBS should not be required.
2. Reset pulses now restore the complete captured DistroAV settings and verify FrameSync, latency, timing mode, and NDI target. Audio FrameSync should remain off when it was off before the reset.

It also adds optional Adaptive Soft Sync. This feature is experimental, off by default, and fully removable. Begin with desktop audio only, mic linking off, a 50 ppm maximum, and a local recording. The controller waits for a long trend and slews correction slowly.

The repository includes a GitHub Actions workflow that produces:

- `SyncGuardian-Setup.exe`
- `SyncGuardian-Portable.zip`
- `SHA256SUMS.txt`

A Windows binary is not precompiled in this source package.
