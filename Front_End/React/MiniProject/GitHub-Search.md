## 1. 案例介绍

GitHub 搜索小案例的主要功能其实就是搜索 GitHub 用户信息和结果展示，示例图如下：

![image-20231107222541322](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311072225434.png)

可以发现，此案例并不难，用 TodoList 案例的知识就能做，主要是学习网络请求，所以代码就直接粘贴出来了。

该案例我们分成了两个组件，分别是 Header 搜索组件、List 列表组件，当然还有个最顶层的 App 组件。

对于发送网络请求，我们这里首先使用 axios 来进行。

## 2. axios 版

App 组件：

```jsx
//创建外壳组件APP
import React, {Component} from 'react'
import Header from './Header/Header';
import List from './List/List'

class App extends Component {

    state = {
        users: [],
        isFirst: true,
        isLoad: false,
        isError: ''
    }

    updateAppState = (stateObj) => {
        this.setState(stateObj)
    }

    render() {
        //通过 ...将状态中的全部赋值过去
        return (
            <div className="container">
                <Header updateAppState={this.updateAppState}/>
                <List {...this.state} />
            </div>

        )
    }
}

export default App  
```

Head 组件：

```jsx
import React, {Component} from 'react'
import axios from 'axios';

export default class Header extends Component {

    search = () => {
        //const {value} = this.KeyValue;
        //连续解构赋值，拿到this下面的KeyValue中的value,并进行重命名为KeyWord
        const {KeyValue: {value: keyWord}} = this;

        //在搜索之前设置,搜索的开始，结束第一次展示
        this.props.updateAppState({isFirst: false, isLoad: true})

        //切记在配置代理了之后一定需要添加相应的路径
        axios.get(`http://localhost:3000/api1/search/users?q=${keyWord}`).then(
            success => {
                this.props.updateAppState({users: success.data.items, isLoad: false});
            },
            error => {
                this.props.updateAppState({isError: error.message, isLoad: false});
            }
        )
    }

    render() {
        return (
            <section className="jumbotron">
                <h3 className="jumbotron-heading">搜索GitHub用户</h3>
                <div>
                    {/*使用ref拿到输入的数据，ref中使用回调函数的形式，在实例对象中创建一个KeyValue的属性，值是该节点*/}
                    <input ref={c => this.KeyValue = c} type="text" placeholder="输入关键词进行搜索"/>&nbsp;
                    <button onClick={this.search}>搜索</button>
                </div>
            </section>
        )
    }
}

```

List 组件：

```jsx
import React, {Component} from 'react';
import './List.css'

class List extends Component {
    render() {
        const {users, isFirst, isLoad, isError} = this.props;
        return (
            <div className="row">
                {
                    //因为不能在JSX语法中使用if，只能是表达式，所以可以用三元运算符(可连续)进行判断。
                    isFirst ? <h1>欢迎进入页面</h1> :
                        isLoad ? <h2>正在搜索页面</h2> :
                            isError !== '' ? <h1>{isError}</h1> :
                                users.map((user) => {
                                    return (
                                        <div className="card" key={user.id}>
                                            <a href={user.html_url} target="_blank" rel="noreferrer">
                                                <img alt="headImg" src={user.avatar_url} style={{width: '100px'}}/>
                                            </a>
                                            {/*user.login是用户名*/}
                                            <p className="card-text">{user.login}</p>
                                        </div>
                                    )
                                })
                }
            </div>
        );
    }
}

export default List;
```

## 3. 跨域问题解决

在请求后端服务器时，可能会出现跨域问题，因此我们这里再配置一个代理配置 `src/setupProxy.js`：

```js
const proxy = require('http-proxy-middleware')

