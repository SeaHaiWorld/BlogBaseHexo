---
title: CSS
date: 2018-12-18
categories: 
- 知识积累
---
## css盒模型
CSS 中的盒子模型是一个用来装 html 标签的容器，它包括：边距，边框，填充，和实际内容
```
Margin(外边距) - 清除边框外的区域，外边距是透明的。
Border(边框) - 围绕在内边距和内容外的边框。
Padding(内边距) - 清除内容周围的区域，内边距是透明的。
Content(内容) - 盒子的内容，显示文本和图像。
```
市面上根据浏览器内核的不同，主要分为了 IE 盒子模型和标准 W3C 盒子模型

CSS3中引入了`box-sizing`属性
```
box-sizing: content-box // 表示标准的W3C盒子模型 
box-sizing: border-box // 表示的是IE盒子模型
box-sizing: padding-box // 表示的是IE盒子模型

W3C 盒子模型：包含 content + padding + border + margin，content 就是盒子的实际宽度
IE 盒子模型：包含content + margin，盒子宽度包含 content、padding 和 border
```
## 回流重绘面试题与优化方案
[回流重绘面试题与优化方案](https://blog.csdn.net/sinat_15951543/article/details/86364109)
## transition 和 animation 的区别
```
    transition 和 animation 都随着时间改变元素的属性值
    transition 注重过渡 需要触发事件 transition 只有开始和结束两帧 即 from...to
    animation 可以逐帧执行 不需要事件的触发
```
## Flex 布局
Flex 就是弹性盒子模型，将容器分为了主轴和垂直轴，它包含的子元素自动成为容器内成员
+ 使用容器

```
    display: flex
```
+ 容器属性

```
    flex-direction 决定主轴方向，row为x，column为y轴
    flex-wrap 决定换行规则
    justify-content 决定主轴对齐方式
    align-items 决定交叉轴的对齐方式
    align-content 决定多行交叉轴对齐方式
```
+ 成员属性

```
    flex-grow 元素是否自适应放大，默认为0，不自动撑起剩余空间
    flex-shrink 元素是否自适应缩小，默认为1
    flex-basis 元素的初始默认宽度，值为 auto 时，长度将根据内容决定
    flex 是 flex-grow flex-shrink flex-basis的缩写
    flex：1 1 auto // 简单的多栏布局
    flex：1 1 33.3% // 简单的三栏布局
```
[圣杯布局](https://www.cnblogs.com/whnba/p/10462787.html)
## display
```
    block       	块类型。默认宽度为父元素宽度，可设置宽高，换行显示。
    none        	元素不显示，并从文档流中移除。
    inline      	行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。
    inline-block  默认宽度为内容宽度，可以设置宽高，同行显示。
    list-item   	象块类型元素一样显示，并添加样式列表标记。
    table       	此元素会作为块级表格来显示。
    inherit     	规定应该从父元素继承 display 属性的值。
```
## 三种元素隐藏方式的异同
```
opacity: 0 // 1.不改变布局 2.可触发已经绑定的事件
visibility: hidden // 1.不改变布局 2.不可触发绑定事件
display: none // 1. 改变布局 2.不可触发绑定事件
```
## 双边距重叠问题（外边距折叠）
多个相邻（兄弟或者父子关系）普通流的块元素垂直方向marigin会重叠
外边距都是正数时，折叠结果是它们两者之间较大的值。
外边距都是负数时，折叠结果是两者绝对值的较大值。
外边距一正一负时，折叠结果是两者的相加的和。
## Position
固定定位fixed：相对于浏览器窗口进行定位。

相对定位relative：出现在它所在的位置上。移动时元素“相对于”起点进行移动。元素仍占据原来的空间。

绝对定位absolute：元素的位置相对于最近已定位父元素（设置了绝对定位或者相对定位），如果没有，那么位置相对于<html>。

粘性定位sticky：sticky定位可以被认为是relative和fixed的混合。粘性定位的元素被视为相对定位，直到它超过指定的阈值，此时它被视为固定的，直到它到达其父母的边界。

默认定位Static：默认值。没有定位，元素出现在正常的流中（忽略top, bottom, left, right 或者 z-index 声明）。

inherit：从父元素继承position属性的值。
## 制造BFC的方式
    float属性不为none.
    position属性不为static和relative.
    display属性为下列之一:table-cell,table-caption,inline-block,flex,or inline-flex.
    overflow属性不为visible.
## CSS3新特性
CSS3边框如border-radius，box-shadow等；CSS3背景如background-size，background-origin等
## 标签选择器
优先级顺序为：id 选择器 > class 选择器 > 标签选择器
样式表优先级顺序为：内联样式> 内部样式 > 外部样式 > 浏览器用户自定义样式 > 浏览器默认样式
## css !important
提高指定CSS样式规则的应用优先权。
## 清除浮动的方法
方法一：使用带clear属性的空元素
在浮动元素后使用一个空元素如`<div class="clear"></div>`，并在CSS中赋予`.clear{clear:both;}`

方法二：使用CSS的overflow属性
给浮动元素的容器添加overflow:hidden;或overflow:auto;可以清除浮动
## calc属性
Calc用户动态计算长度值，任何长度值都可以使用calc()函数计算，需要注意的是，运算符前后都需要保留一个空格，例如：width: calc(100% - 10px)；