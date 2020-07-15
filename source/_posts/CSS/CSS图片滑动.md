---
title: CSS 图片滑动
date: 2019-07-08
tags:
- CSS
categories:
- 前端
- CSS
---
### CSS 图片滑动
鼠标移入图片换为第二张图片，第二张图片从下向上滑动。鼠标移出，第二张图片向下滑动露出第一张图片。
```
//html
<div class="box">
    <img class="img1"/>
    <img class="img2"/>
</div>
```
```
//css
        .box{
            width: 300px;
            height: 800px;
        }
        .img1 {
            width: 200px;
            height: 200px;
            background-color: red;
        }
        .img2 {
            width: 200px;
            height: 200px;
            background-color: blue;
            transition: all 1s ease-in-out;
        }
        .box:hover
            .img2{
            transform: translateY(-100%);
        }
```
![scorll.jpg](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/scorll.gif)