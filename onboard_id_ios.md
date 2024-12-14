# Netki OnboardID SDK - iOS


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#everview)
- [Example Usage](#example-usage)
- [Screen Theme and Advanced Usage](#screen-theme-and-advanced-usage)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Usage](#basic-usage)


## Overview

Integrate directly inside your native iOS mobile application.

Keep your users within your mobile flow while leveraging the power of the Netki OnboardID ML flow. Your application has the ability to start a user on their KYC by triggering our camera control flow algorithms.  We have taken the difficulty out of capturing clear images.

## Example Usage

The OnboardID SDK is quite flexible. The use cases go from no modifications and just a couple function calls to advanced modifications and keeping control of the user data on the mobile device. It is likely that we have experience some use case that would match your business requirements.

### Screen Theme and Advanced Usage

In some cases you will want to match very closely to your application theme. Maybe you have a design team that is
adamant about continuity or just wants to give a more unique experience.

For more information regarding customization of these screens please see
the [Android/iOS Theme Documentation](./onboard_id_theme.md)

## Getting Started

We keep our SDK under a github repository [Github](https://github.com/).

## Installation

### Step 1

On your pod file declare the Netki Library.

```groovy
  pod 'NetkiSDK', :git => 'https://github.com/netkicorp/onboardid-pod.git', :tag => '{latest.version}'

```

### Step 2

Request Camera and File Permissions.

Make sure to declare Camera and File permissions for the app to function properly. You can do this using Xcode UI or by editing the XML file.

```groovy
    INFOPLIST_KEY_NSCameraUsageDescription = "To take pictures";
    INFOPLIST_KEY_NSPhotoLibraryAddUsageDescription = "To store pictures";
    INFOPLIST_KEY_NSPhotoLibraryUsageDescription = "To store pictures";
```

## Basic Usage

The most common example of using our SDK is to import the SDK into your application and when you need it you initialize
it with your keys and then it returns back when completed.

The OnBoardId SDK for Swift is natively asynchronous. Most operations are generated as `async` functions, which you
call from a Task. For additional information about tasks, refer to
the [official Swift documentation](https://developer.apple.com/documentation/swift/task).

All the Async calls to the SDK return an object called `ResultInfo`

```swift
public struct ResultInfo: Codable {
    public let status: RequestStatus
    public let extraData: [String : String]?
    public let errorType: ErrorType?
    public let message: String?
}
```

This object returns a status where you can check the result of the operation, the possible statuses are:

```swift
SUCCESS
ERROR
```

Also, you can access this information through the extension method:

```swift
ResultInfo.isSuccessful()
```

For the error details check the end of this section.

### Step 1

Initialize SDK

Before using any of the methods initialize the SDK as below.

```swift
OnBoardId.shared.initialize()
```

As a parameter you can pass a Environment to use a different environment than production.

```swift
OnBoardId.shared.initialize(environment: Environments.DEV)
```

### Step 2

Configure the SDK passing in the API key provided by Netki as the token.

```swift
Task {
    let result = await OnBoardId.shared.configureWithToken(token: token)
    if (result.isSuccessful()) {
        // TODO: implement successful logic.
    } else {
        // TODO: implement error logic.
    }
}
```

Once the configuration callback block returns, the environment will be configured and ready to proceed.

### Step 3

Get an IdentificationView to start the process to capture the id pictures.

To get the view use the method:

```swift
OnBoardId.shared.getIdentificationView(
    idType: idType,
    idCountry: idCountry,
    identificationDelegate: identificationDelegate
)
```

&nbsp;

Where:

`idType`: The type of id that will be used for the capture process.

The types are:

```swift
DRIVERS_LICENSE
PASSPORT
GOVERNMENT_ID
```

To fetch list of available ids use:

```swift
let availableIdTypes = OnBoardId.shared.getAvailableIdTypes()
```

&nbsp;

`idCountry`: The country that issued the id that will be used for the capture process.

To fetch the list of available countries use:

```swift
let availableCountries = OnBoardId.shared.getAvailableCountries()
```

Once you have your View you can display it as part of your flow, using the identificationDelegate to get the result.

### Step 4

Handling result of the capture process

To validate the result of the process implement a class using IdentificationDelegate.
There are 2 methods that will get called after the IdentificationView is finalized.

```swift
    func onCaptureIdentificationSuccessfully() {
        // TODO: implement successful logic.
    }
    
    func onCaptureIdentificationCancelled(resultInfo: NetkiSDK.ResultInfo) {
        // TODO: implement error logic.
    }
```

### Step 5

Once the capture process is successful, create the transaction for identification process.

To create the transaction for the identification process use:

```swift
Task {
    let result = await OnBoardId.shared.submitIdentification()
    if (result.isSuccessful()) {
        // TODO: implement successful logic.
    } else {
        // TODO: implement error logic.
    }
}
```

This method may return extra data related to the transaction. To access this data, you can check the extraData property in the result:

```swift
let extraData = result.extraData
```

If no extra data is returned, extraData may be `nil`.

If the previous method returns a success status, it means that the data was posted successfully, the result of the identification process will be posted to the defined [backend callback](https://github.com/netkicorp/netkidocs/blob/master/best_practices_internal_callbacks.md), this is an async method.

### Additional methods


#### Re-run Biometrics

To re-submit biometric data in the SDk, follow the steps below:

##### Steps to Re-run Biometrics
Repeat Steps 1 and 2
Begin by completing steps 1 and 2 from the previous section to prepare the environment and initialize the SDK.

##### Start the Biometric Capture Flow
Use the following command to get a vie for biometric data capture, replacing "transaction_id" with the appropriate transaction ID:


```swift
OnBoardId.shared.getBiometricsView(
    transactionId: transaction_id,
    identificationDelegate: identificationDelegate
)
```

##### Handle the Biometric Flow Results
Use the same code specified in Step 4 of the previous section to handle the biometric capture results.

##### Submit the Captured Biometrics
Once the success event is received, submit the captured biometric data by calling:

```swift
Task {
    let result = await OnBoardId.shared.submitBiometrics()
    if (result.isSuccessful()) {
        // TODO: implement successful logic.
    } else {
        // TODO: implement error logic.
    }
}
```

#### Extra data

If you want to set extra data specific to your business use:

```swift
OnBoardId.shared.submitIdentification(additionalData: [
    "key": "value",
])
```

### Error handling

#### Method invocation

In case when the `ResultInfo` returns an error status, it will return an `ErrorType`, the possible values for these are:

```swift
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
