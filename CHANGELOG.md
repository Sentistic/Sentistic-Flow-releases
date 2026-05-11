# Changelog

All notable releases of Sentistic Flow are tracked here. Each entry
mirrors the GitHub Release for the corresponding tag.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

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
