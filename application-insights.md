# Application Insights

With the Application Insights, you can monitor your applications as long as it is running and has network connectivity to Azure. 

## ASP.NET Core Web Host

Install the package

```bash
Microsoft.ApplicationInsights.AspNetCore
```

Then add the service in ConfigureServices\(\) method in the Startup class

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();
    ....
```

Sett up the instrumentation key

```javascript
{
  "ApplicationInsights": {
    "InstrumentationKey": "putinstrumentationkeyhere"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```



