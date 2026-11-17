# MG-WIN Network Studio

A native Windows 10/11 x64 network-health dashboard with a Persian RTL interface.

> **Scope:** this repository contains a read-only network monitor. It is not a VPN or proxy client and does not provide tunneling, server-profile import, routing/DNS changes, system proxy changes, firewall changes, or network-driver installation.

## Source package

The complete Visual Studio/WPF project, tests, build script, and Inno Setup definition are in **[MG-WIN-source.zip](source/MG-WIN-source.zip)**.

## Features

- Live ICMP latency sampling and a rolling latency chart.
- Jitter and packet-loss estimates.
- Network-interface inventory, local IPv4, default gateway, MAC address, and reported link speed.
- Per-adapter upload/download rates and RX/TX totals.
- Local gateway/public-target diagnostics, pause/resume, immediate test, and CSV log export.
- Self-contained Windows x64 publish and per-user MG-WIN-Setup.exe installer. No Portable package.

## Build locally

Extract the source archive on Windows, install .NET 10 SDK and Inno Setup 6 or later, open PowerShell in the extracted MG-WIN folder, then run:

```powershell
.\build.ps1
```

Output: installer\artifacts\MG-WIN-Setup.exe.

## CI installer

The **Build Windows Setup** GitHub Actions workflow expands the source package, runs dependency-free core smoke tests, publishes the self-contained WPF application, compiles the Inno Setup installer, and uploads only MG-WIN-Setup.exe as a workflow artifact. Run it from the Actions tab or wait for it after a push to main.

## Verification notice

The source archive and build pipeline are committed. The installer is not considered verified until a Windows Actions run succeeds and the produced installer is tested on a Windows 10/11 x64 system. The installer is not code-signed unless a valid signing certificate is deliberately added to a release process.
