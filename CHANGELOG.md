# Changelog

All notable releases of Sentistic Flow are tracked here. Each entry
mirrors the GitHub Release for the corresponding tag.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.17.0] — 2026-08-26

### Added
- Analytics **Area Dim Profile** and **Dim Savings** KPIs: typical dim on occupied hours, average dim, and potential lighting energy savings versus always-on at presence dim.
- Observed **walkable circulation mask** for Occupied Area, Utilization, and Dead Zones (sensor FOV ∩ historically walked routes, not the whole floorplan).
- Lighting overlay defaults for a ~10 m occupancy group (10 m radius, 50% absence dim, 60 s hold-off).

### Fixed
- Large-area Analytics refresh no longer OOMs DuckDB on occupancy KPIs; heatmap, dwell, and routes stay usable on multi-day ranges.

## [0.16.0] — 2026-08-25

### Added
- Area Online lighting simulation: occupancy-driven dimming overlay (absence/presence dim, fade times) and an **Area dim** HUD showing usage versus always-on, measured over sensor FOVs only.
- Developer Promo Studio tab in Export for storyboard preview/render (hidden in customer builds).

### Fixed
- Corporate Windows PCs no longer freeze on launch or wake: Flow no longer shells out to PowerShell or `ipconfig` (AppLocker block dialogs). Dual-NIC addresses come from the IP Helper API; the firewall probe runs off the UI thread.

## [0.15.8] — 2026-08-21

### Added
- Heatmap Window: **All loaded** (every session sample) and **All loaded · thinned** (whole session, older samples sparsified so long soaks stay usable).
- Cognito invitation and verification email HTML (`docs/cognito-emails/`).
- Installer ships `logo.png` / `SentisticFlow.ico` for Start Menu and desktop shortcuts.

### Fixed
- Area Online on ~230 sensors no longer slows the whole laptop until Flow is quit (bounded replay/ingest queues, coalesced live MQTT, no GUI-thread drain busy-loop).
- Local Start/Stop only unicasts to the target sensor (no LAN-wide broadcast).
- Pulse Presence / PIR / linger plots no longer vanish when axes auto-range to milliseconds.
- Launch always opens Cloud Login; guest is a distinct session from cancelling a mid-session login.

## [0.15.7] — 2026-08-20

### Added
- Pulse / SLI local demo mode: Flow now opens UDP shadow with `localModeEnable` and `shadowUdpEnable` so play/stop and the 9000 stream work on Pulse firmware `30aeaa62+`.
- USB Flash Firmware: OMNI defaults (esp32 / dio / 80m / 8MB / Force). Encrypted UART reflash is Tools → Flash Encrypted, not on the basic Flash page.

### Fixed
- Local demo wake/play on dual-NIC Windows binds the command socket to the device subnet.
- USB “Erase entire flash” uses `write-flash --erase-all` in the same esptool session so `--force` applies (a separate `erase-flash` was refused on encrypted chips).
- Cloud S3 / Dynamo permission denials show a useful hint instead of empty-area silence.
- Analytics space heatmap SQL covers every sensor and hour; playback All-loaded heatmaps stay complete at MAX speed.

## [0.15.5] — 2026-07-30

### Added
- Video (MP4) export: heatmap style and colour-map controls (same set as Heatmap PNG).
- Frozen builds: `--rtv-child` dispatch so packaged apps can run render / JSON / push CLIs without loose `.py` scripts.

### Fixed
- Export Traffic / Dwell now use the same density classifier as live Area Online (Traffic no longer matches Occupancy on dense sites).
- Cloud-area video / PNG export restores floorplan when only `floorplan.png` (or a materialised copy) is present.
- STATUS hover cards: case-insensitive thing lookup; recover STATUS wildcard after MQTT interrupt (no more stuck “Waiting…”).
- Escape no longer quits the main window (still cancels canvas modes / closes popups).

## [0.15.4] — 2026-07-30


## [0.15.1] — 2026-07-29

### Added
- Month-scale cloud recording cache: faster local scan, cache tree + safe delete, 64-bit download progress/ETA.
- Unified TimeRangePicker shared by Open Recording and Analytics.
- `feature_visibility.json` for selective unlock of developer features on customer PCs.
- Central `safe_delete` guards for all user-data wipes.

### Changed
- Export / `push_detections`: hour-streamed ingest→stage→upload, per-hour S3 skip, promote to `mirror-detections` before clearing staging (no re-download for analytics).
- Analytics integrity/polish (time-of-day, underused sub-areas, chart UX, Inter fonts).

