---
title: 用 async/await 来处理异步
date: 2019-12-16 20:10:33
tags:
- nodejs
- 异步
categories:
- nodejs
---

## 前言
最近在通过 jest-puppeteer E2E 测试用例，使用 async/ await 来实现异步操作，以实现模拟用户操作。

async/await 现在已经被标准化，是时候学习一下了。
### async的用法
作为一个关键字放到函数前面，用于表示函数是一个异步函数，因为 async就是异步的意思，异步函数也就意味着该函数的执行不会阻塞后面代码的执行。写一个 async 函数
```javascript
    async function timeout() {
        return 'hello world'
    }
    timeout();
    console.log('虽然在后面，但是我先执行');
```
<!--more-->
打开浏览器控制台，我们可以看到

![console.png](	https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/console.png)

#### Promise 对象
async 函数返回的是一个 promise 对象，如果要获取到 promise 返回值，我们应该用 then 方法
```javascript
    async function timeout() {
        return 'hello world'
    }
    timeout().then(result => {
    console.log(result);
    })
    console.log('虽然在后面，但是我先执行');
```
控制台如下：

![console1.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/console1.png)

我们获取到了 hello world ,  同时 timeout 的执行也没有阻塞后面代码的执行，和我们刚才说的一致。

#### 抛出错误
如果函数内部抛出错误， promise 对象有一个 catch 方法进行捕获错误。
```javascript
    timeout(false).catch(err => {
        console.log(err)
    })
```
### await 的用法
await 是等待的意思，那么它等待什么呢，它后面跟着什么呢？其实它后面可以放任何表达式，不过我们更多的是放一个返回 promise 对象的表达式。注意 await 关键字只能放到 async 函数里面

现在写一个函数，让它返回 promise 对象，该函数的作用是2s 之后让数值乘以2
```javascript
// 2s 之后返回双倍的值
    function doubleAfter2seconds(num) {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve(2 * num)
            }, 2000);
        } )
    }
```
现在再写一个 async 函数，从而可以使用 await 关键字， await 后面放置的就是返回 promise 对象的一个表达式，所以它后面可以写上 doubleAfter2seconds 函数的调用
```javascript
    async function testResult() {
        let result = await doubleAfter2seconds(30);
        console.log(result);
    }
    testResult();
```
打开控制台，2s 之后，可以看到输出了60。
