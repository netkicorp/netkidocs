# Netki OnboardID SDK - Android


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Example Usage](#example-usage)
- [Getting Started](#getting-started)
- [Installation](#installation)
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

```
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

- Add NetkiSDK as dependency in the app/build.gradle file with the desired version

	// NetkiSDK
	api('com.netki:netkisdk:latest.version')

### Step 3

- Recompile the project.


### Note

In case you are not using the material design themes please add the following parameter to the AndroidManifest.xml in the application tag.

	tools:replace="android:theme"

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

## Advanced Usage




### Individual Screen Controllers


In some cases you will want to run individual screens. We offer the ability to delegate controllers on an ad-hoc basis.

Here we are instantiating and running the controller and telling it to run the capture and review sequence for ID type and FRONT.

### Camera controls invocation

The client has the option to invoke individual camera controls to take pictures of different documents and sides of the document.

The activity responsible for that invocation is: `CameraIdentificationActivity`

The way to invoke it is creating an Intent passing as parameter the DocumentType and the DocumentSide to initiate the camera and then start the activity expecting the result.


See the [Netki Internal Objects](#netki-internal-objects) for values in the enum objects.

```java
// Way to create the intent
CameraIdentificationActivity.createIntent(context: Context, documentType: DocumentType, documentSide: DocumentSide)
```

Example of invocation:

```java
startActivityForResult(
        CameraIdentificationActivity.createIntent(
                context,
                DocumentType.DRIVERS_LICENSE,
                DocumentSide.FRONT
        ), REQUEST_CAMERA_IDENTIFICATION
);
```


### Response Data

There are some cases in which you will want to access the data that was captured by the cameras. Things like the documents that were captured, the data out of the documents, the face coordinates, MRZ, etc.

**NOTE**: this response data is only available when calling the screens individually.  Otherwise the data goes to our server and the transaction concludes transparent. With greater power comes greater responsibility.  

Handling result

The activity will return a success or error response to the element invoking it.
The first step is to validate if the process was finished successfully, for that you need to verify the resultCode

Example:

// To validate the process was

correctresultCode == RESULT_OK

// To validate the process was

correctresultCode == RESULT_CANCELED

If the resultCode is RESULT_CANCELED you can access the information of the error with CameraIdentificationActivity.ERROR_INFORMATION_KEY

Example:

```java
if (resultCode == RESULT_CANCELED) {
        String error = data != null ? data.getStringExtra(CameraIdentificationActivity.ERROR_INFORMATION_KEY) : "";
        Log.e(TAG, error);
}
```

If the result is RESULT_OK it will return a list of: `com.netki.netkisdk.v2.model.Picture`

Each one of this picture will have the following structure:

```java
data class Picture(
        val path: String,
        val barcodes: List<Barcode> = mutableListOf(),
        val type: Picture.Type
) : Serializable {

        enum class Type {
                FRONT,
                BACK,
                SELFIE,
                FACE_EXTRACTED
        }
}
```

Where:
path: where the image was saved, this is an internal memory that is only accessible to the app, it is not accessible for external app or the user

barcodes: List of all the information extracted from the barcodes found in the identification, this could be empty

type: the type of picture returned



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

Example of success call:

```java
if (resultCode == RESULT_OK) {
        List<Picture> pictures = (List<Picture>) data.getSerializableExtra(CameraIdentificationActivity.PICTURES_KEY);
        for (Picture picture : pictures) {
                Log.e("MainActivity", "Image type: " + picture.getType());
                Log.e("MainActivity", "Path: " + picture.getPath());
        }
}
```


## Sample Application

## Reference



## Callbacks

For More Information Regarding Callbacks please see our [Callback Best Practices](./best_practices_internal_callbacks.md) first.

Once you are logged into your dashboard you will find documentation related to what data exists inside those callbacks so that you can map to fields on your side.
