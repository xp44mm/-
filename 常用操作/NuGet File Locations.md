# NuGet File Locations

NuGet directory and file locations across Linux, Mac and Windows operating systems.

## NuGet Cache

Stores downloaded NuGet packages (.nupkg).

**Windows**

- %LocalAppData%\NuGet\Cache
- %UserProfile%\.nuget\packages

## NuGet Configuration

The [NuGet.Config](http://docs.nuget.org/docs/reference/nuget-config-file) file stores user defined NuGet package sources, credentials and proxy settings. The default location for this file is:

**Windows** %AppData%\NuGet\NuGet.Config

## Machine Wide NuGet Configuration

Machine wide configurations are used to define NuGet package sources specific to a machine or a particular IDE, such as Visual Studio.

**Windows** %ProgramData%\NuGet\Config

来源：https://lastexitcode.com/projects/NuGet/FileLocations/

机器范围配置文件内容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="cnblog" value="https://nuget.cnblogs.com/v3/index.json" />
  </packageSources>
  <packageRestore>
    <add key="enabled" value="True" />
    <add key="automatic" value="True" />
  </packageRestore>
  <bindingRedirects>
    <add key="skip" value="False" />
  </bindingRedirects>
  <packageManagement>
    <add key="format" value="0" />
    <add key="disabled" value="False" />
  </packageManagement>
  <disabledPackageSources>
    <add key="Microsoft Visual Studio Offline Packages" value="true" />
  </disabledPackageSources>
  <config>
    <add key="globalPackagesFolder" value="D:\packages" />
  </config>
</configuration>
```

来源：https://blog.csdn.net/lindexi_gd/article/details/79399744