### Fixed
- Export overall progress bar for multi-chunk month jobs; area config default no longer picks the first sibling JSON.
- Staging leftovers no longer fill the disk after upload (`Errno 28`).


## [0.15.0] — 2026-07-27

### Added
- **Analytics dashboard** in Area Online — DuckDB-backed historical KPIs,
  charts, heatmaps, compare mode, and PNG export over cached detections.
- Multi-region Cognito login routing and known-environment discovery.
- Admin **Publish current area to cloud** plus local area-config save.
- Fleet health HUD and status-metrics charts on the Sensors tab.

### Fixed
- Cloud recording playback scrubber no longer pads empty leading hours
  from a wide probe window (e.g. Last 12 h with data only in the last 4 h).
- Large analytics refreshes stream multi-day ranges instead of loading
  every detection row into RAM (Walmart-scale crash path).
- Release `version.json` is attached as a GitHub Release asset so the
  in-app updater can resolve `…/releases/latest/download/version.json`.

## [0.14.2] — 2026-07-15

## [0.14.1] — 2026-07-15

## [0.14.0] — 2026-07-15

## [0.13.1] — 2026-07-13

## [0.13.0] — 2026-07-10

## [0.12.0] — 2026-07-10

## [0.11.0] — 2026-06-24

### Changed
- **User-data layout — single canonical root, dev + installed.** All
  per-user files now live under one root (`%LOCALAPPDATA%\Sentistic Flow\`
  on Windows, `~/Library/Application Support/Sentistic Flow/` on macOS,
  `$XDG_DATA_HOME/sentistic-flow/` on Linux; `<repo>/user_data/` when
  run from source). Same subfolder layout everywhere: `viewer.toml` +
  `aws_regions.toml` + `cognito_presets.toml` at the root, plus
  `certs/`, `areas/`, `cache/`, `recordings/`, `Logs/`.
- AWS credential metadata and HTTPS-presence metadata (non-secret;
  region + masked hint only — secrets stay in the OS keyring) now live
  in the data root instead of a split per-OS folder. Old files are
  migrated automatically on first launch; nothing is overwritten.
- Auto-written `README.txt` at the data root explains the layout in
  plain English so operators can find and edit files without help.
- Log folder is now consistent: dev mode lands in `<repo>/user_data/Logs/`
  (was `logs/`), frozen builds still use the OS-native log location.

### Added
- `Help → Show File Locations…` menu item — pops up every path the
  app reads or writes, with `[x]` / `[ ]` existence tags. Handy for
  support tickets.
- `--show-paths` CLI flag — prints the same dump and exits. Useful for
  CI / scripting.
- Single `resolve_viewer_config_path()` helper centralising
  `--config` / `RTV_CONFIG_FILE` / exe-dir / per-user precedence.

### Fixed
- Existing installs upgrade cleanly: flat-layout files
  (`area_online_config.json`, `shadow_states.json`, `plot_source.json`)
  are moved into the new subfolders on first launch without losing
  data. No reinstall or manual file shuffling needed.
- AWS credentials and HTTPS-presence vaults no longer get split across
  two different folders depending on whether the app was launched from
  source or installed.

## [0.10.0] — 2026-06-22

## [0.9.2] — 2026-05-15

### Added
- Settings dialog: Auto-range checkbox plus Min/Max heatmap range
  controls (persisted to `viewer.toml`).
- Mode-controls strip auto-resizes when switching between Local /
  Demo Online / Area Online so unused mode pages no longer reserve
  space.

### Changed
- AWS region + connection details moved to a gitignored
  `aws_regions.toml` overlay (no secrets in source tree).
- Windows installer renamed to `SentisticFlow-<v>-windows-setup.exe`;
  portable zip dropped from the release matrix.
- Windows `.ico` regenerated straight from `logo.png` (no dark
  rounded background).
- `user_data/` split into `areas/`, `cache/`, `logs/` subfolders.

## [0.9.0] — TBD

First public beta.

### Added
- Real-time multi-sensor floorplan viewer with Local and Area Online modes.
- BLE provisioning of Sentistic OMNI sensors directly from the app.
- Device Manager with claim / release of streaming sensors.
- Shadow inspector for live device-shadow diagnostics.
- In-app update checker (Help → Check for Updates).
- Cross-platform installers: Windows (Inno Setup), macOS (DMG), Linux (AppImage).
- Portable `.zip` distributions for all three platforms.

### Known limitations
- Binaries are not yet code-signed; expect Gatekeeper / SmartScreen prompts.
- macOS requires manual permission grants for Bluetooth and Local Network.
