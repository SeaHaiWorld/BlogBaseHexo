---
title: JS 性能优化之防抖
date: 2019-11-26
tags:
- JavaScript
- 性能优化
categories:
- JavaScript
- 前端
- 性能优化
---
## 前言
在**前端开发**中经常会遇到一些频繁触发的事件：
* scorll、focus
* mousedown、mousemove
* keydown、keyup
 ...

这些频繁触发的事件中，如果包含回调函数则不仅可能导致**性能降低**还可能直接导致**页面崩溃**；如果包含 ajax 请求则会大量占用服务器**带宽**。节流（throttle）和防抖（debounce）,是两个类似又有些不同的优化方案。

我们举个示例代码，实现事件频繁的触发并逐步优化以实现节流（throttle）和防抖（debounce）的效果：

```html
// index.js
<!DOCTYPE html>
<html>
<body>
    <div id = "container" style = "width: 100%;height:200px; line-height: 200px; text-align:center; color: #fff; background-color: #444;font-size: 30px;">
    </div>
</body>
<script>
    var count = 1;
    var container = document.getElementById('container');

    function getUserAction() {
        container.innerHTML = count++;
        console.log(count)
    };
    
    function debounce(){
    }
    
    function throttle(){
    }
    
    container.onmousemove = getUserAction;
</script>
</html>
```
代码效果如下：随着鼠标的移动函数会不断的执行，在一秒内执行了200次余
![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/debounce.png)

如果函数是复杂的**回调函数**或是 ajax 请求，浏览器将完全反应不过来。假设 1 秒触发了 50 次，每个回调就必须在 1000 / 50 = 20ms 内完成，否则就会有卡顿出现。

为了解决这个问题，一般有两种解决方案：
* debounce 防抖
* throttle 节流


## 防抖
### 原理

言简意赅:函数的执行者冷静下来后（不一直抖动后），才真正执行。

深入理解：触发事件时，只在事件触发 n 秒后才执行，如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新事件触发的时间为准，n 秒后才执行。比如频繁触发的某一函数，防抖可以只在最后一次触发后执行。总之，就是要等你触发完事件的 n 秒内不再触发事件，才执行。

在了解了防抖的原理之后，我们要写出防抖函数是很容易的
```html
<script>
...
    function debounce(func,wait){
        var timer;
        return function(){
            clearTimeout(timer);
            timer = setTimeout(func,wait)
            }
    }
    
    container.onmousemove = debounce(getUserAction,1000);
</script> 
```
在使用了 debounce 处理后，在鼠标停止移动的1秒后才会执行一次操作。

我们现在实现了最基本的防抖函数，但是我们在实际使用者会发现存在一些问题：

1. this指向
2. event指向

### 1. this 指向
分别在使用 debounce 前和使用 debounce 后打印this。

```javascript
    function getUserAction(e) {
        console.log(this)
        container.innerHTML = count++;
    };
```
在不使用 debounce 前，我们如果在 getUserAction 打印this。这里指向的是 id 为 container 的元素。

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/debounce.png)

在使用 debounce 后，this应该指向绑定的DOM元素，可this 却不正确的指向了window

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/debounce1.png)

因此我们需要将 this 指向正确的对象。

我们修改下代码：
```javascript
// 第二版
function debounce(func, wait) {
    var timer;
    return function () {
        var context = this;
        clearTimeout(timer)
        timer = setTimeout(function(){
            func.apply(context) // 修复this指向问题
        }, wait);
    }
}
```
现在 this 已经可以正确指向了。


### 2. event 对象
```javascript
    function getUserAction(e) {
        console.log(e)
        container.innerHTML = count++;
    };
```
在不使用 debounce 前，我们如果在 getUserAction 打印e，监听的是 mouseevent 这个事件。

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/debounce2.png)

在使用 debounce 后，e 却不再正确监听，打印为 undefined

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/debounce3.png)

所以我们需要监听正确的事件

我们修改下代码：
```javascript
// 第三版
function debounce(func, wait) {
    var timer;
    return function () {
        var context = this;
        var e = arguments;
        clearTimeout(timer)
        timer = setTimeout(function(){
            func.apply(context,e) // 修复 this、e 指向问题
        }, wait);
    }
}
```

这样我们就用JS实现了防抖，优化了操作时的频繁触发。

## 总结
本文章主要讲述的在前端开发中解决频繁触发事件（滚动、频繁点击...）的重要性，介绍了优化方案。同时带领大家一起了解防抖的原理，并完成了函数的构建。

欢迎大家继续阅读[《JS 性能优化之节流》](https://segmentfault.com/a/1190000021501988)，该文章将主要介绍函数节流。最后，喜欢的朋友记得给作者点个赞！
