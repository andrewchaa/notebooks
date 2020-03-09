---
description: Build native mobile apps using JavaScript and React
---

# React Native

## Getting Started

### Installing dependencies

#### Node, Watchman

```bash
brew install node
brew install watchman
```

#### The React Native CLI & Expo CLI

```bash
npm install -g react-native-cli
npm install -g expo-cli
```

#### Xcode

Command Line Tools

![](.gitbook/assets/image%20%2829%29.png)

### Creating and running a new application

```bash
react-native init AwesomeProject
cd AwesomeProject
react-native run-ios

// or
expo init AwesomeProject
cd AwesomeProject/ios
pod install 

cd AwesomeProject
npm start
```

![](.gitbook/assets/image%20%2814%29.png)



## Styling

### Height and Width

#### Flex Dimensions

Use `flex` in a component's style to have the component expand and shrink dynamically based on available space. Normally you will use `flex: 1`, which tells a component to fill all available space, shared evenly amongst other components with the same parent. The larger the `flex` given, the higher the ratio of space a component will take compared to its siblings.



![](.gitbook/assets/image%20%2812%29.png)

```javascript
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

export default class FlexDimensionsBasics extends Component {
  render() {
    return (
      // Try removing the `flex: 1` on the parent View.
      // The parent will not have dimensions, so the children can't expand.
      // What if you add `height: 300` instead of `flex: 1`?
      <View style={{flex: 1}}>
        <View style={{flex: 1, backgroundColor: 'powderblue'}} />
        <View style={{flex: 2, backgroundColor: 'skyblue'}} />
        <View style={{flex: 3, backgroundColor: 'steelblue'}} />
      </View>
    );
  }
}
```

#### Declaring stylesheets in a separate file

Be sure to name the file in .js extension. It's handy when you want to have the same styles across elements

```css
import { StyleSheet } from 'react-native'

const headerStyle = StyleSheet.create({
  primary: {
    backgroundColor: '#f8fbff',
    height: 70,
    elevation: 0,
    shadowOpacity: 0
  }
})

export { styles, headerStyle }
```

Then you can import and use it

```javascript
import { headerStyle } from '../components/styles';

static navigationOptions = ({ navigation }) => {
  return {
    headerStyle: headerStyle.primary,
    headerTitle: (
      <HeaderLogoTitle>Registration List</HeaderLogoTitle>
    ),
    headerRight: (
      <IconIon.Button
        name="ios-add-circle"
        backgroundColor='transparent'
        style={{marginRight: 0}}
        iconStyle={{color:'#F5740E', fontSize: 35}}
        onPress={() => navigation.navigate('Form')}
      />
    ),
  };
};

```

#### Flex box

* flexDirection: column. default option. It renders child components in the column, vertically.
* flexDirection: row. renders child components in the same row.

#### Round shape view

```javascript
<View style={{
    marginTop: 15,
    marginBottom: 10,
    width: 52,
    height: 52,
    borderRadius: 150/2,
    backgroundColor: '#F5740E',
    alignItems: 'center',
    justifyContent: 'center',
  }}>
  <Text style={{
    fontFamily: 'Segoe UI',
    fontSize: 20,
    fontWeight: '300',
    color: '#FFFFFF'
  }}>+</Text>
</View>

```

## Navigation

#### Navigating to a new screen

```jsx
<Button
  title="Go to Details"
  onPress={() => this.props.navigation.navigate('Details')}
  />
```

#### Going back

```jsx
<Button
 title="Go back"
 onPress={() => this.props.navigation.goBack()}
/>
```

#### Passing parameters to routes

```jsx
<Button
  title="Go to Details"
  onPress={() => {
    /* 1. Navigate to the Details route with params */
    this.props.navigation.navigate('Details', {
      itemId: 86,
      otherParam: 'anything you want here',
    });
  }}
/>

class DetailsScreen extends React.Component {
  render() {
    /* 2. Get the param, provide a fallback value if not available */
    const { navigation } = this.props;
    const itemId = navigation.getParam('itemId', 'NO-ID');
    const otherParam = navigation.getParam('otherParam', 'some default value');
```

#### Opening a full screen modal

Use a stack with mode: 'modal', headerMode: 'none'

```javascript
const AddNewStack = createStackNavigator(
  { New: BoilerRegistrationForm}, 
  { headerLayoutPreset: 'center' }
);
const ModalStack = createStackNavigator(
  { Form: AddNewStack, Scan: ScanBoilerBarcode }, 
  { headerMode: 'none', mode: 'modal' }
);

const RootStack = createBottomTabNavigator({
  MyList: RegistrationStack,
  Home: HomeStack,
  New: ModalStack,
  Account: AccountStack,
  Settings: SettingStack
},
```

### Navigate in redux saga

