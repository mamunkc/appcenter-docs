---
title: Get Started with iOS
description: Get started
keywords: sdk
author: elamalani
ms.author: emalani
ms.date: 06/05/2019
ms.topic: get-started-article
ms.assetid: 513247e0-9a7e-4f7a-b212-43fd32474900
ms.service: vs-appcenter
ms.custom: sdk
ms.tgt_pltfrm: ios
dev_langs:  
 - swift
 - objc
---

# Get Started with iOS

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [React Native](react-native.md)
> * [UWP](uwp.md)
> * [Xamarin](xamarin.md)
> * [Unity](unity.md)
> * [macOS](macos.md)
> * [Cordova](cordova.md)

The App Center SDK uses a modular architecture so you can use any or all of the services.

Let's get started with setting up App Center iOS SDK in your app to use App Center Analytics and App Center Crashes. To add App Center Distribute to your app, look at the documentation for [App Center Distribute](~/sdk/distribute/ios.md).

## 1. Prerequisites

The following requirements must be met to use App Center SDK:

* Your iOS project is set up in Xcode 10 or later on macOS version 10.12 or later.
* You are targeting devices running on iOS 9.0 or later.
* You are not using any other library that provides Crash Reporting functionality (only for App Center Crashes).

## 2. Create your app in the App Center Portal to obtain the App Secret

If you have already created your app in the App Center portal, you can skip this step.

1. Head over to [appcenter.ms](https://appcenter.ms).
2. Sign up or log in and hit the blue button on the top right corner of the portal that says **Add new** and select **Add new app** from the dropdown menu.
3. Enter a name and an optional description for your app.
4. Select **iOS** as the OS and **Objective-C/Swift** as a platform.
5. Hit the button at the bottom right that says **Add new app**.

Once you have created an app, you can obtain its **App Secret** on the **Settings** page on the App Center Portal. At the top right-hand corner of the **Settings** page, click on the **triple vertical dots** and select `Copy app secret` to get your App Secret.

## 3. Add the App Center SDK modules

The App Center SDK for iOS can be integrated into your app via [Cocoapods](https://cocoapods.org) or by manually adding the binaries to your project.

### 3.1  Integration via Cocoapods

1. Add the following dependencies to your `podfile` to include App Center Analytics and App Center Crashes into your app. This will pull in the following frameworks: **AppCenter**, **AppCenterAnalytics**, and **AppCenterCrashes**. Alternatively, you can specify which services you want to use in your app. Each service has its own subspec and they all rely on AppCenter. It will get pulled in automatically.

	```ruby
	# Use the following line to use App Center Analytics and Crashes.
	pod 'AppCenter'

	# Use the following lines if you want to specify which service you want to use.
	pod 'AppCenter/Analytics'
	pod 'AppCenter/Crashes'
	```

2. Run `pod install` to install your newly defined pod and open the project's **.xcworkspace**.

> [!NOTE]
> If you see an error like ```[!] Unable to find a specification for `AppCenter` ```
>  while running `pod install`, please run `pod repo update` to get the latest pods from the Cocoapods repository and then run `pod install`.

Now that you've integrated the frameworks in your application, it's time to start the SDK and make use of the App Center services.

### 3.2 Integration by copying the binaries into your project
Below are the steps on how to integrate the compiled binaries in your Xcode project to set up App Center Analytics and App Center Crashes for your iOS app.

1. Download the [App Center SDK](https://github.com/Microsoft/AppCenter-SDK-Apple/releases) frameworks provided as a zip file.

2. Unzip the file and you will see a folder called **AppCenter-SDK-Apple** that contains different frameworks for each App Center service on each platform folder. The framework called `AppCenter` is required in the project as it contains code that is shared between the different modules.

3. [Optional] Create a subdirectory for 3rd-party libraries.
   * As a best practice, 3rd-party libraries usually reside inside a subdirectory, often called **Vendor**. If the project isn't organized with a subdirectory for libraries, create a **Vendor** subdirectory now.
   * Create a group called **Vendor** inside your Xcode project to mimic your file structure on disk.

4. Open the unzipped **AppCenter-SDK-Apple** folder in Finder and copy the folder into your project's folder at the location where you want it to reside. The folder contains frameworks in subfolders for other platforms that App Center SDK supports, so you might need to delete subfolders that you don't need.

5. Add the SDK frameworks to the project in Xcode:
   * Make sure the Project Navigator is visible (⌘+1).
   * Now drag and drop **AppCenter.framework**, **AppCenterAnalytics.framework** and **AppCenterCrashes.framework** from the Finder (in the location from the previous step) into Xcode's Project Navigator. The **AppCenter.framework** is required to start the SDK. If it is not added to the project, the other modules won't work and your app won't compile.
   * A dialog will appear, make sure your app target is checked. Then click **Finish**.

Now that you've integrated the frameworks in your application, it's time to start the SDK and make use of the App Center services.

## 4. Start the SDK

In order to use App Center, you must opt in to the module(s) that you want to use. By default no modules are started and you will have to explicitly call each of them when starting the SDK.

### 4.1 Add the import statements

Open the project's **AppDelegate** file and add the following import statements:

```objc
@import AppCenter;
@import AppCenterAnalytics;
@import AppCenterCrashes;
```
```swift
import AppCenter
import AppCenterAnalytics
import AppCenterCrashes
```

### 4.2 Add the `start:withServices:` method

Insert the following line to start the SDK in the project's **AppDelegate** class in the `didFinishLaunchingWithOptions` method.
```objc
[MSAppCenter start:@"{Your App Secret}" withServices:@[[MSAnalytics class], [MSCrashes class]]];
```
```swift
MSAppCenter.start("{Your App Secret}", withServices: [MSAnalytics.self, MSCrashes.self])
```

### 4.3 Replace the placeholder with your App Secret

Make sure to replace `{Your App Secret}` text with the actual value for your application. The App Secret can be found on the **Getting Started** page or **Settings** page on the App Center portal.

The Getting Started page contains the above code sample with your App Secret in it, you can just copy-paste the whole sample.

The example above shows how to use the `start:withServices` method and include both App Center Analytics and App Center Crashes.

If you do not want to use one of the two services, remove the corresponding parameter from the method call above.

Unless you explicitly specify each module as a parameter in the start method, you can't use that App Center service. In addition, the `start:withServices` API can be used only once in the lifecycle of your app – all other calls will log a warning to the console and only the modules included in the first call will be available.

For example - If you just want to onboard to App Center Analytics, you should modify the `start:withServices` API call as follows:

```objc
[MSAppCenter start:@"{Your App Secret}" withServices:@[[MSAnalytics class]]];
```
```swift
MSAppCenter.start("{Your App Secret}", withServices: [MSAnalytics.self])
```

Great, you are all set to visualize Analytics and Crashes data on the portal that the SDK collects automatically.

Look at the documentation for [App Center Analytics](~/sdk/analytics/ios.md) and [App Center Crashes](~/sdk/crashes/ios.md) to learn how to customize and use more advanced functionalities of both services.

To learn how to get started with in-app updates, read the documentation of [App Center Distribute](~/sdk/distribute/ios.md).

To learn how to get started with Push, read the documentation of [App Center Push](~/sdk/push/ios.md).
