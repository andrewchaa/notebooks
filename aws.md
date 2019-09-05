# AWS

## API Gateway

#### Enable CORS for an API Gateway REST API Resource



## CI / CD

* CodeBuild
* CodePipeline

#### Install dependencies

```bash
npm i --save aws-sdk
```

#### Create build pipeline files

{% code-tabs %}
{% code-tabs-item title="buildspec.yml" %}
```yaml
version: 0.2
phases:
  install:
    commands:
      - npm install
      - npm test

```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### Create an artefact bucket

```text
navien-backoffice-dev-artifacts
```

####  Configure AWS CodeBuild

* Create build project



## CLI

#### Installation

```bash
# update pip
curl https://bootstrap.pypa.io/get-pip.py | python3

# install awscli
pip install awscli
```



## Route 53

### Routing Traffic to AWS Resources

#### API Gateway API

{% embed url="https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-api-gateway.html" %}