```bash
// Navigation Service
const config = {};

export function setNavigator(nav) {
  if (nav) {
    config.navigator = nav;
  }
}

export function navigate(routeName, params) {
  if (config.navigator && routeName) {
    config.navigator.navigate(routeName)
  }
}

// Screen
render() {
  const { navigation, photo } = this.props;
  NavigationService.setNavigator(navigation)

// Saga
var usePhoto = yield showAlert()
if (usePhoto) {
  yield put(setPhotoSelected(true))
  yield NavigationService.navigate({ routeName :'Form' })
} else {
  yield put(setSnapPreview(false))
  yield NavigationService.navigate({ routeName: 'Snap' })
}

```

## Using Components

### React Native Cross-Platform WebView

{% embed url="https://github.com/react-native-community/react-native-webview" %}

```text
npm i --save react-native-webview
react-native link react-native-webview
```

#### Showing indicator while loading

```javascript
render() {
  return (
    <WebView 
      source={{ uri: 'https://navienuk.com/news/' }}
      startInLoadingState={true}
      renderLoading={() => {
        return (
        <View style={{flex: 1, padding: 20}}>
          <ActivityIndicator/>
        </View>
        )
      }}
      style={{  }}
    />
  );
}

```

#### Scroll up when the keyboard hides inputs

use [https://github.com/APSL/react-native-keyboard-aware-scroll-view](https://github.com/APSL/react-native-keyboard-aware-scroll-view)



## Debugging

### On Chrome Browser

Use "Pause On Caught Exceptions" on Source tab

![](.gitbook/assets/image%20%2823%29.png)

### On VS Code

* Install React Native Tools on VS Code
* add to .vscode/launch.json, 
* on launch.json, click "Add Configuration" button.
* Add relevant debug settings

```javascript
{
    "name": "Debug in Exponent",
    "cwd": "${workspaceFolder}",
    "type": "reactnative",
    "request": "launch",
    "platform": "exponent"
  },
  {
    "name": "Debug Android",
    "cwd": "${workspaceFolder}",
    "type": "reactnative",
    "request": "launch",
    "platform": "android"
  },
  {
    "name": "Debug iOS",
    "cwd": "${workspaceFolder}",
    "type": "reactnative",
    "request": "launch",
    "platform": "ios"
  },
  {
    "name": "Attach to packager",
    "cwd": "${workspaceFolder}",
    "type": "reactnative",
    "request": "attach"
  }

```

### Install standalone react native debugger

{% embed url="https://github.com/jhen0409/react-native-debugger" %}

* Install the latest version

```bash
brew update && brew cask install react-native-debugger
```

* Make sure all debugger clients of React Native are closed, usually are `http://localhost:<port>/debugger-ui`
* Make sure RNDebugger is open and wait state.
* RNDebugger will try connect to debugger proxy, use port `8081` by default, you can create a new debugger window \(macOS: `Command+T`, Linux/Windows: `Ctrl+T`\) to specify the port if you want.
* Enable `Debug JS Remotely` of [developer menu](https://facebook.github.io/react-native/docs/debugging.html#accessing-the-in-app-developer-menu) on your app

## iOS Simulator

#### Suddenly starts running very slow

Check at the menu clicking on Debug -&gt; Toggle Slow Animations. Chances are you accidentally toggled it on.

#### See the list of all available simulators

```bash
xcrun simctl list
```

#### All simulator are missing and unavailable

Simply restart your MacBook

## Platform Specific Code

#### Check platform

```javascript
import {Platform, StyleSheet} from 'react-native';

const styles = StyleSheet.create({
  height: Platform.OS === 'ios' ? 200 : 100,
});
```

## Troubleshooting

#### .xcodeproj is damaged

Remove ios/ folder and regenerated the project

```text
rm -rf ios/
react-native eject // to generate the project as placeholder
react-native link // to link all packages again
react-native run-ios
```

#### library not found for -lRNCPushNotificationIOS

Select push notification as build target. Sync target ios version with app's ios version. In this case, push notification had 12.0 as ios version and the app had 9.0. 

#### Apple will stop accepting submissions of apps that use UIWebView APIs

The latest react-native-webview uses webkit, not UIWebView. Make sure you unlink it first so that it doesn't reference old webview module.

```bash
react-native unlink react-native-webview
react-native link react-native-webview

```

#### Multiple commands produce \*.ttf

The problem seems to be caused by the new autolinking feature in React Native 0.60 - the line `use_native_modules!` in `ios/Podfile` means when you do `pod install`, any pods found in `node_modules` are automatically linked. This means that links to all font files are added to `[CP] Copy Pods Resources` when you do `pod install`.

* Go to project / target / navien / Build Phase
* Remove duplicate reference fonts in "Copy Bundle Resources" NOT in "\[CP\] Copy Pods Resources" on 

#### Unrecognised font family on iOS

Open the workspace in Xcode and add the font to Resources

