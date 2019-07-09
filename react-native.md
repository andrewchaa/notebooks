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

#### The React Native CLI

```bash
npm install -g react-native-cli
```

#### Xcode

Command Line Tools

![](.gitbook/assets/image%20%284%29.png)

### Creating and running a new application

```bash
react-native init AwesomeProject
cd AwesomeProject
react-native run-ios
```

![](.gitbook/assets/image%20%285%29.png)



## The Basics

### Height and Width

#### Flex Dimensions

Use `flex` in a component's style to have the component expand and shrink dynamically based on available space. Normally you will use `flex: 1`, which tells a component to fill all available space, shared evenly amongst other components with the same parent. The larger the `flex` given, the higher the ratio of space a component will take compared to its siblings.



![](.gitbook/assets/image%20%283%29.png)

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

