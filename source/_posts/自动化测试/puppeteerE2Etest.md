---
title: 使用 puppeteer 实现前端 E2E 测试
date: 2019-11-12 20:10:33
tags:
- puppeteer
- nodejs
- E2E 测试
categories:
- 自动化测试
---
## 前言
页面交互一直是前端最为重要的组成部分，可要实现其 UI 层次的自动化测试一直是老大难的问题。过去，我们只能通过人工去点击页面，测试页面交互、数据传输。因此，E2E 测试应需而生，E2E 测试极为便捷的解决了用户层面的交互测试。

## 基于 puppeteer 的 E2E 测试
### 什么是 E2E
E2E（End To End）即端对端测试，属于白盒测试，通过编写测试用例，模拟用户操作，确保页面交互时，组件间通信正常、数据传递如预期。结合 Puppeteer 和谷歌浏览器扩展程序 Puppeteer recorder 可以很快实现一个 E2E 测试用例。
### Puppeteer
Puppeteer 是一个 Node 库。它提供了一个许多高级 API 来通过 DevTools 协议控制 Chromium 或 Chrome。Puppeteer API 是分层次的，反映了浏览器结构。
#### What can puppeteer do?
*   生成页面的屏幕截图和PDF。
*   抓取SPA并生成预渲染内容（即“SSR”）。
*   自动表单提交，UI测试，键盘输入等。
*   创建最新的自动化测试环境。 使用最新的JavaScript和浏览器功能直接在最新版本的Chrome中运行测试。
*   捕获您网站的[时间线跟踪]
*   实现简单的爬虫

#### 安装依赖
新建 node 项目下执行指令：
> cnpm install puppeteer --save
#### 按需引入
> const  puppeteer  =  require('puppeteer')
### Puppeteer recorder
Puppeteer recorder 是一个 Chrome 扩展程序，可记录您的浏览器交互并生成 Puppeteer 脚本。编写用于抓取、测试和监视的 Puppeteer 脚本可能很棘手，而Puppeteer recorder 可以很便捷的实现。

简单的说，Puppeteer recorder 可以帮助你记录在 Chrome 浏览器上执行的操作，并生成 Punppeter 代码。实现“一次操作，生成测试”，极大的缩短了我们书写 E2E 测试的时间。
## 参考阅读
+ [Puppeteer中文文档](https://zhaoqize.github.io/puppeteer-api-zh_CN/#?product=Puppeteer&version=v1.20.0&show=api-class-page)
