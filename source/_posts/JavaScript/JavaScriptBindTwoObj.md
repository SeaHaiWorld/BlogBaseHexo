---
title: JS 实现双向数据绑定 MVVM
date: 2020-02-13
tags:
- JavaScript
categories:
- 前端
- JavaScript
---
### 双向数据绑定
```javascript
Event.onchange; // 事件会在域的内容改变时发生。
Object.defineProperty(obj,key,value) // 在一个对象上存取数据，或者修改数据
```
### 输入框绑定
```javascript
// html
<input id="input" type="text" />

// javascript
const data = {};
const input = document.getElementById('input');
Object.defineProperty(data,'text',{
    set(value){
        input.value = value;
        this.value = value;
    }
});

input.onchange = (e) =>{
    data.text = e.target.value;
}
```
