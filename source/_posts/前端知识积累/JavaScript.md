---
title: JavaScript
date: 2018-12-16
categories: 
- 面试
---
## 闭包
### 定义
闭包函数：有权访问另一个函数作用域中的变量的函数。内部函数可以访问其所在的外部函数中声明的参数和变量，同时让这些变量得以保留即使在其外部函数被返回了之后。
### 作用及优缺点
使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。
### 特性
    1.函数嵌套函数 
    2.函数内部可以引用外部的参数和变量 
    3.参数和变量不会被垃圾回收机制回收
### 闭包内存泄漏缺点的解决
在退出闭包函数之前，将不使用的局部变量删除
### 文章
[深入理解JS闭包](https://blog.csdn.net/cauchy6317/article/details/81167572)
## JS 垃圾回收
JavaScript 中最常用的垃圾收集方式是标记清除（mark-and-sweep）。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。可以使用任何方式来标记变量。比如，可以通过翻转某个特殊的位来记录一个变量何时进入环境，或者使用一个“进入环境的”变量列表及一个“离开环境的”变量列表来跟踪哪个变量发生了变化。说到底，如何标记变量其实并不重要，关键在于采取什么策略。垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记（当然，可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。
## 如何解决异步回调地狱
promise、generator、async/await
## 事件流
HTML 中与 javascript 交互是通过事件驱动来实现的，例如鼠标点击事件 onclick 、页面的滚动事件 onscroll 等等

DOM 标准事件流分为：事件捕获阶段、处于目标阶段、事件冒泡阶段。

分别是从 document 对象到目标节点再到 document 对象
### preventDefault() 
用于阻止元素发生默认的行为
### stopPropagation() 
用于阻止事件冒泡到父元素，阻止任何父事件处理程序被执行。
### addEventListener() 
用于向指定元素添加事件
接收3个参数：要处理的事件名、作为事件处理的函数和一个布尔值。布尔值参数表示在捕获阶段还是在冒泡阶段调用事件处理程序。
### 事件委托
把事件添加到父级上，利用冒泡的原理，等待到达目标子元素冒泡时，通过 e.target.nodeName 判断后，触发事件。一般可以直接使用事件对象或者 addEventListener 来实现
```javascript
window.onload = function(){
　　let oUl = document.getElementById("ul1");
　　oUl.onclick = function(e){
　　　　e = e || window.event;
　　　　let target = e.target || e.srcElement;
　　　　if(target.nodeName.toLowerCase() == 'li'){
　　　　　　　  alert(target.innerHTML);
　　　　}
　　}
}
```
### e.currentTarget 和 e.target
事件冒泡阶段，e.currentTarget 和 e.target 是不相等的，但是在事件的目标阶段，e.currentTarget 和 e.target 是相等的
## 原型及原型链
### prototype (显式原型)
每一个函数都会有一个 prototype 属性，这个属性会指向函数的原型对象，新创建的对象会从原型对象上继承属性和方法。同时 prototype 属性使您有能力向对象添加属性和方法。

prototype 中会包含对象的的构造函数和隐式原型 \__proto\__

显式原型的作用：用来实现基于原型的继承与属性的共享。
### \__proto\__ (隐式原型 )
每一个对象都会有一个 \__proto\__ 属性，这个属性指向该对象的构造函数的原型对象，即隐式原型会指向创建这个对象的构造函数(constructor)的显式原型(prototype)

    对象.__proto__ = 构造函数.prototype
隐式原型的作用：构成原型链，同样用于实现基于原型的继承。
### 原型及原型链
当对象被以某个原型构造时，我们在访问对象的某个属性或方法时，会先在这个对象本身属性上查找，如果没有找到，则会去它的 \__proto\__ (隐式原型)上查找，即它的构造函数的 prototype（显式原型），如果还没有找到就会再在构造函数的 prototype 的 \__proto\__ 上查找，这样一层一层向上查找，直到到 null 还没有找到，则会返回 undefined，这样形成一个链式结构，我们就称这个链式结构为原型链。
[什么是原型和原型链](https://blog.csdn.net/xiaoermingn/article/details/80745117)

![s](https://img2018.cnblogs.com/blog/850375/201907/850375-20190708153139577-2105652554.png)
```javascript
function Parent(parent){
    this.parent = parent;
}

let child = new Parent('Zero');

console.log(child.parent); // 'Zero'
console.log(child.mother); // undefined
console.log(child.father); // undefined

Parent.prototype.parent = 'Two'
Parent.prototype.mother = 'M'
Parent.prototype.father = 'F'

console.log(child.parent); // 这里输出'Zero'，会先在对象本身属性上查找
console.log(child.mother); // 'M'
console.log(child.father); // 'F'

child.parent = 'Two'
console.log(child.parent); // 'Two'
```
```
Parent.prototype:{
    parent: "Two", 
    mother: "M", 
    father: "F", 
    constructor: ƒ,
    __proto__: Object
}
```
![ss](https://img-blog.csdn.net/20180620155400807?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9lcm1pbmdu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
### 原型链的继承
```javascript
function Animal (name) {
// 属性
    this.name = name || 'Animal';
// 实例方法
    this.sleep = function(){
    console.log(this.name + '正在睡觉！');
    }
}
// 原型方法
Animal.prototype.eat = function(food) {

console.log(this.name + '正在吃：' + food);

};
function Cat(){ }
Cat.prototype = new Animal(); //继承
Cat.prototype.name = 'cat';
//　Test Code
var cat = new Cat();
console.log(cat.name); //cat
console.log(cat.eat('fish')); 
console.log(cat.sleep());
console.log(cat instanceof Animal); //true
console.log(cat instanceof Cat); //true
```
### new 操作符干了什么
+ 创建一个新的空对象
+ 然后让这个空对象的 \__proto\__ 指向函数的原型 prototype
+ 将对象作为函数的 this 传进去
+ 返回创建的这个对象

```javascript
function Foo() { };
const a = new Foo();

const obj = new Object();// 创建了一个新的空对象 obj
obj.__proto__ = Foo.prototype;// 让这个 obj 对象的` __proto__`指向函数的原型`prototype`
Foo.call(obj);// this 指向 obj 对象
a = obj;// 将 obj 对象赋给a对象
```
## [JS 监听对象属性的改变](https://www.jianshu.com/p/8fe1382ba135)
## [JS 中的面向对象和它的封装、继承、多态](https://segmentfault.com/a/1190000017342103)
## 如何让事件先冒泡后捕获
先冒泡后捕获的效果，对于同一个事件，监听捕获和冒泡，分别对应相应的处理函数，监听到捕获事件，先暂缓执行，直到冒泡事件被捕获后再执行捕获之间。
## 前端模块化
前端模块化就是复杂的文件编程一个一个独立的模块，比如js文件等等，分成独立的模块有利于重用（复用性）和维护（版本迭代），这样会引来模块之间相互依赖的问题，所以有了commonJS规范，AMD，CMD规范等等，以及用于js打包（编译等处理）的工具webpack
### 说一下Commonjs、AMD和CMD
CommonJS定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}
require()用来引入外部模块；exports对象用于导出当前模块的方法或变量，唯一的导出口；module对象就代表模块本身。
### AMD：异步模块定义
requireJS实现了AMD规范，按照模块加载方法，通过define()函数定义，第一个参数是一个数组，里面定义一些需要依赖的包，第二个参数是一个回调函数，通过变量来引用模块里面的方法，最后通过return来输出。
### CMD：同步模块定义
SeaJS实现了CMD规范
## mouseover和mouseenter的区别
   mouseover：当鼠标移入元素或其子元素都会触发事件，所以有一个重复触发，冒泡的过程。对应的移除事件是mouseout
   mouseenter：当鼠标移入元素本身（不包含元素的子元素）会触发事件，也就是不会冒泡，对应的移除事件是mouseleave

## 原生ajax封装成promise
```javascript
var myajax = function(url,options) {
  return new Promise(function(resolve, reject) {
    var xhr = new XMLHttpRequest();
    xhr.open(options,url);
    xhr.send(data);
    xhr.onreadystatechange=function() {
      if(xhr.status==200&&readyState==4){
            var json = JSON.parse(xhr.responseText);
            resolve(json)
      } else if(xhr.readyState==4&&xhr.statusa!=200){
            reject(xhr.statusText)
      }
    }
    xhr.onerror = function(){
        reject(Error("网络异常"));
    }
  })
}
```
## 判断类型
typeof()(不可以判断数组，会判断成 Object)，instanceof，Object.prototype.toString.call()
## 判断数组
isArray()方法、Object.prototype.toString.call()、instanceof Array
```javascript
let array = new Array();
console.log(Object.prototype.toString.call(a)) // [object Array]
console.log(Array.isArray(a)) // true
console.log(a instanceof Array) // true
```
## 数组去重
法一：indexOf循环去重
```javascript
var arr = [1,3,4,5,6,7,4,3,2,4,5,6,7,3,2];
Array.prototype.filterArray = function (){
	var newArr = [];
	for (var i = 0; i < arr.length; i++) {
		if (newArr.indexOf(arr[i]) == -1 ) {
			newArr.push(arr[i]);
		}
	}
	return newArr
}
```
法二：ES6 Set去重
```javascript
Array.from(new Set(array))
```
法三：Object 键值对去重
把数组的值存成 Object 的 key 值，比如 Object[value1] = true，在判断另一个值的时候，如果 Object[value2]存在的话，就说明该值是重复的。
```javascript
Array.prototype.filterArray = function(){
	for(var i=0, temp={}, result=[], ci; ci=this[i++];){
	    var ordid = ci.ordid
	    if(temp[ordid]){
	        continue
	    }
	    temp[ordid] = true
	    result.push(ci)
	}
    return result
}
```
## Js基本数据类型
基本数据类型：undefined、null、number、boolean、string、symbol
引用数据类型：Object
区别：基本数据类型（存放在栈中），引用数据类型（存放在堆中）
## Promise
Promise对象，保存着未来将要结束的事件 本质分离了异步数据获取和业务逻辑 1、代表异步操作，对象状态不受外部影响。三种状态，pending，fulfilled，rejected，只有异步操作的结果，决定是哪种状态，任何其他操作无法改变这个状态。2、状态不可逆一旦状态改变，就不会再变，promise对象状态改变只有两种可能，从pending到fulfilled或pending到rejected，只要这两种情况发生，状态就凝固了，不可再改变，这个时候就称为定型resolved,
1.then() ：成功后执行的方法，参数是匿名函数，参数有两个，表示的含义不同。当有两个参数时，第一个表示成功后执行，第二个表示失败后执行
2.Catch() :失败后执行，参数是一个匿名函数
async 异步函数，返回一个promise对象，可以用Promise的Api
await 该异步操作,会暂停执行下面的操作,等到该步执行完,再执行下面的操作

API：https://www.cnblogs.com/Alisa-k/p/12672870.html

## 跨域
跨域问题的提出是基于同源策略的概念，也就是协议、域名、端口号三者保持一致才允许通信。跨域可以分为数据请求跨域和DOM查询跨域。
跨域方式：
可以使用的jsonp方式，它需要前后端相互配合，原理是使用src本身支持跨域，缺点是它只支持get方法
利用`<script>`标签没有跨域限制的漏洞，网页可以得到从其他来源动态产生的 JSON 数据。JSONP请求一定需要对方的服务器做支持才可以。
还可以使用CORS设置请求头使其允许跨域，这种方式的优点是支持所有请求方式，也可以进行DOM查询，并且也要求有后端的配合。
还可以使用iframe配合window自带的name属性，这种方式优点是操作简单，不要求后端配合，缺点是name的大小有限制，一般是2M左右。
此外还有h5新增的postMessage方法，通过onmessage的监听来接收数据，好处是使用简单，缺点是早期浏览器不支持。
还可以通过修改document.domain来实现跨域，只需要把两个网页的domain设置为一个主域即可，优点也是支持DOM查询，操作简便，缺点是它只适用于相同主域的子域之间进行跨域。

## 常见web攻击及防范
XSS攻击 跨域脚本攻击
①编码：对敏感字符进行转义，才能进行下一步操作，<>、$、'等字符，alert、$
②过滤：onclick等事件、style、script节点、iframe节点
CSRF攻击 跨站请求伪造
①同源检测
②Token验证
（原理的区别）：XSS：是向网站 A 注入 JS代码，然后执行 JS 里的代码，篡改网站A的内容。CSRF：是利用网站A本身的漏洞，去请求网站A的api。
SQL注入攻击
文件上传漏洞
DDoS攻击
其他攻击手段
## react生命周期函数和react组件的生命周期
React的组件在第一次挂在的时候首先获取父组件传递的props，接着获取初始的state值，
接着经历挂载阶段的三个生命周期函数，也就是ComponentWillMount，render，ComponentDidMount，这三个函数分别代表组件将会挂载，组件渲染，组件挂载完毕三个阶段，
在组件挂载完成后，组件的props和state的任意改变都会导致组建进入更新状态，
在组件更新阶段，如果是props改变，则进入ComponentWillReceiveProps函数，接着进入ComponentShouldUpdate进行判断是否需要更新，
如果是state改变则直接进入ComponentShouldUpdate判定，这个默认是true，当判定不需要更新的话，组件继续运行，
需要更新的话则依次进入ComponentWillMount，render，ComponentDidMount三个函数，
当组件卸载时，会首先进入生命周期函数ComponentWillUnmount,之后才进行卸载
## MVC和MVVM
MVVM即Model-View-ViewModel的简写。即模型-视图-视图模型。模型（Model）指的是后端传递的数据。视图(View)指的是所看到的页面。视图模型(ViewModel)是mvvm模式的核心，它是连接view和model的桥梁。它有两个方向：一是将模型（Model）转化成视图(View)，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将视图(View)转化成模型(Model)，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。这两个方向都实现的，我们称之为数据的双向绑定。
  
MVC是Model-View- Controller的简写。即模型-视图-控制器。M和V指的意思和MVVM中的M和V意思一样。C即Controller指的是页面业务逻辑。使用MVC的目的就是将M和V的代码分离。MVC是单向通信。也就是View跟Model，必须通过Controller来承上启下。MVC和MVVM的区别并不是VM完全取代了C，只是在MVC的基础上增加了一层VM，只不过是弱化了C的概念，ViewModel存在目的在于抽离Controller中展示的业务逻辑，而不是替代Controller，其它视图操作业务等还是应该放在Controller中实现。也就是说MVVM实现的是业务逻辑组件的重用，使开发更高效，结构更清晰，增加代码的复用性。
## js中this的指向
this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象
如果return返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数
### js中的调用模式
    箭头函数
    关键字new调
    显式绑定
    隐式绑定
    默认绑定
## 改变函数内部this指针的指向函数（bind，apply，call的区别）
通过apply和call改变函数的this指向，他们两个函数的第一个参数都是一样的表示要改变指向的那个对象，
第二个参数，apply是数组，而call则是arg1,arg2...这种形式。通过bind改变this作用域会返回一个新的函数，这个函数不会马上执行。
https://www.runoob.com/w3cnote/js-call-apply-bind.html
## 知道哪些ES6，ES7的语法
promise，async/await，let、const、块级作用域、箭头函数、对象展开符
## 箭头函数
箭头函数是匿名函数，不能作为构造函数，不能使用new
箭头函数不绑定arguments，取而代之用rest参数...解决
箭头函数不绑定this，会捕获其所在的上下文的this值，作为自己的this值
箭头函数通过 call() 或 apply() 方法调用一个函数时，只传入了一个参数，对 this 并没有影响。
箭头函数没有原型属性
箭头函数的this不是调用的时候决定的，而是定义的时候它所处的上下文环境决定的
如：箭头函数**定义**在全局，则this是window
对箭头函数的扩展理解：
箭头函数的this看它定义的时候外层是否有函数:
如果有，该箭头函数的this是外部函数的this;
如果没有，则this指向window。
## 块级作用域
let const var的区别：作用域不同
作用域不同
var、let与const区别：
const声明的是常量，必须赋值，如果声明的是复合类型数据，可以修改其属性。let和var声明的是变量，声明时可以不赋值。let const 是块级作用域 var 是函数级作用域。块级作用域是指{ }内有效的作用域。const let不存在变量提升。声明的变量一定要在声明后使用。var存在变量提升。变量可以在声明之前调用，值为undefined。   em根据父元素字体大小决定，rem根据根元素的字体大小决定单位。

什么是块级作用域
块级作用域是指{ }内有效的作用域。
let const 是块级作用域
var 是函数级作用域

是否有变量提升
const let 是块级作用域，不存在变量提升

let特性
块级作用域 {}
在块级作用域内存在变量死区问题
不存在变量提升
## null和undefined的区别
在JavaScript中，null 和 undefined 几乎相等
在 if 语句中 null 和 undefined 都会转为false两者用相等运算符比较也是相等
null表示没有对象，即该处不应该有值
undefined表示缺少值，即此处应该有值，但没有定义
null和undefined转换成number数据类型时，null 默认转成 0，undefined 默认转成 NaN
## indexof和findIndex
indexOf是判断数组中某个元素是否存在,不存在则返回-1
findIndex是用来查找索引的,返回的查找到的符合项的索引
## JavaScript中的轮播实现原理？假如一个页面上有两个轮播，你会怎么实现？
图片轮播的原理就是图片排成一行，然后准备一个只有一张图片大小的容器，对这个容器设置超出部分隐藏，在控制定时器来让这些图片整体左移或右移，这样呈现出来的效果就是图片在轮播了。
如果有两个轮播，可封装一个轮播组件，供两处调用
## Event Loop 
https://blog.csdn.net/itKingOne/article/details/86502910?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param
所以 Event Loop 执行顺序如下所示：
   首先执行同步代码，这属于宏任务
   当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行
   执行所有微任务
   当执行完所有微任务后，如有必要会渲染页面
   然后开始下一轮 Event Loop，执行宏任务中的异步代码，也就是 setTimeout 中的回调函数
## nodejs多线程
## 高并发问题
## EventLoop
Event Loop 事件循环。JS是单线程执行，因此为了更好的区分同步、异步将事件分为了宏任务和微任务。在事件执行时，会先执行同步代码，再执行宏任务执行并拿到一个回调，再执行微任务也拿到一个回调，再微任务的回调拿到后，才拿宏任务的回调。以此循环。
## rem实现原理
