---
title: Thinkjs - 数据库配置
date: 2019-9-16
tags:
- Thinkjs
- 数据库
- MySQL
categories:
- 后端
---
### 配置 database.js 文件
```javascript
    // database.js

    module.exports = {
      handle: mysql,
      database: 'think', // database_name
      prefix: 'think_', // database_table 前缀
      encoding: 'utf8mb4',
      host: '127.0.0.1', // Your MySQL host
      port: '3306', // Your MySQL port
      user: 'user', // Your MySQL userName
      password: 'password', // Your MySQL password
      dateStrings: true
    };
```
<!--more-->
### 配置 adapter.js 文件
```javascript
    // adapter.js

    const database = require('./database.js'); // 引用database.js
    ...
    exports.model = {
      type: 'mysql',
      common: {
      logConnect: isDev,
      logSql: isDev,
      logger: msg => think.logger.info(msg)
     },
     mysql: database 
     };

```


### 或直接在 adapter.js 内配置
```javascript
    // adapter.js
    const mysql = require('think-model-mysql');
    ...
    exports.model = {
      type: 'mysql',
      common: {
      logConnect: isDev,
      logSql: isDev,
      logger: msg => think.logger.info(msg)
     },
     mysql: {
      handle: mysql,
      database: 'think', // database_name
      prefix: 'think_', // database_table 前缀
      encoding: 'utf8mb4',
      host: '127.0.0.1', // Your MySQL host
      port: '3306', // Your MySQL port
      user: 'user', // Your MySQL userName
      password: 'password', // Your MySQL password
      dateStrings: true
      }
    };
```
