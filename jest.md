---
description: Jest is a delightful JavaScript Testing Framework with a focus on simplicity
---

# Jest

## Getting started

### CLI

```text
npm i -g jest
npm i --save-dev jest

jest    // to run tests
jest -u // to update snapshot
```

### Set up

```javascript
// package.json
"scripts": {
  "test": "jest"
},
"jest": {
  "preset": "react-native"
}
```

```text
npm test
```

## Snapshot testing

```javascript
/**
 * @format
 */

import 'react-native';
import React from 'react';
import renderer from 'react-test-renderer';
import moment from 'moment'

import InstallationDate from '../../src/components/InstallationDate';


it('renders the component correctly', () => {
  const tree = renderer.create(
      <InstallationDate item={{
        installationDate: moment().format('DD/MM/YYYY')
      }}
      />
  ).toJSON()

  expect(tree).toMatchSnapshot()
});
```

#### package.json

```javascript
"jest": {
  "preset": "react-native",
  "transformIgnorePatterns": [
    "node_modules/(?!(react-native|aws-amplify-react-native|react-native-elements|react-native-vector-icons|react-native-linear-gradient|react-native-device-info)/)"
  ],
  "setupFiles": [
    "./testenv.js"
  ]
}
```

## Mocking

#### Verify if it's called

```javascript
jest.mock('aws-amplify')
import { Auth } from 'aws-amplify'

it('should sign out the user if a user clicks SignOut', () => {
  wrapper.find('[data-action="signOut"]').simulate('click')
  expect(Auth.signOut).toHaveBeenCalledTimes(1)
})
```

### Mock return value

```javascript
jest.mock('react-native-device-info', () => {
  return {
    getModel: jest.fn(),
    getUniqueId: jest.fn(),
    getDeviceId: jest.fn(),
    getSystemName: jest.fn(x => 'jest test'),
    getSystemVersion: jest.fn(x => '1.6.9'),
    hasNotch: jest.fn(x => true),
    getBrand: jest.fn(x => 'Apple'),
    getApplicationName: jest.fn(x => 'Navien Installer'),
    getBundleId: jest.fn(),
    getVersion: jest.fn(),
    getBuildNumber: jest.fn()
  };
});
```

