# slow-cheetah

## Getting started

[SlowCheetah Visual Studio extension](https://marketplace.visualstudio.com/items?itemName=vscps.SlowCheetah-XMLTransforms) allows you to add and preview transformation to files in your project.  [SlowCheetah NuGet package](https://www.nuget.org/packages/Microsoft.VisualStudio.SlowCheetah) is required for build-time transformations

With the extension, you can easily add transformation files and preview them

![right click and click on &quot;Add Transform&quot;](.gitbook/assets/image%20%289%29.png)

The format should be like this.

```aspnet
<appSettings>
  <add key="blob" 
       value="...." 
       xdt:Transform="SetAttributes" xdt:Locator="Match(key)" />
</appSettings>
```

## Issues

"No destination specified for Copy error" with Visual Studio: [https://github.com/microsoft/slow-cheetah/issues/163](https://github.com/microsoft/slow-cheetah/issues/163)

Close VS and restart it. VS needs restart after downloading and installing slow cheetah

