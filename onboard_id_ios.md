# Netki OnboardID SDK - iOS


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Example Usage](#example-usage)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Advanced Usage](#advanced-usage)
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

* Enable Bitcode - No
* Info.plist - Enter a string for Privacy - Camera Usage Description (e.g. Scanning ID front and back)

We are currently shipping with bitcode off. If you need it enabled let us know and we can enable that.

### Binaries

* Embedded Binaries - Add the following framework files to the project (see SwiftExample project for other details)
	* confirm_sdk.framework
	* netkisdk_ios.framework

### Netki Internal Objects

Below are some of the Netki defined business and application objects. These data structures will be used for parameters and also for return objects.

There are 4 types of documents supported currently in the Netki. Types of ID1 and ID3 standardized international documents. Basically a card and passport standard.

```objectivec
typedef NS_ENUM(NSInteger, NTKDocType) {
	NTKDocTypeDriversLicense,
	NTKDocTypePassport,
	NTKDocTypeGovernmentID,
	NTKDocTypeOther
};
```

In addition to the DOCUMENT TYPE there is an IMAGE TYPE which may associate 1 or more objects to that document type. For instance a driver's license has a FRONT and BACK.

Each document has an associated IMAGE TYPE that indicates if it is a stand alone or part of another document class. Documents of type ID3 will have a FRONT and BACK as expected.

The face image is also a document labeled as SELFIE. That is the only biometric human data.

```objectivec
NTKDocumentImageType const NTKDocumentImageTypeFRONT;
NTKDocumentImageType const NTKDocumentImageTypePASSPORT;
NTKDocumentImageType const NTKDocumentImageTypePASSPORTLASTPAGE;
NTKDocumentImageType const NTKDocumentImageTypeBACK;
NTKDocumentImageType const NTKDocumentImageTypeSELFIE;
```

### Instantiation Of The SDK

Import the NetkiSDK framework.

    #import <netkisdk_ios/NetkiClient.h>

From inside your application you must initialize the SDK by running below with your TOKEN provided by Netki.

```objectivec
- (void)configureWithClientToken:(NSString *)token block:(void (^)(BOOL success, NSError * _Nullable error))block;
```

Once the block returns, the environment will be configured and ready to proceed

### SDK Execution

The 2 character ISO country code of the issuing state **must** be provided. Set the issuing country on the `NetkiClient.issuingCountry` property (default is US) for the documents to be scanned.


Note: there is a list of countries is provided via `[[NetkiClient sharedClient] appContext].countries`. You must provide your own UI for allowing the user to select the issuing country of their ID.

Set the `docType` property on the NetkiClient of the document to be scanned using the `NTKDocType` ENUM


### Delegate Description



```objectivec
NTKCaptureControllerDelegate

@protocol NTKCaptureControllerDelegate <NSObject>
```

Create an instance of the `NTKCameraFlowViewController` and subscribe to its delegate. Then present the view controller modally

When the scan is complete, the camera flow view controller will self-dismiss and the delegate method will be called.

Call `- validateAndCompleteWithBlock:onProgress:` and wait for the transaction ID string in the callback response.

The completion block will return success and either a `Transaction ID` or Error on the event of failure.


## Response Values

There are some cases in which you will want to access the data that was captured by the cameras. Things like the documents that were captured, the data out of the documents, the face coordinates, MRZ, etc.

Refer to the `NTKDocumentImage` object.

Description:

```objectivec
@interface NTKDocumentImage : NSObject

@property (nonatomic, strong, readonly) UIImage *image;
@property (nonatomic, strong, readonly) NTKDocumentImageType imageType;
@property (nonatomic, assign, readonly) NTKDocType docType;
@property (nonatomic, strong, nullable, readonly) NTKCountry *issuingCountry;
@property (nonatomic, strong, readonly) NTKFaceRecognitionInfo *faceRecognitionInfo;
@property (nonatomic, strong, readonly) NTKBarcodeRecognitionInfo *barcodeRecognitionInfo;
@property (nonatomic, strong) UIImage *croppedDocumentFace;
@property (nonatomic, strong) NTKMRZInfo *mrzInfo;

@end
```
The naming convention should be self-explanatory as to what is inside.

If a face was detected on the document you can access it using `croppedDocumentFace` parameter.


If a barcode was detected on the document you can access that data using the `barcodeRecognitionInfo` parameter.

The data inside the barcode will be available inside the `NTKBarcodeDriverLicenseInfo` object.

```objectivec
@interface NTKBarcodeDriverLicenseInfo: NSObject

@property (nonatomic, strong) NSString *documentType;
@property (nonatomic, strong) NSString *firstName;
@property (nonatomic, strong) NSString *middleName;
@property (nonatomic, strong) NSString *lastName;
@property (nonatomic, strong) NSString *gender;
@property (nonatomic, strong) NSString *addressStreet;
@property (nonatomic, strong) NSString *addressCity;
@property (nonatomic, strong) NSString *addressState;
@property (nonatomic, strong) NSString *addressZip;
@property (nonatomic, strong) NSString *licenseNumber;
@property (nonatomic, strong) NSString *issueDate;
@property (nonatomic, strong) NSString *expiryDate;
@property (nonatomic, strong) NSString *birthDate;
@property (nonatomic, strong) NSString *issuingCountry;

@end
```

Access the MRZ info using `NTKMRZInfo` object.


```objectivec
@interface NTKMRZInfo : NSObject
@property (readonly) NSString *birthDate;
@property (readonly) NSString *docType;
@property (readonly) NSString *documentNumber;
@property (readonly) NSString *expiryDate;
@property (readonly) NSString *givenName;
@property (readonly) NSString *invitNumber;
@property (readonly) NSString *issuerAbbr;
@property (readonly) NSString *nameGroup;
@property (readonly) NSString *nationAbbr;
@property (readonly) NSString *personalNumber;
@property (readonly) NSString *sex;
@property (readonly) NSString *surName;
@property (readonly) NSString *visaId;

+ (instancetype)mrzInfoWithDictionary:(NSDictionary *)dictionary;

@end
```
If we scan the BACK PAGE of the passport there may be a passport serial number if we were able to read that 1D barcode. It can be accessed via the `NTKBarcodePassportBackInfo` object.

```objectivec
@interface NTKBarcodePassportBackInfo : NSObject
@property (nonatomic, strong) NSString *licenseNumber;

@end
```

In the case of detection and decoding of barcodes of either 2D ID barcodes or back page passport 1D barcodes we will populate a general barcode recognition object.


```objectivec
@interface NTKBarcodeRecognitionInfo : NSObject
@property (nonatomic, strong) NSDictionary *rawInfo;
@property (nonatomic, strong) NTKBarcodeDriverLicenseInfo *diverLicenseInfo;
@property (nonatomic, strong) NTKBarcodePassportBackInfo *passportBackInfo;

- (instancetype)initWithDriverLicenseRawData:(NSDictionary *)rawData;

@end
```



## Advanced Usage

### Individual Screen Controllers

In some cases you will want to run individual screens. We offer the ability to delegate controllers on an ad-hoc basis.

Here we are instantiating and running the controller and telling it to run the capture and review sequence for ID type and FRONT.

Parameters are:

- issuingCountry
- doctype
- imageType

See above for document type ENUM and Image Type [front/back/etc].

```objectivc
NTKIDCaptureViewController *idCaptureController = [[NTKIDCaptureViewController alloc] initWithIssuingCountry:NetkiClient.sharedClient.issuingCountry doctype:NTKDocTypeDriversLicense imageType:NTKDocumentImageTypeFRONT];
```

Delegate the callback to the `self` class.

```objectivec
idCaptureController.delegate = self;
[self presentViewController:idCaptureController animated:YES completion:nil];
```

### Review Screen Success Auto Continue

Once a user takes a picture they will be given a review screen. On that screen they will need to review the results of the algorithm.  By default we will show a continue and retry button.  The order of these buttons are reversed based on the results of the algorithms. If all pass then continue is the call-to-action (CTA) on the top and retry on the bottom in minor colors. If any fail then the order is reversed with the retry CTA on the top and the continue showing on the bottom next to a warning symbol.

SJS: i need two examples screenshots of each scenario

If you want to take the user to the next screen review screen automatically you can do that. The thought is that if all pass then you can index automatically. Use review validation strategy.

Please see the example below:

```objectivec
- (void)startDocumentFrontDetection {
	NTKIDCaptureViewController *idCaptureController = [[NTKIDCaptureViewController alloc] initWithIssuingCountry:NetkiClient.sharedClient.issuingCountry doctype:NTKDocTypeDriversLicense imageType:NTKDocumentImageTypeFRONT];
	idCaptureController.delegate = self;
	idCaptureController.reviewValidationStrategy = [NTKReviewValidationStrategyCloseIfAllDataValid strategyWithCloseFlag:YES timeout:2.0f];
	[self presentViewController:idCaptureController animated:YES completion:nil];
}
```

### Setting Custom Labels

SJS: screen shots of each of these showing their locations.

```objectivec
@interface NTKCustomTextProvider : NSObject

+ (NSString *)headerForCaptureDLFront;
+ (void)setHeaderForCaptureDLFront:(NSString *)text;

+ (NSString *)headerForCaptureDLBack;
+ (void)setHeaderForCaptureDLBack:(NSString *)text;

+ (NSString *)headerForCapturePassportFront;
+ (void)setHeaderForCapturePassportFront:(NSString *)text;

+ (NSString *)headerForCapturePassportLastPage;
+ (void)setHeaderForCapturePassportLastPage:(NSString *)text;

@end
```

### Phone Confirmation (optional)

In some cases you may with to use our phone validation service. This is a service that is rarely used by our clients. It was surfaced here because we do it in the flagship MyVerify application and it exists.  

This service will take a phone number and check that number to see if it is really a mobile number and send a PIN verification number in a text message.  We do have the ability to return an exception if VIOP checking is part of your contract. See sales for more information.

Sending phone number:

You must use an ISO formatted phone number string. Example: *+1234567890*

```objectivec
- (void)requestSecurityCodeForPhoneNumber:(NSString *)phoneNumber block:(void (^)(BOOL success, NSError * _Nullable error))block;
```

The user will pull that text message and provide it to the application. Send the phone number in again along with the user-provided PIN.

```objectivec
- (void)validateSecurityCode:(NSString *)securityCode forPhoneNumber:(NSString *)phoneNumber block:(void (^)(BOOL success, NSError * _Nullable error))block;
```


## Sample Application


## Callbacks

For More Information Regarding Callbacks please see our [Callback Best Practices](./best_practices_internal_callbacks.md) first.

Once you are logged into your dashboard you will find documentation related to what data exists inside those callbacks so that you can map to fields on your side.
