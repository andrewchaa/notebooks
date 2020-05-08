# ASP.NET Core

## CLI

```bash
# create test project
mkdir EventStormingSample.Test
cd EventStormingSample.Test
dotnet new nunit

# run app and watch file change
dotnet watch run
```

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

### App.Config Transformations

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

### Set environment variable on Service Fabric Web API

**ApplicationPackageRoot / ApplicationManifest.xml**

```markup
<Parameter Name="WebApi_ASPNETCORE_ENVIRONMENT" DefaultValue="" />

<EnvironmentOverrides CodePackageRef="code">
  <EnvironmentVariable 
     Name="ASPNETCORE_ENVIRONMENT" 
     Value="[WebApi_ASPNETCORE_ENVIRONMENT]" />
</EnvironmentOverrides>
```

**ApplicationParameters / Local.1Node.xml, Local.5Node.xml**

```markup
<Parameter Name="WebApi_ASPNETCORE_ENVIRONMENT" Value="Development" />
```

**PackageRoot / ServiceManifest.xml**

```markup
<!-- Code package is your service executable. -->
<CodePackage Name="Code" Version="1.0.0">
  <EntryPoint>
    <ExeHost>
      <Program>ClearBank.CustomerAccounts.ServiceFabric.WebApi.exe</Program>
      <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
  </EntryPoint>
  <EnvironmentVariables>
    <EnvironmentVariable Name="ASPNETCORE_ENVIRONMENT" Value=""/>
  </EnvironmentVariables>
</CodePackage>
```

**Api.cs file**

```csharp
.ConfigureAppConfiguration((context, configBuilder) =>
{
    var environment = context.HostingEnvironment.EnvironmentName;

    configBuilder.SetBasePath(context.HostingEnvironment.ContentRootPath);
    configBuilder.AddJsonFile("appsettings.json", true);
    configBuilder.AddJsonFile($"appsettings.{environment}.json", true);
})
```

**TestApiFactory.cs file**

```csharp
private static readonly string EnvironmentName = 
  Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") ?? "Development";
```

### Typed Configuration for appsettings.json

## Dependency Injection

### Build Dependencies

Install these packages first

* Microsoft.Extensions.Configuration
* Microsoft.Extensions.Configuration.Json
* Microsoft.Extensions.Configuration.EnvironmentVariables

```csharp
private static readonly IServiceCollection Services = new ServiceCollection();

public static IServiceCollection ConfigureServices()
{
    var configBuilder = new ConfigurationBuilder();

    configBuilder.AddEnvironmentVariables("ASPNETCORE_");
    configBuilder.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
#if DEBUG
    configBuilder.AddJsonFile($"appsettings.Development.json", true);
    configBuilder.AddEnvironmentVariables("Metadata:");
#endif

    var config = configBuilder.Build();
    Services.Configure<CosmosDbOptions>(config.GetSection("CosmosDb"))
        .Configure<ServiceBusOptions>(config.GetSection("ServiceBus"))
        .Configure<ElasticLoggingOptions>(config.GetSection("ElasticLogging"))
        ;
    Services.AddLogging(config);

    Services.AddTransient<IEnumerable<IHealthCheck>>(sp =>
    {
        var serviceBusSettings = sp.GetRequiredService<IOptions<ServiceBusOptions>>().Value;
        var cosmosDbSettings = sp.GetRequiredService<IOptions<CosmosDbOptions>>().Value;

        return new List<IHealthCheck>
        {
            new AzureServiceBusTopicHealthCheck(serviceBusSettings.ConnectionString, EventNames.AcknowledgementEvent),
            new CosmosDbHealthCheck(cosmosDbSettings.ConnectionString)
        };
    });

    Services.AddTransient<IAppHealthCheck, AppHealthCheck>()
        .AddTransient<ITopicClientFactory, ServiceBusTopicClientFactory>()
        .AddSingleton<ICosmosDbRepository>(x =>
        {
            var cosmosDbOptions = x.GetRequiredService<IOptions<CosmosDbOptions>>().Value;
            var client = new CosmosClient(cosmosDbOptions.ConnectionString);
            var container = client.GetContainer("fps-metadata", "metadata");
            return new CosmosDbRepository(container);
        });

    Services.AddMediatR(typeof(ListenersServiceRegistrations).Assembly,
        typeof(CreateMetadataInfoCommand).Assembly);

    return Services;
}

public static IServiceProvider Build()
{
    return ConfigureServices()
        .BuildServiceProvider();
}
```

### Replacing dependencies

```csharp
[SetUp]
public void Setup()
{
    _cosmosContainer = new Mock<Container>();

    _services = ListenersServiceRegistrations.ConfigureServices();
    _services.RemoveAll<ICosmosDbRepository>();
    _services.AddSingleton<ICosmosDbRepository>(x => new CosmosDbRepository(_cosmosContainer.Object));
    
    var serviceProvider = _services.BuildServiceProvider();
    _mediator = serviceProvider.GetRequiredService<IMediator>();
}
```

### Tips

#### TimeSpan config

```csharp
{
  "SeatingDuration": "2:30:00"
}

var seatingDuration = Configuration.GetValue<TimeSpan>("SeatingDuration");
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

