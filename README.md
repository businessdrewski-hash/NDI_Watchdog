# Sync Guardian v0.3.2

Sync Guardian is an experimental OBS Studio dock for monitoring DistroAV/NDI receiver sources, detecting stalls and A/V timing drift, and recovering selected receivers without depending on OBS dock repaint activity.

Install it on the **receiving/streaming PC** where the DistroAV sources exist.

## v0.3.2 highlights

- Persistent drift detection now follows **estimated corrected output drift** while Adaptive Soft Sync is active. With Soft Sync off, it follows raw transport drift as before.
- Live diagnostics now emphasizes one color-coded drift line.
- When Adaptive Soft Sync is active, the main line shows both raw drift and the estimated corrected result.
- Rate, direction, and baseline are moved to a shorter secondary line.
- Technical diagnostics are hidden behind a dedicated show/hide control.
- Every major dock section can be collapsed, including source mapping, automation, Adaptive Soft Sync, manual recovery, live diagnostics, and event history.
- Full-group rebuild and targeted reset restoration preserve captured DistroAV and OBS source settings.

## Adaptive Soft Sync

Adaptive Soft Sync is **experimental and disabled by default**.

When enabled, Sync Guardian attaches a private audio filter to the mapped desktop-audio source and applies a heavily limited correction measured in parts per million. A typical -1.5 ms/min drift needs about +25 ppm. The default maximum is 50 ppm and the correction changes by only 1 ppm per second.

The feature can be completely removed from the audio path at any time:

1. Clear **Enable Adaptive Soft Sync**, or
2. Press **Disable and remove Soft Sync filters**.

With it disabled, no Sync Guardian resampling filter remains attached and NDI audio returns to normal pass-through.

### Which drift value triggers recovery?

- **Soft Sync off:** persistent drift detection uses raw transport drift.
- **Soft Sync active:** persistent drift detection uses estimated corrected output drift.

Example: raw drift `-50 ms`, Adaptive Sync correction `+48 ms`, estimated corrected drift `-2 ms`. A `50 ms` persistent drift threshold does not trigger from that corrected result.

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

## GitHub build

Run **Actions > Build Sync Guardian for Windows**. The artifact `sync-guardian-0.3.2-windows-x64` contains:

- `SyncGuardian-Setup.exe`
- `SyncGuardian-Portable.zip`
- `SHA256SUMS.txt`

## Caution

Adaptive Soft Sync changes audio sample count by very small amounts and still needs real OBS/DistroAV validation before use on an important live stream. The monitoring and reset functions remain usable with Adaptive Soft Sync fully disabled.
