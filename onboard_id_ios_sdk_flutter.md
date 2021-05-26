# Netki OnboardID iOS SDK - Flutter Integration


Netki OnboardID SDK Flutter Integration Instructions

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)



## Overview

![flutter.dev](images/flutter.png) 

The NetkiSDK has been designed to work with many installation strategies.  We have the ability to offer a `.framework` file or access to our private `pod` repository for an easy installation into your iOS [Flutter](https://flutter.dev/) Application  


## Installation

The installation is relatively straight forward.  Below is our recommended format for integration. 

1. Install flutter sdk: https://flutter.dev/docs/get-started/install/macos 
2. Create flutter app 
3. cd to ios workspace directory ({flutter_app_dir} -> ios)
4. Create podfile (pod init)
5. Add NetkiSdk pod :  `>> pod 'NetkiSDK', :git => 'https://YOURUSERNAME@bitbucket.org/eimai/ios-pod.git'`
6. `pod install` 
7. open main.dart from flutter lib
8. create flutter method to call method from ios project (main.dart -> launchKYC)
9. create similar method in iOS project (AppDelegate -> launchKYCChannel)
10. `flutter run`

