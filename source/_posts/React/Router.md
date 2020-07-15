---
title: React 入门
date: 2019-6-29
categories:
- React
---
## 路由跳转
在一张页面中，我们该如何去实现页面跳转呢

Router提供了页面跳转渲染的方法，React应用会根据链接地址去决定要渲染的内容。链接地址不再是直接跳转，而是去告诉React，我需要怎么样的内容，让React去条件渲染

### 创建页面
首先，我们快速创建一个 React-app 应用。 删去src目录下的所有文件，新建 index
```javascript
    // index.js
        
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';

    ReactDOM.render(<App/>, document.getElementById('root'));
```
<!--more-->
```javascript   
    // App.js

    import React from 'react';
    import {Router} from 'react-router-dom';
    import Home from './Home';
    import Page1 from './Page1';
    import Page2 from './Page2';
    import Page3 from './Page3';
        
    class App extends React.Component {
        render(){
            return(
                <Router >
                    <div>
                        <Route path="/" component={Home} />
                        <Route path="/Page1" component={Page1} />
                        <Route path="/Page2" component={Page2} />
                        <Route path="/Page3" component={Page3} />
                    </div>
                </Router>
            )
        }
    }
    export default App;
```
依次创建页面组件：Home、Page1、Page2、Page3；同时引入Link进行页面跳转
```javascript
    // Home.js

    import React from 'react';
    import { Link } from 'react-router-dom';
        
    class Home extends React.Component{
        render(){
            return(
                <div>
                    <div>This is Home!</div>
                    <div>
                        <Link to="/Page1/" style={{color:'black'}}>
                            <div>点击跳转到Page1</div>
                        </Link>
                        <Link to="/Page2/" style={{color:'black'}}>
                            <div>点击跳转到Page2</div>
                        </Link>
                        <Link to="/Page3/" style={{color:'black'}}>
                            <div>点击跳转到Page3</div>
                        </Link>
                    </div>
                </div>
            );
        }
    }
    export default Home;
```
```javascript
    //Page1~Page3页面类似，不再赘述
        
    import React from 'react';
    
    class Page1 extends React.Component{
        render(){
            return(
                <div>
                    <div>This is Page1!</div>
                </div>
            );
        }
    }
    export default Page1;
    // Page1~Page3页面类似，不再赘述
```
![page](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/page.png)
![route](https://blogimage-1259219507.cos.ap-chengdu.myqcloud.com/route.png)
### 选择渲染
可以通过 exact 属性来选择渲染时不默认渲染这个页面
```
    <Route exact path="/" component={Home} />
    // 此时，当在跳转到 Page1 时，即使包含了 path="/" ,默认不会渲染 path="/" 的组件
```
## 父子传值
+ 父向子传值：父直接向子传输，子组件通过 props 进行接收
+ 子向父传值：绑定一个事件，将事件传递到子来进行值的更替

### 父向子传值
父直接向子传输，子组件通过 props 进行接收
<!--more-->

```javascript
    // Father.js
        
    import React from "react"
    import Children from "./Children"
         
    class Father extends React.Component {
            render() {
                const arr = [1, 2, 3];
                return (
                    <div>
                        <Children arr={arr}/>
                    </div>
                )
            };
        }
    export default Father;
```
```javascript
    // Children.js
        
    import React from "react"
         
    class Children extends React.Component {
        const {arr} = this.props;
        console.log(arr);
        render() {
            return (
                <div>Children</div>
            )
        }
    }
    export default Children;
```
### 子向父传值
绑定一个事件，将事件传递到子来进行值的更替
```javascript
    // Father.js
        
    import React from "react"
    import Children from "./Children"
         
    class Father extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
                parentText: "parent",
            }
        }
         
        fn(data) {
            this.setState({
                parentText: data //把父组件中的parentText替换为子组件传递的值
            },() =>{
                console.log(this.state.parentText);
            });
        }
        render() {
            const {parentText} = this.state;
            return (
                <div>
                    <Children fn={this.fn.bind(this)} />
                    <p>{parentText}</p>
                </div>
            )
        }
    }
    export default Father;  
```
```javascript
    // Children.js
    import React from "react"
         
    class Children extends React.Component {
        constructor(props) {
            super(props);
            this.state = ({
            childText: "child"
            })
        }
        handleClick(text) {
            this.props.fn(text)//这个地方把值传递给了props的事件当中
        }
        render() {
            return (
                <div>
                    <button 
                        onClick={this.handleClick.bind(this, this.state.childText)}
                    >
                    click me
                    </button>
                </div>
            )
        }
    }  
    export default Children;
```
## 数据请求
### axios
安装axios，这里使用的cnpm
>   npm isntall axios --save

```javascript
    import axios from "axios";

    componentDidMount() {
        this.listRender();
    }

    listRender = e => {
        const _this = this;
        axios.get("url").then(res => {
            _this.setState({ data: res.data });
        });
    }
```
<!--more-->
### ajax
安装 jquery ，引用 $
>    npm isntall jquery --save

```javascript
    import $ from 'jquery';

    componentDidMount() {
            this.listRender();
    }
    
    listRender = e => {
        $.ajax({
            type: "POST",
            url: "url",
            dataType: "json",
        }).then(res => {
        this.setState({
                list: res.data.data,
            });
        });
    }
```
### fetch
```javascript
    componentDidMount() {
        this.listRender();
    }

    listRender = e => {
        fetch("url", {
          method: "GET"
        })
        .then(res => res.json())
        .then(data => {
            this.setState({ data: data });
        });
    }
```
## 双向绑定
### ref 的 DOM 操作
React.js 当中提供了 ref 属性来帮助我们获取已经挂载的元素的 DOM 节点，你以给某个 JSX 元素加上 ref属性：
```javascript
    //输入框自动聚焦,按钮绑定搜索
    class AutoFocusInput extends Component {
      componentDidMount () {
        this.input.focus()
      }
      
      handleClick = e => {
            const _post =
                {
                    search: this.input.value
      }
      
      render () {
        return (
          <input ref={(input) => this.input = input} />
          <button onClick={this.handleClick.bind(this)} />
        )
      }
    }
    
    ReactDOM.render(
      <AutoFocusInput />,
      document.getElementById('root')
    )
```
我们可以给任意代表 HTML 元素标签加上 ref 从而获取到它 DOM 元素然后调用 DOM API。但是记住一个原则：能不用 ref 就不用。特别是要避免用 ref 来做 React.js 本来就可以帮助你做到的页面自动更新的操作和事件监听。多余的 DOM 操作其实是代码里面的“噪音”，不利于我们理解和维护。

顺带一提的是，其实可以给组件标签也加上 ref ，例如：
```javascript
    <Clock ref={(clock) => this.clock = clock} />
```
