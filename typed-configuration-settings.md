# ASP.NET Core

## Configuration

#### Typed Configuration Settings

By default, [app configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2) is supported and configured out of the box. Just create a class for your config setting and do services.Configure

```csharp
// typed config class
public class CoinbaseSettings
{
    public string Secret { get; set; }
    public string AccessKey { get; set; }
    public string PassPhrase { get; set; }
    public string HostUri { get; set; }
}


// Startup
public void ConfigureServices(IServiceCollection services) 
{
    ...
    services.Configure<CoinbaseSettings>(Configuration.GetSection("CoinbaseSettings"));
}
```

 The related settings are represented in [option pattern](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options?view=aspnetcore-2.2). Inject the option into the class that uses the settings.

```csharp
private readonly CoinbaseSettings _coinbaseSettings;

public PositionsController(IOptions<CoinbaseSettings> options)
{
    _coinbaseSettings = options.Value;
}

...

var coinbaseApi = RestService.For<ICoinbaseProApi>(_coinbaseSettings.HostUri);
```

#### App.Config Transformations

In a not-so-idealistic situation, you have to deal with app.config, even though ASP.NET Core supports appSettings.json out of the box. In this case, you can use MSBuild task to convert the config file \(which is an xml\) yourself

Create App.config files

```text
App.Config
App.Debug.Config
```

Those files should begin with

```markup
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
</configuration>
```

Use Dot.Ndt.Tools

```markup
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.DotNet.Xdt.Tools" Version="2.0.0" />
</ItemGroup>

<Target Name="ApplyXdtConfigTransform" BeforeTargets="_CopyAppConfigFile">
  <PropertyGroup>
    <_SourceWebConfig>$(MSBuildThisFileDirectory)App.config</_SourceWebConfig>
    <_XdtTransform>$(MSBuildThisFileDirectory)App.$(Configuration).config</_XdtTransform>
    <_TargetWebConfig>$(IntermediateOutputPath)$(TargetFileName).config</_TargetWebConfig>
    </PropertyGroup>
  <Exec Command="dotnet transform-xdt --xml &quot;$(_SourceWebConfig)&quot; --transform &quot;$(_XdtTransform)&quot; --output &quot;$(_TargetWebConfig)&quot;" Condition="Exists('$(_XdtTransform)')" />
  <ItemGroup>
    <AppConfigWithTargetPath Remove="App.config" />
    <AppConfigWithTargetPath Include="$(IntermediateOutputPath)$(TargetFileName).config">
    <TargetPath>$(TargetFileName).config</TargetPath>
    </AppConfigWithTargetPath>
  </ItemGroup>
</Target>
```

Then, dotnet publish will do the transformation

```text
dotnet publish "{YourProject}.csproj" 
  -c Release 
  --framework netcoreapp2.0 
  --force 
  --output "obj\Release\Package\" --verbosity d
```

