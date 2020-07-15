---
title: WebStorm 添加右键菜单
date:2020-01-04
tags:
- WebStorm
- 注册命令
categories:
- 软件配置
---
### 打开 Windows 注册器

`Windows + R` 打开运行  =>  输入 `regedit`  => 确定

### 添加 WebStorm 右键打开文件

找到 `HKEY_CLASSES_ROOT/*/shell`   
在 `shell` 内新建**项**,命名为 `Open With WebStorm` 
在 `Open With WebStorm` 项上右键新建字符串值 `icon`   
在 `Open With WebStorm` 项内右侧
双击默认数值填写右键菜单的名称 `Open With WebStorm` 
双击 `icon` 数值填写 `Webstorm路径`
如：`"E:\WebStorm 2019.3.2\bin\webstorm64.exe"`  
在 `Open With WebStorm` 项下新建 `command` 项 
项内默认填写 `Webstorm路径 %1`
如：`"E:\WebStorm 2019.3.2\bin\webstorm64.exe %1"`  
此时右键文件时菜单项会有 `Open With WebStorm`
<!-- more-->
### 添加 WebStorm 右键打开文件夹

找到 `HKEY_CLASSES_ROOT/Directory/shell`  
在 `shell` 内新建**项**,命名为 `Open Floder With WebStorm`
在 `Open Floder With WebStorm` 项上右键新建字符串值 `icon`  
在 `Open Floder With WebStorm` 项内右侧
双击默认数值填写你右键菜单的名称 `Open Floder With WebStorm`
双击 `icon` 数值填写`Webstorm路径`
如：`"E:\WebStorm 2019.3.2\bin\webstorm64.exe"`  
在 `Open Floder With WebStorm` 项下新建 `command` 项 
项内默认填写 `Webstorm路径 %1`
如：`"E:\WebStorm 2019.3.2\bin\webstorm64.exe %1"`  
此时右键文件时菜单项会有 `Open Floder With WebStorm`
