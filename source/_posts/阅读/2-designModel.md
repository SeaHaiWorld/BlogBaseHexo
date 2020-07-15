---
title: 观察者模式
date: 2020-02-21
categories: 
- 设计模式
---
## _What is the Observer Mode?_
**发布者( Subject ) + 订阅者( Observer ) = 观察者模式**

观察者模式定义了对象之间的一对多的依赖关系，当对象( Subject )发生状态改变时，其依赖者( Observer )都会收到状态的改变并更新。

打个比方。你家里所有人都比较关心你，当你有了对象你告诉他们，我有对象了！他们就会接受到你的消息，知道你有对象了（然而这是不可能的~）。

这里把你的家人就比作订阅者，而你作为发布者，你发布了新的状态（有女朋友了），他们收到状态的改变就会更新。

## _React_ 中的观察者模式
### _Context_ (上下文)
Context (上下文) 是典型的观察者模式。Context 通过组件树提供了一个传递数据的方法，避免了在每一个层级手动的传递数据。
#### **API**
React.createContext：创建一个上下文的容器(组件), defaultValue可以设置共享的默认数据
```javascript
const {Provider, Consumer} = React.createContext(defaultValue);
```
Provider: 和他的名字一样。生产共享数据的地方。可以看做发布者
```javascript
<Provider value={/*共享的数据*/} />
```
Consumer: 可以看做观察者。是获取 Provider 产生的数据。（必须在生产者层次之下，才能通过回调的方式拿到共享的数据源）
```javascript
<Consumer>
  {value => /*根据上下文  进行渲染相应内容*/}
</Consumer>
```
#### **缺点**
* 不能处理异步的情况
* 没有 action ， state 的值都是被直接修改，state 的数据安全性不及 redux。
* context ，并不会减少代码量。Provider 和 Consumer 必须来自同一 React.createContext 调用，发布订阅的单一性

## 从 _Redux_ 到 _Dva_
先说说 Redux ，Redux 是一个用于状态管理的 js 框架，是 Flux 架构的一种实现（如图）

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016011503.png)

| Property | Type | Rule | Description |
| :-----: | :----: | :----: | :----: |
| Store | Object | Subject | 所有状态集中存储的地方。并全局唯一，称为状态树 |
| Action | Object | Action | 标识为一个动作类型，里面包含一个 type 属性 |
| Reducer | function | Method | 在 store 分发 action 时提供更新状态树中状态的方法 |
| View | Object | Observer | 视图层 |