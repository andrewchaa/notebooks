# slow-cheetah

[SlowCheetah Visual Studio extension](https://marketplace.visualstudio.com/items?itemName=vscps.SlowCheetah-XMLTransforms) allows you to add and preview transformation to files in your project.  [SlowCheetah NuGet package](https://www.nuget.org/packages/Microsoft.VisualStudio.SlowCheetah) is required for build-time transformations

With the extension, you can easily add transformation files and preview them

![right click and click on &quot;Add Transform&quot;](.gitbook/assets/image%20%285%29.png)

The format should be like this.

```aspnet
<appSettings>
  <add key="blob" 
       value="...." 
       xdt:Transform="SetAttributes" xdt:Locator="Match(key)" />
</appSettings>
```

