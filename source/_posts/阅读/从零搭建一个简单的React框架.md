---
title: 读《从零编写一个简单 React 框架》有感
date: 2020-02-15
tags: 
- 框架
- 注册命令
categories: 
- React
---
## _What is Virtual DOM_ ？
_Virtual DOM_ 就是将 _DOM_ 抽象成一个以 JavaScript 对象为节点的虚拟 _DOM_ 树，对这颗抽象树进行创建节点，删除节点以及修改节点的操作
```javascript
const obj = {
    tag:'div',
    attrs:{
    className:"test"
    },
    children:[
    tag:'span',
    attrs:{
    className:"text"
    },]
}
```
## _Why use Virtual DOM_ ？
在直接操作 _DOM_ 时浏览器会执行该操作。当在频繁操作 _DOM_ 元素时，不断重复执行操作，大大影响性能。
_React Virtual DOM_ 经过 _diff_ 算法得出一些需要修改的最小单位，在实现最少的 _DOM_ 操作前提下更新视图
## _How to create Virtual DOM_ ？
* 基于 _babel_ 的在线编辑器，可以将先把代码变成抽象语法树
* 进行对应的处理
* 输出成浏览器可以识别的代码‑即 js 对象

```javascript
class App extends React.Component{
render(){
    return <div>123</div>
    }
}
```
会被编译成
```javascript
...
_createClass(App, [{
    key: "render",
    value: function render() {
    return React.createElement("div", null, "123");
    }
}]
);
```
最核心的一段 jsx 代码, `return <div>123</div>` 被转换成了： `return
React.createElement("div", null, "123")`;
可见，我们在用 React 写 jsx 代码时，都会被转换成 `React.createElement` 的形式


## _Implement a Simple React frame_
简单的说：_React frame_ = _Virtual DOM_ + diff 算法 + 生命周期优化 
那么，要实现框架，首先要有把 _Virtual DOM_ 转换为真实的 _DOM_ 的方法
### 创建 React 对象
```javascript
const React = {};
React.createElement = function(tag, attrs, ...children) {
return {
    tag,
    attrs,
    children
    };
};
export default React;
```
上述方法是为了将 _Babel_ 转换的代码中把对应的参数变成一个特定格式(_DOM_ 结构)的对象，然后返回
### _render_ 方法的实现
* 定义 ReactDOM 对象
* 实现 render 方法，实现虚实转换
* 考虑子节点的嵌套，对子节点递归渲染子节点

```javascript
// ReactDom
const ReactDom = {};
//vnode 虚拟 dom ，即 js 对象
//container 即对应的根标签 包裹元素
const render = function(vnode, container) {
    return container.appendChild(_render(vnode));
};
ReactDom.render = render;
```
```javascript
//_render 方法
//_render 方法，接受虚拟 dom 对象，返回真实 dom 对象：
export function _render(vnode) {
  console.log('_render');
  // 如果传入的是 null ,字符串或者数字 那么直接转换成真实 dom 
  if (vnode === undefined || vnode === null || typeof vnode === 'boolean')
    vnode = '';
  if (typeof vnode === 'number') vnode = String(vnode);

  if (typeof vnode === 'string') {
    let textNode = document.createTextNode(vnode);
    return textNode;
  }
  if (typeof vnode.tag === 'function') {
    const component = createComponent(vnode.tag, vnode.attrs);
    setComponentProps(component, vnode.attrs);
    return component.base;
  }

  const dom = document.createElement(vnode.tag);
  // 传入的是个 dom 元素，而且它有属性，需要一个 handleDealAttrs 方法，处理成真实 dom 的属性
  if (vnode.attrs) {
    Object.keys(vnode.attrs).forEach(key => {
      const value = vnode.attrs[key];
      handleDealAttrs(dom, key, value);
    });
  }

  vnode.children && vnode.children.forEach(child => render(child, dom)); // 递归渲染子节点

  return dom;
}
```
```javascript
function handleDealAttrs(dom, name, value) {
  if (name === 'className') name = 'class';
  if (/on\w+/.test(name)) {
    name = name.toLowerCase();
    dom[name] = value || '';
  } else if (name === 'style') {
    if (!value || typeof value === 'string') {
      dom.style.cssText = value || '';
    } else if (value && typeof value === 'object') {
      for (let name in value) {
        dom.style[name] =
          typeof value[name] === 'number' ? value[name] + 'px' : value[name];
      }
    }
  } else {
    if (name in dom) {
      dom[name] = value || '';
    }
    if (value) {
      dom.setAttribute(name, value);
    } else {
      dom.removeAttribute(name);
    }
  }
}
```
### _diff_ 算法的实现
两种 diff 算法：
* 将新 _Virtual DOM_ 和旧 _DOM_ 对比
* 将新 _Virtual DOM_ 和旧 _Virtual DOM_ 对比

### _Component_ 生命周期优化

