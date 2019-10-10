## 克隆Knockout项目文件到本地:

确保是原始文件,即使代码看起来不合理不要修改它.

首先修复找不到名称错误.

找到合并后的文件`knockout-3.4.2.debug.js`

从`    "@types/knockout": "^3.4.45"`找到声明文件.



## 重构 knockout/build/

此文件夹描述文件打包的文件:
knockout使用Require.js (AMD),合并文件.
入口点:

```
knockout\build\fragments\extern-pre.js
knockout\build\knockout-raw.js
```
可以从中得出全局变量,这些变量放在打包文件的最前面.

文件的依赖关系从此文件得出:
```
knockout\build\fragments\source-references.js
```

### 重构心得

#### 全局变量

查找`window.`, 它要么引用全局变量,需要导入.要么修改全局变量,有副作用.或需要导出.

#### 类型不存在属性

如果需要不检查名称调用不存在的类型成员,就将对象断言成`any`,如:

```typescript
export const amdRequire = window.require
```

会提示:

>TS2339	(TS) 类型“Window”上不存在属性“require”。	

将代码修改为:

```typescript
export const amdRequire = (<any>window).require
```

错误信息消失.

如果是局部变量,则在局部变量的声明处使用显式`<any>`声明类型.

如果是构造函数则使用如下方法:

```typescript
const Person:any = function(...){...}
```



另外一种除错方法,使用索引法:

```typescript
export const JSON = window["JSON"]
```

下节将根据从`source-references.js`得来的依赖顺序,从前往后逐个文件进行重构:
## 重构 knockout/src/

重构的模式:

重构前修改文件的扩展名,将js文件改为同名的ts文件,如文件`utils.js`改名为`utils.ts`

删除最小化辅助保留名称的代码,正则表达式匹配:

```typescript
/^\s*ko.export(.*)$/ 
```

所有名字空间向上移动一级,删除根名字空间`ko.`:

```typescript
/\bko\./ to `/*$0*/`
```



删除明显的且影响编译的垫片代码,因为重构后的代码只兼容最新的es标准:

```typescript
if (!Function.prototype['bind']) {/*...*/}
```

此代码是垫片代码,最新标准的代码一定不会进入`if`块中的.



原有代码的结构形式将如下:

```typescript
ko.utils = (function () {
    // ...private block
    return {
 	//...export block
    }
}());

```

首先,修改为如下:

```typescript
//ko.utils = (function () {
    // ...private block
    //return {
 	//...export block
    //}
//}());
```

然后用正则表达式手工替换,分两步.先注释掉返回对象形如`a:a`的属性
```typescript
`^\s+(\w+):\s*\1` to  `//$0` 
```
返回对象中新定义的属性:
```typescript
`^\s+(\w+):` to `const $1 =` 
```
最后手工删除每个属性值末尾的逗号,编辑器会给出不允许使用逗号的提示.

替换`ko.utils.`为`/*ko.utils.*/`,引用本模块中的成员不需要前缀了.

解决找不到名称错误:

名称是全局变量,需将名称导入到本模块:

```typescript
import { jQueryInstance } from './build'
```


(TS) 对象可能为“未定义”。在名称后面加叹号`!`

```typescript
if (ieVersion! <= 7) {
```




(TS) 提供的参数与调用目标的任何签名都不匹配。

原因是函数签名列表比较长,将函数定义的最后一个参数改为可选参数试试.



(TS) 类型“KnockoutVirtualElement”上不存在属性“data”。修改为动态调用:

```typescript
innerTextNode['data'] = value;
```

在文件的最底部导出函数,保持列表为空后面的模块用到时返回来再添加名称:

```typescript
export { deferError }
```

