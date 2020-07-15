---
title: Ubuntu 配置阿里云源
date:  2019-07-01
tags:
- Vim
categories:
- Ubuntu
---
## 如何换成国内最快的阿里云源
### 第一步：备份源文件
>cd /etc/apt/  

然后会显示下面的源文件 sources.list 
输入命令 
>sudo cp sources.list sources.list.bak 

将 sources.list 备份到 sources.list.bak
<!--more-->
### 第二步：替换源文件
阿里云源
```
    deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse  
    ##测试版源  
    deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse  
    # 源码  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse  
    ##测试版源  
    deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse  
    # Canonical 合作伙伴和附加  
    deb http://archive.canonical.com/ubuntu/ xenial partner  
    deb http://extras.ubuntu.com/ubuntu/ xenial main  
```
替换并保存 
>sudo vim sources.list

修改文件，替换成阿里云源即可

### 第三步：更新源和软件
>sudo apt-get update 更新源  

>sudo apt-get upgrade 更新软件
