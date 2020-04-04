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

### Coding time

Let's start by creating our directory structure. Once you are done creating the directories your project should look like this.

```objectivec
├── ReactNativeCameraBoilerplate
       ├── src
       │   ├── containers
       │   ├── components
       │   ├── theme
       └── index.js
```

Now create a **camera container** component and a root container component

```objectivec
$ touch src/containers/CameraScreen.js
$ touch src/containers/RootContainer.js
```

Create a **camera component**. React Native allows to write really modular code with components as the driving force allowing you to reuse code easily.

```objectivec
$ mkdir src/components/Camera
$ touch src/components/Camera/Camera.js
```

Edit the **Camera.js** component. The finished result should look similar to this, a functional component that can be imported into any screens that need it.

```objectivec
// src/component/Camera/Camera.js
import React from 'react';
import {StyleSheet} from 'react-native';
import {RNCamera} from 'react-native-camera';

const Camera = ({children, cameraRef, whiteBalanceMode, flashMode, zoom}) => {
  return (
    <RNCamera
      ref={cameraRef}
      style={styles.container}
      flashMode={flashMode}
      whiteBalance={whiteBalanceMode}
      captureAudio={false}
      zoom={zoom}>
      {children}
    </RNCamera>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});

export default Camera;
```

### Install prop-types

The[ prop-types](https://github.com/facebook/prop-types) library will help us reduce errors once we deploy this app to the app/play store. We get runtime type checking for React props. React will check props passed to our camera component against   the prop type definitions set, and warn in development if they don't match.

```objectivec
$ npm install prop-types
```

Once install update Camera.js to look like the following. Notice that we are also setting the default props which are the default unless the parent overrides them.

```objectivec
// src/component/Camera/Camera.js
import React from 'react';
import {StyleSheet} from 'react-native';
import {RNCamera} from 'react-native-camera';
import PropTypes from 'prop-types';

const Camera = ({children, cameraRef, whiteBalanceMode, flashMode, zoom}) => {
  return (
    <RNCamera
      ref={cameraRef}
      style={styles.container}
      flashMode={flashMode}
      whiteBalance={whiteBalanceMode}
      captureAudio={false}
      zoom={zoom}>
      {children}
    </RNCamera>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});

Camera.defaultProps = {
  children: null,
  cameraRef: () => null,
  whiteBalanceMode: RNCamera.Constants.WhiteBalance.auto,
  flashMode: RNCamera.Constants.FlashMode.auto,
  zoom: 0,
};

Camera.propTypes = {
  children: PropTypes.node,
  cameraRef: PropTypes.fuc,
  whiteBalanceMode: PropTypes.string,
  flashMode: PropTypes.string,
  zoom: PropTypes.number,
};

export default Camera;

```

Edit the camera container and import our camera component

```objectivec
// src/containers/CameraScreen.js
import React, {Component} from 'react';
import {View, Text} from 'react-native';

import Camera from '../components/Camera/Camera';

export default class CameraScreen extends Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <View>
        <Camera />
      </View>
    );
  }
}
```

### Build the camera layout

Install **@nartc/react-native-barcode-mask** and ****it's dependency **react-native-reanimated** to get us a nice viewfinder so we don't have to build one from scratch. Also import SafeAreaView from React Native so we can handle iPhoneX notches.

```objectivec
$ npm install @nartc/react-native-barcode-mask
$ npm install react-native-reanimated
$ cd ios && pod install && cd ..
```

Here is the updated code. Also don't forget to recompile your app after linking native modules.

```objectivec
// src/containers/CameraScreen.js
import React, {Component} from 'react';
import {SafeAreaView, StyleSheet} from 'react-native';

import Camera from '../components/Camera/Camera';
import {BarcodeMask} from '@nartc/react-native-barcode-mask';

export default class CameraScreen extends Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <SafeAreaView style={styles.container}>
        <Camera>
          <BarcodeMask />
        </Camera>
      </SafeAreaView>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
});

```

### Display a camera in our app

Start by editing **RootContainer.js** and import **CameraScreen.js**

```objectivec
// src/containers/RootContainer.js
import React from 'react';
import CameraScreen from './CameraScreen';

const RootComponent = () => <CameraScreen />;

export default RootComponent;
```

Now let's update **App.js** to display the camera. Start by removing everything already there and replace it with the following.

```objectivec
/**
 * ReactNativeCameraBoilerplate
 * https://github.com/jqn/reactNativeCameraBoilerplate
 * App.js
 */

import React from 'react';
import {StatusBar} from 'react-native';
import RootContainer from './src/containers/RootContainer';

const App = () => {
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <RootContainer />
    </>
  );
};

export default App;

```

### Build nicer error components for the camera



