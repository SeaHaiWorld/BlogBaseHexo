---
title: JS 事件流的概念
date: 2020-01-13
tags:
- JavaScript
- DOM
categories:
- 前端
- JavaScript
---
![事件流的概念.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/%E4%BA%8B%E4%BB%B6%E6%B5%81.png)
### 事件流
* 捕获事件流：从根节点开始执行，一直往子节点查找执行，直到查找执行到目标节点。
* 冒泡事件流：从目标节点开始执行，一直往父节点冒泡查找执行，直到查到到根节点。

### 阻止冒泡
e.stopPropagation() 可以做到阻止冒泡事件
### 事件捕获
addEventListener() 可以实现事件的捕获
#### 语法
```
document.addEventListener(event, function, useCapture)
```
####     参数
```javascript
event：监听的事件
func：事件捕获后触发的函数
useCapture：boolean 对象。
为 true 时，在事件捕获阶段触发函数
为 false 时，在事件冒泡阶段触发函数
```
