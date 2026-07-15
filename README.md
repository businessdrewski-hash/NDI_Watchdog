# Sync Guardian v0.3.0

Sync Guardian is an experimental OBS Studio dock for monitoring DistroAV/NDI receiver sources, detecting stalls and A/V timing drift, and recovering selected receivers without depending on OBS dock repaint activity.

Install it on the **receiving/streaming PC** where the DistroAV sources exist.

## v0.3.0 highlights

- Background watchdog runs independently of the Qt dock, so resets do not wait for mouse hover or OBS focus.
- Every reset captures and restores the complete DistroAV source settings. FrameSync, timing mode, latency, and NDI source selection are verified after restoration.
- Drift and rate are displayed once per second while measurements continue every 250 ms.
- Offset uses a 10-second median and rate uses robust 60/120-second trend windows.
- Plain-English direction text explains whether video is gradually dragging or rushing relative to desktop audio.
- Dark-theme help text has higher contrast.
- Optional **Adaptive Soft Sync** can apply a tiny, slowly changing audio-rate correction instead of waiting for a large reset.
- The Windows workflow builds both a portable package and an installer EXE.

## Adaptive Soft Sync

Adaptive Soft Sync is **experimental and disabled by default**.

When enabled, Sync Guardian attaches a private audio filter to the mapped desktop-audio source and applies a heavily limited correction measured in parts per million. A typical -1.5 ms/min drift needs about +25 ppm. The default maximum is 50 ppm and the correction changes by only 1 ppm per second.

The feature can be completely removed from the audio path at any time:

1. Clear **Enable Adaptive Soft Sync**, or
2. Press **Disable and remove Soft Sync filters**.

With it disabled, no Sync Guardian resampling filter remains attached and NDI audio returns to normal pass-through. Leave **Apply the same correction to the mapped mic** off unless the mic should intentionally follow the exact desktop-audio rate.

The ordinary drift threshold continues to watch the **raw transport drift** even while Soft Sync is holding the audible output closer. When a receiver reset occurs, accumulated Soft Sync trim is recentered so correction does not grow without a reset fallback.

## Install

### Installer

Run `SyncGuardian-Setup.exe` while OBS is closed.

### Portable install

Extract `SyncGuardian-Portable.zip`, then copy its `obs-plugins` and `data` folders into:

```text
C:\Program Files\obs-studio\
```

The DLL should end up at:

```text
C:\Program Files\obs-studio\obs-plugins\64bit\sync-guardian.dll
```

Start OBS and open **Docks > Sync Guardian**.

## Initial test

1. Map NDI Video, Desktop Audio, and Mic.
2. Keep automation on **Observe only**.
3. Keep Adaptive Soft Sync **off** for the initial baseline test.
4. Confirm audio FrameSync remains in the exact state selected in each DistroAV source after manual and automatic resets.
5. For a Soft Sync test, enable it for desktop audio only, leave mic linking off, and make a local recording for at least an hour.

## GitHub build

Run **Actions > Build Sync Guardian for Windows**. The artifact `sync-guardian-0.3.0-windows-x64` contains:

- `SyncGuardian-Setup.exe`
- `SyncGuardian-Portable.zip`
- `SHA256SUMS.txt`

## Caution

This is a controlled-test build. Adaptive Soft Sync changes audio sample count by very small amounts and needs real OBS/DistroAV validation before use on an important live stream. The reset watchdog and pass-through monitoring can be used with Soft Sync fully disabled.
