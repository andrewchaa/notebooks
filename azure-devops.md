# Azure DevOps

## Resources

* [https://docs.microsoft.com/en-us/azure/devops/pipelines/release/index?view=azure-devops](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/index?view=azure-devops)

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

### Variables

* Relese.EnvironmentName
* Release.Environments.{stage-name}.status

### Conditions

#### notIn

* Evaluates `True` if left parameter is not equal to any right parameter
* Min parameters: 1. Max parameters: N
* Converts right parameters to match type of left parameter. Equality comparison evaluates `False` if conversion fails.
* Ordinal ignore-case comparison for Strings
* Short-circuits after first match
* Example: `notIn('D', 'A', 'B', 'C')` \(returns True\)
* and\(succeeded\(\), notIn\(variables\['Release.EnvironmentName'\], 'blue', 'green'\)\)

### Manage the names for new releases

The names of releases for a release pipeline are, by default, sequentially numbered. The first release is named **Release-1**, the next release is **Release-2**, and so on. You can change this naming scheme by editing the release name format mask. In the **Options** tab of a release pipeline, edit the **Release name format** property in the **General** page.

When specifying the format mask, you can use the following pre-defined variables.

| Variable | Description |
| :--- | :--- |
| **Rev:rr** | An auto-incremented number with at least the specified number of digits. |
| **Date / Date:MMddyy** | The current date, with the default format **MMddyy**. Any combinations of M/MM/MMM/MMMM, d/dd/ddd/dddd, y/yy/yyyy/yyyy, h/hh/H/HH, m/mm, s/ss are supported. |
| **System.TeamProject** | The name of the project to which this build belongs. |
| **Release.ReleaseId** | The ID of the release, which is unique across all releases in the project. |
| **Release.DefinitionName** | The name of the release pipeline to which the current release belongs. |
| **Build.BuildNumber** | The number of the build contained in the release. If a release has multiple builds, this is the number of the [primary build](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/artifacts?view=azure-devops#primary-source). |
| **Build.DefinitionName** | The pipeline name of the build contained in the release. If a release has multiple builds, this is the pipeline name of the [primary build](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/artifacts?view=azure-devops#primary-source). |
| **Artifact.ArtifactType** | The type of the artifact source linked with the release. For example, this can be **Azure Pipelines** or **Jenkins**. |
| **Build.SourceBranch** | The branch of the [primary artifact source](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/artifacts?view=azure-devops#primary-source). For Git, this is of the form **master** if the branch is **refs/heads/master**. For Team Foundation Version Control, this is of the form **branch** if the root server path for the workspace is **$/teamproject/branch**. This variable is not set for Jenkins or other artifact sources. |
| _Custom variable_ | The value of a global configuration property defined in the release pipeline. |

For example, the release name format `Release $(Rev:rrr) for build $(Build.BuildNumber) $(Build.DefinitionName)` will create releases with names such as **Release 002 for build 20170213.2 MySampleAppBuild**.



