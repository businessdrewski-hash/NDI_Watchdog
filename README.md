# Sync Guardian for OBS Studio

Sync Guardian is an experimental OBS Studio dock plugin for monitoring and recovering DistroAV/NDI receiver sources.

It tracks incoming video and audio timing, detects sustained stalls or changes in their normal timestamp relationship, records diagnostics, and can rebuild an affected DistroAV receiver while restoring its original source settings.

> Install Sync Guardian on the **receiving/streaming PC** where the NDI sources are added to OBS.

## Features

- Monitors one NDI video source and up to two NDI audio sources
- Tracks packet age, timestamp movement, and estimated A/V offset
- Establishes a normal timing baseline automatically
- Detects persistent drift, video stalls, and audio stalls
- Supports manual, confirmation-based, or automatic recovery
- Applies cooldowns and reset limits to avoid repeated recovery loops
- Logs events and incident diagnostics
- Preserves the original DistroAV source settings after a reset

## Operating modes

| Mode | Behavior |
|---|---|
| **Observe only** | Monitors and logs without changing sources. |
| **Ask before resetting** | Requests confirmation before performing recovery. |
| **Fully automatic** | Performs a targeted reset after a problem remains active long enough. |

Start with **Observe only** until the source mappings and reported timing values have been verified.

## Installation

A compiled package should contain:

```text
obs-plugins\64bit\sync-guardian.dll
data\obs-plugins\sync-guardian\locale\en-US.ini
```

1. Close OBS completely.
2. Extract the downloaded plugin package.
3. Copy the included `obs-plugins` and `data` folders into:

```text
C:\Program Files\obs-studio\
```

4. Confirm the DLL is located at:

```text
C:\Program Files\obs-studio\obs-plugins\64bit\sync-guardian.dll
```

5. Start OBS and open **Docks → Sync Guardian**.

Do not place the plugin DLL in `bin\64bit`.

## Setup

1. Create the required DistroAV receiver sources in OBS.
2. Open **Docks → Sync Guardian**.
3. Click **Refresh Source List**.
4. Select the appropriate video and audio sources.
5. Leave the mode on **Observe only** while the baseline calibrates.
6. Test the manual reset controls before enabling automatic recovery.

## How recovery works

Sync Guardian briefly changes a DistroAV receiver setting that causes the receiver to rebuild, then restores the complete original source configuration.

Recovery can target:

- Video only
- Desktop audio only
- Microphone only
- Both audio sources
- The complete mapped source group

Automatic recovery is intentionally conservative. A brief timestamp irregularity alone is not enough to trigger a reset.

## Understanding the A/V estimate

The displayed A/V value is based on source timestamps and callback timing. It is most useful for detecting a change from the calibrated baseline.

It is not direct analysis of the visible picture or audible content, and it does not expose the exact internal OBS audio-buffer or DistroAV FrameSync queue depth.

## Logs

Configuration and diagnostic files are normally stored under:

```text
%APPDATA%\obs-studio\plugin_config\sync-guardian\
```

The main OBS log includes entries beginning with:

```text
[sync-guardian]
```

Open it through **Help → Log Files → View Current Log**.

## Troubleshooting

### The dock does not appear

- Confirm the DLL is in `obs-plugins\64bit`.
- Restart OBS completely.
- Check the DLL properties for an **Unblock** option.
- Search the OBS log for `sync-guardian` or `Failed to load module`.

### The source list is empty

- Confirm the plugin is installed on the receiving PC.
- Confirm the DistroAV receiver sources already exist in OBS.
- Click **Refresh Source List**.
- Confirm DistroAV loaded successfully in the OBS log.

### Automatic recovery does not run

A grace period, persistence timer, cooldown, inactive source, inactive output, or hourly reset limit may be preventing recovery. Review the dock status and event log.

## Building from source

The repository includes a GitHub Actions workflow for building the Windows x64 plugin.

1. Open the repository's **Actions** tab.
2. Run **Build Sync Guardian for Windows**.
3. Download the completed Windows artifact.
4. Install the resulting `obs-plugins` and `data` folders as described above.

## Uninstall

Close OBS and remove:

```text
C:\Program Files\obs-studio\obs-plugins\64bit\sync-guardian.dll
C:\Program Files\obs-studio\data\obs-plugins\sync-guardian\
```

Saved configuration and logs may also be removed from:

```text
%APPDATA%\obs-studio\plugin_config\sync-guardian\
```

## Disclaimer

Sync Guardian is an independent experimental project and is not affiliated with or endorsed by OBS Studio, DistroAV, or NDI. Test recovery behavior before relying on it in a live production environment.
