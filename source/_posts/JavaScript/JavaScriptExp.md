---
title: JS 正则匹配及常用表达式
date: 2019-10-26
tags:
- JavaScript
- 正则匹配
- ES6
categories:
- JavaScript
- 前端
---
## 正则
用于模式匹配 **String** 对象的一些用以执行正则表达式模式匹配和检索替换操作的方法

### String.search
参数是一个正则表达式,返回一个与之匹配的字串的起始位置,如果找不到匹配的字串,它将返回-1.

```javascript
var str = 'javascript';
var reg = /script/i;
console.log(str.search(reg));   // 4
```

### String.replace
用以执行检索与替换操作，其中第一个参数是正则表达式，第二个参数是要进行替换的字符串。
如果正则表达式中设置了修饰符 g ，那么源字符串中所有模式匹配的字串都将替换成第二个参数指定的字符串，如果不带修饰符 g ，则只替换所匹配的第一个字串。如果在替换字符串中出现了 $ 加数字，那么将用与指定的子表达式相匹配的文本来替换这两个字符。第二个参数可以是函数，该函数能够动态的计算替换字符串。

```javascript
var reg = /j(ava)sc(ri)pt/g;
console.log(str.replace(reg, '$1'));    // 'ava'
console.log(str.replace(reg, '$2'));    // 'ri'
console.log(str);   // 'javscript',原字符串保持不变,字符串永远不会变化,除非另外赋值.

var str2 = str.replace(reg, (a, b, c, d, e) => {
    console.log(a); // 'javascript' 匹配字符串
    console.log(b); // 'ava'    $1
    console.log(c); // 'ri'     $2
    console.log(d); // 0    匹配位置
    console.log(e);
    return 1;  // 匹配结果
});
console.log(str2);  // 1
```

### String.match
参数是一个正则表达式，返回的是一个由匹配结果组成的数组。
如果该正则表达式设置了修饰符 g ，则该方法返回的数组包含字符串中的所有匹配结果.

```javascript
// match
var str = 'javascriptjava';
var reg = /java/;
console.log(str.match(reg));   // ['java', 0]

var text = 'Visit my blog at http://www.example.com/~david';
var url = /(\w+):\/\/([\w.]+)\/(\S*)/;
var result = text.match(url);
console.log(result);
console.log(result.length); // 4
for (var i = 0; i < result.length; i++) {
    console.log(result[i]); // 'http://www.example.com/~david',http,'www.example.com','~david'
}
```

### String.split
将调用它的字符串拆分为一个子串组成的数组，参数可以是一个正则表达式。

```javascript
// split
var str = 'abcde';
console.log(str.split('c')); // ['ab', 'de']

// 可以指定分隔符,允许两边可以留有任意多的空白字符
var str = '1,  2,3, 4,    5';
console.log(str.split(/\s*,\s*/));  // [1, 2, 3, 4, 5]
```

## 常用正则表达式
### 日期匹配
- 时间匹配
> 3232319:22:33231321
```
\d{2}:\d{2}:\d{2}
```
> 19:22:33
- 年月日匹配
> 3231971-01-022313
```
\d{4}-\d{2}-\d{2}
```
> 1971-01-02
### 邮箱匹配
> 15615616@qq.com.com
```
([a-zA-Z0-9._-])+@([a-zA-Z0-9_-])+[.]{1}+([a-zA-Z0-9_-])
```
> 15615616@qq.com
### 网址匹配
> https://api.123xxx.cn/ssssssssssss/
```
[\w]{4,5}://+[\w\d.:]+/
```
> https://api.123xxx.cn/
### 手机号码
> 15680173097231
```
1[34578]\d{9}
```
> 15680173097
### 身份证号
*   15位、18位数字
```
/^\d{15}|\d{18}$/
```
*   短身份证号码(数字、字母x结尾)
```
^([0-9]){7,18}(x|X)?$ 或 ^\d{8,18}|[0-9x]{8,18}|[0-9X]{8,18}?$
```
### QQ号
```
/[1-9][0-9]{4,}/
```
