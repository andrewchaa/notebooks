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

In a not-so-idealistic situation, you have to deal with app.config, even though ASP.NET Core supports appSettings.json out of the box. Still it's possible. You just need a bit of magic to your project file. 

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

Update your project .csproj file. This works out of the box for ASP.NET Core. For console app, you have to install [slow-cheetah](https://github.com/Microsoft/slow-cheetah)

```markup
<ItemGroup>
  <None Update="App.config">
    <TransformOnBuild>true</TransformOnBuild>
  </None>
  <None Update="App.Debug.config">
    <IsTransformFile>true</IsTransformFile>
    <DependentUpon>App.config</DependentUpon>
  </None>
  <None Update="App.Release.config">
    <IsTransformFile>true</IsTransformFile>
    <DependentUpon>App.config</DependentUpon>
  </None>
</ItemGroup>
```

## Controller

### Response types

#### Plain Text result

```csharp
[HttpGet("[action]")]
public ContentResult Fps()
{
    var content = @"digraph G {
        ServiceBus [shape=box, label="""", image=""/azure-service-bus.svg""]
        ServiceBus -> Manager
        }";
    return Content(content);
}
```

## Handling files

### File Providers

#### Read file content

```csharp
var fileInfo = provider.GetFileInfo("content.json");
var stream = fileInfo.CreateReadStream();
var serializer = new JsonSerializer();

ServicesManifest manifest;
using (var reader = new StreamReader(stream))
using (var textReader = new JsonTextReader(reader))
{
    manifest = serializer.Deserialize<ServicesManifest>(textReader);
}

```

