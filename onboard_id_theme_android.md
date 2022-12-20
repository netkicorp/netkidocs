# Netki OnboardID Themed SDK - Android


Dramatically Reduce Onboarding Costs While Stopping Fraud.

## Table of Contents

- [Overview](#overview)
- [Front Identification Instructions Page](#front-identification-instructions-page)
- [Back Identification Instructions Page](#back-identification-instructions-page)
- [Front Passport Instructions Page](#front-passport-instructions-page)
- [Selfie Instructions Page](#selfie-instructions-page)
- [Image Capture Screen](#image-capture-screen)



## Overview

The NetkiSDK provides the ability for the clients implementing it to configure and personalize some of the UI elements to make it fit better with their application and show a more natural transition between their app and the SDK.
This document shows what elements can be customized and the way the they are customized.



The colors that all the elements follow are:

```html
<color name="netki_primary_color">#483090</color>
<color name="netki_accent_color">#4890C0</color>
<color name="netki_text_accent_elements_color">#FFFFFF</color>
<color name="netki_background_color">#FFFFFF</color>
<color name="netki_error_color">#B00020</color>
<color name="netki_success_color">#00a152</color>
<color name="netki_text_instructions_color">#FFFFFF</color>
<color name="netki_secondary_button_color">#FFFFFF</color>
<color name="netki_camera_button_color">#FFFFFF</color>
```

In order to change the color of the elements the client needs to override these previous colors with their own colors defined in their `colors.xml` file.  Below I show the screens and the correspondent color for each one.

## Front Identification Instructions Page


<img src="./images/android_font_instructionsId_page.png" alt="front instruction screen" width="350px" align="left" />

<br />

- **1 Instruction Text**

    **Key**: `netki_text_instructions_color`

<br />

- **2 Instructions Background Image**

This is a full image that the client can replace with their own. To replace it the client needs to add an image named `front_instructions_id.jpg`, in the correspondent `drawable` folder for each resolution.

The client can also add one single resource in the `drawable` folder. If this strategy is chosen the image should be a high resolution image so it can scale properly to all the resolutions.

* **3 Back Button**
    * Background color: `netki_secondary_button_color`
    * Text color: `netki_accent_color`
    * Continue Button:
    * Background color: `netki_accent_color`
    * Text color: `netki_text_accent_elements_color`

<br /><br /><br /><br /><br />

An example of a fully customized page for front instructions is:


<img src="./images/android_full_customized_front_instructions.png" alt="front instruction screen" width="350px" />

## Back Identification Instructions Page

Similar to the front instructional page we provide a way to set your own instructional page for the capture of the back image.

This is a full image that the client can replace with their own. To replace it the client needs to add an image named `back_instructions_id.jpg`, and place it in the correspondent `drawable` folder for each resolution. The client can also just add one single resource in the `drawable` folder. This image must be high resolution in the same way that it will be for the front.  

<img src="./images/android_back_instructions_id.png" alt="back instruction screen" width="350px" />


## Front Passport Instructions Page

This follows the same instructions as the previous 2 instruction pages.

Add an image named `front_instructions_passport.jpg` in the correspondent `drawable` folder for each resolution. The client can also just add one single resource in the `drawable` folder.

<img src="./images/android_passport_front_instructions.png" alt="back instruction screen" width="350px" />

## Selfie Instructions Page

This follows the same instructions as the previous 3 instruction pages.

Add an image named `selfie_instructions.jpg` in the correspondent `drawable` folder for each resolution. The client can also just add one single resource in the `drawable` folder.

<img src="./images/android_selfie_instructions.png" alt="back instruction screen" width="350px" />

## Text for Instructions page

If you want to set a custom text for any of the instruction pages, you can do it by defining the following properties in your strings file.

```
<resources>
    <string name="front_id_instruction">Your custom text for front here</string>
    <string name="back_id_instruction">Your custom text for back here</string>
    <string name="selfie_instruction">Your custom text for selfie here</string>
</resources>

```

Note: If it does not exist yet, you have to create a folder for the specific language that you want to overwrite, in example for English `values-en`, Spanish `values-es` and so on.

## Image Capture Screen

This is a dynamic screen so many of the features are not able to be changed.  Our machine learning computer vision algorithms are working on this page and guiding the user in a way that will facilitate a good capture of their ID.  

Below are the elements which can be altered

**Picture Capture Button**

- The color of the button is defined with: `netki_camera_button_color`
- The color of the icon is defined with: `netki_accent_color`


<img src="./images/android_image_capture1.png" alt="android image capture raw" width="350px" align="left" />
<img src="./images/android_image_capture2.png" alt="android image capture changed" width="350px" />


## Image Review Screen

The review screens are controlled by the same theme settings as above.  The button and accent button colors will control these features. 

<img src="./images/android_color_review_buttons.png" alt="android image capture raw" width="350px" align="left" />
<img src="./images/android_color_review_button_2.png" alt="android image capture changed" width="350px" />

## Buttons corner radius

To configure the corner radius for the buttons you can overwrite the property `netki_button_corner_radius`, by default this is set to value 4dp, you can set a value between 0dp and 32dp.

Example
`dimens.xml`
```
<resources>
    <dimen name="netki_button_corner_radius">32dp</dimen>
</resources>
```

### Instructions page
![4_inst](https://user-images.githubusercontent.com/15677171/117327572-5b79dc00-ae58-11eb-8c6d-c14245f999ec.jpg)
![32_inst](https://user-images.githubusercontent.com/15677171/117327576-5c127280-ae58-11eb-991f-b2b9a08c500f.jpg)

### Review page
![4_rev](https://user-images.githubusercontent.com/15677171/117327676-7cdac800-ae58-11eb-96ce-949fe7f9b024.jpg)
![32_rev](https://user-images.githubusercontent.com/15677171/117327677-7cdac800-ae58-11eb-83ba-9d087949c4dd.jpg)



 
