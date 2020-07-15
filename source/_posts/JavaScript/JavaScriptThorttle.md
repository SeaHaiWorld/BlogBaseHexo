---
title: JS 性能优化之节流
date: 2019-11-26
tags:
- JavaScript
- 性能优化
categories:
- JavaScript
- 前端
- 性能优化
---
### 前言

在 [《JS 性能优化之防抖》](https://segmentfault.com/a/1190000021501607#shareToWeibo)中，我们了解了为什么要限制事件的频繁触发，以及如何做限制：
* throttle 节流
* debounce 防抖

### 节流
#### 原理：
在一定时间内，持续触发时间，事件只会在设定的时间内触发一次。
> 深入理解: 开闸放水。我想让你1s只流出多少水你就只能流多少水，多的水只能等到下个周期才能流出。

#### 使用时间戳
当触发事件的时候，我们取出当前的时间戳，然后减去之前的时间戳(最一开始值设为 0 )，如果大于设置的时间周期，就执行函数，然后更新时间戳为当前的时间戳，如果小于，就不执行。
```javascript
// 时间戳
function throttle(func, wait) {
    var context, args;
    var old = 0;

    return function() {
        var now = +new Date();
        context = this;
        args = arguments;
        if (now - old > wait) {
            func.apply(context, args);
            old = now;
        }
    }
}
```
#### 使用定时器
当触发事件的时候，我们设置一个定时器，再触发事件的时候，如果定时器存在，就不执行，直到定时器执行，然后执行函数，清空定时器，这样就可以设置下个定时器。
```javascript
// 定时器
function throttle(func, wait) {
    var context, args;
    var timer;
    return function() {
        context = this;
        args = arguments;
        if (!timer) {
            timer = setTimeout(function(){
                timeout = null;
                func.apply(context, args)
            }, wait)
        }
    }
}
```
### 推荐阅读
[JS 性能优化之防抖](https://segmentfault.com/a/1190000021501607#shareToWeibo)
