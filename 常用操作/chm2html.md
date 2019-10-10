# chm转html

我们通常见到的chm电子书文件是用系统自带的hh.exe来进行观看的，其实hh.exe也有一个命令可以将chm转换为html。hh命令如下： 

```
hh -decompile [html保存路径] [chm文件] 
```

例如 

```
hh -decompile d:\天龙八部.chm 
```

注意文件名不能有空格，可以将文件放置在单独的文件夹中，与其他无关文件隔离开来。



可以用批处理让这个反编译的操作更加简单。批处理程序如下：

```bat
@echo off 
title CHM电子书反编器BAT版 
color a 
echo. 
set /p urlfile=请把要反编的CHM电子书拖进来(再按回车键)： 
copy %urlfile% chmfile.chm > nul 
hh -decompile .\CHM chmfile.chm 
rem del /q chmfile.chm > nul 可以将这句话前面的rem去掉，去掉后反编译成功后则删除chm源文件。 
echo. 
echo 反编文件成功。保存在.\CHM文件夹中，按任意键退出！ 
rem pause > nul 
exit
```



  