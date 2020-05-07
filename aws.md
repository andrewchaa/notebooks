# AWS

## Resources

* .NET SDK: [https://docs.aws.amazon.com/sdk-for-net/index.html](https://docs.aws.amazon.com/sdk-for-net/index.html)

## API Gateway

### Usage Plan

* Meter your API usage. Enforce throttling and quota limit on each API key
* Create API Key

### Securing API Endpoints

1. Rate limit: API Key
2. IAM: AWS\_IAM
3. Cognito: Cognito User Pool
4. Custom Authorizer: Lambda

#### API Key

* Use usage plan and create an API Key
* Select an endpoint. Settings &gt; API Key Required to true
* Deploy the API

#### AWS IAM

* Cognito Identity Pool has Authenticated / Unauthenticated Role: navien-backoffice-dev-00000000-authRole
* The Authenticated Role \(navien-backoffice-dev-00000000-authRole\) has a list of policies: PolicyAPIGWapisauth
* The policy \(PolicyAPIGWapisauth\) has "execute-api:Invoke" permission to the API Gateway resource: "arn:aws:execute-api:eu-west-1:0000000000:gpjbhizqdi/\*/GET/installers"

### Enable CORS for an API Gateway REST API Resource

[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a browser security feature that restricts cross-origin HTTP requests that are initiated from scripts running in the browser

When a browser receives a non-simple HTTP request, the [CORS protocol](https://fetch.spec.whatwg.org/#http-cors-protocol) requires the browser to send a preflight request to the server and wait for approval \(or a request for credentials\) from the server before sending the actual request. The _preflight request_ appears to your API as an HTTP request that:

* Includes an `Origin` header.
* Uses the `OPTIONS` method.
* Includes the following headers:
  * `Access-Control-Request-Method`
  * `Access-Control-Request-Headers`

Enable CORS on API Gateway

![](.gitbook/assets/image%20%289%29.png)

## CI / CD

* CodeBuild
* CodePipeline

#### Install dependencies

```bash
npm i --save aws-sdk
```

#### Create build pipeline files

{% code title="buildspec.yml" %}
```yaml
version: 0.2
phases:
  install:
    commands:
      - npm install
      - npm test

```
{% endcode %}

#### Create an artefact bucket

```text
navien-backoffice-dev-artifacts
```

####  Configure AWS CodeBuild

* Create build project



## CLI

### Installation

```bash
# update pip
curl https://bootstrap.pypa.io/get-pip.py | python3

# install awscli
pip install awscli

# configure
aws configure
```

## Cognito

* User Pool
* Federated Identities
* Sync

### User Pool

* Registration
* Verify user email / phone
* Secure sign-in
* Forgotten password
* Change password
* Sign out



![](.gitbook/assets/image.png)

### List users

```bash
aws cognito-idp list-users --user-pool-id eu-west-1_xxxxx --filter "sub=\"c41d95e9-65bf-4d3b-9c08-xxxxxxxxx\""
```







## DynamoDb

#### Scanning a table

Retrieve all items in the table. As a general rule, you should design your applications to avoid performing scans.

```text
// Return all of the data in the table
{
    TableName:  "Music"
}
```



## Route 53

### Routing Traffic to AWS Resources

#### API Gateway API

{% embed url="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-api-gateway.html" %}



