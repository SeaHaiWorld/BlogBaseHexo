---
title: HTML
date: 2018-12-16
categories: 
- 知识积累
---
## 脚本`<script>`放在`<head>`和放到`<body>`底部的区别

## WebSocket 的实现和应用
WebSocket 是 HTML5 中的协议，支持持久连续，HTTP 协议不支持持久性连接。HTTP1.0和HTTP 1.1都不支持持久性的链接

HTTP 通过请求界定，一个请求对应一个响应。HTTP1.1中加入了 keep-alive 属性，可以发送多个请求，但每个请求只对应一个响应

WebSocket 基于 HTTP 协议，握手是与 HTTP 相同的，在 HTTP 中加入两个属性告诉服务器是 WebSocket
### 实现
    Upgrade: webSocket
    Connection: Upgrade
### 应用
WebSocket 解决的问题通过第一个 HTTP request 建立了 TCP 连接之后，之后的交换数据都不需要再发 HTTP 请求了，使得这个长连接变成了一个真·长连接
## Web Quality（无障碍）
在 img 加入 alt 属性，在图片无法加载或者盲人语言模式，会看到或读出 alt 的内容
## BOM属性对象方法
BOM 也叫浏览器对象模型，它提供了很多对象，用于访问浏览器的功能。 
location 属性：herf、reload、replace、hash
history 属性：go、back、forward
## HTML5 drag api

    dragstart：事件主体是被拖放元素，在开始拖放被拖放元素时触发
    darg：事件主体是被拖放元素，在正在拖放被拖放元素时触发
    dragenter：事件主体是目标元素，在被拖放元素进入某元素时触发
    dragover：事件主体是目标元素，在被拖放在某元素内移动时触发
    dragleave：事件主体是目标元素，在被拖放元素移出目标元素是触发
    drop：事件主体是目标元素，在目标元素完全接受被拖放元素时触发
    dragend：事件主体是被拖放元素，在整个拖放操作结束时触发
## 讲讲 Viewport 和移动端布局
### Viewport
```html
<meta 
   name="viewport"  
   content="width=device-width,
            maximum-scale=1,
            minimum-scale=1,
            initial-scale=1,
            user-scalable=no"
>
```
```
width=device-width: 是自适应手机屏幕的尺寸宽度。
maximum-scale:是缩放比例的最大值。
minimum-scale:是缩放比例的最小值。
inital-scale:是缩放的初始化。
user-scalable:是用户的可以缩放的操作。
```
### 移动端布局
+ 使用 @media 媒体查询可以针对不同的媒体类型定义不同的样式，可以针对不同屏幕的大小，编写多套样式，从而达到自适应的效果。
+ 使用百分比实现响应式布局，但计算相对困难
+ 使用 css3 中新的单位 vw / vh
+ 使用 rem ( em )与 fontSize 进行 px 的转换，实现移动端

## 前端提高页面加载速度
合并资源，减少请求数，gzip 压缩，lazyLoad，雪碧图，
## HTML5新增的元素
新增了语义化标签header，footer，nav
在表单方面，为了增强表单，为input增加了color，emial,data ,range等类型
存储方面，提供了sessionStorage，localStorage
多媒体方面规定了音频和视频元素audio和vedio，另外还有地理定位，canvas画布，拖放drag，websocket协议
## 重绘和重排
[重绘和重排](https://www.cnblogs.com/soyxiaobi/p/9963019.html)
## children 属性与 childNodes 属性
差别 childNodes 属性返回所有的节点，包括文本节点、注释节点； children 属性只返回元素节点
如：span是一个元素节点，还有两个文本节点
## 判断事件冒泡
通过 event.bubbles 属性可以判断该事件是否可以冒泡