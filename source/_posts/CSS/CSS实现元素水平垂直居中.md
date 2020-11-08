---
title: CSS实现水平垂直居中
date: 2020-02-12
tags:
- CSS
- 面试
categories:
- CSS
---
### 文本居中
```html
<div class="box">
    <span class="child">inline和inline-block元素</span>
</div>

.box {
    width: 400px;
    height: 400px;
    background: #ccc;
    display: table-cell;
    text-align: center; // 文本居中
    vertical-align: middle; // 垂直居中
}
.box .child {
    display: inline-block;
    width: 100px;
    height: 100px;
    background: red;
}
```
### 元素居中
#### 误区：CSS 元素定位为左上角
![css.jpg](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/css%E5%B1%85%E4%B8%AD.jpg)
#### FLEX模型实现
```html
<div class="box">
    <div class="child"></div>
</div>

.box {
    width: 400px;
    height: 400px;
    background: #ccc;
    display:flex;
    justify-content:center; // X轴居中
    align-items: center; // Y轴居中
}
.box .child {
    display: inline-block;
    width: 100px;
    height: 100px;
    background: red;
}
```
#### 绝对定位 + margin 实现（已知宽高）
绝对定位和 margin -left:-自身宽度一半，margin-top: -自身高度的一半 (已知宽高)
```
<div class="box"></div>

.box {
    position: absolute;
    width: 100px;
    height: 100px;
    background: red;
    left: 50%;
    top: 50%;
    color: #fff;
    margin-left: -50px;
    margin-top: -50px;
}
```
#### 绝对定位 + transform 实现（未知宽高）
```
    <div class="box"></div>

    .box {
           position: absolute;
           width: 200px;
           height: 200px;
           background-color: #1E90FF;
           top: 50%;
           left: 50%;
           transform: translate(-50%, -50%);
    }
```

