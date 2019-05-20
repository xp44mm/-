# 配制Visual Studio和Node.js

## ConEmu

这是一个命令行程序，可以从 https://www.fosshub.com/ConEmu.html 下载安装。

安装完成后，用管理员启动这个程序，从右上角的菜单中选择`setting...`, 在右侧的树选择`General`，如下设置：

Choose your startup task or even a shell with arguments:

```
{Shells::cmd(Admin)}
```



## 配置visual studio

菜单[工具]->[选项...]

左侧:[项目和解决方案],[Web Package Management],[程序包还原]:所有选项都设为`False`

左侧:[项目和解决方案],[Web Package Management],[外部Web工具]:外部工具的位置:$(PATH)向上至顶部以获得最高优先级.

visual studio工具->扩展和更新...

[ ] Add New File
[ ] CodeMaid
[ ] EF Core Power Tools
[ ] File Icons
[ ] Project File Tools



引用安装包的版本号是根据目标框架版本号推算出的，不要加版本号：

```xml
<PackageReference Include="Microsoft.AspNetCore.All"/>
```
详见网址：
https://docs.microsoft.com/zh-cn/dotnet/core/tools/csproj#implicit-package-references



`Microsoft.AspNetCore.App`已经取代了`Microsoft.AspNetCore.All`，详见网址：

https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage?view=aspnetcore-2.1#migrate









### ESLint设置

选项，文本编辑器，javascript/typescript，Linting，常规。

项目文件夹下的配置文件将完全覆盖默认配置文件。

visual studio 自带ESLint功能，只需修改默认配置文件：

```js
"rules": {
    'semi': ['off'],
    'no-console': "warn",
}
```

规则可以用数字或字符串配置：

| 数字 | 字符串  | 含义 | 动作       |
| ---- | ------- | ---- | ---------- |
| 0    | 'off'   | 关闭 | 不检查     |
| 1    | 'warn'  | 警告 | 检查可通过 |
| 2    | 'error' | 错误 | 检查不通过 |

如果有额外选项，可以用数组常量，依次排列选项。

### 如何重新安装和更新包

[何时重新安装包](https://docs.microsoft.com/zh-cn/nuget/consume-packages/reinstalling-and-updating-packages#when-to-reinstall-a-package)中描述了大量有关对包的引用可能在 Visual Studio 项目中损坏的情况。 在这些情况下，卸载并重新安装同一版本的包会将这些应用还原为正常工作状态。 更新包仅意味着安装更新版本，这常常将包还原为正常工作状态。

更新和重新安装包按以下方式实现：

| 方法                                                         | 更新                                           | 重新安装                                                     |
| ------------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------------------ |
| 包管理器控制台（[使用 Update-Package](https://docs.microsoft.com/zh-cn/nuget/consume-packages/reinstalling-and-updating-packages#using-update-package)中所述） | `Update-Package`命令                           | `Update-Package -reinstall` 命令                             |
| 包管理器 UI                                                  | 在“更新”选项卡上，选择一个或多个包并选择“更新” | 在“已安装”选项卡上，选择一个包，记录其名称，然后选择“卸载”。 切换到“浏览”选项卡，搜索包名称并选中，然后选择“安装”。 |
| nuget.exe CLI                                                | `nuget update` 命令                            | 对于所有包，删除包文件夹，然后运行 `nuget install`。对于单个包，删除包文件夹并使用 `nuget install <id>`再安装一个。 |



## 安装或升级node.js

从网站 https://nodejs.org/en/download/ 下载最新版安装文件.



### 安装或升级npm

以管理员身份打开命令行,执行如下命令

```
npm i -g npm
```

如果有错误,错误信息为文件已经存在,或文件不能重命名,可以手动删除再次执行安装命令,

可能的文件是node.js安装目录`C:\Program Files\nodejs`下的文件:

```
npm
npm.cmd
npx
npx.cmd
```



查看npm版本:

```
npm -v
```



Try the latest stable version of npm

```
npm install -g npm@latest
npm install -g npm@next
```



查看npm全局包：

```
npm list -g --depth=0
```



### npm插件

npm-check-updates工具用于更新`package.json`中的包版本到最新版,参考:http://blog.csdn.net/wkl305268748/article/details/76641323

安装插件:

```
npm install -g npm-check-updates@latest
```



### 升级全局的本地包

```
npm install -g
```



查看哪些包有更新：

```
npm -g outdated
```

例如,我们发现全局包`@angular/cli`需要更新, 就执行:

```
 npm i -g @angular/cli@latest
```



### 使用NPM查找包的所有版本

```
npm view react-hot-loader versions
```

此命令查找包`react-hot-loader`的所有版本。屏幕打印出该包所有版本的数组。也可以使用简写：

```
npm v immutable versions
```



### 本地安装位置

`npm install` saves any specified packages into `dependencies` by default. Additionally, you can control where and how they get saved with some additional flags:

* `-P, --save-prod`: Package will appear in your `dependencies`. This is the default unless `-D` or `-O` are present.
* `-D, --save-dev`: Package will appear in your `devDependencies`.
* `-O, --save-optional`: Package will appear in your `optionalDependencies`.
* `--no-save`: Prevents saving to `dependencies`.



### 置命令行于项目文件夹

在解决方案资源管理器中,单击选中击ASP.NET项目,右击[在文件资源管理器中打开文件夹],在地址栏输入:

```
ConEmu64.exe
```

命令行以`#`提示，表示是管理员模式，以`$`提示则是非管理员模式，欲进入管理员模式:

在命令行窗口的标签上右击,[restart or duplicate],[restart as Adimin],以置命令行于项目文件夹。



### NPM Unexpected end of JSON input while parsing near

解决办法：

这是服务器连接的问题，多换几个站点，重试。

first:

```
npm i --registry=https://registry.npm.taobao.org --loglevel=silly
```

then:

```
npm cache clean --force
```

验证缓存：
```
# npm cache verify
Cache verified and compressed (~\AppData\Roaming\npm-cache\_cacache):
Content verified: 2503 (45460300 bytes)
Content garbage-collected: 1 (87138 bytes)
Index entries: 4304
Finished in 14.909s
```


### Chrome浏览器启用 Experimental JavaScript 

地址栏输入：

```
chrome://flags/
```

在页面最顶部的搜索栏输入：

```
JavaScript
```

找到可用的选项进行修改，即可。



