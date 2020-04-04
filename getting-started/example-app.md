---
description: Building a Camera App
---

# Example App

### Create a new application

Run the following commands from your terminal

```bash
$ npx react-native init ReactNativeCamera
$ cd ReactNativeCamera
$ npx react-native start
$ npx react-native run-ios
```

### Install and link dependencies

[React Native Camera](https://react-native-community.github.io/react-native-camera/docs/installation)

```bash
$ npm install react-native-camera --save
$ cd ios && pod install && cd ..
```

### iOS required steps

Add permissions with usage descriptions to your app's Info.plist

`ios/ReactNativeCameraBoilerplage/Info.plis`

```objectivec
<!-- Required with iOS 10 and higher -->
<key>NSCameraUsageDescription</key>
<string>Your message to user when the camera is accessed for the first time</string>

<!-- Required with iOS 11 and higher: include this only if you are planning to use the camera roll -->
<key>NSPhotoLibraryAddUsageDescription</key>
<string>Your message to user when the photo library is accessed for the first time</string>

<!-- Include this only if you are planning to use the camera roll -->
<key>NSPhotoLibraryUsageDescription</key>
<string>Your message to user when the photo library is accessed for the first time</string>

<!-- Include this only if you are planning to use the microphone for video recording -->
<key>NSMicrophoneUsageDescription</key>
<string>Your message to user when the microphone is accessed for the first time</string>
```

### Android required steps

Add permissions to your AndroidManifest.xml file

`android/app/src/main/AndroidManifest.xml`

```objectivec
<!-- Required -->
<uses-permission android:name="android.permission.CAMERA" />

<!-- Include this only if you are planning to use the camera roll -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

<!-- Include this only if you are planning to use the microphone for video recording -->
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
```

{% hint style="info" %}
Make sure to shut down your app and run it again so the changes are applied
{% endhint %}



