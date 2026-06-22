# Sentistic Software Releases

Public release downloads for all Sentistic desktop applications.

---

## Products

| Product | Latest release | Updater manifest |
|---|---|---|
| **Sentistic Flow** | [Releases → `flow-v*`](../../releases?q=flow-v) | [`dist/flow/version.json`](dist/flow/version.json) |
| **Sentistic Planner** | [Releases → `planner-v*`](../../releases?q=planner-v) | [`dist/planner/version.json`](dist/planner/version.json) |

---

## Sentistic Flow

Real-time multi-sensor floorplan viewer for Sentistic OMNI radar/lidar devices.

### Download

Grab the latest installer from the **[Releases page → flow-v\* tags](../../releases?q=flow-v)**.

| Platform | Recommended download | Alternative |
|---|---|---|
| **Windows 10/11 (x64)** | `SentisticFlow-<version>-windows-setup.exe` (installer) | `SentisticFlow-<version>-windows.zip` (portable) |
| **macOS 12+ (Apple Silicon & Intel)** | `SentisticFlow-<version>-macos.dmg` | `SentisticFlow-<version>-macos.zip` |
| **Linux (x86_64)** | `SentisticFlow-<version>-linux.AppImage` | `SentisticFlow-<version>-linux.zip` |

### Auto-update manifest

The in-app updater fetches:
```
https://raw.githubusercontent.com/Sentistic/Sentistic-Flow-releases/main/dist/flow/version.json
```

---

## Sentistic Planner

DWG/DXF-driven sensor & luminaire installation planner.

Import a CAD floor plan, place Sentistic sensors, validate coverage, and export annotated DXF, CSVs, and a PDF installation guide.

### Download

Grab the latest installer from the **[Releases page → planner-v\* tags](../../releases?q=planner-v)**.

| Platform | Recommended download | Alternative |
|---|---|---|
| **Windows 10/11 (x64)** | `SentisticPlannerSetup-<version>.exe` (installer) | `SentisticPlanner-<version>-windows.zip` (portable) |
| **macOS 12+ (Apple Silicon)** | `SentisticPlanner-<version>.dmg` | `SentisticPlanner-<version>-macos.zip` |
| **Linux (x86_64)** | — | `SentisticPlanner-<version>-linux.zip` |

### Auto-update manifest

```
https://raw.githubusercontent.com/Sentistic/Sentistic-Flow-releases/main/dist/planner/version.json
```

---

## Verify a download

Every release includes a `SHA256SUMS-*.txt` file. To verify:

```bash
# macOS / Linux
sha256sum --check SHA256SUMS-*.txt

# Windows (PowerShell)
Get-FileHash SentisticFlowSetup-*.exe -Algorithm SHA256
```

---

## Where do my settings live?

**Sentistic Flow** stores per-user configuration outside the install directory so upgrades never overwrite your settings:

| Platform | Path |
|---|---|
| Windows | `%LOCALAPPDATA%\Sentistic\SentisticFlow\` |
| macOS | `~/Library/Application Support/Sentistic/SentisticFlow/` |
| Linux | `~/.local/share/Sentistic/SentisticFlow/` |

**Sentistic Planner** stores plan files and outputs in the `runs/` and `outputs/` folders next to the plan DXF, and user config in a similar per-user path.

---

## Issues & support

- Bug reports: use the [Issue Tracker](../../issues)
- Contact: info@sentistic.com


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

**Your settings are preserved across upgrades.** Updates only replace
the application binary. Everything stored in your per-user data folder
(`viewer.toml`, `shadow_states.json`, area-online configs, sensor
settings, logs) is untouched. See *Where do my settings live?* below.

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

## Known limitations of the v0.9.x beta

We're shipping early so you can start using it. The following items
are tracked and will be addressed in future releases:

| Limitation | Workaround | Tracked for |
|---|---|---|
| **macOS Bluetooth (BLE) provisioning may be silently denied** on macOS 12+ for unsigned apps | Grant Bluetooth permission to *Sentistic Flow* under System Settings → Privacy & Security → Bluetooth, or provision sensors from a Windows / Linux machine on the same LAN as a one-time setup | v1.0 (paid signing) |
| **Windows SmartScreen** flags the installer as "unrecognised app" | Click *More info* → *Run anyway* | v1.0 (EV cert) |
| **Linux** does not auto-open firewall ports | Run the bundled `setup_linux_firewall.sh` (auto-detects ufw / firewalld) | Documented |
| **Auto-updater** notifies and links to the download page; it does **not** auto-install | Click the link, install manually, restart the app | v1.x |
| **AWS IoT certificates** are not bundled (security) | Drop your `certs/` folder next to the app/installer; see *Where do certs go?* below | By design |

---

## Where do my settings live?

Per-user state — `viewer.toml`, `shadow_states.json`, area-online
configs, logs — lives **outside** the install folder, in the standard
per-user data directory for each OS:

| OS | Settings folder | Logs folder |
|---|---|---|
| Windows | `%LOCALAPPDATA%\Sentistic Flow\` | `%LOCALAPPDATA%\Sentistic Flow\Logs\` |
| macOS | `~/Library/Application Support/Sentistic Flow/` | `~/Library/Logs/Sentistic Flow/` |
| Linux | `~/.config/sentistic-flow/` and `~/.local/share/sentistic-flow/` | `~/.local/state/sentistic-flow/logs/` |

Open them quickly from inside the app:
**Help → Open Data Folder** / **Help → Open Logs Folder**.

This is why upgrades and reinstalls never wipe your configuration —
the installer (or new `.app` / `.AppImage`) only touches the binary.

---

## Where do certs go?

For **Area Online** (cloud) mode, place your `certs/` folder **next to
the executable**:

| OS | Path |
|---|---|
| Windows (installer) | `C:\Program Files\Sentistic Flow\certs\` |
| Windows (portable zip) | next to `SentisticFlow.exe` |
| macOS | next to `SentisticFlow.app` (NOT inside the app — macOS will discard it on update) |
| Linux | next to the `.AppImage` |

**Local mode** (UDP-only) works without any certs.

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
- The log file — find it via **Help → Open Logs Folder** in the app:
  - Windows: `%LOCALAPPDATA%\Sentistic Flow\Logs\sentistic-flow.log`
  - macOS: `~/Library/Logs/Sentistic Flow/sentistic-flow.log`
  - Linux: `~/.local/state/sentistic-flow/logs/sentistic-flow.log`

---

## License

The Sentistic Flow application is © Sentistic. Binaries published in
this repository are provided under the terms in [`LICENSE`](LICENSE).

---

*Sentistic Flow is built and maintained by Sentistic. Visit
<https://sentistic.com> to learn more about our sensing platform.*
