---
title: JSON WEB TOKEN 入门
date: 2020-01-07 20:10:33
tags:
- https
- token
- JWT
categories:
- 用户认证
---
## 前言
在[《基于Token的用户认证》](https://segmentfault.com/a/1190000021499728)中我们了解了 Token 认证的基本流程，而 JSON Web Token（缩写 JWT）是基于 Token 原理目前最流行的跨域认证解决方案，本文介绍它的原理和用法。

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/JSON%20WEB%20TOKEN.png)
## JWT 的原理
JWT 的原理：服务器认证以后，生成一个 JSON 对象，发回给用户，就像下面这样。

```
{
    "userId": "小花",
    "role": "admin",
    "lastLoginTime": "2019-11-24 19:24:24"
}
```
用户与服务端通信的时候，都要发回这个 JSON 对象。服务器完全只靠这个对象验证用户身份。为保障安全性，服务器在生成这个对象的时候，会用算法生成签名（详见后文）。
### 数据结构

* * *


JWT 的三个部分依次如下。
*   HEADER（头部）
*   PAYLOAD（负载）
*   SIGNATURE（签名）

写成一行，就是下面的样子。
*   HEADER.PAYLOAD.SIGNATURE

实际 JWT 大概就像下面这样。

![image.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/JWTexmple.png)

它是一个很长的字符串，中间用点（`.`）分隔成三个部分，HEADER 和 PAYLOAD 是 JSON 对象，均用 `base64Url` 转换为字符串。注意，JWT 内部是没有换行的，这里只是为了便于展示，将它写成了几行。

#### HEADER（头部）

* * *

HEADER 部分是一个 JSON 对象，描述 JWT 的元数据，一般是这个样子：
```
{
    "alg": "HS256",
    "typ": "JWT"
}
```
上面代码中，`alg` 属性表示签名的算法（`algorithm`），默认是 HMAC SHA256（写成 HS256）；`typ` 属性表示这个令牌（token）的类型（`type`），JWT 令牌统一写为 `JWT` 。

HEADER 是个 JSON 对象，会用 `base64Url` 算法转换为字符串。
#### PAYLOAD（负载）

* * *
PAYLOAD 部分也是一个 JSON 对象，描述服务中需要负载的信息，一般是这个样子：
```
{
    "userId": "小花",
    "role": "admin",
    "lastLoginTime": "2019-11-24 19:24:24"
}
```
PAYLOAD 部分也是一个 JSON 对象，会用 `base64Url` 算法转换为字符串。
#### SIGNATURE（签名）

* * *

SIGNATURE 部分是对前两部分的签名，防止数据篡改。

首先，需要指定一个密钥（secret），再使用 Header 里面指定的签名算法，按照下面的公式产生签名。

```
HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload) + "." +
    base64UrlEncode(your-256-bit-secret)
    )
```

### 投入使用

* * *

![Token.png](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/Token.jpg)

**用户登录**
> 用户首次登录，将用户密码发送给服务器

**服务器生成 token**
> 验证通过后，将用户的基本信息和密码结合我们设置的密钥 secret 通过 JWT 生成 token 

**存储**
> 用户收到服务器经过 JWT 编码返回的 token 后，可以将token储存在 Cookie 里面，也可以储存在 localStorage。

**通信**
> 用户每次与服务器通信，都要带上这个 token 。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域。更好的做法是放在 HTTP 请求的头信息`Authorization`字段里面。

```
    Authorization: Bearer <token>
    // 头信息 Authorization 字段
```

另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

## 参考阅读
[基于Token的用户认证](https://segmentfault.com/a/1190000021499728)
[jsonwebtoken生成与解析token](https://www.jianshu.com/p/ed17e00c4484)
