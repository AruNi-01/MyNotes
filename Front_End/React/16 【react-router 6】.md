

> 关于路由的知识已在`11【react-router 5】`中进行说明，这里主要是对于5版本的api的变换说明

## 1. 概述

官方文档：[Home v6.4.1 | React Router](https://reactrouter.com/en/main)React Router 以三个不同的包发布到 npm 上，它们分别为：

1. react-router: 路由的核心库，提供了很多的：组件、钩子。
2. 包含 react-router 所有内容，并添加一些专门用于 DOM 的组件，例如 `<BrowserRouter>`等。
3. react-router-native: 包括react-router所有内容，并添加一些专门用于ReactNative的API，例如:`<NativeRouter>`等。

与React Router 5.x 版本相比，改变了什么？

1. 内置组件的变化：移除`<Switch/>` ，新增 `<Routes/>`等。

2. 语法的变化：`component={About}` 变为 `element={<About/>}`等。

3. 新增多个hook：`useParams`、`useNavigate`、`useMatch`等。

4. **官方明确推荐函数式组件了**！！！

   ......

安装：

```bash
npm install react-router-dom@6
yarn add react-router-dom@6
```

## 2. Routes 与 Route

1. v6版本中移出了先前的 `<Switch>`，引入了新的替代者：`<Routes>`。

2. `<Routes>` 和 `<Route>` 要配合使用，且必须要用 `<Routes>` 包裹 `<Route>`。

3. `<Route>` 相当于一个 if 语句，如果其路径与当前 URL 匹配，则呈现其对应的组件。

4. `<Route caseSensitive>` 属性用于指定：匹配时是否区分大小写（默认为 false）。

5. 当 URL 发生变化时，`<Routes>` 都会查看其所有子 ` <Route>`  元素以找到最佳匹配并呈现组件 。

6. **`<Route>` 也可以嵌套使用**（组多级路由），且可配合 `useRoutes()` 配置 “路由表” ，但需要通过 `<Outlet>` 组件来渲染其子路由。

**Route**：

Route 用来定义一个访问路径与 React 组件之间的关系。比如说，如果你希望用户访问 `https://your_site.com/about` 的时候加载 `<About />` 这个 React 页面，那么你就需要用 Route:

```jsx
<Route path="/about" element={<About />} />
```

**Routes**：

Routes 是用来包住路由访问路径(Route)的。它决定用户在浏览器中输入的路径到对应加载什么 React 组件，因此绝大多数情况下，Routes 的唯一作用是用来包住一系列的 `Route`，比如如下

```jsx
import { Routes, Route } from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  );
}
```

在这里，Routes 告诉了 React Router 每当用户访问根地址时，加载 `Home` 这个页面，而当用户访问 `/about` 时，就加载 `<About />` 页面。

**完整代码**

```jsx
<Routes>
    /*path属性用于定义路径，element属性用于定义当前路径所对应的组件*/
    <Route path="/login" element={<Login />}></Route>

    /*用于定义嵌套路由，home是一级路由，对应的路径/home*/
    <Route path="home" element={<Home />}>
           /*test1 和 test2 是二级路由,对应的路径是/home/test1 或 /home/test2*/
          <Route path="test1" element={<Test/>}></Route>
          <Route path="test2" element={<Test2/>}></Route>
    </Route>
	
    //Route也可以不写element属性, 这时就是用于展示嵌套的路由 .所对应的路径是/users/xxx
    <Route path="users">
       <Route path="xxx" element={<Demo />} />
    </Route>
</Routes>
```

## 3. Navigate

作用：**只要 `<Navigate>` 组件被渲染，就会修改路径，切换视图**。

Navigate 中 `replace` 属性用于控制跳转模式（push 或 replace，默认是 push）。

> 相当于 5 版本的 Redirect，Redirect 语义化会更好。

使用 Navigate 组件实现重定向，当访问 / 时，重定向至 /about，即默认展示 About 组件：

```jsx
<Routes>
    <Route path="/about" element={<About/>}></Route>
    <Route path="/home" element={<Home/>}></Route>
    <Route path="/" element={<Navigate to="/about"/>}></Route>
</Routes>
```

Navigate 还可以实现编程式导航：

```jsx
export default function Home() {
    const [sum, setSum] = React.useState(1);

    return (
        <div>
            <h2>Home 内容</h2>

            {/* 根据sum的值决定是否切换视图 */}
            {
                sum === 1
                    ? <h4>sum的值为{sum}</h4>
                    : <Navigate to="/about" replace={true} />
            }
            <button onClick={() => setSum(2)}>点我将sum变为2</button>
        </div>
    );
}
```

## 4. NavLink

作用: 与 `<Link>` 组件类似，且可实现导航点击后的 “高亮” 效果。

NavLink 默认 className 是 active，也可以自己指定覆盖它。

在 Router5 中，我们想自定义 active style 只需要指定一个 `activeClassName=""` 属性，而 Router6 中需要将 className 属性写成一个函数：

```jsx
{/* 自定义点击后的样式，需要将 className 指定为一个函数，其参数是一个对象，对象内的 isActive 变量(这里直接解构赋值了)决定是否点击激活了该路由 */}
<NavLink to="/home" className={ ({isActive}) => isActive ? 'list-group-item style01' : 'list-group-item' }>Home</NavLink>
```

这样当点击了 /home 时，两个样式都能显示出来了，list-group-item 是 bootstrap 样式库，style01 是我自定义的：

```css
.style01 {
    background-color: #71bb71 !important;   /* bootstrap 的 css 样式优先级高，因此用 important */
}
```

一般有很多 NavLink 都需要用同样的样式，所以一般会把这个函数抽离出来：

```jsx
function computedClassName({isActive}) {
    return isActive ? 'list-group-item style01' : 'list-group-item';
}

<NavLink to="/home" className={ computedClassName }>Home</NavLink>
```

默认情况下，当 Home 的子组件匹配成功，Home 的导航也会高亮，而当 NavLink 上添加了 `end` 属性后，若 Home 的子组件匹配成功，则 Home 的导航没有高亮效果：

````jsx
<NavLink to="/home" className={ computedClassName } end>Home</NavLink>
````

## 5. useRoutes 路由表注册路由

 `useRoutes()`：它是一个路由表，可根据此动态创建 `<Routes>` 和 `<Route>`，就不用我们写那些路由规则了，只用管 Link 的编写。

```jsx
import React from "react";
import {Navigate, NavLink, useRoutes} from "react-router-dom";
import "./App.css";
import About from "./pages/About";
import Home from "./pages/Home";
import Header from "./components/Header";

export default function App() {
    const element = useRoutes([
        {
            path: "/about",
            element: <About/>
        },
        {
            path: "/home",
            element: <Home/>
        },
        {
            path: "/",
            element: <Navigate to="/home"/>
        }
    ]);

    return (
        <div>
            <div className="row">
                <div className="col-xs-offset-2 col-xs-8">
                    <Header/>
                </div>
            </div>
            <div className="row">
                <div className="col-xs-2 col-xs-offset-2">
                    <div className="list-group">
                        <NavLink to="/about" className="list-group-item">About</NavLink>
                        <NavLink to="/home" className="list-group-item">Home</NavLink>
                    </div>
                </div>
                <div className="col-xs-6">
                    <div className="panel">
                        <div className="panel-body">
                            {/* Routes == Switch，而且 Routes 是必须的 */}
                            {/* <Routes> */}
                                {/* element == component */}
                                {/* <Route path="/about" element={<About/>}></Route> */}
                                {/* <Route path="/home" element={<Home/>}></Route> */}
                                {/* Navigate 相当于 Redirect */}
                                {/* <Route path="/" element={<Navigate to="/home"/>}></Route> */}
                            {/* </Routes> */}

                            {/* 用 useRoutes 代替 Routes 注册路由 */}
                            {element}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    )
}
```

一般在项目中，会在 src 目录下创建一个 routes 文件夹，所有的路由规则都放里面。

`routes/index.js`

```js
import About from "../pages/About"
import Home from "../pages/Home"
import { Navigate } from "react-router-dom"

const routes = [
    {
        path: "/about",
        element: <About/>
    },
    {
        path: "/home",
        element: <Home/>
    },
    {
        path: "/",
        element: <Navigate to="/home"/>
    }
]

export default routes
```

`App.js`

```jsx
const element = useRoutes(routes);
// ......
{/* 用 useRoutes 代替 Routes 注册路由 */}
{element}
```

## 6. 嵌套路由

路由结构如下：

- /about，About组件
- /home，Home组件
    - /home/news，News组件
    - /home/message，Message组件

![image-20221027122205526](https://i0.hdslb.com/bfs/album/8a6da962aea796cb490cb74582edd7e18dab8a65.png)

在路由表中用 **`children`** 来编写嵌套路由，`routes/index.js`

```jsx
import { Navigate } from "react-router-dom"
import About from "../pages/About"
import Home from "../pages/Home"
import News from "../pages/Home/News"
import Message from "../pages/Home/Message"

const routes = [
    {
        path: "/about",
        element: <About/>
    },
    {
        path: "/home",
        element: <Home/>,
        children: [
            {
                path: "news",
                element: <News/>
            },
            {
                path: "message",
                element: <Message/>
            }
        ]
    },
    {
        path: "/",
        element: <Navigate to="/home"/>
    }
]

export default routes

```

编写完路由表后，还需要编写组件的呈现位置，此时需要使用到 `<Outlet>`：

- 作用：当 `<Route>` 产生嵌套时，**渲染其对应的后续子路由**；
- 注意不要和 useRoutes 弄混了，useRoutes 是整体页面注册路由使用，而 Outlet 只是一个子路由的占位，根据不同的子路由显示不同的组件。

路由链接中的 `to` 属性值，可以是：

- `to="/home/news"`，全路径 (推荐这样写，不然直接看不知道是不是子路由)
- `to="./news"`，相对路径；
- `to="news"`，直接写子路由路径。

`Home/index.jsx`：

```jsx
import React from 'react';
import {NavLink, Outlet} from "react-router-dom";

export default function Home() {
    return (
        <div>
            <h2>Home 内容</h2>
            <ul className="nav nav-tabs">
                <li>
                    <NavLink className="list-group-item" to="news">News</NavLink>
                </li>
                <li>
                    <NavLink className="list-group-item" to="./message">Message</NavLink>
                </li>
            </ul>

            <Outlet/>

        </div>
    );
}
```

## 7. 路由传递参数

需求描述：点击“消息1”，显示其 id、title 和 content。

![371844431d4c4553a1fbfd9d01fb140a](https://i0.hdslb.com/bfs/album/cd93da0f0a1b8cb3c10a95316774424e8a90c2a3.gif)

router5 中我们知道，有三种参数传递方式，params、search、state，router6 也一样，只是写法有些不同。

### 7.1 传递 params 参数

路由表中写占位参数，`routes/index.js`：

```jsx
        {
            path: "message",
            element: <Message/>,
            children: [
                {
                    path: "detail/:id/:title/:content",
                    element: <Detail/>
                }
            ]
        }
```

Message 组件传递参数：

```jsx
import React from 'react';
import { Link, Outlet } from 'react-router-dom';

export default function Message() {
    const [messages] = React.useState([
        {id: '001', title: 'message001', content: 'message001 content'},
        {id: '002', title: 'message002', content: 'message002 content'},
        {id: '003', title: 'message003', content: 'message003 content'}
    ]);

    return (
        <div>
            <ul>
                {
                    messages.map((message) => {
                        return (
                            <li key={message.id}>
                                <Link to={`detail/${message.id}/${message.title}/${message.content}`}>{message.title}</Link>
                            </li>
                        );
                    })
                }
            </ul>
            <hr/>
            <Outlet/>
        </div>
    );
}
```

Detail 组件接收参数，router5 中参数是放在 props 中的 history/location/match 中的，而函数式组件没有实例，没有 this，因此需要使用 Hooks：

- `useParams()`：回当前匹配路由的 `params` 参数，类似于 router5 中的`match.params`；
- `useMatch(path)`：返回指定 path 匹配的信息，里面也有 params，对标 router5 中的路由组件的 `match` 属性。

```jsx
import React from 'react'
import { useParams } from 'react-router-dom'

export default function Detail() {
    const {id, title, content} = useParams();

    // 使用的不多
    // const {params:{id,title,content}}= useMatch("/home/message/detail/:id/:title/:content");

    return (
        <ul>
            <li>id: {id}</li>
            <li>title: {title}</li>
            <li>content: {content}</li>
        </ul>
    )
}
```



**总结**，获取params参数有两种方式：

1. 使用 **useParams**
    **`const {id,title,content} = useParams();`**
2. 使用 **useMatch**
    **`const {params: {id,title,content}}= useMatch("/home/message/detail/:id/:title/:content");`**
    ![image-20221027221736759](https://i0.hdslb.com/bfs/album/3d4eb676c19c7cf4962b27e7bfedff9da5afe804.png)

### 11.2 传递 search 参数

演示的需求和上面 `params` 参数一样，所以只修改关键部分

`routes/index.js` 不用写占位参数了：

```jsx
        {
            path: "message",
            element: <Message/>,
            children: [
                // {
                //     path: "detail/:id/:title/:content",
                //     element: <Detail/>
                // }
                {
                    path: "detail",
                    element: <Detail/>
                }
            ]
        }
```

`Message/index.jsx`（Message组件）：

```jsx
<li key={message.id}>
    {/* <Link to={`detail/${message.id}/${message.title}/${message.content}`}>{message.title}</Link> */}

    <Link to={`detail?id=${message.id}&title=${message.title}&content=${message.content}`}>{message.title}</Link>
</li>
```

`Detail/index.jsx`（Detail组件），同样需要使用 Hooks：

- `useSearchParams()`：用于读取和修改当前位置的 URL 中的查询字符串。
    返回一个包含两个值的数组，内容分别为：当前的 seaech 参数、更新 search 的函数。
- `useLocation()`：获取当前 location 信息，对标 5.x 中的路由组件的 `location` 属性。

**使用 useSearchParams**

```jsx
import React from 'react'
import { useSearchParams } from 'react-router-dom'

export default function Detail() {
    // const {id, title, content} = useParams();
    // 使用的不多
    // const {params:{id,title,content}}= useMatch("/home/message/detail/:id/:title/:content");

    const [searchParams, setSearchParams] = useSearchParams();
    const id = searchParams.get('id');
    const title = searchParams.get('title');
    const content = searchParams.get('content');

    return (
        <ul>
            <li>id: {id}</li>
            <li>title: {title}</li>
            <li>content: {content}</li>
            <button onClick={() => setSearchParams({id: '009', title: 'message009', content: 'message009 content'})}>更新 search params</button>
        </ul>
    )
}
```



**使用 useLocation**，和 router5 很类似。

> 记得下载安装qs：`npm install --save qs`

```jsx
import { useLocation } from 'react-router-dom'
import qs from "qs";

export default function Detail() {
  const {search} = useLocation();
  const {id,title,content} = qs.parse(search.slice(1));

  return (
    <ul>
      <li>id: {id}</li>
      <li>title: {title}</li>
      <li>content: {content}</li>
    </ul>
  )
}
```

**总结**，获取 search 参数，有两种写法：

1. 使用 useSearchParams
2. 使用 useLocation

### 11.3 传递 state 参数

`routes/index.js` 也不用写占位参数：

```js
        {
            path:"message",
            element:<Message/>,
            children:[
                {
                    path:"detail",
                    element:<Detail/>
                }
            ]
        },
```

`Message/index.jsx`（Message组件），router5 中需要在 Link 标签中的 to 属性中写一个对象，把 pathname 和 state 传进去：

```jsx
<Link to={ {pathname: `/home/message/detail`, state: {id: message.id, title: message.title}} }>{message.title}</Link>
```

而 router6 中不用这么麻烦，直接写一个 state 属性即可：

```jsx
<li key={message.id}>
    {/* <Link to={`detail/${message.id}/${message.title}/${message.content}`}>{message.title}</Link> */}

    {/* <Link to={`detail?id=${message.id}&title=${message.title}&content=${message.content}`}>{message.title}</Link> */}

    <Link to="detail" state={{ id: message.id, title: message.title, content: message.content }}>{message.title}</Link>
</li>
```

`Detail/index.jsx`（Detail组件），使用 useLocation，从中读取 state：

```jsx
import React from 'react'
import { useLocation } from 'react-router-dom'

export default function Detail() {
    // const {id, title, content} = useParams();
    // 使用的不多
    // const {params:{id,title,content}}= useMatch("/home/message/detail/:id/:title/:content");

    // const [searchParams, setSearchParams] = useSearchParams();
    // const id = searchParams.get('id');
    // const title = searchParams.get('title');
    // const content = searchParams.get('content');
    // 使用 location
    // const { search } = useLocation();
    // const { id, title, content } = qs.parse(search.slice(1));

    const { state: { id, title, content } } = useLocation();

    return (
        <ul>
            <li>id: {id}</li>
            <li>title: {title}</li>
            <li>content: {content}</li>
            {/* <button onClick={() => setSearchParams({ id: '009', title: 'message009', content: 'message009 content' })}>更新 search params</button> */}
        </ul>
    )
}
```

> **刷新页面后对路由 state 参数的影响**：
>
> 在以前版本中，BrowserRouter 没有任何影响，因为 state 保存在history对象中；HashRouter 刷新后会导致路由 state 参数的丢失。
>
> 但在 V6 版本中，HashRouter 在页面刷新后不会导致路由 state 参数的丢失，但是现在网站基本也没看过路径有个`#`，所以我们使用`BrowserRouter`就行了。



## 8. 编程式路由导航

在 router5 中，我们实现编程式路由导航使用的时 `this.props.history` 上面的 API，但在函数式组件中，就得使用 Hooks 了。

我们在学习 Navigate 组件时，说 **只要 `<Navigate>` 组件被渲染，就会修改路径，切换视图**。但是编程式导航一般是在 function 中跳转路由，因此又得使用 Hooks 了，Navigate 有个 useNavigate Hook，可以返回一个 navigate，用于跳转。

案例还是和`7. 路由传递参数`一样，只是换了种方式跳转路由、传参数。

### 8.1 编程式导航下，路由传递 params 参数

```jsx
import React,{useState} from 'react'
import { NavLink,Outlet,useNavigate } from 'react-router-dom'

export default function Message() {
    const [message] = useState([
        {id:"001",title:"消息1",content:"窗前明月光"},
        {id:"002",title:"消息2",content:"疑是地上霜"},
        {id:"003",title:"消息3",content:"举头望明月"},
        {id:"004",title:"消息4",content:"低头思故乡"}
    ])

    // 只要 `<Navigate>` 组件被渲染，就会修改路径，切换视图，但编程式导航一般是在 function 中跳转路由，
    // 因此又得使用 Hooks 了，Navigate 有个 useNavigate Hook，可以返回一个 navigate，用于跳转。
    const navigate = useNavigate();
    function handleClick(msgObj){
        const {id,title,content} = msgObj
        // navigate 方法有两个参数，第一个是跳转路径，第二个是一个对象，可以添加 state、配置 replace
        navigate(`detail/${id}/${title}/${content}`,{replace:false})
    }

    return (
        <div>
            <ul>
            {
                message.map(msgObj => {
                    return (
                        <li key={msgObj.id}>
                            <NavLink to={`detail/${msgObj.id}/${msgObj.title}/${msgObj.content}`} >{msgObj.title}</NavLink>
                            <button onClick={() => handleClick(msgObj)}>查看消息详情</button>
                        </li>
                    )
                })
            }
            </ul>
            <hr />
            <Outlet/>
        </div>
    )
}
```

### 8.2 编程式导航下，路由传递search参数

```jsx
export default function Message() {

    const navigate = useNavigate();

    function handleClick(msgObj){
        const {id,title,content} = msgObj
        // navigate 方法有两个参数，第一个是跳转路径，第二个是一个对象，可以添加 state、配置 replace
        navigate(`detail?id=${id}&title=${title}&content=${content}`, {replace:false})
    }

    return (
        <div>
            <ul>
            {
                message.map(msgObj => {
                    return (
                        <li key={msgObj.id}>
                            <NavLink to={`detail?id=${msgObj.id}&title=${msgObj.title}&content=${msgObj.content}`} >{msgObj.title}</NavLink>
                            <button onClick={() => handleClick(msgObj)}>查看消息详情</button>
                        </li>
                    )
                })
            }
            </ul>
            <hr />
            <Outlet/>
        </div>
    )
}
```

### 8.3 编程式导航下，路由传递state参数

```jsx
export default function Message() {

    const navigate = useNavigate();

    function handleClick(msgObj){
        const {id,title,content} = msgObj
        // navigate 方法有两个参数，第一个是跳转路径，第二个是一个对象，可以添加 state、配置 replace
        navigate("detail", {
            replace: false,
            state:{
                id,
                title,
                content
            }
        })
    }

    return (
        <div>
            <ul>
            {
                message.map(msgObj => {
                    return (
                        <li key={msgObj.id}>
                            <NavLink to="detail" state={{ id: msgObj.id, title: msgObj.title, content: msgObj.content }} >{msgObj.title}</NavLink>
                            <button onClick={() => handleClick(msgObj)}>查看消息详情</button>
                        </li>
                    )
                })
            }
            </ul>
            <hr />
            <Outlet/>
        </div>
    )
}
```

### 8.4 withRouter 的替换者

router5 版本的时候，借助 `this.prosp.history` 对象上的 API 对操作路由跳转、前进、后退。

具体来说就是 **使用 `withRouter` 函数来对我们导出的 `Header` 组件进行包装这样我们就能获得一个拥有 `history` 对象的一般组件**：

```jsx
// withRouter 可包装一般组件，返回一个新组件，能让其有路由组件的 API
export default withRouter(Header);
```

但 router6 版本中已废除 withRouter，**可以直接用 useNavigate 实现**：

````jsx
import React from 'react';
import { useNavigate } from 'react-router-dom';

function Header() {
    const navigate = useNavigate();

    function back() {
        navigate(-1);
    }

    function forward() {
        navigate(1);
    }

    function go(n) {
        navigate(n);
    }

    return (
        <div className="page-header">
            <h2>React Router Demo</h2>
            <button onClick={back}>回退</button> &nbsp;
            <button onClick={forward}>前进</button> &nbsp;
            <button onClick={() => go(2)}>Go 2</button>
        </div>
    );
}

export default Header;
````

## 9. 如何设置默认页路径 (如 404 页)

在上文的路由列表 `Routes` 中，我们可以加入一个 `catch all` 的默认页面，比如用来作 404 页面。

我们只要在最后加入 `path` 为 `*` 的一个路径，意为匹配所有路径，即可

```js
import { Navigate } from "react-router-dom"
import About from "../pages/About"
import Home from "../pages/Home"
import News from "../pages/Home/News"
import Message from "../pages/Home/Message"
import Detail from "../pages/Home/Message/Detail"

const routes = [
    {
        path: "/about",
        element: <About/>
    },
    // ......
    {
        path: "/",
        element: <Navigate to="/home"/>
    },
    {
        path: "*",
        element: <NotFound/>
    }
]
        
        
function NotFound() {
    return <h1>这是 404 Not Found...</h1>
}

export default routes
```

## 10. 其他 Router6 Hooks

太多了，官网查看即可：https://reactrouter.com/en/main/hooks/use-action-data

