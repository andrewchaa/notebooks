# AWS

## API Gateway

#### Enable CORS for an API Gateway REST API Resource

[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a browser security feature that restricts cross-origin HTTP requests that are initiated from scripts running in the browser

When a browser receives a non-simple HTTP request, the [CORS protocol](https://fetch.spec.whatwg.org/#http-cors-protocol) requires the browser to send a preflight request to the server and wait for approval \(or a request for credentials\) from the server before sending the actual request. The _preflight request_ appears to your API as an HTTP request that:

* Includes an `Origin` header.
* Uses the `OPTIONS` method.
* Includes the following headers:
  * `Access-Control-Request-Method`
  * `Access-Control-Request-Headers`

Enable CORS on API Gateway

![](.gitbook/assets/image%20%283%29.png)

## CI / CD

* CodeBuild
* CodePipeline

#### Install dependencies

```
npm i --save aws-sdk
```

#### Create build pipeline files

```
version: 0.2
phases:
  install:
    commands:
      - npm install
      - npm test

```

#### Create an artefact bucket

```
navien-backoffice-dev-artifacts
```

####  Configure AWS CodeBuild

* Create build project



## CLI

#### Installation

```
# update pip
curl https://bootstrap.pypa.io/get-pip.py | python3

# install awscli
pip install awscli
```



## DynamoDb

#### Scanning a table

Retrieve all items in the table. As a general rule, you should design your applications to avoid performing scans.

```
// Return all of the data in the table
{
    TableName:  "Music"
}
```



## Route 53

### Routing Traffic to AWS Resources

#### API Gateway API

{% embed url="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-api-gateway.html" %}



