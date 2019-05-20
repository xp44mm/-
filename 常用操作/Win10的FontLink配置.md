Win10的FontLink配置



注册表:

```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink
```



新建多字符串值（REG_MULTI_SZ），命名为【Consolas】

把「Segoe UI」的内容都复制进「Consolas」，重新登录即可。