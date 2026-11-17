# MG-WIN Network Studio

Native Windows 10/11 x64 desktop application with a Persian right-to-left interface using the **Tanha** font.

## Current deliverable

The Windows build workflow reconstructs a SHA-256-verified source snapshot, downloads the pinned Tanha TTF, runs the core smoke tests, builds the complete .NET 10 WPF solution, publishes a self-contained Windows x64 application, and compiles **MG-WIN-Setup.exe** with Inno Setup.

**No Portable package is produced.** The installer is uploaded as the `MG-WIN-Setup` artifact of the GitHub Actions run and is retained for 14 days.

## Network monitor capabilities

- Live ICMP latency samples, rolling latency chart, jitter estimate, and packet-loss estimate.
- Network adapter inventory, IPv4 address, gateway, MAC address, and reported link speed.
- Per-adapter download/upload rates and byte counters.
- Basic connectivity diagnostics, manual ping, pause/resume, and CSV log export.
- Persian RTL layout with embedded Tanha font.

## Build and retrieve the installer

1. Open the **Actions** tab.
2. Select **Build Windows Setup**.
3. Open the latest successful run for the `main` branch.
4. Download the `MG-WIN-Setup` artifact.

The run summary records the installer SHA-256 hash. A failed workflow is never treated as a release.

## Source snapshot

The reproducible source snapshot is stored under `source/archive/` in eight binary chunks. CI reassembles the chunks, verifies the complete archive SHA-256, and extracts it before any build step. The exact snapshot hash is `759b3eb822b9913ae9caf353e9875ff715ca5601f1d8fab4af2f293d43d6b575`.

## Tanha font

The WPF window uses Tanha for its Persian interface. The font is fetched at build time from the upstream repository at pinned commit `bd6f121ec33899e5b180c9fbdfff8e0b7bff8b55`; its license notice is included in the application source snapshot. CI checks the TTF signature and expected file size before compiling.

## Scope and safety

MG-WIN is a read-only network-health monitor. It does not create VPN/proxy tunnels, import proxy server profiles, alter Windows routes or DNS, change system proxy settings, install network drivers, or modify Windows Firewall.

## Verification status

Only a successful Windows Actions run producing a non-empty `MG-WIN-Setup.exe` artifact establishes that the current commit compiles and packages successfully. Runtime verification on an actual Windows 10/11 desktop is still recommended before general distribution. The installer is not code-signed.
