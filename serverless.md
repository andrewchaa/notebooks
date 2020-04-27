---
description: >-
  Everything you need to develop, deploy, monitor and secure serverless
  applications on any cloud.
---

# serverless

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

{% embed url="https://serverless.com/framework/docs/providers/aws/examples/hello-world/csharp/" %}

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

## Creating a function

#### Handler

```csharp
using Amazon.Lambda.Core;
using System;

[assembly:LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace Navien.Installers.UserPoolFunctions
{
    public class Handler
    {
       public Response ListInstallers(Request request)
       {
           return new Response("Go Serverless v1.0! Your function executed successfully!", request);
       }
    }

    public class Response
    {
      public string Message {get; set;}
      public Request Request {get; set;}

      public Response(string message, Request request){
        Message = message;
        Request = request;
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

{% embed url="https://build.sh" %}

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

#### serverless.yml

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

