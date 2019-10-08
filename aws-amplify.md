---
description: >-
  An opinionated, category-based client framework for building scalable mobile
  and web apps
---

# AWS Amplify

## Getting Started

#### Install amplify

```bash
npm install -g @aws-amplify/cli
amplify configure
```

## React

#### Create a react app and install amplify

```bash
npm i --save @aws-amplify/api @aws-amplify/pubsub
npm i --save aws-amplify-react
```

#### Set Up Your Backend

Initialise the project on the cloud

```bash
amplify init
amplify status
```

#### Add API and Database

```bash
amplify add api
```

API

* select rest
* label for category: registrationsapi
* path: registrations

lambda

* lambda resource: navienlambda
* lambda function name: registrationslambda
* function template: CRUD

dynamodb

* 1st column: userId \(string\), partition key
* 2nd column: registrationId \(string\), sort key

#### Integrate into your app

```javascript
import API from '@aws-amplify/api'
import PubSub from '@aws-amplify/pubsub';
import config from './aws-exports'
API.configure(config)             // Configure Amplify
PubSub.configure(config);
```

#### Launch your App

```bash
amplify add hosting
amplify publish
```

#### Troubleshooting

If anything goes wrong, remove everything and create it again

```bash
amplify delete
```

## Authentication

#### Sign out

```javascript
import { Auth } from 'aws-amplify';

Auth.signOut()
    .then(data => console.log(data))
    .catch(err => console.log(err));

// By doing this, you are revoking all the auth tokens(id token, access token and refresh token)
// which means the user is signed out from all the devices
// Note: although the tokens are revoked, the AWS credentials will remain valid until they expire (which by default is 1 hour)
Auth.signOut({ global: true })
    .then(data => console.log(data))
    .catch(err => console.log(err));
```

In order to sign out and to go to sign page, you have to pass onStateChange props to the component.

```javascript
// App.js
const AppContainer = createAppContainer(RootStack);

class App extends React.Component {
  constructor(props) {
      super(props);
  }
  // pass authenticator props to screenProps
  render() {
      return (
          <AppContainer screenProps={{...this.props}} />
      );
  }
}

// signout component
class Account extends Component {
  constructor(props) {
    super(props);
    this.signOut = this.signOut.bind(this);

  signOut() {
    Auth.signOut().then(() => this.props.screenProps.onStateChange('signedOut', null)).catch(err => this.error(err));
  }

```

#### handling possible promise rejection

```text
Possible Unhandled Promise Rejection (id: 0):
Error: No credentials, applicationId or region
```

This happens with AWS Analytics. Disable it for now

```text
import awsExports from './aws-exports';
Amplify.configure({
   ...awsExports,
   Analytics: { 
       disabled: true
   }
});
```

#### Using Auth Components

For React and React Native apps, the simplest way to add authentication flows into your app is to use the `withAuthenticator` Higher Order Component.

```javascript
import { withAuthenticator } from 'aws-amplify-react'; // or 'aws-amplify-react-native';
import Amplify from 'aws-amplify';
// Get the aws resources configuration parameters
import awsconfig from './aws-exports'; // if you are using Amplify CLI

Amplify.configure(awsconfig);

// ...

export default withAuthenticator(App);
```

#### Get UserId

```javascript
const user = yield Auth.currentAuthenticatedUser()
const { attributes: { sub }} = user
```

## RESTful API

#### Configure App

```javascript
import API from '@aws-amplify/api'
import PubSub from '@aws-amplify/pubsub';
import config from './aws-exports'
API.configure(config);
PubSub.configure(config);
```

#### Get

```javascript
let apiName = 'MyApiName';
let path = '/path'; 
let myInit = { // OPTIONAL
    headers: {}, // OPTIONAL
    response: true, // OPTIONAL (return the entire Axios response object instead of only response.data)
    queryStringParameters: {  // OPTIONAL
        name: 'param'
    }
}
API.get(apiName, path, myInit).then(response => {
    // Add your code here
}).catch(error => {
    console.log(error.response)
});
```

## Storage

#### Add storage

```bash
amplify add storage
amplify push
```

#### API Usage

```javascript
uploadFile = (evt) => {
    const file = evt.target.files[0];
    const name = file.name;

    Storage.put(name, file).then(() => {
      this.setState({ file: name });
    })
  }
```

## Troubleshooting

#### IdentityPool 'eu-west-1:ad9e9db8-b2f7-4543-9099-8ee2a746f0c6' not found.

Update auth and go through all options again.

```bash
amplify update auth
```

