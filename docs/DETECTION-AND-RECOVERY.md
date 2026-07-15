# Detection and recovery model

Sync Guardian samples timestamps every 250 ms in a background watchdog and refreshes the visible dock every 1000 ms.

## Drift values

- **Raw transport drift:** filtered DistroAV video/audio timestamp movement relative to the calibrated baseline.
- **Corrected output drift:** raw transport drift plus the accumulated Adaptive Soft Sync correction.

The main diagnostic line shows raw drift. When Adaptive Soft Sync is active, it also shows the estimated corrected output drift.

## Persistent drift threshold

- Adaptive Soft Sync disabled: threshold uses raw transport drift.
- Adaptive Soft Sync active and attached: threshold uses corrected output drift.

This prevents a drift already corrected at the audio output from unnecessarily triggering a receiver reset.

## Stall recovery

Stall detection remains based on packet/timestamp freshness and is not affected by Adaptive Soft Sync.

Reset pulses capture the source's DistroAV settings and OBS-level runtime state, temporarily invert FrameSync to recreate the receiver, then restore and verify the saved state. Full-group rebuild uses the same preservation path for every mapped source.
