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

All platforms share a single version number, and only changes relevant to SDK integrators and
their end users are listed here. The one exception is **13.0.x**: an iOS-only fix means iOS,
Flutter and React Native are at 13.0.1 while Android is at 13.0.0 — see the 13.0.1 entry.

> **Note on versioning:** Starting with **12.0.0**, every platform ships as a single self-contained
> artifact published to its public registry (see the [Scope](#scope) table above).

---

## [13.0.1] - 2026-09-09

### Fixed
- **iOS: 13.0.0 could not be compiled against.** Building an app against NetkiSDK 13.0.0 failed with
  `Unable to resolve module dependency: 'NetkiCV'`, because an internal module leaked into the
  published Swift interface. Use **13.0.1**; do not use 13.0.0. Android was unaffected and stays on
  **13.0.0** — there is no 13.0.1 Android artifact. (iOS)

## [13.0.0] - 2026-09-08

### Added
- **On-device document recognition.** The SDK now recognises whether a capture is actually the ID
  document that was asked for — a national ID front, a national ID back or a passport — rather than
  only detecting a rectangle. (Android + iOS)
- **On-device screen-capture detection.** The SDK now detects when a capture is a photograph of a
  screen rather than a physical document. (Android + iOS)

  Both run on the device, are enabled per country by the backend, and report their result with the
  captured picture. Neither shows anything to the end user in this release, and no integration
  change is required to adopt them.

### Changed
- **`IdCountry` carries four new properties.** They describe which on-device checks the backend has
  enabled for a country, and they are set for you.

  If your code builds an `IdCountry` from another `IdCountry` — for example to substitute a
  localized country name — copy the value and change the field you need instead of listing the
  properties one by one. Rebuilding it property by property silently drops the new ones and resets
  them to defaults:

  ```swift
  // Preserves everything, including properties added in future releases
  var localized = idCountry
  localized.name = localizedName
  ```

  ```kotlin
  val localized = idCountry.copy(name = localizedName)
  ```

### Upgrading from 12.x
- **iOS integrators must do a clean build.** `IdCountry`'s initializer gained parameters, which
  changes its compiled symbol even though existing call sites still compile. An incremental build
  can fail with `Undefined symbol: NetkiSDK.IdCountry.init(...)`. Delete your derived data and
  rebuild:

  ```
  rm -rf ~/Library/Developer/Xcode/DerivedData/<YourApp>-*
  ```

## [12.1.0] - 2026-08-05

### Changed
- **Improved error diagnostics.** Improved crash and error reporting so SDK-related issues are easier
  to diagnose, with less noise in reported events. (Android + iOS)

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
- **Flutter available on pub.dev.** The Flutter package (`netki_sdk`) is now published to pub.dev and
  installs like any standard package: `netki_sdk: ^12.0.0`.

### Added
- **Accessibility (WCAG 2.2 AA).** Capture, Liveness, and Validation screens now meet WCAG 2.2 AA —
  screen-reader semantics, color contrast, Dynamic Type / font scaling, focus order, and minimum
  tap-target sizes. (Android + iOS)

## [11.6.0] - 2026-06-12

### Fixed
- **iOS NFC passport reader integration.** Resolved a CocoaPods integration issue. NFC passport
  reading now integrates cleanly out of the box. (iOS)

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

- **SSN / TIN capture** support.
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
