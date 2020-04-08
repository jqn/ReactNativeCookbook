---
description: React Native Mobx integration
---

# Mobx

Install dependencies

```text
$ npm install mobx mobx-react
```

Mobx uses decorators so we have to make a few configuration changes because React Native doesn't support them natively.

```text
$ npm install @babel/plugin-proposal-decorators
```

Modify our babel.config.js

```text
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [['@babel/plugin-proposal-decorators', {legacy: true}]],
};
```

{% embed url="https://www.youtube.com/watch?v=Hy0Xg6p1VB4" %}

```jsx
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 *
 * @format
 * @flow
 */

import React from 'react';
import {SafeAreaView, StatusBar} from 'react-native';
import Main from './src/containers/Main/Main';
import {Provider} from 'mobx-react';
import AppStore from './src/store';

const store = (window.store = new AppStore());

const App = () => {
  return (
    <>
      <Provider store={store}>
        <StatusBar barStyle="dark-content" />
        <SafeAreaView style={{flex: 1}}>
          <Main />
        </SafeAreaView>
      </Provider>
    </>
  );
};

export default App;

```

```jsx
import {observable, action} from 'mobx';

class AppStore {
  @observable title = 'Hello world';
  @action onChangeText = val => {
    this.title = val;
  };
}

export default AppStore;
```

```jsx
import React, {Component} from 'react';
import {View, Text, TextInput} from 'react-native';
import {inject, observer} from 'mobx-react';

@inject('store')
@observer
class Main extends Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <View>
        <Text>{this.props.store.title} </Text>
        <TextInput
          style={{height: 40, borderColor: 'gray', borderWidth: 1}}
          onChangeText={this.props.store.onChangeText}
        />
      </View>
    );
  }
}

export default Main;

```

