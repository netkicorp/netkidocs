# Netki OnboardID SDK - iOS


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Example Usage](#example-usage)
- [Getting Started](#getting-started)
- [Sample Application](#sample-application)
- [Callbacks](#callbacks)

## Overview

Integrate directly inside your native iOS mobile application.

Keep your users within your mobile flow while leveraging the power of the Netki OnboardID ML flow. Your application has the ability to start a user on their KYC by triggering our camera control flow algorithms.  We have taken the difficulty out of capturing clear images.

## Example Usage

The OnboardID SDK is quite flexible. The use cases go from no modifications and just a couple function calls to advanced modifications and keeping control of the user data on the mobile device. It is likely that we have experience some use case that would match your business requirements.

### Basic Usage

The most common example of using our SDK is to import the SDK into your application and when you need it you initialize it with your keys and then it returns back when completed.

### Individual Screen Controls

We provide the ability for the mobile device to control the function of individual screens. You may want to send the user to a very detailed instruction page that is tailored to your demographics before each picture capture page. You know your audience better than us.  

### Screen Theme and Advanced Usage

In some cases you will want to match very closely to your application theme.  Maybe you have a design team that is adamant about continuity or just want to give a more unique experience.  

We allow the following aspects of the UI to be altered.

**Picture Review Screen**

- Button Font Color
- Button Colors
- Continue button show/hide
- Continue countdown timeout


**Picture Capture Screen**

- Single notification text

For more information regarding customization of these screens please see the [iOS Theme Documentation](./onboard_id_theme_iOS.md)


## Getting Started

We keep our SDK under a private repository that is powered by [Artifactory](https://jfrog.com/artifactory/).

We will need an email and your name so that we can create an Artifactory account for the Netki Repository. This will be the name that is extracted from the tech admin portion of the onboarding questionnaire.

## Installation


Below you will find instructions on how to install and configure the Netki OnboardID SDK for iOS.

### Settings

SJS: are we forced to be bitcode off?

* Enable Bitcode - No
* Info.plist - Enter a string for Privacy - Camera Usage Description (e.g. Scanning ID front and back)

### Binaries

* Embedded Binaries - Add the following framework files to the project (see SwiftExample project for other details)
	* confirm_sdk.framework
	* netkisdk_ios.framework

### Instantiation Of The SDK

Import the NetkiSDK framework in the AppDelegate

    #import <netkisdk_ios/NetkiClient.h>

From the AppDelegate, call below passing your TOKEN provided by Netki:

```obj-c
- (void)configureWithClientToken:(NSString *)token block:(void (^)(BOOL success, NSError * _Nullable error))block;
```

Once the block returns, the environment will be configured and ready to proceed

### SDK Execution

The 2 character ISO country code of the issuing state **must** be provided. Set the issuing country on the `NetkiClient.issuingCountry` property (default is US) for the documents to be scanned.


Note: there is a list of countries is provided via `[[NetkiClient sharedClient] appContext].countries`. You must provide your own UI for allowing the user to select the issuing country of their ID.

Set the `docType` property on the NetkiClient of the document to be scanned using the `NTKDocType` ENUM

* NTKDocTypeDriversLicense
* NTKDocTypePassport
* NTKDocTypeGovernmentID


Create an instance of the `NTKCameraFlowViewController` and subscribe to its delegate. Then present the view controller modally

When the scan is complete, the camera flow view controller will self-dismiss and the delegate method will be called.

Call `- validateAndCompleteWithBlock:onProgress:` and wait for the transaction ID string in the callback response.

The completion block will return success and either a `Transaction ID` or Error on the event of failure.


### Phone Confirmation (optional)

In some cases you may with to use our phone validation service. This is a service that is rarely used by our clients. It was surfaced here because we do it in the flagship MyVerify application and it exists.  

This service will take a phone number and check that number to see if it is really a mobile number and send a PIN verification number in a text message.  We do have the ability to return an exception if VIOP checking is part of your contract. See sales for more information.

Sending phone number:

You must use an ISO formatted phone number string. Example: *+1234567890*

```obj-c
- (void)requestSecurityCodeForPhoneNumber:(NSString *)phoneNumber block:(void (^)(BOOL success, NSError * _Nullable error))block;
```

The user will pull that text message and provide it to the application. Send the phone number in again along with the user-provided PIN.

```obj-c
- (void)validateSecurityCode:(NSString *)securityCode forPhoneNumber:(NSString *)phoneNumber block:(void (^)(BOOL success, NSError * _Nullable error))block;
```


## Sample Application


## Callbacks

For More Information Regarding Callbacks please see our [Callback Best Practices](./best_practices_internal_callbacks.md) first.

Once you are logged into your dashboard you will find documentation related to what data exists inside those callbacks so that you can map to fields on your side.
