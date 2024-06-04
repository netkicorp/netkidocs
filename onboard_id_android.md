# Netki OnboardID SDK - Android

## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#everview)
- [Example Usage](#example-usage)
- [Screen Theme and Advanced Usage](#screen-theme-and-advanced-usage)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Individual camera controls](#individual-camera-controls)

## Overview

Integrate directly inside your native Android mobile application.

Keep your users within your mobile flow while leveraging the power of the Netki OnboardID ML flow. Your application has
the ability to start a user on their KYC by triggering our camera control flow algorithms. We have taken the difficulty
out of capturing clear images.

## Example Usage

The OnboardID SDK is quite flexible. The use cases go from no modifications and just a couple function calls to advanced
modifications and keeping control of the user data on the mobile device. It is likely that we have experienced some use
case that would match your business requirements.

### Screen Theme and Advanced Usage

In some cases you will want to match very closely to your application theme. Maybe you have a design team that is
adamant about continuity or just wants to give a more unique experience.

For more information regarding customization of these screens please see
the [Android/iOS Theme Documentation](./onboard_id_theme.md)

## Getting Started

We keep our SDK under a repository that is powered by [Artifactory](https://jfrog.com/artifactory/).

## Installation

### Step 1

On the build.gradle file of the project add the maven Netki repository and the huawei in the repositories for all projects.

```groovy
allprojects {
    repositories {
        
        // Netki repo configuration
        maven {
            url "https://art.myverify.io//netki/libs-release-local/"
        }

        // Required repositories
        google()
        jcenter()
        maven { url "https://developer.huawei.com/repo/" }
    }
}
```

### Step 2

Add NetkiSDK as dependency in the app/build.gradle file with the desired version

```groovy
implementation 'com.netki:netkisdk:${latest.version}'
```

## Basic Usage

The most common example of using our SDK is to import the SDK into your application and when you need it you initialize
it with your keys and then it returns back when completed.

The OnBoardId SDK for Kotlin is natively asynchronous. Most operations are generated as suspend functions, which you
call from a coroutine. For additional information about coroutines, refer to
the [official Kotlin documentation](https://kotlinlang.org/docs/coroutines-overview.html).

All the Async calls to the SDK return an object called `ResultInfo`

```kotlin
class ResultInfo(
    val status: RequestStatus,
    val errorType: ErrorType? = null,
    val message: String? = null
)
```

This object returns a status where you can check the result of the operation, the possible statuses are:

```kotlin
SUCCESS
ERROR
```

Also, you can access this information through the extension method:

```kotlin
ResultInfo.isSuccessful()
```

For the error details check the end of this section.

### Step 1

Initialize SDK

Before using any of the methods initialize the SDK as below.

```kotlin
OnBoardId.initialize(applicationContext)
```

### Step 2

Configure the SDK passing in the API key provided by Netki as the token.

```kotlin
launch {
    val result = OnBoardId.configureWithToken(token)
    if (result.isSuccessful()) {
        // TODO: implement successful logic.
    } else {
        // TODO: implement error logic.
    }
}
```

Once the configuration callback block returns, the environment will be configured and ready to proceed.

### Step 3

Get an intent to start the process to capture the id pictures.

To get the intent use the method:

```kotlin
val intent = OnBoardId.getIdentificationIntent(
    idtype,
    idCountry
)
```

&nbsp;

Where:

`idType`: The type of id that will be used for the capture process.

The types are:

```kotlin
DRIVERS_LICENSE
PASSPORT
GOVERNMENT_ID
```

To fetch list of available ids use:

```kotlin
val availableIdTypes = OnBoardId.getAvailableIdTypes()
```

&nbsp;

`idCountry`: The country that issued the id that will be used for the capture process.

To fetch the list of available countries use:

```kotlin
val availableCountries = OnBoardId.getAvailableCountries()
```

&nbsp;

Once you have your intent you start the activity expecting for a result. For additional information refer
to [Android official documentation](https://developer.android.com/training/basics/intents/result).

### Step 4

Handling result of the capture process

To validate the result of the process use the value of `resultCode`

```kotlin
if (result.resultCode == Activity.RESULT_OK) {
    // TODO: implement successful logic.
} else if (result.resultCode == Activity.RESULT_CANCELED) {
    // TODO: implement error logic.
}
```

### Step 5

Once the capture process is successful, create the transaction for identification process.

To create the transaction for the identification process use:

```kotlin
launch {
    val result = OnBoardId.submitIdentification()
    if (result.isSuccessful()) {
        // TODO: implement successful logic.
    } else {
        // TODO: implement error logic.
    }
}
```

If the previous method returns a success status, it means that the data was posted successfully, the result of the
identification process will be posted to the
defined [backend callback](https://github.com/netkicorp/netkidocs/blob/master/best_practices_internal_callbacks.md),
this is an async method.

### Additional methods

If you want to set extra data specific to your business use:

```kotlin
val businessMetadata: Map<String, String> = mapOf("key" to "value")

OnBoardId.setBusinessMetadata(businessMetadata)
```

### Error handling

#### Method invocation

In case when the `ResultInfo` returns an error status, it will return an `ErrorType`, the possible values for these are:

```kotlin
NO_INTERNET
INVALID_DATA
INVALID_TOKEN
INVALID_ACCESS_CODE
INVALID_PHONE_NUMBER
INVALID_CONFIRMATION_CODE
UNEXPECTED_ERROR
USER_CANCEL_IDENTIFICATION
```

In case that there is more information about the error, you can find it in the `message` inside `ResultInfo`.

## Individual camera controls

We provide the ability for the mobile device to control the function of individual screens. You may want to send the
user to a very detailed instruction page that is tailored to your demographics before each picture capture page. You
know your audience better than us.

The client has the option to invoke individual camera controls to take pictures of different documents and sides of the
document.

### Step 1

Get an intent to start the process to capture the id pictures.

To get the intent use the method:

```kotlin
val intent = getCaptureIdIntent(
    idType,
    pictureType,
    captureIdProperties
)
```

&nbsp;

Where:

`idType`: The type of id that will be captured.

The types are:

```kotlin
DRIVERS_LICENSE
PASSPORT
GOVERNMENT_ID
```

`pictureType`: The type of picture that will be captured.

The types supported are:

```kotlin
FRONT
BACK
SELFIE
```

`captureIdProperties`: Extra properties for the capture of the id.

The options are:

```kotlin
readBarcodeBackId // default: false, only applies for idType:DRIVERS_LICENSE/GOVERNMENT_ID, pictureType:BACK
validateLiveness  // default: false, only applies for pictureType:SELFIE
```

&nbsp;

Once you have your intent you start the activity expecting for a result. For additional information refer
to [Android official documentation](https://developer.android.com/training/basics/intents/result).

### Step 2

Handling result of the capture id process

To validate the result of the process use the value of `resultCode`

```kotlin
if (result.resultCode == Activity.RESULT_OK) {
    // TODO: implement successful logic.
} else if (result.resultCode == Activity.RESULT_CANCELED) {
    // TODO: implement error logic.
}
```

If the result is `RESULT_OK`, it returns a list of the picture captured that you can access using:

```kotlin
result.data?.extras?.getSerializable(ResultInfoExtraData.EXTRA_DATA_PICTURES_KEY.description) as List<Picture>
```

The picture object is:

```kotlin
class Picture(
    val path: String? = null,
    val barcodes: MutableList<Barcode> = mutableListOf(),
    val passportContent: PassportContent? = null,
    val livenessInformation: LivenessInformation? = null,
    val type: PictureType
)
```

### Error handling

In case when the resultCode is `RESULT_CANCELED` you can get the `ErrorType` using:

```kotlin
result.data?.extras?.getSerializable(ResultInfoExtraData.ERROR_TYPE.description) as ErrorType
```

```kotlin
USER_CANCEL_IDENTIFICATION
UNEXPECTED_ERROR
```

In case that there is more information about the error, you can find it using:

```kotlin
result.data?.extras?.getString(ResultInfoExtraData.MESSAGE.description)
```
