# 11 【react-router 5】

## 1.准备

### 1.1 SPA

SPA ，单页应用程序，它比传统的 Web 应用程序更快，因为它们在 Web 浏览器本身而不是在服务器上执行逻辑。在初始页面加载后，**只有数据来回发送**，而不是整个 HTML，这会降低带宽。它们可以独立请求标记和数据，并直接在浏览器中呈现页面。

SPA 整个应用只有 **一个完整的页面**，点击页面中的链接 **不会刷新页面**，只会做页面的 **局部更新**。数据都通过 ajax 请求获取，并在前端异步展现。

所以，可以这样理解 SPA：**单页面，多组件**。想要应用 SPA，就得先了解路由技术了。

### 1.2 什么是路由？

路由是根据不同的 URL path 展示不同的内容或页面，在 SPA 应用中，大部分页面结果不改变，只改变部分内容的使用

一个路由其实就是一个映射关系（k:v）：key 为路径，value可能是 function 或者是 component

**后端路由**：

- value 是 function，用来处理客户端提交的请求

- 注册路由：`router.get(path,function(req,res))`

- 工作过程：当 node 接收一个请求的时候，根据请求路径找到匹配的路由，调用路由中的函数来处理请求，返回响应的数据

**前端路由**：

- 浏览器端路由，value 是 Component，用于展示页面内容

- 注册路由：`< Route path="/test" component={Test}>`

- 工作过程：当浏览器的 path 变为 `/test` 的时候，当前路由组件就会变成 Test 组件

**优点**：

- 用户体验好，不需要每次都从服务器全部获取整个 HTML，快速展现给用户

**缺点**：

- SPA 无法记住之前页面滚动的位置，再次回到页面时无法记住滚动的位置
- 使用浏览器的前进和后退键会重新请求，没有合理利用缓存

### 1.3 前端路由的原理

前端路由的主要依靠的时 history ，也就是浏览器的历史记录，history 中会记录 URL path 的变化，可以前进和后退。

> history 是 BOM 对象下的一个属性，在 H5 中新增了一些操作 history 的 API（history.js）

浏览器的历史记录就类似于一个栈的数据结构，前进就相当于入栈，后退就相当于出栈

并且历史记录上可以采用 `listen` 来监听请求路由的改变，从而判断是否改变了 URL path

在 H5 中新增了 `createBrowserHistory` 的 API ，用于创建一个 history 栈，允许我们手动操作浏览器的历史记录

新增 API：`pushState` ，`replaceState`，原理类似于 Hash 实现。 用 H5 实现，单页路由的 URL 不会多出一个 `#` 号，这样会更加的美观

## 2.react-router-dom 的理解和使用

react 的路由有三类：

- web【主要适用于前端】
- native【主要适用于本地】
- anywhere【任何地方】

我们主要使用 web，即 `react-router-dom`（专门给 web 人员使用的库）：

1. 一个 react 的仓库
2. 很常用，基本是每个应用都会使用的这个库
3. 专门来实现 SPA 应用



脚手架中并没有安装 react-router-dom，先安装：

```sh
npm i react-router-dom@5
```

首先我们要明确好页面的布局 ，分好导航区、展示区。

要引入 `react-router-dom` 库，暴露一些属性 `Link、BrowserRouter...`

```js
import { Link, BrowserRouter, Route } from 'react-router-dom'
```

导航区的 a 标签改为 Link 标签，编写路由链接（引起路径变化）：

```jsx
{/*编写路由链接（引起路径变化）*/}
<Link className="list-group-item" to="/about">About</Link>
<Link className="list-group-item" to="/home">Home</Link>
```

注册路由（路径变化后对应的组件），需要用 `Route` 标签：

```jsx
{/*注册路由（路径变化后对应的组件）*/}
<Route path="/about" component={About}></Route>
<Route path="/home" component={Home}></Route>
```

