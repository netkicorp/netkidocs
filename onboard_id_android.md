# Netki OnboardID SDK - Android


## OnboardID SDK

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Example Usage](#example-usage)
- [Getting Started](#getting-started)
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



## Sample Application


## Callbacks

For More Information Regarding Callbacks please see our [Callback Best Practices](./best_practices_internal_callbacks.md) first.

Once you are logged into your dashboard you will find documentation related to what data exists inside those callbacks so that you can map to fields on your side.
