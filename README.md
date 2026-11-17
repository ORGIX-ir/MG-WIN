# Moghimi V2Ray — Windows

**Native Windows client workspace.** This repository is independent from the Android repository and uses its own Windows versioning, build pipeline, and release track.

> **Development status: foundation / pre-alpha.** This repository does not yet contain a production-ready VPN tunnel, a configured Windows backend, or a verified cross-platform subscription link. The UI must never claim that a tunnel is connected until the engine, routes, DNS, and end-to-end traffic checks have succeeded.

## Reference implementation

The Android behavior reference is the immutable source commit:

- Repository: https://github.com/MOGHIMI-IR/MOGHIMI-VPN
- Tag: `moghimi-beta-87`
- Commit: `791805b20c09fa3e60b516296dbe0b5e9f5e7ecb`
- Android app: 2.0.8 Beta 87
- Android versionCode: 4000087
- Pinned upstream: `2dust/v2rayNG` commit `ef06bac9942a8550217ebe057daefd28964d436d`

See [the source audit](docs/ANDROID_REFERENCE_AUDIT.md) and [the Windows parity matrix](docs/WINDOWS_PARITY_MATRIX.md).

## Technology baseline

- .NET 10 LTS, WPF, C#, MVVM, Windows x64
- Modular projects: App, Core, Backend, Network, VpnEngine, Licensing, Security, Updates, Tests
- Candidate tunnel engine: sing-box 1.14.4, pending validation against sanitized production-format configuration samples
- Secret storage: Windows DPAPI / Credential Manager; never put service-role or secret keys in the desktop client
- CI: Windows GitHub Actions build and test; no installer release until engine and backend gates pass

## Important parity findings

- Android's Moghimi cloud group ID is `moghimi-cloud`; it is separate from personal profiles.
- Android's cloud sync reads active `v2ray_entries` and signs private `v2ray-files` objects for temporary download. Windows must not query the production Android database directly; a documented, authorized Windows API is required.
- Android's entitlement manager uses a 20-second status cache, a 60-second release cache, and a 60-second stale-status grace only for a temporary API failure while network connectivity is available.
- Android's API device identity is derived from Android ID + Android package name. That identity cannot be fabricated on Windows. A real cross-platform account/device-linking contract is required.
- Android Smart Connect tests only the Moghimi cloud group, chooses the lowest positive real outbound-delay result, and has a 90-second overall timeout. The pinned upstream's configurable real-ping concurrency defaults to 16; the Windows project requirement asks for a maximum of 4. This discrepancy is explicitly tracked rather than silently claimed to be identical.
- Moghimi-owned cloud profiles must not expose copy, share, or export actions. Personal profiles remain user-manageable under product policy.

## Build

On Windows with the .NET 10 SDK:

```powershell
dotnet restore Moghimi.Windows.sln
dotnet build Moghimi.Windows.sln -c Release
dotnet test Moghimi.Windows.sln -c Release
```

CI runs on `windows-latest`. The local execution environment used to initialize this repository does not include the .NET SDK, so successful compilation must be confirmed by the repository's Windows Actions run.

## What is deliberately not enabled yet

1. Real VPN tunnel start/stop and route/DNS changes.
2. Production subscription calls and cross-platform device linking.
3. Moghimi cloud configuration delivery for Windows.
4. Update download / installer activation.
5. Smart Connect probing against production servers.
6. Installer signing and release publication.

These are blocked on backend authorization/API contracts and sanitized representative configuration samples, not replaced with mock data.

## Security and release rules

- No production Android database migrations or Edge Function edits are performed from this repository.
- No service-role key, backend secret, user token, or raw server profile belongs in source control.
- Cloud profiles are treated as secrets and are excluded from copy/export/share UX.
- No security product, Defender, driver-signature enforcement, or SmartScreen setting is disabled.
- Every distributed third-party binary must be pinned, hash-verified, license-noticed, and signature-checked where applicable.
- An installer is not a release candidate until actual Windows tunnel, DNS, route cleanup, network-recovery, quota, and updater tests pass.

## License

The application source license and third-party distribution notices must be finalized before shipping. The Android upstream identifies GPL-3.0-covered v2rayNG code; any included engine has its own license and source-distribution obligations. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
