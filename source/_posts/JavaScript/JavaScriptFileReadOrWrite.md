---
title: JS 文件二进制上传/读取的机制
date: 2020-02-07
tags:
- JavaScript
- 文件读写
- ES6
categories:
- JavaScript
- 前端
---
## 前言

最近写项目时遇到了向后台请求文件时，后台返回传输文件格式为二进制流的情况。正好借此机会了解文件二进制上传/读取的机制。
## Blob 对象
文档：[BLOB - MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

Blob对象表示一个不可变的, 原始数据的类似文件对象，可以作为二进制文件存放的容器
 
### 构造函数
```javascript
    var blob = new Blob(array[optional], options[optional]);
```
* 第一个参数：任意类型的对象的混合体
* 第二个参数：用于指定将要放入 Blob 中的数据的类型

### 基本属性
*   size ：Blob 对象包含的字节数。(只读)
*   type ：Blob 对象包含的数据类型，如果类型未知则返回空字符串

## 文件二进制传输之上传
### 创建 FormData 对象异步上传二进制文件
文档：[FormData - MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)
```javascript
FormData.append(); // 添加键值对
FormData.delete(); // 删除键值对
FormData.entries(); // 返回允许遍历此对象中包含的所有键/值对的迭代器
```
创建一个空 FormData 对象，向 FormData 对象中添加文件,创建 xhr 请求发送至服务器
```javascript
    let formData = new FormData();
    formData.append('file', file)
    let xhr = new XMLHttpRequest();
    xhr.open('POST', url, true);
    xhr.send(formData);
```
## 文件二进制传输之读取
文件二进制流转文件下载的方法：
* 通过 FileReader 对象实现异步读取二进制文件
* 通过 Blob 对象和 msSaveBlob(blob, fileName) 以本地方式保存文件
* 通过 createObjectURL 把 Blob 对象指向到一个 URL, 再赋予到a标签

### 创建 FileReader 对象异步读取二进制文件
文档：[FileReader - MDN Web Docs](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)
```javascript
FileReader.onload; //事件在读取完成后触发。
FileReader.readAsDataURL(); // 转换为base64
```
#### 验证码示例
```html
    <img id="captcha" alt="captcha" src=""/>
```
```javascript
    // xhr
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url, true);
    xhr.responseType = "blob";
    xhr.onload = function () {
        // 请求完成
        if (this.status === 200) {
            // 返回200
            let blob = this.response;
            let reader = new FileReader(); // 创建 FileReader() 对象
            reader.readAsDataURL(blob); // 转换为base64，直接放入src
            let captcha = document.getElementById('captcha');
            reader.onload = function(e){
                captcha.src = e.target.result; // 输入文件流
                console.log(captcha.src);
            };
            captcha.onload = function() {
                window.URL.revokeObjectURL(captcha.src); // 释放
            };
        }
    };
    // 发送ajax请求
    xhr.send();
```
#### 下载文件示例
```javascript
    let blob = this.response;
    let reader = new FileReader();  
    reader.readAsDataURL(blob); // 转换为base64，直接放入a标签href  
    reader.onload = function(e) {  
        // 转换完成，创建一个a标签用于下载  
        let a = document.createElement('a');  
        a.href = e.target.result;
        a.download = 'fileName';
    };
    a.onload = function() {
        window.URL.revokeObjectURL(a.href); // 释放
    };
    document.body.appendChild(a); // 修复firefox中无法触发click  
    a.click();  
    document.body.removeChild(a);
```
### 通过 msSaveBlob(blob, fileName) 保存文件
```javascript
    if (window.navigator.msSaveOrOpenBlob) {  
        navigator.msSaveBlob(blob, fileName);  
    }
```

## 参考资料
[JavaScript 中 Blob 对象（掘金网）](https://juejin.im/entry/5937c98eac502e0068cf31ae)
[JS : Blob() 转换二进制下载文件流实例（CSDN论坛）](https://blog.csdn.net/weixin_34113237/article/details/88030350)