# OnboardID SDK — Changelog

All notable, developer-facing changes to the OnboardID mobile SDKs are documented here.

## Scope

This changelog covers the OnboardID mobile SDKs and their published packages:

| Platform | Package | Registry |
|----------|---------|----------|
| Android | `com.netki:netkisdk` | [Maven Central](https://central.sonatype.com/artifact/com.netki/netkisdk) |
| iOS | `NetkiSDK` | [CocoaPods](https://cocoapods.org/pods/NetkiSDK) |
| Flutter | `netki_sdk` | [pub.dev](https://pub.dev/packages/netki_sdk) |
| React Native | `@netki/netki-mobilesdk` | [npm](https://www.npmjs.com/package/@netki/netki-mobilesdk) |

All platforms share a single version number. Only changes visible to SDK integrators or end
users are listed here — internal build, CI, and tooling changes are omitted.

> **Note on versioning:** OnboardID SDKs were distributed privately prior to the public-registry
> launch (see [9.0.0](#900)). Starting with **12.0.0**, every platform ships as a single
> self-contained artifact published to its public registry, and the public version tracks the
> unified ecosystem version.

---

## [12.1.0] - 2026-08-05

### Changed
- **Improved error diagnostics.** Reworked crash and error reporting (Sentry) with richer breadcrumbs
  and scope context and significantly reduced noise, making SDK-related issues easier to diagnose. The
  reported context now includes the SDK version. (Android + iOS)

## [12.0.1] - 2026-07-14

### Fixed
- **Review screen auto-scroll.** When document validations complete, the review screen now scrolls
  to the confirm/retry actions automatically so users no longer have to scroll to continue.
  (Android + iOS)

## [12.0.0] - 2026-07-13

### Changed
- **Single self-contained artifact.** NetkiSDK now bundles all of its native dependencies. Integrators
  no longer need to add private/companion repositories or a separate computer-vision dependency:
  - **Android** — remove the `art.myverify.io` Maven repository from `settings.gradle`; just declare
    `implementation 'com.netki:netkisdk:<version>'`.
  - **iOS** — remove the explicit `pod 'NetkiCV'` line from your `Podfile`; just `pod 'NetkiSDK'`.

  This matches the single-artifact integration model used by comparable identity SDKs.
- **Flutter available on pub.dev.** The Flutter package (`netki_sdk`) is now published to pub.dev and
  installs like any standard package: `netki_sdk: ^12.0.0`.

### Added
- **Accessibility (WCAG 2.2 AA).** Capture, Liveness, and Validation screens now meet WCAG 2.2 AA —
  screen-reader semantics, color contrast, Dynamic Type / font scaling, focus order, and minimum
  tap-target sizes. (Android + iOS)

## [11.6.0] - 2026-06-12

### Fixed
- **iOS NFC passport reader integration.** Resolved a CocoaPods integration failure
  (`NFCPassportReader` dispatch-thunk ABI mismatch) that could occur when consuming the SDK with
  library-evolution enabled. NFC passport reading integrates cleanly out of the box. (iOS)

## [11.5.0] - 2026-05-07

### Changed
- Maintenance release: stability and capture-quality improvements across Android and iOS.

## [11.3.0] - 2026-05

### Added
- **Country validation.** When starting the identification flow, the SDK now validates that the
  supplied country is a member of its supported-country list and surfaces an error for invalid input.

### Changed
- **Transaction metadata format parity.** Android and iOS now emit identical transaction metadata,
  and only populated fields are sent — empty and null nodes are omitted.
- **Richer transaction metadata** for improved integration diagnostics.

## [11.2.0] - 2026-04

### Added
- **`isInitialized()` API.** New method to query whether the SDK has finished initializing before
  starting a flow. (Android + iOS)

## [11.0.0] - 2026

### Changed
- **UX refresh.** Broad user-experience upgrades across the capture and validation flows.

## [9.0.0] - 2026-04-23

### Added
- **Public registry availability.** The OnboardID SDKs are now published to public package registries
  — Android on **Maven Central** (`com.netki:netkisdk`), iOS on **CocoaPods** (`NetkiSDK`), and
  React Native on **npm** (`@netki/netki-mobilesdk`) — so no private-registry credentials are required
  to integrate.

> The 9.0.x public packages were published alongside the 11.x development line and were unified into
> the single public 12.x line at [12.0.0](#1200).

## [8.0.0] - 2024-11

### Changed
- Android 14 support and refreshed platform library baselines across Android and iOS.

---

### 7.x — Platform modernization (2024)

- **CameraX / new camera pipeline** for improved capture on Android and iOS.
- **Migration to AndroidX (Android) and Swift (iOS).**
- **On-device passport MRZ** detection.
- **Local storage** and **offline upload** — images can be captured and uploaded in the background.

### 6.x

- **SSN / TIN capture** support (SDK and app).
- **Asynchronous transaction submission** endpoint.
- **Video-injection instructions** and **server-driven liveness algorithm selection**.

### 5.x

- **Liveness** detection (v1, then multi-capture v2) and programmatic UI customization.
- **On-device barcode & MRZ decoding**, plus **2D barcode** validation.
- **Real-time image-quality feedback** — edge detection, document-centered checks, improved cropping,
  light and glare measurement.
- **Capture UX guides**, localized feedback messaging, focus-on-touch, and configurable capture-retry
  behavior.
- **Geolocation capture**, **device make/model reporting**, per-client header tags, deeplink metadata,
  forced account authentication, and broad performance improvements.

---

*This changelog documents developer-facing SDK changes. For integration instructions, see the
[Android](./onboard_id_android.md), [iOS](./onboard_id_ios.md), and
[Flutter](./onboard_id_flutter.md) guides.*
