# Netki OnboardID SDK - Android

The OnboardID SDK enables you to integrate identity verification directly into your Android application. Users can capture ID documents and selfies without leaving your app.

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
  - [Individual Camera Controls](#individual-camera-controls)
- [API Reference](#api-reference)
- [Error Handling](#error-handling)
- [Theming](#theming)

## Requirements

- Android API level 21 (Lollipop) or higher
- Camera permission
- Internet permission

Add the following permissions to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Installation

### Step 1: Add the Netki Repository

In your project's `build.gradle` file, add the Netki maven repository:

```groovy
allprojects {
    repositories {
        google()
        mavenCentral()

        // Netki SDK repository
        maven {
            url "https://art.myverify.io/netki/libs-release-local/"
        }

        // Required for ML models
        maven { url "https://developer.huawei.com/repo/" }
    }
}
```

### Step 2: Add the Dependency

In your app's `build.gradle` file, add the SDK dependency:

```groovy
implementation 'com.netki:netkisdk:${latest.version}'
```

> **Note:** Replace `${latest.version}` with the current version. Check [Maven Central](https://central.sonatype.com/artifact/com.netki/netkisdk) for available versions.

## Core Integration

This section covers everything you need to integrate the SDK. Complete these 4 steps for a fully working implementation.

The SDK uses Kotlin coroutines for asynchronous operations. For more information, see the [Kotlin coroutines documentation](https://kotlinlang.org/docs/coroutines-overview.html).

### Step 1: Initialize the SDK

Initialize the SDK with your application context. Call this once, typically in your `Application` class or before starting the identification flow.

```kotlin
OnBoardId.initialize(applicationContext)
```

### Step 2: Configure with Your API Token

Configure the SDK with the API token provided by Netki. This is a suspend function that must be called from a coroutine.

```kotlin
lifecycleScope.launch {
    val result = OnBoardId.configureWithToken(token)

    if (result.isSuccessful()) {
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

Create an intent to launch the document capture UI.

```kotlin
// Get available options
val availableIdTypes = OnBoardId.getAvailableIdTypes()
val availableCountries = OnBoardId.getAvailableCountries()

// Create the intent
val intent = OnBoardId.getIdentificationIntent(
    idType = selectedIdType,
    idCountry = selectedCountry
)

// Launch the capture activity
identificationLauncher.launch(intent)
```

**ID Types:**

| Type | Description |
|------|-------------|
| `IdType.DRIVERS_LICENSE` | Driver's license |
| `IdType.PASSPORT` | Passport |
| `IdType.GOVERNMENT_ID` | National ID card |

**Handling the Result:**

Register an activity result launcher to handle the capture result:

```kotlin
private val identificationLauncher = registerForActivityResult(
    ActivityResultContracts.StartActivityForResult()
) { result ->
    if (result.resultCode == Activity.RESULT_OK) {
        // Capture successful - proceed to submit
        submitIdentification()
    } else {
        // User cancelled or error occurred
        handleCaptureError(result)
    }
}
```

### Step 4: Submit the Identification

After a successful capture, submit the data to Netki for processing.

```kotlin
lifecycleScope.launch {
    val result = OnBoardId.submitIdentification()

    if (result.isSuccessful()) {
        // Success - transaction is being processed
        // Results will be sent to your backend callback
        val transactionData = result.extraData
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

```kotlin
lifecycleScope.launch {
    val result = OnBoardId.requestSecurityCode(phoneNumber = "+14155551234")

    if (result.isSuccessful()) {
        // Code sent - show UI for user to enter the code
    } else {
        when (result.errorType) {
            ErrorType.INVALID_PHONE_NUMBER -> showInvalidPhoneError()
            else -> showGenericError(result.message)
        }
    }
}
```

#### Confirm the Security Code

Verify the code entered by the user:

```kotlin
lifecycleScope.launch {
    val result = OnBoardId.confirmSecurityCode(
        phoneNumber = "+14155551234",
        securityCode = userEnteredCode
    )

    if (result.isSuccessful()) {
        // Phone verified - proceed with identification
    } else {
        when (result.errorType) {
            ErrorType.INVALID_CONFIRMATION_CODE -> showInvalidCodeError()
            else -> showGenericError(result.message)
        }
    }
}
```

### Biometrics Re-capture

If you need to re-capture biometric data for an existing transaction (e.g., after a failed liveness check), use the biometrics flow.

**Prerequisites:** Complete Steps 1 and 2 from Core Integration first.

```kotlin
// Get the biometrics capture intent
val intent = OnBoardId.getBiometricsIntent(transactionId)

// Launch the capture activity
biometricsLauncher.launch(intent)
```

After capturing, submit the biometrics:

```kotlin
lifecycleScope.launch {
    val result = OnBoardId.submitBiometrics()

    if (result.isSuccessful()) {
        // Biometrics submitted successfully
    } else {
        // Handle submission error
    }
}
```

### Configuration Options

#### Business Metadata

Attach custom metadata to the transaction for your internal tracking:

```kotlin
val metadata = mapOf(
    "internal_user_id" to "12345",
    "signup_source" to "mobile_app"
)
OnBoardId.setBusinessMetadata(metadata)
```

#### Client GUID

Set a custom identifier to link the transaction to your system:

```kotlin
OnBoardId.setClientGuid("your-unique-identifier")
```

#### Geolocation

Capture the user's location with the transaction:

```kotlin
OnBoardId.setLocation(lat = "37.7749", lon = "-122.4194")
```

#### Liveness Detection

Enable or disable liveness detection for selfie capture:

```kotlin
OnBoardId.setLivenessSettings(enabled = true)
```

### Individual Camera Controls

For advanced use cases, you can capture document images individually instead of using the full identification flow. This is useful when you need custom screens between captures.

```kotlin
val intent = OnBoardId.getCaptureIdIntent(
    idType = IdType.DRIVERS_LICENSE,
    pictureType = PictureType.FRONT,
    captureIdProperties = CaptureIdProperties(
        readBarcodeBackId = false,
        validateLiveness = false
    )
)

captureIdLauncher.launch(intent)
```

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `idType` | `IdType` | Document type: `DRIVERS_LICENSE`, `PASSPORT`, `GOVERNMENT_ID` |
| `pictureType` | `PictureType` | Image to capture: `FRONT`, `BACK`, `SELFIE` |
| `captureIdProperties` | `CaptureIdProperties` | Optional capture settings |

**Capture Properties:**

| Property | Default | Description |
|----------|---------|-------------|
| `readBarcodeBackId` | `false` | Read barcode on back of ID (driver's license/government ID only) |
| `validateLiveness` | `false` | Enable liveness detection (selfie only) |

**Handling the Result:**

```kotlin
private val captureIdLauncher = registerForActivityResult(
    ActivityResultContracts.StartActivityForResult()
) { result ->
    if (result.resultCode == Activity.RESULT_OK) {
        val pictures = result.data?.extras?.getSerializable(
            ResultInfo.ExtraData.PICTURES.description
        ) as? List<Picture>

        // Process captured pictures - see Picture model in API Reference for available fields
    } else {
        val errorType = result.data?.extras?.getSerializable(
            ResultInfo.ExtraData.ERROR_TYPE.description
        ) as? ErrorType
        // Handle capture error
    }
}
```

---

## Error Handling

All SDK operations return a `ResultInfo` object:

```kotlin
data class ResultInfo(
    val status: RequestStatus,
    val extraData: Map<String, String>?,
    val errorType: ErrorType?,
    val message: String?
)
```

Check the result using:

```kotlin
if (result.isSuccessful()) {
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
| `initialize(context)` | Unit | Initialize the SDK. Call once before any other methods. |
| `configureWithToken(token, accessCode?)` | ResultInfo | Configure SDK with API credentials. Suspend function. |
| `getIdentificationIntent(idType, idCountry)` | Intent | Get intent to launch the identification capture flow. |
| `submitIdentification(additionalData?)` | ResultInfo | Submit captured identification data. Suspend function. |
| `getBiometricsIntent(transactionId)` | Intent | Get intent to re-capture biometrics for a transaction. |
| `submitBiometrics(additionalData?)` | ResultInfo | Submit re-captured biometric data. Suspend function. |
| `getCaptureIdIntent(idType, pictureType, properties?)` | Intent | Get intent for individual document/selfie capture. |
| `requestSecurityCode(phoneNumber)` | ResultInfo | Send SMS verification code. Suspend function. |
| `confirmSecurityCode(phoneNumber, code)` | ResultInfo | Verify SMS code. Suspend function. |
| `setBusinessMetadata(metadata)` | Unit | Attach custom metadata to the transaction. |
| `setClientGuid(clientGuid)` | Unit | Set custom identifier for the transaction. |
| `setLocation(lat, lon)` | Unit | Set user geolocation. |
| `setLivenessSettings(enabled)` | Unit | Enable/disable liveness detection. |
| `getAvailableIdTypes()` | List\<IdType> | Get list of supported document types. |
| `getAvailableCountries()` | List\<IdCountry> | Get list of supported countries. |
| `getBusinessConfiguration()` | BusinessConfiguration | Get client-specific configuration. |

### Models

#### ResultInfo

Returned by all async SDK operations.

```kotlin
data class ResultInfo(
    val status: RequestStatus,
    val extraData: Map<String, String>? = null,
    val errorType: ErrorType? = null,
    val message: String? = null
)
```

#### Picture

Represents a captured image with extracted data.

```kotlin
data class Picture(
    val path: String?,
    val barcodes: List<Barcode>,
    val passportContent: PassportContent?,
    val ePassportContent: EPassportContent?,
    val livenessInformation: LivenessInformation?,
    val type: PictureType
)
```

#### Barcode

Data extracted from document barcodes (typically driver's licenses).

```kotlin
data class Barcode(
    val rawValue: String?,
    val displayValue: String?,
    val valueFormat: Int?,
    val driverLicense: DriverLicense?
)

data class DriverLicense(
    val documentType: String?,
    val firstName: String?,
    val middleName: String?,
    val lastName: String?,
    val gender: String?,
    val addressStreet: String?,
    val addressCity: String?,
    val addressState: String?,
    val addressZip: String?,
    val licenseNumber: String?,
    val issueDate: String?,
    val expiryDate: String?,
    val birthDate: String?,
    val height: String?,
    val eyeColor: String?,
    val issuingCountry: String?
)
```

#### PassportContent

Data extracted from passport MRZ (Machine Readable Zone).

```kotlin
data class PassportContent(
    val text: String?,
    val passportType: String?,
    val countryCode: String?,
    val lastName: String?,
    val firstName: String?,
    val passportNumber: String?,
    val nationality: String?,
    val birthDate: String?,
    val gender: String?,
    val expirationDate: String?,
    val isValid: Boolean?,
    val confidence: Float?
)
```

#### LivenessInformation

Liveness detection results from selfie capture.

```kotlin
data class LivenessInformation(
    val isLive: Boolean?,
    val livenessScore: Float?,
    val livenessActionAttempts: LivenessActionAttempts?
)
```

#### EPassportContent

Data extracted from NFC-enabled passports.

```kotlin
data class EPassportContent(
    val isValid: Boolean,
    val documentCode: String?,
    val issuingState: String?,
    val primaryIdentifier: String?,
    val secondaryIdentifier: String?,
    val nationality: String?,
    val documentNumber: String?,
    val dateOfBirth: String?,
    val gender: Gender?,
    val dateOfExpiry: String?,
    val faceImagePath: String?,
    val additionalPersonalData: AdditionalPersonalData?
)
```

#### AdditionalPersonalData

Extended personal data from NFC passports.

```kotlin
data class AdditionalPersonalData(
    val nameOfHolder: String?,
    val placeOfBirth: String?,
    val permanentAddress: String?,
    val telephone: String?,
    val profession: String?,
    val personalNumber: String?,
    val custodyInformation: String?,
    val title: String?
)
```

#### IdCountry

Represents a supported country for document capture.

```kotlin
data class IdCountry(
    val name: String,
    val alpha2: String,
    val alpha3: String,
    val countryCallingCode: String,
    val hasBarcodeId: Boolean,
    val flag: String?,
    val isBanned: Boolean,
    val hasNfcPassport: Boolean
)
```

#### BusinessConfiguration

Client-specific configuration returned by `getBusinessConfiguration()`.

```kotlin
data class BusinessConfiguration(
    val name: String?,
    val identificationId: String?,
    val welcomeMessage: String?,
    val logoLight: String?,
    val idRequiredFields: List<IdRequiredField>?,
    val minimumAppVersion: String?,
    val phonePinTimeout: Int?,
    val phoneRetryAttemptLimit: Int?,
    val phoneUseAutomaticBypass: Boolean?,
    val clientGuid: String?,
    val redirectBackLink: String?,
    val completedMessage: String?,
    val isGeolocationEnabled: Boolean
)
```

#### CaptureIdProperties

Configuration for individual camera capture.

```kotlin
data class CaptureIdProperties(
    val readBarcodeBackId: Boolean = false,
    val validateLiveness: Boolean = false
)
```

### Enums

#### RequestStatus

```kotlin
enum class RequestStatus {
    SUCCESS,
    ERROR
}
```

#### IdType

```kotlin
enum class IdType(val type: String, val displayName: String) {
    DRIVERS_LICENSE("drivers_license", "Driver License"),
    PASSPORT("passport", "Passport"),
    GOVERNMENT_ID("government_id", "Government ID")
}
```

#### PictureType

```kotlin
enum class PictureType(val type: String) {
    FRONT("front"),
    BACK("back"),
    SELFIE("selfie"),
    LIVENESS("liveness_image"),
    EPASSPORT("e-passport")
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
