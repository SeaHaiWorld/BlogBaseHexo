---
title: Thinkjs - CRUD 操作
date: 2019-9-22
tags:
- Thinkjs
- 数据库
- MySQL
categories:
- 后端
---
### 增
**添加单条数据**
使用 add 方法可以添加一条数据，返回值为插入数据的 id。如：
```javascript
      async addAction(){
        const model = this.model('user');
        const id = await model.add({name: 'xxx', id: 'yyy'});
      }
```
#### **添加多条数据**
使用 addMany 方法可以添加多条数据，如：
```javascript
  async addAction(){
    const model = this.model('user');
    const insertId = await model.addMany(
     [
       {name: 'xxx1', pwd: 'yyy1'},
       {name: 'xxx2', pwd: 'yyy2'}
     ]
   )}
```
>注：addMany添加多条数据时应该插入一个数组
<!--more-->

### 删
使用 delete 方法来删除数据。如：
```javascript
  async deleteAction(){
    let model = this.model('user');
    let affectedRows = await model.where({id: ['>', 100]}).delete();
  }
```
### 改
使用 update 方法来更新数据。如：
```javascript
   module.exports = class extends think.Controller {
     async updateAction(){
       let model = this.model('user');
       let affectedRows = await model.where({name: 'thinkjs'}).update({email: 'admin@thinkjs.org'});
     }
   }
```
### 查
#### **查询单条数据**
使用 find 方法查询单条数据，返回值为对象。如：
```javascript
  async listAction(){
    const model = this.model('user');
    const data = await model.where({name: 'thinkjs'}).find();
    // data will returns {name: 'thinkjs', email: 'admin@thinkjs.org', ...}
  } 
```
>注：如果数据表没有对应的数据，那么返回值为空对象 {}，可以通过 think.isEmpty 方法来判断返回值是否为空。


#### **查询指定字段**
使用 getField 方法查询指定字段的值。如：
```javascript
// 获取单个字段的所有列表 
  async listAction(){
    const data = await this.model('user').getField('c_id');
    // data = [1, 2, 3, 4, 5]
  }

// 指定个数获取单个字段的列表
  async listAction(){
    const data = await this.model('user').getField('c_id', 3);
    // data = [1, 2, 3]
  }

// 获取单个字段的一个值
  async listAction(){
    const data = await this.model('user').getField('c_id', true);
    // data = 1
  }

// 获取多个字段的所有列表
  async listAction(){
    const data = await this.model('user').getField('c_id,d_name');
    // data = {c_id: [1, 2, 3, 4, 5], d_name: ['a', 'b', 'c', 'd', 'e']}
  }

// 获取指定个数的多个字段的所有列表
  async listAction(){
    const data = await this.model('user').getField('c_id,d_name', 3);
    // data = {c_id: [1, 2, 3], d_name: ['a', 'b', 'c']}
  }

// 获取多个字段的单一值 
  async listAction(){
    const data = await this.model('user').getField('c_id,d_name', true);
    // data = {c_id: 1, d_name: 'a'}
  }
```
#### **查询多条数据**
使用 select 方法查询多条数据，返回值为数组。如：
```javascript
  async listAction(){
    const model = this.model('user');
    const data = await model.limit(2).select();
    // data will returns 
    [{name: 'test', email: 'test@thinkjs.org', ...}
    {name: 'thinkjs', email: 'admin@thinkjs.org', ...}]
  }
```
#### **查询数据条数**
使用 count 方法来方便查询总条数。如：
```javascript
      async listAction(){
        const model = this.model('user');
        const data = await model.count();
      }
```
#### **分页查询数据**
使用 countSelect 方法实现分页查询。如：
```javascript
      async listAction(){
        const model = this.model('user');
        const data = await model.page(currentPage, pageSize).countSelect();
      }
```
返回如下:
```javascript
    {
        data: array //当前页下的数据列表
        count: number,  //总条数
        totalPages: number,  //总页数
        pageSize: number,  //每页显示的条数
        currentPage: number,  //当前页
    }
```
