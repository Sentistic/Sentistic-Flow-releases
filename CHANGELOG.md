# Changelog

All notable releases of Sentistic Flow are tracked here. Each entry
mirrors the GitHub Release for the corresponding tag.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

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
