# Netki OnboardID Themed SDK - React Native

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

```js
// Colors
PrimaryButtonBackgroundColor
SecondaryButtonBackgroundColor
PrimaryButtonTextColor
SecondaryButtonTextColor
InstructionsTitleTextColor
SuccessColor
ErrorColor


// Appearance
ButtonsCornerRadius

// Text
TitleForDriverLicenseInstructionsFront
TitleForDriverLicenseInstructionsBack
TitleForPassportInstructions
TitleForPassportLastPageInstructions
TitleForSelfieInstructions
TitleForGovermentIdInstructionsFront
TitleForGovermentIdInstructionsBack


// Instruction images
DriverLicenseFrontInstructionBackgroundImage
DriverLicenseBackInstructionBackgroundImage
PassportInstructionBackgroundImage
PassportLastPageInstructionBackgroundImage
GovermentIdFrontInstructionBackgroundImage
GovermentIdBackInstructionBackgroundImage
SelfieInstructionBackgroundImage

```

## Customizing elements

For each of the customizable elements, the SDK has a setter method that can be used to override the default value.  

Those setters are exposed through

```js
netkiSDK.set
```

To customize the different elements use the methods:

```js
// Colors
// @param hexString(String): new color in hex format, for example "32a852"
netkiSDK.setCOLOR_TO_OVERRIDE(hexString);

```

```js
// Appearance
// @param cornerRadius(float): the radius for the corner of the buttons, for example 15.0
netkiSDK.setButtonsCornerRadius(cornerRadius);
```

```js
// Text
// @param text(String): to override the instructions in the specific page, for example "Remove sunglasses and mask"
netkiSDK.setTEXT_TO_OVERRIDE(text);
```

```js
// Instruction images
// @param path(String): of the image/video to display, support local files or urls, for example "https://test.com/image1234.jpg"
netkiSDK.setINSTRUCTION_RESOURCE_TO_OVERRIDE(resolveAssetSource(path));
```

## Examples

### Front Identification Instructions Page

<img src="./images/android_font_instructionsId_page.png" alt="front instruction screen" width="350px" align="center" />

To customize this screen we can override the elements:

1 - TitleForDriverLicenseInstructionsFront, InstructionsTitleTextColor  
2 - DriverLicenseFrontInstructionBackgroundImage  
3 - SecondaryButtonBackgroundColor, SecondaryButtonTextColor  
4 - PrimaryButtonBackgroundColor, PrimaryButtonTextColor  

After overriding these values the screen would look like this:

<img src="./images/android_full_customized_front_instructions.png" alt="front instruction screen" width="350px" />

### Image Review Screen

The review screens are controlled by the same theme settings as above. The primary and secondary colors will
control these features.

1 - SecondaryButtonBackgroundColor, SecondaryButtonTextColor  
2 - PrimaryButtonBackgroundColor, PrimaryButtonTextColor  

<img src="./images/android_color_review_button_2.png" alt="android image capture changed" width="350px" />

## Buttons corner radius

When you set the corner radius it will modify the buttons as shown below:

![4_rev](https://user-images.githubusercontent.com/15677171/117327676-7cdac800-ae58-11eb-96ce-949fe7f9b024.jpg)
![32_rev](https://user-images.githubusercontent.com/15677171/117327677-7cdac800-ae58-11eb-83ba-9d087949c4dd.jpg)
