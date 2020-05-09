---
description: >-
  Everything you need to develop, deploy, monitor and secure serverless
  applications on any cloud.
---

# Serverless Framework

## Resources

* Examples: [https://github.com/serverless/examples](https://github.com/serverless/examples)

## Getting Started

Run this command in your terminal:

```text
curl -o- -L https://slss.io/install | bash
```

After installation completes, open another terminal window, then run this command:

```text
serverless
```

Follow the prompts. If you have a previously installed version, you can upgrade by running:

```text
serverless upgrade
```

## Hello World in C\#

### Create a service

```text
sls create --template aws-csharp --path myService
```

Using the `create` command we can specify one of the available [templates](https://serverless.com/framework/docs/providers/aws/cli-reference/create#available-templates). For this example use aws-csharp with the `--template` or shorthand `-t` flag.

The `--path` or shorthand `-p` is the location to be created with the template service files. Change directories into this new folder.

### Build using .NET Core CLI tools and create zip package <a id="2-build-using-net-core-2x-cli-tools-and-create-zip-package"></a>

```text
# Linux or Mac OS
./build.sh
```

```text
# Windows PowerShell
./build.cmd
```

### Deploy <a id="3-deploy"></a>

```text
sls deploy
```

This will deploy your function to AWS Lambda based on the settings in `serverless.yml`.

### Invoke deployed function <a id="4-invoke-deployed-function"></a>

```text
sls invoke -f hello
```

Invoke deployed function with command `invoke` and `--function` or shorthand `-f`.

In your terminal window you should see the response from AWS Lambda.

```text
{
    "Message": "Go Serverless v1.0! Your function executed successfully!",
    "Request": {
        "Key1": null,
        "Key2": null,
        "Key3": null
    }
}
```

## Creating a function in C\#

### Packages to install 

* Amazon.Lambda.APIGatewayEvents
* Amazon.Lambda.Core
* Amazon.Lambda.Logging.AspNetCore
* Amazon.Lambda.Serialization.Json

### Handler

```csharp
using Amazon.Lambda.Core;
using System;

[assembly:LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace Navien.Installers.UserPoolFunctions
{
    public class Handler
    {
        public async Task<IList<UserType>> ListInstallers(Request request)
        {
            AWSCredentials credentials = new BasicAWSCredentials("xxxxxx", 
                "xxxxxxxxxxxxxxxxx");
            var client = new AmazonCognitoIdentityProviderClient(credentials);
            var response = await client.ListUsersAsync(new ListUsersRequest
            {
                UserPoolId = "eu-west-1_xxxxxx"
            });

            return response.Users;
            // return new Response("Go Serverless v1.0! Your function executed successfully!", request);
        }
    }

    public class Request
    {
      public string Key1 {get; set;}
      public string Key2 {get; set;}
      public string Key3 {get; set;}

      public Request(string key1, string key2, string key3){
        Key1 = key1;
        Key2 = key2;
        Key3 = key3;
      }
    }
}
```

#### Supply credentials

```csharp
AWSCredentials credentials = new BasicAWSCredentials("xxxxxx", 
    "xxxxxxxxxxxxxxxxx");
var client = new AmazonCognitoIdentityProviderClient(credentials);
```

### build.sh\_

```bash
#!/bin/bash

#install zip on debian OS, since microsoft/dotnet container doesn't have zip by default
if [ -f /etc/debian_version ]
then
  apt -qq update
  apt -qq -y install zip
fi

dotnet restore
dotnet lambda package --configuration release --framework netcoreapp2.1 --output-package bin/release/netcoreapp2.1/userpoolfunctions.zip
```

### serverless.yml

```yaml
service: userpoolfunctions
provider:
  name: aws
  runtime: dotnetcore2.1

# you can overwrite defaults here
  stage: dev
  region: eu-west-1

# you can add statements to the Lambda function's IAM Role here
  iamRoleStatements:
    - Effect: "Allow"
      Action:
        - "cognito-idp:DescribeUserPoolClient"
      Resource: 
        - "arn:aws:cognito-idp:eu-west-1:000000000:userpool/eu-west-1_xxxxxxx"
package:
  individually: true

functions:
  listInstallers:
    handler: CsharpHandlers::Navien.Installers.UserPoolFunctions.Handler::ListInstallers

    # you can add packaging information here
    package:
      artifact: bin/release/netcoreapp2.1/userpoolfunctions.zip
```

### Output format of a Lambda function for proxy integration

```javascript
{
    "isBase64Encoded": true|false,
    "statusCode": httpStatusCode,
    "headers": { "headerName": "headerValue", ... },
    "multiValueHeaders": { "headerName": ["headerValue", "headerValue2", ...], ... },
    "body": "..."
}
```

## Configuration

### HTTP Endpoints

```yaml
functions: 
  get-index: 
    handler: functions/get-index.handler
    events:
      - http: 
          path: /
          method: get
          cors: true
```

#### Authorizer

```yaml
# aws_iam
get-restaurants: 
  handler: functions/get-restaurants.handler
  events:
    - http:
        path: /restaurants/
        method: get
        authorizer: aws_iam

# user pool
search-restaurants:
  handler: functions/search-restaurants.handler
  events:
    - http:
        path: /restaurants/search
        method: post
        authorizer:
          arn: arn:aws:coginto-idp:us-east-1:1111111:userpool/us-east-1_bxxxx
```

### iamRoleStatements

```yaml
plugins: 
  - serverless-pseudo-parameters
  
provider:
  name: aws
  runtime: nodejs6.10
  iamRoleStatements:
    - Effect: Allow
      Action: dynamodb:scan
      Resource: arn:aws:dynamodb:#{AWS::Region}:#{AWS::AccountId}:table/restaurants
    - Effect: Allow
      Action: execute-api:Invoke
      Resource: arn:aws:execute-api:#{AWS::Region}:#{AWS::AccountId}:*/*/GET/restaurants
```

### Resources

```yaml
resources:
  Resources:
    restaurantsTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: restaurants
        AttributeDefinitions: 
          - AttributeName: name
            AttributeType: S
        KeySchema:
          - AttributeName: name
            AttributeType: HASH
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
```

## Authentication

### IdToken

```javascript
function init() {
  AWS.config.region = AWS_REGION
  AWSCognito.config.region = AWS_REGION
  
  var data = {
    UserPoolId: COGNITO_USER_POOL_ID,
    ClientId: CLIENT_ID
  }
  userPool = new AWSCognito.CognitoIdentityServiceProvider.CognitoUserPool(data)
  cognitoUser = userPool.getCurrentUser()
  
  if (cognitoUser != null) {
    cognitoUser.getSession(function(err, session) {
      idToken = session.idToken.jwtToken
    }
  })
}

function searchRestaurants() {
  var xhr = new XMLHttpRequest()
  xhr.open('POST', SEARCH_URL, true)
  xhr.setRequestHeader("Content-Type", "application/json")
  xhr.setRequestHeader("Authorization", idToken)
  xhr.send(JSON.stringify({ theme })
}
```

