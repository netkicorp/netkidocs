# Netki OnboardID SDK - Flutter

The OnboardID SDK enables you to integrate identity verification directly into your Flutter application. Users can capture ID documents and selfies without leaving your app.

---

### Quick Start

To get up and running, complete the [Installation](#installation) and [Core Integration](#core-integration) sections. Everything else is optional.

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Core Integration](#core-integration)
- [Optional Features](#optional-features)
  - [Phone Verification](#phone-verification)
  - [Biometrics Re-capture](#biometrics-re-capture)
  - [Configuration Options](#configuration-options)
- [API Reference](#api-reference)
- [Error Handling](#error-handling)
- [Theming](#theming)

## Requirements

- Flutter SDK 3.3.0 or higher
- Dart SDK 3.11.1 or higher
- iOS 17.0 or higher
- Android API level 24 (Android 7.0) or higher

### iOS Setup

Add the following keys to your `Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>Camera access is required to capture ID documents</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Photo library access is required to store captured images</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>Photo library access is required to store captured images</string>
```

### Android Setup

Add the following permissions to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Installation

### Step 1: Add the Dependency

In your `pubspec.yaml`, add the Netki SDK:

```yaml
dependencies:
  netki_sdk: ^${latest.version}
```

Then run:

```bash
flutter pub get
```

> **TODO:** Package repository URL to be defined once published to production.

### Step 2: Import the SDK

In your Dart files, import the SDK:

```dart
import 'package:netki_sdk/netki_sdk.dart';
```

## Core Integration

This section covers everything you need to integrate the SDK. Complete these 4 steps for a fully working implementation.

The SDK uses Dart async/await for asynchronous operations. For more information, see the [Dart async documentation](https://dart.dev/codelabs/async-await).

### Step 1: Initialize the SDK

Initialize the SDK once, typically in your app's startup or before starting the identification flow.

```dart
await NetkiSdk.instance.initialize();
```

### Step 2: Configure with Your API Token

Configure the SDK with the API token provided by Netki.

```dart
final result = await NetkiSdk.instance.configureWithToken(token: token);

if (result.isSuccessful) {
    // SDK is ready - proceed to capture
} else {
    // Handle configuration error
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | String | Yes | Your API token from Netki |
| `accessCode` | String | No | Optional access code for the transaction |

### Step 3: Start the Identification Flow

Get available options and start the identification capture.

```dart
// Get available options
final availableIdTypes = await NetkiSdk.instance.getAvailableIdTypes();
final availableCountries = await NetkiSdk.instance.getAvailableCountries();

// Start the capture flow
await NetkiSdk.instance.startIdentification(
    idType: selectedIdType,
    idCountry: selectedCountry,
);
```

**ID Types:**

| Type | Description |
|------|-------------|
| `IdType.driversLicense` | Driver's license |
| `IdType.passport` | Passport |
| `IdType.governmentId` | National ID card |

**Handling the Result:**

Subscribe to the identification events stream to receive capture results:

```dart
NetkiSdk.instance.identificationEvents.listen((event) {
    switch (event) {
        case IdentificationSuccess(:final extraData):
            // Capture successful - proceed to submit
            submitIdentification();
        case IdentificationCancelled(:final resultInfo):
            // User cancelled or error occurred
            handleCancellation(resultInfo);
        case IdentificationUnknown(:final event):
            // Unknown event
            print('Unknown event: $event');
    }
});
```

### Step 4: Submit the Identification

After a successful capture, submit the data to Netki for processing.

```dart
final result = await NetkiSdk.instance.submitIdentification();

if (result.isSuccessful) {
    // Success - transaction is being processed
    // Results will be sent to your backend callback
    final transactionData = result.extraData;
} else {
    // Handle submission error
}
```

Once submitted successfully, the identification results will be delivered to your configured [backend callback](./api/best_practices_internal_callbacks.md).

---

## Optional Features

The following features extend the core integration. Add them based on your requirements.

### Phone Verification

Add phone number verification to your identification flow. This sends an SMS code to the user's phone for verification.

**Prerequisites:** Complete Steps 1 and 2 from Core Integration first.

#### Request a Security Code

Send an SMS verification code to the user's phone:

```dart
final result = await NetkiSdk.instance.requestSecurityCode(
    phoneNumber: '+14155551234',
);

if (result.isSuccessful) {
    // Code sent - show UI for user to enter the code
} else {
    // Handle error
}
```

#### Confirm the Security Code

Verify the code entered by the user:

```dart
final result = await NetkiSdk.instance.confirmSecurityCode(
    phoneNumber: '+14155551234',
    securityCode: userEnteredCode,
);

if (result.isSuccessful) {
    // Phone verified - proceed with identification
} else {
    // Handle error
}
```

### Biometrics Re-capture

If you need to re-capture biometric data for an existing transaction (e.g., after a failed liveness check), use the biometrics flow.

**Prerequisites:** Complete Steps 1 and 2 from Core Integration first.

```dart
// Start biometrics capture
await NetkiSdk.instance.startBiometrics(transactionId: transactionId);
```

After capturing, submit the biometrics:

```dart
final result = await NetkiSdk.instance.submitBiometrics();

if (result.isSuccessful) {
    // Biometrics submitted successfully
} else {
    // Handle submission error
}
```

### Configuration Options

#### Business Metadata

Attach custom metadata to the transaction for your internal tracking:

```dart
await NetkiSdk.instance.setBusinessMetadata(metadata: {
    'internal_user_id': '12345',
    'signup_source': 'mobile_app',
});
```

#### Client GUID

Set a custom identifier to link the transaction to your system:

```dart
await NetkiSdk.instance.setClientGuid(clientGuid: 'your-unique-identifier');
```

#### Geolocation

Capture the user's location with the transaction:

```dart
await NetkiSdk.instance.setLocation(lat: '37.7749', lon: '-122.4194');
```

#### Liveness Detection

Enable or disable liveness detection for selfie capture:

```dart
await NetkiSdk.instance.setLivenessSettings(enabled: true);
```

---

## Error Handling

All SDK operations return a `ResultInfo` object:

```dart
class ResultInfo {
    final RequestStatus status;
    final Map<String, String>? extraData;
    final ErrorType? errorType;
    final String? message;

    bool get isSuccessful => status == RequestStatus.SUCCESS;
}
```

Check the result using:

```dart
if (result.isSuccessful) {
    // Handle success
} else {
    // Handle error based on result.errorType and result.message
}
```

**Error Types:**

| Error Type | Description |
|------------|-------------|
| `ErrorType.NO_INTERNET` | No network connection available |
| `ErrorType.INVALID_DATA` | Invalid data provided to the SDK |
| `ErrorType.INVALID_TOKEN` | API token is invalid or expired |
| `ErrorType.INVALID_ACCESS_CODE` | Access code is invalid |
| `ErrorType.INVALID_PHONE_NUMBER` | Phone number format is invalid |
| `ErrorType.INVALID_CONFIRMATION_CODE` | SMS confirmation code is incorrect |
| `ErrorType.USER_CANCEL_IDENTIFICATION` | User cancelled the identification flow |
| `ErrorType.TRANSACTION_NOT_FOUND` | Transaction ID does not exist |
| `ErrorType.CONFIGURATION_ERROR` | SDK configuration error |
| `ErrorType.UNEXPECTED_ERROR` | An unexpected error occurred |

---

## API Reference

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `initialize()` | Future\<void> | Initialize the SDK. Call once before any other methods. |
| `configureWithToken({token, accessCode})` | Future\<ResultInfo> | Configure SDK with API credentials. |
| `startIdentification({idType, idCountry})` | Future\<void> | Start the identification capture flow. |
| `submitIdentification({additionalData})` | Future\<ResultInfo> | Submit captured identification data. |
| `startBiometrics({transactionId})` | Future\<void> | Start biometrics re-capture for a transaction. |
| `submitBiometrics({additionalData})` | Future\<ResultInfo> | Submit re-captured biometric data. |
| `requestSecurityCode({phoneNumber})` | Future\<ResultInfo> | Send SMS verification code. |
| `confirmSecurityCode({phoneNumber, securityCode})` | Future\<ResultInfo> | Verify SMS code. |
| `bypassSecurityCode({phoneNumber})` | Future\<void> | Bypass security code (if allowed). |
| `setBusinessMetadata({metadata})` | Future\<void> | Attach custom metadata to the transaction. |
| `setClientGuid({clientGuid})` | Future\<void> | Set custom identifier for the transaction. |
| `setLocation({lat, lon})` | Future\<void> | Set user geolocation. |
| `setLivenessSettings({enabled})` | Future\<void> | Enable/disable liveness detection. |
| `getAvailableIdTypes()` | Future\<List\<IdType>> | Get list of supported document types. |
| `getAvailableCountries()` | Future\<List\<IdCountry>> | Get list of supported countries. |
| `getBusinessConfiguration()` | Future\<BusinessConfiguration> | Get client-specific configuration. |

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `instance` | NetkiSdk | Singleton instance of the SDK. |
| `identificationEvents` | Stream\<IdentificationEvent> | Stream of identification capture events. |

### Models

#### ResultInfo

Returned by all async SDK operations.

```dart
class ResultInfo {
    final RequestStatus status;
    final Map<String, String>? extraData;
    final ErrorType? errorType;
    final String? message;

    bool get isSuccessful => status == RequestStatus.SUCCESS;
}
```

#### IdentificationEvent

Events emitted during the identification flow.

```dart
sealed class IdentificationEvent {}

class IdentificationSuccess extends IdentificationEvent {
    final Map<String, dynamic>? extraData;
}

class IdentificationCancelled extends IdentificationEvent {
    final ResultInfo? resultInfo;
}

class IdentificationUnknown extends IdentificationEvent {
    final String event;
}
```

#### IdCountry

Represents a supported country for document capture.

```dart
class IdCountry {
    final String name;
    final String alpha2;
    final String alpha3;
    final String countryCallingCode;
    final bool hasBarcodeId;
    final String? flag;
    final bool isBanned;
    final bool hasNfcPassport;
}
```

#### BusinessConfiguration

Client-specific configuration returned by `getBusinessConfiguration()`.

```dart
class BusinessConfiguration {
    final String? name;
    final String? identificationId;
    final String? welcomeMessage;
    final String? logoLight;
    final String? minimumAppVersion;
    final int? phonePinTimeout;
    final int? phoneRetryAttemptLimit;
    final bool? phoneUseAutomaticBypass;
    final String? clientGuid;
    final String? redirectBackLink;
    final String? completedMessage;
    final bool isGeolocationEnabled;
}
```

### Enums

#### RequestStatus

```dart
enum RequestStatus {
    SUCCESS,
    ERROR,
}
```

#### IdType

```dart
enum IdType {
    driversLicense,
    passport,
    governmentId,
    biometrics,
}
```

#### PictureType

```dart
enum PictureType {
    front,
    back,
    liveness,
    selfie,
    epassport,
}
```

#### ErrorType

| Value | Description |
|-------|-------------|
| `NO_INTERNET` | No network connection available |
| `INVALID_DATA` | Invalid data provided to the SDK |
| `INVALID_TOKEN` | API token is invalid or expired |
| `INVALID_ACCESS_CODE` | Access code is invalid |
| `INVALID_PHONE_NUMBER` | Phone number format is invalid |
| `INVALID_CONFIRMATION_CODE` | SMS confirmation code is incorrect |
| `USER_CANCEL_IDENTIFICATION` | User cancelled the identification flow |
| `TRANSACTION_NOT_FOUND` | Transaction ID does not exist |
| `CONFIGURATION_ERROR` | SDK configuration error |
| `UNEXPECTED_ERROR` | An unexpected error occurred |

#### AdditionalDataField

Fields that can be passed to `submitIdentification()` or `submitBiometrics()`:

| Field | Description |
|-------|-------------|
| `AdditionalDataField.firstName` | User's first name |
| `AdditionalDataField.lastName` | User's last name |
| `AdditionalDataField.middleName` | User's middle name |
| `AdditionalDataField.alias` | Alternative name |
| `AdditionalDataField.clientGuid` | Custom client identifier |
| `AdditionalDataField.gender` | Gender |
| `AdditionalDataField.countryCode` | Country code |
| `AdditionalDataField.birthDate` | Date of birth |
| `AdditionalDataField.deathDate` | Date of death |
| `AdditionalDataField.birthLocation` | Place of birth |
| `AdditionalDataField.phoneNumber` | Phone number |
| `AdditionalDataField.email` | Email address |
| `AdditionalDataField.ssn` | Social Security Number |
| `AdditionalDataField.tin` | Tax Identification Number |
| `AdditionalDataField.duiNumber` | DUI number |
| `AdditionalDataField.medicalLicense` | Medical license number |
| `AdditionalDataField.investorType` | Type of investor |
| `AdditionalDataField.isAccreditedInvestor` | Accredited investor status |
| `AdditionalDataField.isPhoneValidated` | Phone validation status |

---

## Theming

To customize the look and feel of the SDK screens to match your app's design, see the [Theme Documentation](./onboard_id_theme_v2.md).
