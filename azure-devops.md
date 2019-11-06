# Azure DevOps

## Creating a pipeline for ASP.NET Core Web App

#### azure-pipelines.yml

On the root of the repo directory

```
trigger:
- master    #build trigger on code commit on master branch

pool:
  vmImage: 'ubuntu-latest'    #agent vm

variables:
  azureSubscription: <your subscription name>
  WebAppName: <your app name on azure portal>  
  buildConfiguration: 'Release'

steps:

# pull source code, resore packages, and build the project
- script: dotnet build --configuration $(buildConfiguration)
  displayName: 'dotnet build $(buildConfiguration)'

# package .net assemblies, html, and javascripts into a zip  
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    publishWebProjects: True
    arguments: '--configuration $(BuildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: True  

# deploy the zipped package into azure web app
- task: AzureRmWebAppDeployment@3
  inputs:
    appType: webAppLinux
    azureSubscription: $(azureSubscription)
    WebAppName: $(WebAppName)
    Package: $(System.ArtifactsDirectory)/**/*.zip

```

## Release Pipeline

### Conditions

#### notIn

* Evaluates `True` if left parameter is not equal to any right parameter
* Min parameters: 1. Max parameters: N
* Converts right parameters to match type of left parameter. Equality comparison evaluates `False` if conversion fails.
* Ordinal ignore-case comparison for Strings
* Short-circuits after first match
* Example: `notIn('D', 'A', 'B', 'C')` \(returns True\)