这样之后我们还需要一步，加个路由器，在上面我们写了两组路由，同时还会报错指示我们需要添加 `Router` 来解决错误，**这就是需要我们添加路由器来管理路由**，如果我们在 Link 和 Route 中分别用路由器管理，那这样是实现不了的，只有 **在一个路由器的管理下才能进行页面的跳转工作**。

因此我们也可以在 Link 和 Route 标签的 **最外层标签采用 `BrowserRouter`(或者`HashRouter`) 包裹**，但是这样当我们的路由过多时，我们要不停的更改标签包裹的位置，因此我们可以回到 `App.jsx` 目录下的 `index.js` 文件，**将整个 App 组件标签采用 `BrowserRouter` 标签去包裹**，这样整个 App 组件都在**一个路由器**的管理下。

`index.js`：

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import {BrowserRouter} from "react-router-dom";

import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
    <React.StrictMode>
        <BrowserRouter>
            <App />
        </BrowserRouter>
    </React.StrictMode>
)
```

`App.jsx`：

```jsx
import React, {Component} from "react";
import {Link, Route} from "react-router-dom";
import About from "./components/About";
import Home from "./components/Home";

export default class App extends Component {
    render() {
        return (
            <div>
                <div className="row">
                    <div className="col-xs-offset-2 col-xs-8">
                        <div className="page-header"><h2>React Router Demo</h2></div>
                    </div>
                </div>
                <div className="row">
                    <div className="col-xs-2 col-xs-offset-2">
                        <div className="list-group">
                            {/*原生 HTML 靠 <a> 跳转页面*/}
                            {/*<a className="list-group-item active" href="./about.html">About</a>
                            <a className="list-group-item" href="./home.html">Home</a>*/}

                            {/*编写路由链接（引起路径变化）*/}
                            <Link className="list-group-item" to="/about">About</Link>
                            <Link className="list-group-item" to="/home">Home</Link>
                        </div>
                    </div>
                    <div className="col-xs-6">
                        <div className="panel">
                            <div className="panel-body">
                                {/*<h3>我是About的内容</h3>*/}

                                {/*注册路由（路径变化后对应的组件*/}
                                <Route path="/about" component={About}></Route>
                                <Route path="/home" component={Home}></Route>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        )
    }
}
```

About 路由和 Home 路由都只是一行简单的 h2 标签，就不展示代码了。效果如下：

![](https://i0.hdslb.com/bfs/album/d1516434cc58795b9846722a542361d032aefe86.png)

再顺便说一下 `Switch` 标签的使用，一般要加上：

```jsx
{/* Switch，匹配到一个路由后就不往下匹配了，否则每次都会把所有路由匹配一遍，效率低 */}
<Switch>
    {/*路由组件：注册路由（路径变化后对应的组件）*/}
    <Route path="/about" component={About}></Route>
    <Route path="/home" component={Home}></Route>
