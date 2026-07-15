# Validation notes — v0.3.0

Performed as source-level and dependency-free validation on 2026-07-15.

## Completed

- Python behavior model passes for detection targets and default safeguards.
- Model verifies both original FrameSync states: off pulses on then restores off; on pulses off then restores on.
- Model verifies Soft Sync direction, 50 ppm clamping, slew limiting, accumulated trim accounting, and fully disabled behavior.
- Safety-critical reset timing no longer uses `QTimer::singleShot`.
- Reset restoration uses the complete saved settings object and verifies FrameSync, latency, sync mode, and NDI target.
- Soft Sync tracks pre-filter audio timestamps separately from corrected output timestamps.
- Drift reset fallback continues to use raw transport drift, and successful recovery targets recenter accumulated Soft Sync trim.
- UI display timer is 1000 ms; watchdog sampling remains 250 ms.
- GitHub Actions metadata and Inno Setup metadata are v0.3.0.

## Required Windows validation

This Linux environment cannot compile or load the Windows OBS plugin. Before production use:

1. Build with the included GitHub Actions workflow.
2. Confirm OBS starts without plugin-load errors.
3. With audio FrameSync off, run each reset button and verify it remains off afterward.
4. Leave OBS unfocused and the dock untouched while forcing a stale source; confirm recovery occurs without mouse movement.
5. Run at least one hour with Soft Sync disabled to establish the normal transport drift.
6. Run a separate local-recording test with desktop-only Soft Sync enabled, default 50 ppm maximum, and mic linking off.
7. Inspect sharp A/V events and listen for clicks, cuts, pitch changes, repeated samples, or buffer warnings.
8. Confirm disabling Soft Sync removes its private filters and returns audio to pass-through.

Treat Adaptive Soft Sync as experimental until these checks pass.