module.exports = function (app) {
    app.use(
        proxy('/api1', {
            target: 'http://localhost:5000',
            changeOrigin: true,
            pathRewrite: {'^/api1': ''}
        })
    )
}
```

http://localhost:5000 为服务端，提供了请求 GitHub 搜索的接口，这里就不展示了，可以自己用 Node.js 或者 Java/Golang 起一个服务就行。

> 说明：
>
> React 脚手架提供了一个配置文件，可以配置多个代理。
>
> 1. 第一步：在src下创建代理配置文件：`setupProxy.js`
>
> 2. 编写 `setupProxy.js` 配置具体代理规则：
>
>     ```js
>     const proxy = require('http-proxy-middleware')	// 安装脚手架时已安装 http-proxy-middleware
>             
>     module.exports = function(app) {
>       app.use(
>         proxy('/api1', {  	// api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
>           target: 'http://localhost:5000', 	// 配置转发目标地址(能返回数据的服务器地址)
>           changeOrigin: true, 	// 控制服务器接收到的请求头中host字段的值
>           /*
>           	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
>           	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
>           	changeOrigin默认值为false，但我们一般将changeOrigin值设为true，欺骗下浏览器，避免可能的错误
>           */
>           pathRewrite: {'^/api1': ''} 	// 去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
>         }),
>                 
>         proxy('/api2', { 
>           target: 'http://localhost:5001',
>           changeOrigin: true,
>           pathRewrite: {'^/api2': ''}
>         })
>       )
>     }
>     ```
>
> 说明：
>
> 1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
> 2. 缺点：配置繁琐，前端请求资源时必须加前缀。

## 4. PubSubJS 版

前面的案例有个小问题，App 组件其实并没有用到 state 中的数据，它只是为了传递 Header 和 List 之间的数据，起一个父桥梁的作用，因为我们目前还没学怎么进行兄弟组件间的通信。

实际上用到 state 状态的是 List 组件，它需要展示 state 中的数据信息，并且 Header 组件需要传递数据到 List 组件中。

为了实现该功能，我们不一定需要父 App 组件这个桥梁，这里借助一个第三方工具来实现，那就是 [PubSubJS](https://github.com/mroderick/PubSubJS)，其实就是一个 JS 版的发布订阅工具，利用该工具就可以实现任意组件间的通信了。该工具的使用也很简单，看 README 的示例即可，主要就是通过回调来实现的通信。

下面开始改造：

**一、首先 将 App 组件中的 state 移到 List 组件中去**。

**二、List 组件中订阅消息，当接收到 state 更新的消息后，即可更新 state**。

List 组件什么时候订阅消息呢？那肯定是页面一加载完成就要开始订阅了，所以这里使用到 `componentDidMount` 钩子：

```jsx
import React, {Component} from 'react';
import PubSub from 'pubsub-js';
import './List.css'

class List extends Component {

    state = {
        users: [],
        isFirst: true,
        isLoad: false,
        isError: ''
    }

    // 组件挂载完后就订阅
    componentDidMount() {
         this.token = PubSub.subscribe('LIST_STATE_TOPIC', (_, stateObj) => {
            this.setState(stateObj)
        })
    }

    // 组件将要卸载时取消订阅
    componentWillUnmount() {
        PubSub.unsubscribe(this.token)
    }

    render() {
        const {users, isFirst, isLoad, isError} = this.state;
        return (
            <div className="row">
                {
                    //因为不能在JSX语法中使用if，只能是表达式，所以可以用三元运算符(可连续)进行判断。
                    isFirst ? <h1>欢迎进入页面</h1> :
                        isLoad ? <h2>正在搜索页面</h2> :
                            isError !== '' ? <h1>{isError}</h1> :
                                users.map((user) => {
                                    return (
                                        <div className="card" key={user.id}>
                                            <a href={user.html_url} target="_blank" rel="noreferrer">
                                                <img alt="headImg" src={user.avatar_url} style={{width: '100px'}}/>
                                            </a>
                                            {/*user.login是用户名*/}
                                            <p className="card-text">{user.login}</p>
                                        </div>
                                    )
                                })
                }
            </div>
        );
    }
}

export default List;
```

**三、当 Header 组件中数据发生变化时，发布消息**：

```jsx
import React, {Component} from 'react'
import PubSub from "pubsub-js";
import axios from 'axios';

export default class Header extends Component {

    search = () => {
        //const {value} = this.KeyValue;
        //连续解构赋值，拿到this下面的KeyValue中的value,并进行重命名为KeyWord
        const {KeyValue: {value: keyWord}} = this;

        //在搜索之前设置,搜索的开始，结束第一次展示
        PubSub.publish('LIST_STATE_TOPIC', {isFirst: false, isLoad: true});

        //切记在配置代理了之后一定需要添加相应的路径
        axios.get(`http://localhost:3000/api1/search/users?q=${keyWord}`).then(
            success => {
                PubSub.publish('LIST_STATE_TOPIC', {users: success.data.items, isLoad: false});
            },
            error => {
                PubSub.publish('LIST_STATE_TOPIC', {isError: error.message, isLoad: false});
            }
        )
    }