</Switch>
```



## 3.路由组件和一般组件

在我们前面的内容中，我们是把组件 Home 和组件 About 当成是一般组件来使用，我们将它们写在了 src 目录下的 components 文件夹下，但是我们又会发现它和普通的组件又有点不同，对于普通组件而言，我们在引入它们的时候我们是通过标签的形式来引用的。但是在上面我们可以看到，我们把它当作路由来引用时，我们是通过 `{Home}` 来引用的。

从这一点我们就可以认定一般组件和路由组件存在着差异：

1. **写法不同**：

    - 一般组件：`<Demo/>`；
    - 路由组件：`<Route path="/demo" component={Demo}/>`。

2. 为了规范书写，它们 **存放的位置不同**：

    - 路由组件放在 `pages`/`views` 文件夹中；

    - 一般组件放在 `components` 文件夹中。

3. 最重要的一点是它们 **接收到的 `props` 不同**：

    - 在一般组件中，如果我们不进行传递，props 就不会收到值；

    - **对于路由组件，它会接收到 3 个固定的 props，分别是 `history` 、`location` 和 `match`**。

        ![image-20221025230429965](https://i0.hdslb.com/bfs/album/f4e260e444ed34c142745674a5ea57a5731de492.png)

        重要、常用的属性：

        ```yaml
        history:
            go: ƒ go(n)
            goBack: ƒ goBack()
            goForward: ƒ goForward()
            push: ƒ push(path, state)
            replace: ƒ replace(path, state)
        location:
            pathname: "/about"
            search: ""
            state: undefined
        
        match:
            params: {}
            path: "/about"
            url: "/about"
        ```

        

将刚刚的 Demo 修整规范，把 Header 抽取出来，当成一般组件：

![image-20231108154537291](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311081545628.png)

## 4.NavLink 标签

### 4.1 基本使用

NavLink 标签是和 Link 标签作用相同的，但是它又比 Link 更加强大。

在前面的 demo 中，会发现点击的按钮并没有出现高亮的效果，正常情况下我们给标签多添加一个 `active` 的类就可以实现高亮的效果。而 NavLink 标签正可以帮助我们实现这一步。

当我们 **选中某个 NavLink 标签时，就会自动的在类上添加一个 `active` 属性**：

```html
<NavLink className="list-group-item" to="/about">About</NavLink>
```

当然 NavLink 标签是默认的添加上 `active` 类，我们也可以改变它，在标签上添加一个属性 `activeClassName`。

如下代码，就写了 `activeClassName`，当点击的时候就会触发这个 class 的样式

```jsx
{/* NavLink在点击的时候就会去找 style01 所指定的 class 的值，如果不添加默认是 active */}
<NavLink activeClassName="style01" className="list-group-item" to="/home">Home</NavLink>
```

### 4.2 NavLink 封装

在上面的 NavLink 标签种，只有 `to` 属性不一样，我们每次都需要重复的去写这些样式名称或者是 `activeClassName` ，这并不是一个很好的情况，代码过于冗余。

那我们是不是可以想想办法封装一下它们呢？我们可以采用 `MyNavLink` 组件，对 NavLink 进行封装。

首先我们需要新建一个 MyNavLink 组件，return 一个结构：

```jsx
import React, {Component} from 'react';
import {NavLink} from "react-router-dom";

 // 封装 NaviLink
class MyNavLink extends Component {
    render() {
        return (
            // 通过 {...对象} 展开所有 MyNavLink 传递过来的属性，就放在了 NavLink 标签上
            <NavLink activeClassName="style01" className="list-group-item" {...this.props}></NavLink>
        );
    }
}

export default MyNavLink;
```

这样直接使用 MyNavLink 标签即可：

```jsx
{/* 使用自己封装的 MyNavLink */}
<MyNavLink to="/about">About</MyNavLink>
<MyNavLink to="/home">Home</MyNavLink>
```

你可以会觉得很神奇，MyNavLink 的标签体内容（About、Home）是怎么传递给 NavLink 的呢？

有一点非常重要的是，**标签体内容会成为 props 中的一个 `children` 属性**，因此我们在调用 `MyNavLink` 时，在标签体中写的内容，都会成为 `props` 中的一部分，NavLink 在展开的时候，就能获取到，而 **在标签中写 `children="xxx"` 属性与在标签体中直接写 `"xxx"` 是一样的效果**。

## 5.多级路由样式丢失问题

拿上面的案例来说，有一个 css 样式文件路径为 `public/css/bootstrap.css`。此时，加载该样式的路径为：

![image-20221025231114257](https://i0.hdslb.com/bfs/album/8ba0d1fa3f50ca170ec0768d4347bbd8d52cfe12.png)

但是在写路由的时候，有的时候就会出现多级路由：

```jsx
<MyNavLink to = "/aarynlu/about" >About</MyNavLink>
// ......
<Route path="/aarynlu/about"component={About}/>
```

这个时候如果刷新页面，就会出现样式因为路径问题加载失败，因为 `localhost:3000` 这个服务器是 webpack 内置搭建的，默认使用 public 作为根路径，如果 css 样式引入使用的是相对位置：

```html
<link rel="stylesheet" href="./css/bootstrap.css">
```

**在 `aarynlu/about` 路径下刷新页面** 时，那么请求的 css 路径就会变为如下：

![image-20231108165420991](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311081654528.png)

很明显，路径中多了 `/aarynlu/`，肯定是找不到这个 css 文件的，而 webpack 规定了：**如果请求了一个不存在的资源，就会返回 public下面的 `index.html`**，所以才会显示 200 状态码（指请求 `index.html` 成功）。我们可以来看看 response：

![image-20231108165624354](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311081656424.png)

解决这个问题，有三个方法：

1. 样式加载使用 **绝对路径**：

    ```html
     <link href="/css/bootstrap.css" rel="stylesheet">
    ```

2. 使用 **`%PUBLIC_URL%`**，代理 public 文件夹的绝对路径：

    ```html
    <link href="%PUBLIC_URL%/css/bootstrap.css" rel="stylesheet">
    ```

3. 使用 **`HashRouter`**：因为 HashRouter 会添加 `#`，**默认不会处理 `#` 后面的路径**，所以刷新时不会管 `#` 后面的路径，也就能请求到正确的资源了。

