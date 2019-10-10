# nuget

### 升级NuGet程序包

注意选择相应的VisualStudio版本，不要变更扩展包相关NuGet项的主要版本号。

https://docs.microsoft.com/en-us/nuget/tools/ps-ref-update-package

NuGet版本号由三部分组成:`Major.Minor.Patch`，中文名为主要版本，次要版本，补丁版本

打开NuGet界面，先选择性升级可以更新到最新版的包。然后用命令行升级所有包到次要版最新。

ToHighestMinor: Constrains upgrades to only versions with the same Major version as the currently installed package.

```
Update-Package -ToHighestMinor
```

升级所有包到次要版最新，`Major`相同，`Minor.Patch`最新。

参考：

```
Update-Package
```

升级所有包到最新版。`Major.Minor.Patch`都是最新。

```
Update-Package -Safe
```

升级所有包的第三部分到最新版，`Major.Minor`相同，`Patch`最新。 `-Safe` is an alias of `-ToHighestPatch`.



### NuGet找不到资产文件 project.assets.json

解决办法：
使用 cmd cd 目前 .net core 项目文件夹中
运行 dotnet build