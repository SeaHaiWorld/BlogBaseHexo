---
title: JS 中几种常见的遍历
date: 2019-8-25
tags:
- JavaScript
- 函数遍历
- ES6
categories:
- JavaScript
- 前端
---
### forEach
按照数组或对象中的顺序对当前的元素做一些什么，具体做什么，随便
```javascript
   arr[ ].forEach((item, index, array)  {
        // foreach不支持在循环时添加删除操作
        // 随便做什么
        // item：当前项，index：当前项的索引， array：原始数组
        // 匿名函数中的this 都是指向Window
        // 可以没有返回
      })
```

```javascript
   var ary = [1,2,3,4,5];
   var res = ary.forEach((item, index, arr)  {
       arr[index] = item * 10;
   })
```
console.log(res) - undefined

console.log(ary) - Array (5) [10, 20, 30, 40, 50]
<!--more-->

### filter
即过滤器。

例如将数组中大于10的元素放到一个新的数组中，即将数组中的每一项和10做比较，大于10的项放到一个新的数组中。
```javascript
   var temp = [0, 5, 10, 20, 30].filter((item, index)  {
       console.log(item); // 数组中的每一项
       console.log(index); // 每一项的索引
       return item 10 // 返回大于10的项
   })
```
console.log(temp) - Array (2) [20, 30]

### map
即克隆。

将原始数组中每一项克隆，放到一个新的数组中，结束时，得到一个新的数组，原始数组不变，新数组中的顺序和原始数组中一样
```javascript
   arr[ ].map((item, index, array)  {
       // 针对每一项做点什么
       // item：当前项，index：当前项的索引， array：原始数组
       // 可以返回一个对象，用于数组元素转对象
       return XXX // 返回操作后的新项
   })
```
``` javascript
   var arr2 = [1,2,3,4,5]
   var res2 = arr2.map((item, index, array)  {
       return item * 100
   })
```
console.log(res2) - Array (5) [100, 200, 300, 400, 500] // 原数组拷贝了一份，并进行了修改

console.log(arr2) - Array (5) [1, 2, 3, 4, 5] // 原数组并未发生变化
```javascript
   // 数组元素转对象
   var arr3 = [1,2,3,4,5]
   var obj = arr3.map((item)  ({
           key: item,
           value: item,
   }));
```
console.log(obj) - Array (5) [{key: 1}, {key: 2}, {key: 3}, {key: 4}, {key: 5}]
### Object.keys()
返回对象中每一项的key的数组
```javascript
   var obj = {'0':'a','1':{'key':'1'},'2':10,'3':'xxx'}
      var keysArr = Object.keys(obj);
``` 
console.log(keysArr); - Array (4) ['0', '1', '2', '3']

结合forEach消除空格
```javascript
   removeBlankSpace = obj  Object.keys(values)
      .forEach((key)  {
         values[key] = values[key].trim()
      }
   )
```
结合map获取对象的值
```javascript
   getValue = obj 
      Object.keys(obj)
         .map(key  obj[key])
            .join(',')
```
### reduce
累加器。

从数组中第一项开始，每检查一项，就和前面的总和加在一起，加到最后返回总和
```javascript
   var temp = [1, 2, 3, 4 ,5]
   .reduce((accumulator, current, index, array)  {
       console.log(accumulator) // 累加器
       console.log(currentValue) // 当前值
       console.log(currentIndex) // 当前项的索引
       console.log(array) // 原始数组
       return accumulator + currentValue; // 返回所有项的总和
   });
```
console.log(temp) - Number 15
### Set
ES6 中新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set本身是一个构造函数，用来生成 Set 数据结构。可通过 add() 方法向 Set 结构加入成员， Set 结构不会添加重复的值，通过 Array.from 方法将 Set 结构转为数组。
```javascript
   const array = [1, 1, 2, 2, 3];
   function removeDedupe(array) {
     const set = new Set(array)
     return Array.from(set);
   }
```
console.log(set) - Set (3) [1, 2, 3]

console.log(array) - Array (3) [1, 2, 3]

Set数据结构类似于数组，但成员的值都是唯一的，没有重复的值。
```javascript
   let set = new Set();
   set.add({});
   set.size // 1
   set.add({});
   set.size // 2
```
Set数据结构中，两个对象总视为不相等的（即使为空）

#### Set 结构的实例操作方法
```javascript
Set.prototype.add(value) // 添加某个值，返回 Set 结构本身。
Set.prototype.has(value)// 返回一个布尔值，表示该值是否为Set的成员。
Set.prototype.delete(value)// 删除某个值，返回一个布尔值，表示删除是否成功。
Set.prototype.clear()// 清除所有成员，没有返回值。
```
#### Set 结构的实例遍历方法
```javascript
Set.prototype.keys() // 返回键名的遍历器
Set.prototype.values() // 返回键值的遍历器
Set.prototype.entries() // 返回键值对的遍历器
Set.prototype.forEach() // 使用回调函数遍历每个成员
```
