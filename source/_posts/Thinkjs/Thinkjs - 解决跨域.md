---
title: Thinkjs - 解决跨域问题
date: 2019-10-05
tags:
- Thinkjs
- Kors
categories:
- 后端
---
### 场景
学习 Thinkjs 3.0 ，但是前后台分离，产生了跨域问题

### 问题描述
![Thinkjs](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/跨域例子.png)
<!--more-->
koa2 的跨域问题可以通过引入koa-cors2来解决

### 解决方案
#### 安装 kcors
```
    npm install kcors –save 
```
#### 引入kcors
在中间件 middleware.js 中引入 kcors
```
    const kcors = require('kcors'); // 引入kcors
```
```
    module.exports = [
    ...
      {
        handle: kcors, // 处理跨域
        options: {}
      },
    ...
    }
```

### 参考文档

* [kcors - npm](https://www.npmjs.com/package/koa-cors)
* [Thinkjs 官方文档](https://thinkjs.org/zh-cn/doc/3.0/index.html)
