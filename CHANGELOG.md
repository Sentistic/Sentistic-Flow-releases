# Changelog

All notable releases of Sentistic Flow are tracked here. Each entry
mirrors the GitHub Release for the corresponding tag.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

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
