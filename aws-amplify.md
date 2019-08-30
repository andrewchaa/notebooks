---
description: >-
  An opinionated, category-based client framework for building scalable mobile
  and web apps
---

# AWS Amplify

## Common

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

## Get Started

Install the CLI

Install [Node.js®](https://nodejs.org/en/download/) and [npm](https://www.npmjs.com/get-npm) if they are not already on your machine.

```bash
npm install -g @aws-amplify/cli
amplify configure
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

