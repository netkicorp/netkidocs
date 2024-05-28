# Netki OnboardID Themed SDK - Android/iOS

Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Elements to customize](#elements-to-customize)
- [Customizing elements](#customizing-elements)
- [Examples](#examples)

## Overview

The NetkiSDK provides the ability for the clients implementing it to configure and personalize some UI elements to make
it fit better with their application and show a more natural transition between their app and the SDK.
This document shows what elements can be customized and the way they are customized.

## Elements to customize

The list of elements that can be customized are:

```kotlin
// Colors
PrimaryButtonColor
PrimaryButtonTextColor
SecondaryButtonColor
SecondaryButtonTextColor
InstructionsTextColor

// Appearance
ButtonsCornerRadius

// Text
DriverLicenseInstructionsFrontText
DriverLicenseInstructionsBackText
GovernmentIdInstructionsFrontText
GovernmentIdInstructionsBackText
PassportInstructionsText
SelfieInstructionsText

// Instruction images
DriverLicenseInstructionsFrontImagePath
DriverLicenseInstructionsBackImagePath
GovernmentIdInstructionsFrontImagePath
GovernmentIdInstructionsBackImagePath
PassportInstructionsImagePath
SelfieInstructionsImagePath
DriverLicenseInstructionsFrontVideoPath
DriverLicenseInstructionsBackVideoPath
GovernmentIdInstructionsFrontVideoPath
GovernmentIdInstructionsBackVideoPath
PassportInstructionsVideoPath
SelfieInstructionsVideoPath
```

## Customizing elements

For each of the customizable elements, the SDK has a setter method that can be used to override the default value.  

Those setters are exposed through

```kotlin
OnBoardId.UiCustomization
```

To customize the different elements use the methods:

```kotlin
// Colors
// @param hexString(String): new color in hex format, for example "#FF4890C0"
OnboardId.UiCustomization.setCOLOR_TO_OVERRIDE(hexString) 
```

```kotlin
// Appearance
// @param cornerRadius(float): the radius for the corner of the buttons, for example 32f
OnboardId.UiCustomization.setButtonsCornerRadius(cornerRadius)
```

```kotlin
// Text
// @param text(String): to override the instructions in the specific page, for example "Remove sunglasses and mask"
OnboardId.UiCustomization.setTEXT_TO_OVERRIDE(text) 
```

```kotlin
// Instruction images
// @param path(String): of the image/video to display, support local files or urls, for example "https://test.com/image1234.jpg"
OnboardId.UiCustomization.setINSTRUCTION_RESOURCE_TO_OVERRIDE(path)
```

## Examples

### Front Identification Instructions Page

<img src="./images/android_font_instructionsId_page.png" alt="front instruction screen" width="350px" align="center" />

To customize this screen we can override the elements:

1 - DriverLicenseInstructionsFrontText, InstructionsTextColor  
2 - DriverLicenseInstructionsFrontImagePath  
3 - SecondaryButtonColor, SecondaryButtonTextColor  
4 - PrimaryButtonColor, PrimaryButtonTextColor  


After overriding this values the screen would look like this:


<img src="./images/android_full_customized_front_instructions.png" alt="front instruction screen" width="350px" />

### Image Review Screen

The review screens are controlled by the same theme settings as above. The primary and secondary colors will
control these features.

1 - SecondaryButtonColor, SecondaryButtonTextColor  
2 - PrimaryButtonColor, PrimaryButtonTextColor  

<img src="./images/android_color_review_button_2.png" alt="android image capture changed" width="350px" />

## Buttons corner radius

When you set the corner radius it will modify the buttons as it shows below:

![4_rev](https://user-images.githubusercontent.com/15677171/117327676-7cdac800-ae58-11eb-96ce-949fe7f9b024.jpg)
![32_rev](https://user-images.githubusercontent.com/15677171/117327677-7cdac800-ae58-11eb-83ba-9d087949c4dd.jpg)

 
