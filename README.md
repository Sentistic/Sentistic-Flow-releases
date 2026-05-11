# Sentistic Flow

Real-time multi-sensor floorplan viewer for Sentistic OMNI radar/lidar devices.

This repository hosts **public release downloads only**. The application
source code is maintained privately by Sentistic.

---

## Download

Grab the latest installer for your operating system from the
**[Releases page](../../releases/latest)**.

| Platform | Recommended download | Alternative |
|---|---|---|
| **Windows 10/11 (x64)** | `SentisticFlowSetup-<version>.exe` (installer) | `SentisticFlow-<version>-windows.zip` (portable) |
| **macOS 12+ (Apple Silicon & Intel)** | `SentisticFlow-<version>.dmg` | `SentisticFlow-<version>-macos.zip` |
| **Linux (x86_64)** | `SentisticFlow-<version>-linux.AppImage` | `SentisticFlow-<version>-linux.zip` |

Every release also publishes a [`version.json`](../../releases/latest/download/version.json)
manifest that the in-app updater consumes to notify you when a new build
is available.

---

## Install

### Windows

1. Download `SentisticFlowSetup-<version>.exe`.
2. Double-click to run the installer. Accept the UAC prompt.
3. The installer adds a Start Menu shortcut, registers an uninstaller,
   and creates the firewall rule needed for sensor UDP traffic
   (ports 9000–9002).
4. Launch **Sentistic Flow** from the Start Menu.

> Prefer no installer? Download the `-windows.zip`, extract anywhere,
> and run `SentisticFlow.exe` (or `RUN_ME.bat`). You may need to
> manually allow inbound UDP 9000–9002 in Windows Defender Firewall on
> the first run.

### macOS

1. Download `SentisticFlow-<version>.dmg`.
2. Open the DMG and drag **Sentistic Flow** into the **Applications**
   folder.
3. Right-click the app → **Open** the first time (Gatekeeper warning is
   expected — see *Why is macOS warning me?* below).
4. Grant **Bluetooth** and **Local Network** access when prompted —
   these are required for sensor provisioning and discovery.

> Prefer the portable zip? Download `-macos.zip`, unzip, and
> double-click `RUN_ME.command` (it strips the quarantine attribute and
> launches the app).

### Linux

1. Download `SentisticFlow-<version>-linux.AppImage`.
2. Make it executable and run it:

   ```bash
   chmod +x SentisticFlow-*-linux.AppImage
   ./SentisticFlow-*-linux.AppImage
   ```

The AppImage bundles its own Qt and Python runtime; no system packages
are required beyond `libfuse2` (already present on most distros).

---

## Auto-updates

Sentistic Flow checks this repository's `version.json` once on startup
(and on demand via **Help → Check for Updates…**). When a newer build is
available, the app shows a non-modal dialog with a download link and a
release-notes link. The app never installs updates automatically — you
stay in control.

You can disable the startup check by editing `viewer.toml`:

```toml
update_check_on_startup = false
```

Or point it at a different manifest entirely:

```toml
update_manifest_url = "https://example.com/your-manifest.json"
```

---

## Verify a download

The SHA-256 hash for every artefact is published in
[`version.json`](../../releases/latest/download/version.json) under
`platforms.<os>.sha256`. Verify locally with:

```bash
# macOS / Linux
shasum -a 256 SentisticFlow-*.dmg
shasum -a 256 SentisticFlow-*-linux.AppImage

# Windows (PowerShell)
Get-FileHash .\SentisticFlowSetup-*.exe -Algorithm SHA256
```

---

## Why is macOS warning me / Windows SmartScreen blocking the install?

These early builds are **not yet code-signed or notarised**. macOS
Gatekeeper and Windows SmartScreen flag any unsigned executable from
the internet. Workarounds:

- **macOS** — Right-click → **Open**, or run
  `xattr -dr com.apple.quarantine /Applications/SentisticFlow.app` once.
- **Windows** — Click *More info* → *Run anyway* on the SmartScreen
  prompt.

Code-signing certificates and Apple notarisation are on the roadmap.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| App won't launch on macOS, no error | `xattr -dr com.apple.quarantine /Applications/SentisticFlow.app` |
| AppImage exits immediately | Install `libfuse2` (`sudo apt install libfuse2`) |
| Sensors don't appear in *Network Available* | Allow inbound UDP 9000–9002 in your firewall; verify the PC and sensors share a subnet |
| BLE provisioning sees no devices | macOS only — grant Bluetooth permission to Sentistic Flow under System Settings → Privacy & Security → Bluetooth |
| Help → Check for Updates always says "up to date" | Confirm `update_manifest_url` in `viewer.toml` matches this repository's manifest URL |

---

## Reporting issues

Please file bug reports against this repository's
[Issues tab](../../issues). Include:

- App version (Help → About)
- Operating system and version
- A description of what you did, what you expected, and what happened
- The log file (`sentistic-flow.log`, alongside the executable)

---

## License

The Sentistic Flow application is © Sentistic. Binaries published in
this repository are provided under the terms in [`LICENSE`](LICENSE).

---

*Sentistic Flow is built and maintained by Sentistic. Visit
<https://sentistic.com> to learn more about our sensing platform.*
