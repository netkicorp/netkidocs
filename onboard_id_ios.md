# Netki OnboardID SDK - iOS

The OnboardID SDK enables you to integrate identity verification directly into your iOS application. Users can capture ID documents and selfies without leaving your app.

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

- iOS 13.0 or higher
- Camera permission
- Photo Library permission

Add the following keys to your `Info.plist`:

```xml
<key>NSCameraUsageDescription</key>
<string>Camera access is required to capture ID documents</string>
<key>NSPhotoLibraryUsageDescription</key>
<string>Photo library access is required to store captured images</string>
<key>NSPhotoLibraryAddUsageDescription</key>
<string>Photo library access is required to store captured images</string>
```

## Installation

### Step 1: Add the Dependency

In your `Podfile`, add the Netki SDK:

```ruby
pod 'NetkiSDK', '~> {latest.version}'
```

Then run:

```bash
pod install
```

> **Note:** Replace `{latest.version}` with the current version. Check [CocoaPods](https://cocoapods.org/pods/NetkiSDK) for available versions.

### Step 2: Import the SDK

In your Swift files, import the SDK:

```swift
import NetkiSDK
```

## Core Integration

This section covers everything you need to integrate the SDK. Complete these 4 steps for a fully working implementation.

The SDK uses Swift async/await for asynchronous operations. For more information, see the [Swift concurrency documentation](https://developer.apple.com/documentation/swift/concurrency).

### Step 1: Initialize the SDK

Initialize the SDK once, typically in your `AppDelegate` or before starting the identification flow.

```swift
OnBoardId.shared.initialize()
```

### Step 2: Configure with Your API Token

Configure the SDK with the API token provided by Netki. This is an async function that must be called from a Task.

```swift
Task {
    let result = await OnBoardId.shared.configureWithToken(token: token)

    if result.isSuccessful() {
        // SDK is ready - proceed to capture
    } else {
        // Handle configuration error
    }
}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | String | Yes | Your API token from Netki |
| `accessCode` | String | No | Optional access code for the transaction |

### Step 3: Start the Identification Flow

Create a view to display the document capture UI.

```swift
let availableIdTypes = OnBoardId.shared.getAvailableIdTypes()
let availableCountries = OnBoardId.shared.getAvailableCountries()

let identificationView = OnBoardId.shared.getIdentificationView(
    idType: selectedIdType,
    idCountry: selectedCountry,
    identificationDelegate: self
)
```

**ID Types:**

| Type | Description |
|------|-------------|
| `IdType.DRIVERS_LICENSE` | Driver's license |
| `IdType.PASSPORT` | Passport |
| `IdType.GOVERNMENT_ID` | National ID card |

**Handling the Result:**

Implement the `IdentificationDelegate` protocol to receive capture results:

```swift
extension YourViewController: IdentificationDelegate {
    func onCaptureIdentificationSuccessfully() {
        // Capture successful - proceed to submit
        submitIdentification()
    }

    func onCaptureIdentificationCancelled(resultInfo: ResultInfo) {
        // User cancelled or error occurred
    }
}
```

### Step 4: Submit the Identification

After a successful capture, submit the data to Netki for processing.

```swift
Task {
    let result = await OnBoardId.shared.submitIdentification()

    if result.isSuccessful() {
        // Success - transaction is being processed
        // Results will be sent to your backend callback
        let transactionData = result.extraData
    } else {
        // Handle submission error
    }
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

```swift
Task {
    let result = await OnBoardId.shared.requestSecurityCode(phoneNumber: "+14155551234")

    if result.isSuccessful() {
        // Code sent - show UI for user to enter the code
    } else {
        // Handle error
    }
}
```

#### Confirm the Security Code

Verify the code entered by the user:

```swift
Task {
    let result = await OnBoardId.shared.confirmSecurityCode(
        phoneNumber: "+14155551234",
        securityCode: userEnteredCode
    )

    if result.isSuccessful() {
        // Phone verified - proceed with identification
    } else {
        // Handle error
    }
}
```

### Biometrics Re-capture

If you need to re-capture biometric data for an existing transaction (e.g., after a failed liveness check), use the biometrics flow.

**Prerequisites:** Complete Steps 1 and 2 from Core Integration first.

```swift
let biometricsView = OnBoardId.shared.getBiometricsView(
    transactionId: transactionId,
    identificationDelegate: self
)
```

After capturing, submit the biometrics:

```swift
Task {
    let result = await OnBoardId.shared.submitBiometrics()

    if result.isSuccessful() {
        // Biometrics submitted successfully
    } else {
        // Handle submission error
    }
}
```

### Configuration Options

#### Business Metadata

Attach custom metadata to the transaction for your internal tracking:

```swift
let metadata = [
    "internal_user_id": "12345",
    "signup_source": "mobile_app"
]
OnBoardId.shared.setBusinessMetadata(businessMetadata: metadata)
```

#### Client GUID

Set a custom identifier to link the transaction to your system:

```swift
OnBoardId.shared.setClientGuid(clientGuid: "your-unique-identifier")
```

#### Geolocation

Capture the user's location with the transaction:

```swift
OnBoardId.shared.setLocation(lat: "37.7749", lon: "-122.4194")
```

#### Liveness Detection

Enable or disable liveness detection for selfie capture:

```swift
OnBoardId.shared.setLivenessSettings(enabled: true)
```

---

## Error Handling

All SDK operations return a `ResultInfo` object:

```swift
struct ResultInfo {
    let status: RequestStatus
    let extraData: [String: String]?
    let errorType: ErrorType?
    let message: String?
}
```

Check the result using:

```swift
if result.isSuccessful() {
    // Handle success
} else {
    // Handle error based on result.errorType and result.message
}
```

**Error Types:**

| Error Type | Description |
|------------|-------------|
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

---

## API Reference

### Methods

| Method | Returns | Description |
|--------|---------|-------------|
| `initialize()` | Void | Initialize the SDK. Call once before any other methods. |
| `configureWithToken(token:accessCode:)` | ResultInfo | Configure SDK with API credentials. Async function. |
| `getIdentificationView(idType:idCountry:identificationDelegate:)` | IdentificationView | Get view to launch the identification capture flow. |
| `submitIdentification(additionalData:)` | ResultInfo | Submit captured identification data. Async function. |
| `getBiometricsView(transactionId:identificationDelegate:)` | IdentificationView | Get view to re-capture biometrics for a transaction. |
| `submitBiometrics(additionalData:)` | ResultInfo | Submit re-captured biometric data. Async function. |
| `requestSecurityCode(phoneNumber:)` | ResultInfo | Send SMS verification code. Async function. |
| `confirmSecurityCode(phoneNumber:securityCode:)` | ResultInfo | Verify SMS code. Async function. |
| `setBusinessMetadata(businessMetadata:)` | Void | Attach custom metadata to the transaction. |
| `setClientGuid(clientGuid:)` | Void | Set custom identifier for the transaction. |
| `setLocation(lat:lon:)` | Void | Set user geolocation. |
| `setLivenessSettings(enabled:)` | Void | Enable/disable liveness detection. |
| `getAvailableIdTypes()` | [IdType] | Get list of supported document types. |
| `getAvailableCountries()` | [IdCountry] | Get list of supported countries. |
| `getBusinessConfiguration()` | BusinessConfiguration | Get client-specific configuration. |

### Models

#### ResultInfo

Returned by all async SDK operations.

```swift
struct ResultInfo {
    let status: RequestStatus
    let extraData: [String: String]?
    let errorType: ErrorType?
    let message: String?
}
```

#### Picture

Represents a captured image with extracted data.

```swift
struct Picture {
    let path: String?
    let barcodes: [Barcode]
    let passportContent: PassportContent?
    let ePassportContent: EPassportContent?
    let livenessInformation: LivenessInformation?
    let type: PictureType
}
```

#### Barcode

Data extracted from document barcodes (typically driver's licenses).

```swift
struct Barcode {
    let rawValue: String?
    let displayValue: String?
    let valueFormat: Int?
    let driverLicense: DriverLicense?
}

struct DriverLicense {
    let documentType: String?
    let firstName: String?
    let middleName: String?
    let lastName: String?
    let gender: String?
    let addressStreet: String?
    let addressCity: String?
    let addressState: String?
    let addressZip: String?
    let licenseNumber: String?
    let issueDate: String?
    let expiryDate: String?
    let birthDate: String?
    let height: String?
    let eyeColor: String?
    let issuingCountry: String?
}
```

#### PassportContent

Data extracted from passport MRZ (Machine Readable Zone).

```swift
struct PassportContent {
    let text: String?
    let passportType: String?
    let countryCode: String?
    let lastName: String?
    let firstName: String?
    let passportNumber: String?
    let nationality: String?
    let birthDate: String?
    let gender: String?
    let expirationDate: String?
    let isValid: Bool?
    let confidence: Float?
}
```

#### LivenessInformation

Liveness detection results from selfie capture.

```swift
struct LivenessInformation {
    let isLive: Bool?
    let livenessScore: Float?
    let livenessActionAttempts: LivenessActionAttempts?
}
```

#### EPassportContent

Data extracted from NFC-enabled passports.

```swift
struct EPassportContent {
    let isValid: Bool
    let documentCode: String?
    let issuingState: String?
    let primaryIdentifier: String?
    let secondaryIdentifier: String?
    let nationality: String?
    let documentNumber: String?
    let dateOfBirth: String?
    let gender: Gender?
    let dateOfExpiry: String?
    let faceImagePath: String?
    let additionalPersonalData: AdditionalPersonalData?
}
```

#### AdditionalPersonalData

Extended personal data from NFC passports.

```swift
struct AdditionalPersonalData {
    let nameOfHolder: String?
    let placeOfBirth: String?
    let permanentAddress: String?
    let telephone: String?
    let profession: String?
    let personalNumber: String?
    let custodyInformation: String?
    let title: String?
}
```

#### IdCountry

Represents a supported country for document capture.

```swift
struct IdCountry {
    let name: String
    let alpha2: String
    let alpha3: String
    let countryCallingCode: String
    let hasBarcodeId: Bool
    let flag: String?
    let isBanned: Bool
    let hasNfcPassport: Bool
}
```

#### BusinessConfiguration

Client-specific configuration returned by `getBusinessConfiguration()`.

```swift
struct BusinessConfiguration {
    let name: String?
    let identificationId: String?
    let welcomeMessage: String?
    let logoLight: String?
    let idRequiredFields: [IdRequiredField]?
    let minimumAppVersion: String?
    let phonePinTimeout: Int?
    let phoneRetryAttemptLimit: Int?
    let phoneUseAutomaticBypass: Bool?
    let clientGuid: String?
    let redirectBackLink: String?
    let completedMessage: String?
    let isGeolocationEnabled: Bool
}
```

### Enums

#### RequestStatus

```swift
enum RequestStatus {
    case SUCCESS
    case ERROR
}
```

#### IdType

```swift
enum IdType {
    case DRIVERS_LICENSE
    case PASSPORT
    case GOVERNMENT_ID
}
```

#### PictureType

```swift
enum PictureType {
    case FRONT
    case BACK
    case SELFIE
    case LIVENESS
    case EPASSPORT
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

| Field | Key | Description |
|-------|-----|-------------|
| `FIRST_NAME` | "first_name" | User's first name |
| `LAST_NAME` | "last_name" | User's last name |
| `MIDDLE_NAME` | "middle_name" | User's middle name |
| `ALIAS` | "alias" | Alternative name |
| `CLIENT_GUID` | "client_guid" | Custom client identifier |
| `GENDER` | "gender" | Gender |
| `COUNTRY_CODE` | "country_code" | Country code |
| `BIRTH_DATE` | "birth_date" | Date of birth |
| `DEATH_DATE` | "death_date" | Date of death |
| `BIRTH_LOCATION` | "birth_location" | Place of birth |
| `PHONE_NUMBER` | "phone_number" | Phone number |
| `EMAIL` | "email" | Email address |
| `SSN` | "ssn" | Social Security Number |
| `TIN` | "tin" | Tax Identification Number |
| `DUI_NUMBER` | "dui_number" | DUI number |
| `MEDICAL_LICENSE` | "medical_license" | Medical license number |
| `INVESTOR_TYPE` | "investor_type" | Type of investor |
| `IS_ACCREDITED_INVESTOR` | "is_accredited_investor" | Accredited investor status |
| `IS_PHONE_VALIDATED` | "is_phone_validated" | Phone validation status |

---

## Theming

To customize the look and feel of the SDK screens to match your app's design, see the [Theme Documentation](./onboard_id_theme.md).