## 6.模糊匹配和精准匹配

路由的匹配有两种形式，一种是精准匹配一种是模糊匹配，React 中默认开启的是模糊匹配：

- 模糊匹配可以理解为，在匹配路由时，只要有匹配到的就好了

- 精准匹配就是，两者必须相同

比如：

```html
<MyNavLink to = "/home/a/b" >Home</MyNavLink>
```

此时该标签匹配的路由，分为三个部分 home a b；将会根据这个先后顺序匹配路由。

如下，`/home/a/b` 就可以匹配到下面的路由：

```html
<Route path="/home"component={Home}/>
```

但是如果是下面这个就会失败，也就是说他是根据路径一级一级查询的，可以包含前面那一部分，但并不是只包含部分就可以。

```html
<Route path="/a" component={Home}/>
```

当然也可以使用精确的匹配，加上属性 ` exact={true}` 即可。如下这样就精确的匹配 `/home`，则上面的 `/home/a/b` 就不行了

```html
<Route exact={true}  path="/home" component={Home}/>
或者
<Route exact path="/home" component={Home}/>
```

需要注意一个点：**严格匹配不要随便开启，有时候会导致无法匹配二级路由**（后面嵌套路由会讲到）。

## 7.Switch 解决相同路径问题

首先我们看一段这样的代码

```html
<Route path="/home" component={Home}></Route>
<Route path="/about" component={About}></Route>
<Route path="/about" component={About}></Route>
```

这是两个路由组件，在2，3行中，我们同时使用了相同的路径 `/about`

