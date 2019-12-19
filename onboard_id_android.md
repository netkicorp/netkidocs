# Netki OnboardID SDK - Android


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Example Usage](#example-usage)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Netki Internal Objects](#netki-internal-objects)
- [Advanced Usage](#advanced-usage)
- [Sample Application](#sample-application)
- [Callbacks](#callbacks)

## Overview

Integrate directly inside your native Android mobile application.

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

For more information regarding customization of these screens please see the [Android Theme Documentation](./onboard_id_theme_android.md)


## Getting Started

We keep our SDK under a private repository that is powered by [Artifactory](https://jfrog.com/artifactory/).

We will need an email and your name so that we can create an Artifactory account for the Netki Repository. This will be the name that is extracted from the tech admin portion of the onboarding questionnaire.

## Installation

### Step 1

- On the build.gradle file of the project add the maven Netki repository in the repositories for all projects

```java
allprojects {
    repositories {
        google()
        jcenter()
        // Netki repo configuration
        maven {
            url "https://art.myverify.io//netki/libs-release-local/"
            credentials {
                username = "YOUR_USER"
                password = "YOUR_PASSWORD"
            }
        }
    }
}
```
### Step 2

- Enable `renderscript` in the android `gradle` configuration

```java
android {
    ...

    defaultConfig {       
        ...
        renderscriptTargetApi 28
        renderscriptSupportModeEnabled true
    }
}
```

### Step 3

- Add NetkiSDK as dependency in the app/build.gradle file with the desired version

	// NetkiSDK
	api('com.netki:netkisdk:latest.version')

### Step 4

- Recompile the project.


### Note

Depending the configuration of your project you can get an error about `android:allowBackup` and/or `android:theme` not correct
In that case add the following in your `AndroidManifest.xml` file

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
package="com.netkiapp">
    ...
    <application
        ...
        tools:replace="android:allowBackup,android:theme">
        ...
    </application>

</manifest>
```
In case of an error related with

`Binary XML file line #37: Binary XML file line #37: Error inflating class`
Make sure that your theme extends from Material Design theme

For example

```xml
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="@style/Theme.MaterialComponents.Light.DarkActionBar">
        <!-- Customize your theme here. -->
    </style>

</resources>
```

## Basic Usage

### Step 1

Get an instance of the Netki class


```Java
Netki netki = Netki.INSTANCE;
```

For Koltin you can access the methods directly from the `Netki` object

### Step 2

Initialize SDK

Before using any of the methods initialize it as below.

```java
Netki.initialize(applicationContext)
```

### Step 3

Configure the SDK passing in the API key provided by Netki as the token and implementing a callback block.

```java
Netki.configureWithClientToken(token: String, callback: NetkiCallback)
```

Once the configuration callback block returns, the environment will be configured and ready to proceed



### Step 6

- Set the issuing country for the documents to be scanned by calling

```
Netki.setIssuingCountry(country: Country)
```

a white-list country array is provided via `Netki.getBusinessContext().getCountryList()`

### Step 7

- Set the documentType property on the NetkiSDK of the document to be scanned using the `DocumentType` ENUM

```
Netki.setDocumentType(documentType: DocumentType)
```
```
enum class DocumentType {
        DRIVERS_LICENSE,
        PASSPORT,
        GOVERNMENT_ID;
}
```

### Step 8

- Start an ActivityForResult/Intent to launch the identification process calling

```
IdentificationProcessActivity.createIntent(context)
```

This could throw an error, IllegalStateException with a message showing the error.
The main 2 reasons to throw this error are, running the SDK in a virtual device or not having frontal camera in the device running the SDK.

To validate the result of the IdentificationProcessActivity in the callback use the `resultCode`

```
resultCode == Activity.RESULT_OK
```

means that the process was succesfully done and we can send the data to netki

```
resultCode != Activity.RESULT_OK
```

means that the process was not finished succesfully and we need to start again the flow before sending the data to netki

If there was an error during the process you can find the information in the data that we returnt into the result like this

```
data.getStringExtra(IdentificationProcessActivity.ERROR_INFORMATION_KEY)
```

in other cases for example the user going back from that flow this will be empty

### Step 9

- On successful identification process activity response, call

```
Netki.validateAndComplete(callback: NetkiVerifyDataCallback)
```

to finish the transaction

### Step 10

- The callback will respond with the transaction ID once the identity data has been uploaded to the Netki servers.

## Individual camera controls

- The client has the option to invoke individual camera controls to take pictures of different documents and sides of the document.

### Starting the camera control

The activity responsible for that invocation is:


    CameraIdentificationActivity


The way to invoke it is creating an Intent passing as parameter the `DocumentType` and the `DocumentSide` to initiate the camera and then start the activity expecting the result.

```java
CameraIdentificationActivity.createIntent(context: Context, documentType: DocumentType, documentSide: DocumentSide)
```

For more information regarding `DocumentType` and `DocumentSide` object classes see [Netki Internal Objects](#netki-internal-objects)


Example of invocation

```java
startActivityForResult(
        CameraIdentificationActivity.createIntent(
                context,
                DocumentType.DRIVERS_LICENSE,
                DocumentSide.FRONT
        ), REQUEST_CAMERA_IDENTIFICATION
);
```

### Handling camera control result

The activity will return a success or error response to the element invoking it.

The first step is to validate if the process was finished successfully, for that you need to verify the `resultCode`, the 2 options are

```java
resultCode == RESULT_OK
```

```java
resultCode == RESULT_CANCELED
```

### Error result

If the `resultCode` is `RESULT_CANCELED` you can access the information of the error this way:

```java
CameraIdentificationActivity.ERROR_INFORMATION_KEY
```

Example:

```java
if (resultCode == RESULT_CANCELED) {
        String error = data != null ? data.getStringExtra(CameraIdentificationActivity.ERROR_INFORMATION_KEY) : "";
        Log.e(TAG, error);
}
```

### Success result

If the result is `RESULT_OK` the control will return a list of `Picture`

```java
data class Picture(
        val path: String,
        val barcodes: List<Barcode> = mutableListOf(),
        val passportContent: PassportContent? = null,
        val type: Picture.Type
)
```

where:

- **path**: where the image was saved, this is an internal memory that is only accesible to the app, it is not accesible for external app or the user
- **barcodes**: List of all the information extracted from the barcodes found in the identification, this could be empty. This will be available in case that back of DriverLicense was selected
- **passportContent**: Information readed from the MRZ in the passport, this could be null. This will be available in case that front of Passport was selected
- **type**: the type of picture returned


```java
enum class Type {
                FRONT,
                BACK,
                SELFIE,
                FACE_EXTRACTED
        }
```

The barcode object has the following structure:

```java
data class Barcode(
        val rawValue: String,
        val displayValue: String,
        val valueFormat: Int = 0,
        val email: Barcode.Email,
        val phone: Barcode.Phone,
        val sms: Barcode.Sms,
        val wifi: Barcode.WiFi,
        val url: Barcode.UrlBookmark,
        val geoPoint: Barcode.GeoPoint,
        val calendarEvent: Barcode.CalendarEvent,
        val contactInfo: Barcode.ContactInfo,
        val driverLicense: Barcode.DriverLicense
) {

        data class Email(
                val type: Int = 0,
                val address: String,
                val subject: String,
                val body: String
        ) {
                companion object {
                        const val UNKNOWN = 0
                        const val WORK = 1
                        const val HOME = 2
                }
        }

        data class Phone(
                val type: Int = 0,
                val number: String
        ) {
                companion object {
                        const val UNKNOWN = 0
                        const val WORK = 1
                        const val HOME = 2
                        const val FAX = 3
                        const val MOBILE = 4
                }
        }

        data class Sms(
                val message: String,
                val phoneNumber: String
        )

        data class WiFi(
                val ssid: String,
                val password: String,
                val encryptionType: Int = 0
        )

        data class UrlBookmark(
                val title: String,
                val url: String
        )

        data class GeoPoint(
                val lat: Double = 0.toDouble(),
                val lng: Double = 0.toDouble()
        )

        data class CalendarEvent(
                val summary: String,
                val description: String,
                val location: String,
                val organizer: String,
                val status: String,
                val start: Barcode.CalendarDateTime,
                val end: Barcode.CalendarDateTime
        )

        data class CalendarDateTime(
                val year: Int = 0,
                val month: Int = 0,
                val day: Int = 0,
                val hours: Int = 0,
                val minutes: Int = 0,
                val seconds: Int = 0,
                val isUtc: Boolean = false,
                val rawValue: String
        )

        data class ContactInfo(
                val name: Barcode.PersonName,
                val organization: String,
                val title: String,
                val phones: Array<Barcode.Phone>,
                val emails: Array<Barcode.Email>,
                val urls: Array<String>,
                val addresses: Array<Barcode.Address>
        )

        data class PersonName(
                val formattedName: String,
                val pronunciation: String,
                val prefix: String,
                val first: String,
                val middle: String,
                val last: String,
                val suffix: String
        )

        data class Address(
                val type: Int = 0,
                val addressLines: Array<String>
        ) {
                companion object {
                        const val UNKNOWN = 0
                        const val WORK = 1
                        const val HOME = 2
                }
         }

        data class DriverLicense(
                val documentType: String,
                val firstName: String,
                val middleName: String,
                val lastName: String,
                val gender: String,
                val addressStreet: String,
                val addressCity: String,
                val addressState: String,
                val addressZip: String,
                val licenseNumber: String,
                val issueDate: String,
                val expiryDate: String,
                val birthDate: String,
                val issuingCountry: String
        )
}
```

The passport object has the following structure

```java
data class PassportContent(
    val text: String,
    val nameGroup: String,
    val givenName: String,
    val surName: String,
    val birthDate: String,
    val docType: String,
    val issuer: String,
    val documentNumber: String,
    val nation: String,
    val sex: String,
    val expiryDate: String,
    val personalNumber: String,
    val visaId: String,
    val invitNumber: String
)
```

Example of success response:

```java
if (resultCode == RESULT_OK) {
        List<Picture> pictures = (List<Picture>) data.getSerializableExtra(CameraIdentificationActivity.PICTURES_KEY);
        for (Picture picture : pictures) {
                Log.e("MainActivity", "Image type: " + picture.getType());
                Log.e("MainActivity", "Path: " + picture.getPath());
        }
}
```

## Advanced Usage


### SMS Phone Validation

This is optional. Most SDK users do not use this.

In some cases you may with to use our phone validation service. This is a service that is rarely used by our clients. It was surfaced here because we do it in the flagship MyVerify application and it exists.  

This service will take a phone number and check that number to see if it is really a mobile number and send a PIN verification number in a text message.  We do have the ability to return an exception if VIOP checking is part of your contract. See sales for more information.

You must use an ISO formatted phone number string. Excluding the + or adding other characters will return a validation error.

Example: *+1234567890*

Sending phone number and receiving a PIN via SMS:


```java
Netki.requestSecurityCode(phoneNumber: String, callback: NetkiCallback)
```

### PIN Validation

To validate the code received by SMS call:

```java
Netki.validateSecurityCode(phoneNumber: String, securityCode: String, callback: NetkiCallback)
```

User the same formatted phone number and the 6-digit security PIN that was sent via SMS.


## Extra configuration

There are 2 extra methods that allow some extra configuration for the SDK

### Auto finish in full success

In case you want to auto close the review screen when all the validations are successful you can do it with the method

```
Netki.INSTANCE.autoFinishOnSuccess(finish: Boolean, secondsToFinish: Int)
```

where

- finish: is a flag to change between auto finishing when all validation is successful and not auto finishing, deafult to false
- secondsToFinish: is the seconds that the SDK will display the review screen before auto finishing it

### Set custom instructions camera screen

- If you want to display a custom message in the camera screen, you can invoke the next method

```java
Netki.INSTANCE.setMessagesInstruction(takePictureInstructions: String, secondsDisplayingInstructions: Int)
```
or
```java
Netki.INSTANCE.setMessagesInstruction(takePictureInstructions: String, retakePictureInstructions: String, secondsDisplayingInstructions: Int)
```

Where:

- takePictureInstructions: custom message for the first attempt to try to take the picture
- retakePictureInstructions: custom message for when the user is retrying to take the picture
- secondsDisplayingInstructions: time that the instructions will be displayed before showing the normal messages. The limit that the message will be displayed 10 seconds before enabling the manual capture

## Netki Internal Objects


Below are some of the Netki defined business and application objects. These data structures will be used for parameters and also for return objects.

These variables will be used as parameters in other areas of the application.

There are 4 types of documents supported currently in the Netki. Types of ID1 and ID3 standardized international documents. Basically a card and passport standard.


```java
// com.netki.netkisdk.v2.model.DocumentType options
enum class DocumentType {
        DRIVERS_LICENSE,
        PASSPORT,
        GOVERNMENT_ID;
}
```

In addition to the DOCUMENT TYPE there is an IMAGE TYPE which may associate 1 or more objects to that document type. For instance a driver's license has a FRONT and BACK.

Each document has an associated IMAGE ORIENTATION or TYPE that indicates if it is a stand alone or part of another document class. Documents of type ID3 will have a FRONT and BACK as expected.

The face image is also a document labeled as SELFIE. That is the only biometric human data.

```java
// com.netki.netkisdk.v2.model.DocumentSide
enum class DocumentSide {
        FRONT,
        BACK,
        SELFIE
}
```


## Sample Application


## Callbacks

For More Information Regarding Callbacks please see our [Callback Best Practices](./best_practices_internal_callbacks.md) first.

Once you are logged into your dashboard you will find documentation related to what data exists inside those callbacks so that you can map to fields on your side.