    render() {
        return (
            <section className="jumbotron">
                <h3 className="jumbotron-heading">搜索GitHub用户</h3>
                <div>
                    {/*使用ref拿到输入的数据，ref中使用回调函数的形式，在实例对象中创建一个KeyValue的属性，值是该节点*/}
                    <input ref={c => this.KeyValue = c} type="text" placeholder="输入关键词进行搜索"/>&nbsp;
                    <button onClick={this.search}>搜索</button>
                </div>
            </section>
        )
    }
}

```

## 5. fetch 版

我们在浏览器上发起请求一般都是用 XHR（传统 Ajax），一些常用的库 jQuery、axios（非服务端）其实都是对 XHR 的封装，使其更易用。

虽然 axios 是 promise 风格，能解决回调地狱问题，但 jQuery、axios 第三方库都得安装。

而 fetch 是 window 自带的原生函数，也是 promise 风格，它不再使用 XML 对象提交 ajax 请求。

fetch 版如下：

```jsx
    search = async () => {
        //const {value} = this.KeyValue;
        //连续解构赋值，拿到this下面的KeyValue中的value,并进行重命名为KeyWord
        const {KeyValue: {value: keyWord}} = this;

        //在搜索之前设置,搜索的开始，结束第一次展示
        PubSub.publish('LIST_STATE_TOPIC', {isFirst: false, isLoad: true});

        // axios 发送网络请求
        // axios.get(`http://localhost:3000/api1/search/users?q=${keyWord}`).then(
        //     success => {
        //         PubSub.publish('LIST_STATE_TOPIC', {users: success.data.items, isLoad: false});
        //     },
        //     error => {
        //         PubSub.publish('LIST_STATE_TOPIC', {isError: error.message, isLoad: false});
        //     }
        // )

        // fetch 发送网络请求
        // fetch(`http://localhost:3000/api1/search/users?q=${keyWord}`).then(
        //     resolve => {    // 判断是否成功连接服务器
        //         console.log('连接服务器成功');
        //         return resolve.json(); // resolve.json() 返回一个 promise 实例，获取的数据在其中
        //     },
        //     error => {
        //         console.log('连接服务器失败', error.message);
        //         // 中断 promise 链，否则 error 的返回值是 undefined，是非 promise 实例，会导致后面的 .then 走成功的函数
        //         return new Promise(() => {
        //         })    // 返回一个初始化状态的 promise
        //     }
        // ).then(     // resolve 中返回了一个 promise 实例，promise 支持链式调用
        //     // 这里才是真正的获取数据
        //     response => {
        //         console.log('获取数据成功', response);
        //     },
        //     error => {
        //         console.log('获取数据失败', error.message)
        //     }
        // )

        /* promise 小知识：
            1. 如果 promise 的 .then 方法中，成功函数的返回值是一个 promise 实例，那么就会把该返回值作为 .then 的返回值。
            2. 由于 await 关键字需要等待一个 Promise 对象的结果，而 async 函数会自动把函数的返回值封装为一个 Promise 对象，
               所以我们只能在 async 函数中使用 await 关键字。
         */

        // fetch 发送网络请求（await 优化版，async 函数才能使用 await）
        try {
            // 等待 fetch 函数返回的 Promise 对象完成
            const resolve = await fetch(`http://localhost:3000/api1/search/users?q=${keyWord}`);
            // 等待 resolve.json() 返回的 Promise 对象完成，并获取该 Promise 对象的解析结果
            const response = await resolve.json();
            PubSub.publish('LIST_STATE_TOPIC', {users: response.items, isLoad: false});
        } catch (err) {     // await 只会等待成功的返回，因此需要 catch error
            PubSub.publish('LIST_STATE_TOPIC', {isError: err.message, isLoad: false});
        }
    }
```

## 6. 总结

1. 设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。

2. ES6小知识点：解构赋值+重命名

    ```js
    let obj = {a:{b:1}}
    const {a} = obj; 	//传统解构赋值
    const {a:{b}} = obj; 	//连续解构赋值
    const {a:{b:value}} = obj; 	//连续解构赋值+重命名
    ```

    

3. 消息订阅与发布机制

    - 先订阅，再发布（理解：有一种隔空对话的感觉）
    - 适用于任意组件间通信
    - 要在组件的componentWillUnmount中取消订阅

4. fetch发送请求（关注分离的设计思想）

    ```js
    try {
        const response= await fetch(`/api1/search/users2?q=${keyWord}`)
        const data = await response.json()
        console.log(data);
    } catch (error) {
        console.log('请求出错',error);
    }
    ```

    





