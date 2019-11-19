# Azure DevOps

## Creating a pipeline for ASP.NET Core Web App

#### azure-pipelines.yml

On the root of the repo directory

```yaml
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

## Build Pipeline

### File matching patterns

* `*` matches zero or more characters within a file or directory name
* `?` matches any single character within a file or directory name.
* `[]` matches a set or range of characters within a file or directory name.
* `**` recursive wildcard. For example, `/hello/**/*` matches all descendants of `/hello`.

#### Extended globbing

* `?(hello|world)` - matches `hello` or `world` zero or one times
* `*(hello|world)` - zero or more occurrences
* `+(hello|world)` - one or more occurrences
* `@(hello|world)` - exactly once
* `!(hello|world)` - not `hello` or `world`

## Release Pipeline

### Conditions

#### notIn

* Evaluates `True` if left parameter is not equal to any right parameter
* Min parameters: 1. Max parameters: N
* Converts right parameters to match type of left parameter. Equality comparison evaluates `False` if conversion fails.
* Ordinal ignore-case comparison for Strings
* Short-circuits after first match
* Example: `notIn('D', 'A', 'B', 'C')` \(returns True\)