![image-20221026132313014](https://i0.hdslb.com/bfs/album/e00d583691642002dc359bac26e0ca06f7cea976.png)

我们发现它出现了两个 `about` 组件的内容，那这是为什么呢？

其实是因为，`Route` 的机制，当匹配上了第一个 `/about` 组件后，它还会继续向下匹配，因此会出现两个 About 组件，这时我们可以采用 `Switch` 组件进行包裹

```html
<Switch>
    <Route path="/home" component={Home}></Route>
    <Route path="/about" component={About}></Route>
    <Route path="/about" component={About}></Route>
</Switch>
```

在使用 `Switch` 时，我们需要先从 `react-router-dom` 中暴露出 `Switch` 组件

这样我们就能成功的解决掉这个问题了

## 8.路由重定向

在配置好路由，最开始打开页面的时候，应该是不会匹配到任意一个组件。这个时候页面就显得极其不合适，此时应该默认的匹配到一个组件。

这个时候我们就需要时候 Redirecrt 进行默认匹配了。

```html
<Redirect to="/home" />
```

当我们加上这条语句时，**页面找不到指定路径时，就会重定向到 `/home` 页面下**，因此当我们请求 3000 端口时，就会重定向到 `/home` 这样就能够实现我们想要的效果了。

如下的代码就是默认匹配 `/home` 路径所到的组件

```html
<Switch>
    <Route path="/about"component={About}/>
    {/* exact={true}：开启严格匹配的模式，路径必须一致 */}
    <Route   path="/home" component={Home}/>
    {/* Redirect: 如果上面的都没有匹配到，就匹配到这个路径下面 */}
    <Redirect to = "/home"/>
</Switch>
```

## 9.嵌套路由

嵌套路由也就是我们前面有提及的二级路由，但是嵌套路由包括了二级、三级...还有很多级路由。

下面我们在前面案例的基础上，在 Home 路由组件中再嵌套一层组件，里面分为Header 区和内容区，效果如下：

![image-20231108223758319](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311082237432.png)

我们将我们的嵌套内容写在相应的组件里面，News 和 Message 组件是在 Home 组件内的：

```jsx
import React, {Component} from 'react';
import News from "./News";
import Message from "./Message";
import {Route, Switch} from "react-router-dom";
import MyNavLink from "../../components/MyNavLink";

class Home extends Component {
    render() {
        return (
            <div>
                <h2>Home 内容</h2>
                <ul className="nav nav-tabs">
                    <li>
                        <MyNavLink className="list-group-item" to="/home/news">News</MyNavLink>
                    </li>
                    <li>
                        <MyNavLink className="list-group-item" to="/home/message">Message</MyNavLink>
                    </li>
                </ul>

                {/* 注册路由 */}
                <Switch>
                    <Route path="/home/news" component={News}/>
                    <Route path="/home/message" component={Message}/>
                </Switch>
            </div>
        );
    }
}

export default Home;
```



React 中 **路由得注册是有顺序的，逐层的**，在匹配得时候，因为 Home 组件是先注册的，因此在匹配的时候先去找 home 路由，由于是模糊匹配（**注意，`/home` 路由不要开精准匹配**），会成功的匹配，成功后再在 Home 组件里面去匹配相应的路由，从而找到 `/home/news` 进行匹配，因此找到 News 组件，进行匹配渲染。

> 如果开启精确匹配的话，第一步的 `/home/news` 匹配 `/home` 就会卡住不动，从而执行最后的 Redirect，这个时候就达不到想要的效果了。

## 10.传递参数

接着上面的案例，这里需要实现的效果是，点击 Message 的消息列表，在下方展示出消息的详细内容（Detail 组件）。

![image-20221026132713300](https://i0.hdslb.com/bfs/album/fa7796c368394294427cf86ca9eb16a3ddae2a42.png)

可以发现，其实就是一个三级路由，参考前面的很快就能写出来，但是 Detail 组件中的结构都是相同的，只是数据不同（根据不同的 Message），因此我们不要直接写死，而是要根据哪个 Message，把对应的数据传递给 Detail 展示就好了。

我们首先别把 Message 写死，使用动态数据：
```jsx
import React, {Component} from 'react';
import {Link, Route} from "react-router-dom";
import Detail from "./Detail";

class Message extends Component {

    state = {
        messageArr: [
            {id: '01', title: '消息 1''},
            {id: '02', title: '消息 2''},
            {id: '03', title: '消息 3''}
        ],
    }

    render() {
        const {messageArr} = this.state

        return (
            <div>
                <ul>
                    {
                        messageArr.map((message) => {
                            return (
                                <li key={message.id}>
                                    <Link to={`/home/message/detail/${message.id}`}>{message.title}</Link>
                                </li>
                            )
                        })
                    }
                </ul>
                <hr/>
                <Route path={`/home/message/detail/???`} component={Detail}></Route>
            </div>
        );
    }
}

export default Message;
```

Detail 组件：

```jsx
import React, {Component} from 'react';

// mock 数据
// const details = [
//     {id: '01', content: '消息 1 详情'},
//     {id: '02', content: '消息 2 详情'},
//     {id: '03', content: '消息 3 详情'},
// ]

class Detail extends Component {
    render() {
        console.log(this.props)
        return (
            <div>
                <ul>
                    <li>title: msg title</li>
                    <li>content: Detail content</li>
                </ul>
            </div>
        );
    }
}

export default Detail;
```

可以发现，Route 标签中需要把 message 的 id 和 title 传递给 Detail（Detail 中再拿到对应的详情），所以就涉及到了路由参数的传递。有三种方式：

- 传递 params 参数；
- 传递 search 参数；
- 传递 state 参数。

### 10.1 传递 params 参数

通过将数据拼接在路由地址末尾来实现数据的传递，我们将消息列表的 id 写在了路由地址后面：

```jsx
{/* 向路由组件传递 params 参数 */}
<Link to={`/home/message/detail/${message.id}/${message.title}`}>{message.title}</Link>
```

在注册路由时，我们可以通过 `:参数名` 来声明接收并传递数据：

```jsx
{/* Route 声明接收 params（id, title）并传递 */}
<Route path={`/home/message/detail/:id/:title`} component={Detail}></Route>
```

这样我们既成功的实现了路由的跳转，又将需要获取的数据传递给了 Detail 组件。

我们在 Detail 组件中使用 `this.props` 来查看当前接收的数据。

我们可以发现，我们传递的数据被接收到了对象的 match 属性下的 params 中：

![image-20231109095857793](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311090959826.png)

因此通过 params 数据中的 `id` 值，在详细内容的数据集中查找出指定 `id` 的详细内容即可：

```js
import React, {Component} from 'react';

// mock 数据
const details = [
    {id: '01', content: '消息 1 详情'},
    {id: '02', content: '消息 2 详情'},
    {id: '03', content: '消息 3 详情'},
]

class Detail extends Component {
    render() {
        // 接收 params 参数
        const {id, title} = this.props.match.params;
        const findDetail = details.find((detail) => {
            return detail.id === id;
        })

        return (
            <div>
                <ul>
                    <li>title: {title}</li>
                    <li>content: {findDetail.content}</li>
                </ul>
            </div>
        );
    }
}

export default Detail;
```

最后渲染数据即可：

![image-20231109100328078](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311091003976.png)

### 10.2 传递 search 参数

我们还可以采用传递 search 参数的方法来实现，search 参数其实就是在地址栏上搜索什么，就会以下面这种格式呈现：

```text
https://www.bing.com/search?q=react&mkt=zh-CN
```

- 问号后面的就是 search 参数，参数之间是用 & 连接的。

我们先在 Link 中采用 `?` 符号的方式来表示后面的为可用数据：

```jsx
{/* 向路由组件传递 search 参数 */}
<Link to={`/home/message/detail/?id=${message.id}&title=${message.title}`}>{message.title}</Link>
```

采用 `search` 传递的方式，无需在 Route 中再次声明传递，可以在 Detail 组件中直接获取到。数据保存在了 `location` 对象下的 `search` 中，是一种字符串的形式保存的：

![image-20221026132937804](https://i0.hdslb.com/bfs/album/69449e3f4ed6426440f3396601426796fecbe283.png)

我们可以引用一个库来进行字符串到对象的转化：

>   `qs` 是一个 npm 仓库所管理的包，可通过 `yarn add qs` 命令进行安装。
>
>   1. `qs.parse()` 将URL解析成对象的形式
>
>   2. `qs.stringify()` 将对象 序列化成 URL 的形式，以&进行拼接
>
>   ```js
>   // nodejs中调试
>   const qs = require('qs');
>   
>   // 1.qs.parse()
>   const str = "username='admin'&password='123456'";
>   console.log(qs.parse(str)); 
>   // Object { username: "admin", password: "123456" }
>   
>   // 2.qs.stringify()
>   const a = qs.stringify({ username: 'admin', password: '123456' });
>   console.log(a); 
>   // username=admin&password=123456
>   ```

我们可以采用 `parse` 方法，将字符串转化为键值对形式的对象

```js
// 接收 search 参数，是一个 querystring 形式
const {search} = this.props.location;
const {id, title} = qs.parse(search.slice(1));  // 从?后面开始截取字符串
```

### 10.3 传递 state 参数

采用传递 state 参数的方法，有一个优势，**它不会将数据携带到地址栏上，采用内部的状态来维护**。

首先，我们需要在 Link 中注册跳转时，传递一个路由对象，包括一个 跳转地址名，一个 state 数据，这样我们就可以在 Detail 组件中获取到这个传递的 state 数据：

> 注意：采用这种方式传递，也无需声明接收

```jsx
{/* 向路由组件传递 state 参数，to 属性接收一个对象，里面指定路径，state */}
<Link to={ {pathname: `/home/message/detail`, state: {id: message.id, title: message.title}} }>{message.title}</Link>
```

我们可以在 Detail 组件中的 location 对象下的 state 中取出我们所传递的数据

```js
const { id, title } = this.props.location.state
```

![image-20221026133411288](https://i0.hdslb.com/bfs/album/6853ae0a50d6f85112b8c667b88f789fb74df793.png)

不过这样会有一个隐藏的 bug，由于 state 方式没有在地址栏上体现参数，所以当清除了浏览器缓存后（把浏览器 history 清空了），在地址栏直接输入 `http://localhost:3000/home/message/detail`，会报错，因为此时 `this.props.location.state` 为 `undefined`（因为 state 还没传递，要点击一下消息 Link 才传递）。

为了解决这个问题，可以在获取不到 `state` 时，则用空对象代替，这样不会报错，只是会展示空值：

```js
// 接收 state 参数
const {id, title} = this.props.location.state || {};

const findDetail = details.find((detail) => {
    return detail.id === id;
}) || {};
```

注意：**这里的 state 和组件三大属性里的 state 有所不同**。

### 10.4 小结

1. params 参数：
    - 路由链接(携带参数)：`<Link to='/demo/test/tom/18'}>详情</Link>`
    - 注册路由(声明接收)：`<Route path="/demo/test/:name/:age" component={Test}/>`
    - 接收参数：`this.props.match.params`
2. search 参数：
    - 路由链接(携带参数)：`<Link to='/demo/test?name=tom&age=18'}>详情</Link>`
    - 注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
    - 接收参数：`this.props.location.search`
    - 备注：获取到的 search 是 urlencoded 编码字符串，需要借助 querystring 解析
3. state 参数：
    - 路由链接(携带参数)：`<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>`
    - 注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
    - 接收参数：`this.props.location.state`
    - 备注：刷新也可以保留住参数，为了防止清除缓存后刷新，可以使用空对象代替 undefined

## 11.路由跳转

### 11.1 push 与 replace 模式

默认情况下，开启的是 push 模式，也就是说，每次点击跳转，都会向栈中压入一个新的地址，在点击返回时，可以返回到上一个打开的地址。

当我们在读消息的时候，有时候我们可能会不喜欢这种繁琐的跳转，我们可以开启 replace 模式，这种模式与 push 模式不同，它会将当前地址**替换**成点击的地址，也就是 **替换了新的栈顶**。

我们只需要在需要开启的链接上加上 `replace` 即可。

```jsx
<Link replace to={ {pathname: `/home/message/detail`, state: {id: message.id, title: message.title}} }>{message.title}</Link>
```

这样当从 `/home/news` 点击到 `/home/message` -> `/home/message/detail`（把消息都点一遍），当回退时，直接会返回 `/home/news` 页面，因为当点击 `/home/message/detail` 时把 `/home/message` 替换了。

### 11.2 编程式路由导航

我们之前的 Demo 都是借助 `<Link>` 或 `<NavLink>` 实现的路由导航，是需要鼠标点击才能跳转。但如果我要实现从 `/home/news` 三秒后自动跳转到 `/home/message`，如何实现呢？

这就要使用编程式的路由导航了，借助 `this.prosp.history` 对象上的 API 操作路由跳转、前进、后退等：

- `this.prosp.history.push(path)`：push 方式跳转；
- `this.prosp.history.replace(path)`：replace 方式跳转；
- `this.prosp.history.goBack()`：后退；
- `this.prosp.history.goForward()`：前进；
- `this.prosp.history.go(n)`：前进/后退 |±n| 步。

我们可以采用绑定事件的方式实现路由的跳转，我们在 News 组件的 render 中启动定时器，3s 后跳转到 `/home/message` 路由即可：

```jsx
class News extends Component {
    render() {
        setTimeout(() => {
            this.props.history.push('/home/message');
        }, 3000);

        return (
            <div>
                <ul>
                    <li>news001</li>
                    <li>news002</li>
                    <li>news003</li>
                </ul>
            </div>
        );
    }
}
```

下面再展示一下编程式路由导航传递参数的方式，其实和使用标签的形式差不多，只是换成了函数：

```js
showByPush = (id, title) => {
    //push 跳转+携带params参数
    // this.props.history.push(`/home/message/detail/${id}/${title}`)

    //push 跳转+携带search参数
    // this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

    //push 跳转+携带state参数
    this.props.history.push(`/home/message/detail`, {id, title})
}

showByReplace = (id, title) => {
    //replace 跳转+携带params参数
    //this.props.history.replace(`/home/message/detail/${id}/${title}`)

    //replace 跳转+携带search参数
    // this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

    //replace 跳转+携带state参数
    this.props.history.replace(`/home/message/detail`, {id, title})
}
```

### 11.3 withRouter

当我们需要在页面内部添加回退前进等按钮时，由于这些组件可能是通过 **一般组件** 的方式去编写，因此 **无法获得 history 对象**，只有路由组件才能获取到 history 对象，因此我们需要如何解决这个问题呢

我们可以利用 `react-router-dom` 对象下的 `withRouter` 函数来 **对导出的 `Header` 组件进行包装**，这样我们就能获得一个拥有 `history` 对象的一般组件。

我们需要对哪个组件包装就在哪个组件下引入：

```js
import React, {Component} from 'react';
import {withRouter} from "react-router-dom";

class Header extends Component {

    // 编程式路由导航，一般组件中不能直接使用，得用 withRouter 包装该组件
    back = () => {
        this.props.history.goBack();
    }
    forward = () => {
        this.props.history.goForward();
    }

    render() {
        return (
            <div className="page-header">
                <h2>React Router Demo</h2>
                <button onClick={this.back}>后退</button> &nbsp;
                <button onClick={this.forward}>前进</button>
            </div>
        );
    }
}

// withRouter 可包装一般组件，返回一个新组件，能让其有路由组件的 API
export default withRouter(Header);
```

## 12.BrowserRouter 和 HashRouter 的区别

**标签不同**：

```jsx
<BrowserRouter>
    <App />
</BrowserRouter>

<HashRouter>
    <App />
</HashRouter>
```

**它们的底层实现原理不一样**：

- 对于 BrowserRouter 来说它使用的是 React 为它封装的 history API ，这里的 history 和浏览器中的 history 有所不同！通过操作这些 API 来实现路由的保存等操作，但是这些 API 是 H5 中提出的，因此不兼容 IE9 以下版本。
- 对于 HashRouter 而言，它实现的原理是通过 URL 的哈希值。用一个简单的解释就是可以理解为是锚点跳转，因为锚点跳转会保存历史记录，从而让 HashRouter 有了相关的前进后退操作，HashRouter 不会将 `#` 符号后面的内容请求。兼容性更好！

**地址栏的表现形式不一样**：

- HashRouter 的路径中包含 `#` ，例如 `localhost:3000/#/demo/test`

**刷新后路由 state 参数改变**：

- 在 BrowserRouter 中，state 保存在 history 对象中，刷新不会丢失
- **HashRouter 刷新会丢失 state**

