## 1. setState 扩展

对象式和函数式 setState，其实都是针对 setState 的第一个参数来说的。

### 1.1 对象式 setState

看下面的例子，控制台会输出什么呢？

```js
add = () => {
    const { count } = this.state
    this.setState({
        count: count + 1
    })
    console.log(this.state.count);
}
```

我们发现显示的 `count` 和我们控制台输出的 `count` 值是不一样的。

![image-20221027095134650](https://i0.hdslb.com/bfs/album/a4746c99b4345fa4e08eec081fcc0d9fb7a7553b.png)

这是因为，我们调用的 `setState` 是同步事件，但是它的作用是让 React 去更新数据，而 **React 不会立即的去更新数据，这是一个异步的任务**，因此我们输出的 `count` 值会是状态更新之前的数据。“React **状态更新是异步的**”

那我们要如何实现同步呢？其实 **在 `setState` 调用的第二个参数**，我们可以接收一个函数，这个函数会 **在状态更新完毕并且界面更新之后调用**。

`setState(stateChange, [callback])` ------ 对象式的 setState

        1. stateChange 为状态改变对象(该对象可以体现出状态的更改)
        2. callback 是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用

```js
add = () => {
    const {count} = this.state;
    // this.setState({count: count + 1});
    // console.log(this.state.count);   // 输出 +1 之前的值，因为 setState 是异步更新状态

    // 输出 +1 之后的值，第二个参数是一个可选函数，它在状态更新完毕、界面也更新后(render调用后)才被调用
    this.setState({count: count + 1}, () => {
            console.log(this.state.count);
        }
    )
}
```

### 1.2 函数式 setState

函数式的 `setState` 也是接收两个参数，`setState(updater, [callback])` ------ 函数式的 setState：

1. updater 为返回 stateChange 对象的函数。**updater 可以接收到 state 和 props**。
2. callback 是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。

与对象式 `setState` 不同的是，我们传递的第一个参数 `updater` 可以接收到 2 个参数 `state` 和 `props`。

```js
add = () => {
    // 对象式的 setState，可以收到 state 和 props
    this.setState(
        (state, props) => ({count: state.count + 1}),
        () => {
            console.log(this.state.count);
        }
    )
}
```

我们在第一个参数中传入了一个函数，这个函数可以接收到 `state` ，我们通过更新 `state` 中的 `count` 值，来驱动页面的更新

利用函数式 `setState` 的优势还是很不错的，可以直接获得 `state` 和 `props`

> 可以理解为对象式的 `setState` 是函数式 `setState` 的语法糖

## 2. 路由组件的 lazyLoad

懒加载在 React 中用的最多的就是路由组件了，如果我们有 100 个路由组件，但是用户只点击了几个，这就会有很大的消耗，因此我们需要做懒加载处理，**我们点击哪个时，才去加载哪一个**。

首先我们需要从 `react` 库中暴露一个 `lazy` 函数：

- `React.lazy()` 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。

```js
import React, { Component, lazy} from 'react';
```

然后我们需要更改引入组件的方式，不再直接使用 `import...from`，采用 `lazy` 函数包裹：

```js
// import About from "./pages/About";
// import Home from "./pages/Home";

// 路由组件懒加载
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
```

我们会遇到这样的错误，提示我们用一个标签包裹

![image-20221027095800440](https://i0.hdslb.com/bfs/album/956ae433c7a0f17a76ea7e5665e8f2e94ab3b925.png)

这里是因为，当我们网速慢的时候，路由组件就会有可能加载不出来，页面就会白屏，它需要我们来指定一个路由组件加载的东西，相对于 loading：

> `React.Suspense` 可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。
>
> 在未来，我们计划让 `Suspense` 处理更多的场景，如数据获取等。你可以在 [我们的路线图](https://zh-hans.reactjs.org/blog/2018/11/27/react-16-roadmap.html) 了解这一点。

```jsx
<Suspense fallback={<Loading/>}>
    <Switch>
        <Route path="/about" component={About}></Route>
        <Route path="/home" component={Home}></Route>
        <Redirect to = "/home"/>
    </Switch>
</Suspense>
```

`fallback` 会在页面还没有加载出来的时候显示。因为 loading 是作为一个兜底的存在，因此 **loading 是 必须提前引入的，不能懒加载**。

![image-20231110104334586](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311101043044.png)

lazyLoad 的用法：

- [当内容正在加载时显示后备方案](https://react.docschina.org/reference/react/Suspense#displaying-a-fallback-while-content-is-loading)
- [同时展示内容](https://react.docschina.org/reference/react/Suspense#revealing-content-together-at-once)
- [逐步加载内容](https://react.docschina.org/reference/react/Suspense#revealing-nested-content-as-it-loads)
- [在新内容加载时展示过时内容](https://react.docschina.org/reference/react/Suspense#showing-stale-content-while-fresh-content-is-loading)
- [阻止隐藏已经显示的内容](https://react.docschina.org/reference/react/Suspense#preventing-already-revealed-content-from-hiding)
- [表明 transition 正在发生](https://react.docschina.org/reference/react/Suspense#indicating-that-a-transition-is-happening)
- [在导航时重置 Suspense 边界](https://react.docschina.org/reference/react/Suspense#resetting-suspense-boundaries-on-navigation)
- [为服务器错误和客户端内容提供后备方案](https://react.docschina.org/reference/react/Suspense#providing-a-fallback-for-server-errors-and-client-only-content)

## 3. Hooks

(1). Hook是 React 16.8.0 版本增加的新特性/新语法
(2). 可以让你 **在函数组件中使用 state 以及其他的 React 特性**

三个常用的 Hook：

1. State Hook: `React.useState()`
2. Effect Hook: `React.useEffect()`
3. Ref Hook: `React.useRef()`

React 官网所有 Hooks：https://react.docschina.org/reference/react/hooks

### 3.1 State Hook

State Hook 让函数组件也可以有 state 状态, 并进行状态数据的读写操作

语法: `const [xxx, setXxx] = React.useState(initValue)` ，`useState()` 说明:

- 参数: 第一次 **初始化** 指定的值在内部作缓存
- 返回值: 包含 2 个元素的数组，第 1 个为 **内部当前状态值**，第 2 个为 **更新状态值的函数**

`setXxx()` 2 种写法:

- `setXxx(newValue)`: 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
- `setXxx(value => newValue)`: 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值

```jsx
// 函数式组件
function Demo() {

    // state hook，[状态值, 更新状态值的方法
    const [count, setCount] = React.useState(0);
    const [persons, setPerson] = React.useState(
        [
            {name: 'Tom', age: 18},
            {name: 'Jerry', age: 18},
            {name: 'Jack', age: 18}
        ]
    );

    function add() {
        // setCount(count + 1)

        setCount(oldValue => {
            return oldValue + 1;
        })
    }

    function addPersonAge() {
        // setPerson({name: 'Jack', age: 20});

        setPerson(oldPersonArr => {
            return oldPersonArr.map(person => {
                // 把 person 对象展开，然后修改 age 属性
                return {...person, age: person.age + 1}
            });
        })
    }

    return (
        <div>
            <span>Count: {count}</span> &nbsp;
            <button onClick={add}>Add 1</button>
            
            <br/> <br/> <br/>
            
            <span>Persons:
                {
                    persons.map(person => {
                        return <li key={person.name}>{person.name} - {person.age}</li>
                    })
                }
            </span>
            <br/>
            <button onClick={addPersonAge}>Change Person Info</button>
        </div>
    )
}
```

你可以会有这样一个疑问，如果 Demo 函数被多次执行时，state 是否都会重新被赋予初始值呢？答案是不会的，React 底层做了处理，否则 Hook 就没有任何意义了。

如果有多个 state，则需要使用多个 `useState()`

### 3.2 Effect Hook

**Effect Hook** 可以让你在 **函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)**。React中的副作用操作:

- 发 ajax 请求数据获取
- 设置订阅 / 启动定时器

- 手动更改真实 DOM

语法和说明，`useEffect()` 接收两个参数：

- 第一个参数是一个回调函数；
- 第二个参数是一个数组，里面存放监测的数据，用于控制上面回调函数中操作的执行。

```jsx
React.useEffect(() => { 
    // 副作用操作，会在 DidMount 和 DidUpdate 后调用。
    // 但 useEffect 第二个参数是 [] 时，就表示不监测数据的变化，相当于只是 DidMount。
    
    // return 函数在组件卸载前执行，相当于 WillUnmount
    return () => { 	
        // 在此做一些收尾工作, 比如清除定时器/取消订阅等
    }
}, [stateValue]) 	// 监测的数据，如果指定的是 [], 回调函数只会在第一次 render() 后执行
```

可以把 useEffect Hook 看做如下三个函数的组合：

- `componentDidMount()`
- `componentDidUpdate()`
- `componentWillUnmount()`

案例：

```jsx
function Demo() {

    // state hook，[状态值, 更新状态值的方法
    const [count, setCount] = React.useState(0);

    // effect hook，副作用钩子，两个参数，第一个是回调函数，第二个是监测数组
    React.useEffect(() => {
        // 副作用操作，会在 DidMount 和 DidUpdate 后调用。
        // 但 useEffect 第二个参数是 [] 时，就表示不监测数据的变化，相当于只是 DidMount。
        let timer = setInterval(() => {
            setCount(count => count + 1);
        }, 1000);

        // return 函数在组件卸载前执行，相当于 WillUnmount
        return () => {
            // 在此做一些收尾工作, 比如清除定时器/取消订阅等
            clearInterval(timer);
        }
    }, []);    // 监测的数据，如果指定的是 [], 回调函数只会在第一次 render() 后执行

    function add() {
        // setCount(count + 1)

        setCount(oldValue => {
            return oldValue + 1;
        })
    }

    function unmount() {
        // 新版创建/卸载根容器写法，index.js 中：
        // const root = ReactDOM.createRoot(document.getElementById('root'));
        // root.render(<App />);
        //
        // export default root;
        
        root.unmount();
    }

    return (
        <div>
            <span>Count: {count}</span> &nbsp;
            <button onClick={add}>Add 1</button> &nbsp;
            <button onClick={unmount}>Unmount Component</button>
        </div>
    )
}
```



useEffect 可以实现分别监测某个/某些 state 数据，来实现分别的。

### 3.3 Ref Hook

Ref Hook 可以在 **函数组件中存储/查找组件内的标签或任意其它数据**。
语法：`const refContainer = React.useRef()`
作用：保存标签对象，功能与 `React.createRef()`一样

```jsx
function Demo() {
    // ref hook，存储/查找组件内的标签或任意其它数据
    const myRef = React.useRef();

    function showInput() {
        alert(myRef.current.value);
    }

    return (
        <div>
            <input type="text" ref={myRef}/> &nbsp;
            <button onClick={showInput}>Click Show Input</button>
        </div>
    )
}
```

### 3.4 Reducer Hook

Reducer Hook 可以 **收集所有操作某个 state 的方案**，方便对同一个 state 集中操作。

直接看示例，根据点击不同的按钮，实现对 num 的加减重置操作：

```jsx
function Demo() {
    
    // reducer hook，收集所有操作某个数据的方案，方便调用。参数：
    //    · 第一个参数是 reducer 函数；
    //    · 第二个参数是 state 初始值；
    //    · 第三个参数是初始化状态的函数，用于复杂初始化（可选）。
    // 返回值：
    //    · num 是初始化好的 state；
    //    · dispatch 可以实现传入不同的 action 对象，来执行不同的操作
    const [num, dispatch] = React.useReducer(numReducer, 0);

    // reducer 函数，参数：
    //    · 第一个参数是当前 state 值；
    //    · 第二个参数是 action 对象（对象内容根据传入时来决定，一般用法是传入 type 和 payload）
    // 返回值是经过一系列操作后的新 state。
    function numReducer(currentValue, action) {
        switch (action.type) {
            case 'ADD':
                return currentValue + action.payload;
            case 'SUB':
                return currentValue - action.payload;
            case 'RESET':
                return 0;
            default:
                return currentValue;
        }
    }

    return (
        <div>
            {/* 使用 dispatch 时，传递 action 对象即可，当前 state 值会自动读取到，不用传递 */}
            <button onClick={ () => {dispatch({type: 'ADD', payload: 2})} }>Add 2</button> &nbsp;
            <button onClick={ () => {dispatch({type: 'SUB', payload: 3})} }>Sub 3</button> &nbsp;
            <button onClick={ () => {dispatch({type: 'RESET'})} }>Reset 0</button>
        </div>
    )
}
```

使用了 Reducer Hook，就用为每一个按钮都写一个类似的函数了，而是使用 reducer 统一操作。

### 3.5 Context Hook

见 Context 中的内容。

## 4. Context

### 4.1 为什么需要 Conetxt

Context 设计目的是为了 **共享** 那些 **对于一个组件树（父子组件搭建而成）而言是“全局”的数据**，某个组件可以随时使用这个组件树上的数据，例如当前认证的用户、主题或首选语言。

举个例子，在下面的代码中，我们通过一个 “theme” 属性手动调整一个按钮组件的样式：

```js
class A extends React.Component {
  render() {
    return <B theme="dark" />;
  }
}

function B(props) {
  // B 组件接受一个额外的“theme”属性，然后传递给 C 组件。
  // 如果应用中每一个单独的按钮都需要知道 theme 的值，这会是件很麻烦的事，因为必须将这个值层层传递所有组件。
  return (
    <div>
      <C theme={props.theme} />
    </div>
  );
}

class C extends React.Component {
  render() {
    return h4>我从A组件接收到的主题模式:{this.props.theme}</h4>
  }
}
```

使用 context，我们可以避免通过中间元素传递 props，下面来看看 context 是怎么解决的。

### 4.2 如何使用 Context

`XxxContext = React.createContext(defaultValue)`：

- 参数是 context 默认值，当往上层父组件中找不到指定的 context 内容时，就使用该值；
- 返回值 xxxConetxt 中有一个 Provider 组件，用于往下传递 context，其中可通过 `value` 属性指定 context 的内容（可以是任何类型）。

Context 的搜索流程是从外层查找最近的 Provider，然后返回它的 Context 值。如果没有找到 Provider，则会返回 Context 中设置的默认值。

在使用时，需要在组件中引入 `ThemeContext `，然后直接从 `this.context` 上取值即可：

```js
static contextType = XxxContext ;
```

函数式组件使用有一点不同，因为其没有 static 和 this，所以需要使用 Context 中的 Consumer 组件。

下面我们用一个案例，来看看怎么指定 context 和获取 context。

```jsx
import React, {Component} from 'react';

// 创建 context
const NameContext = React.createContext('defaultName');

function Demo() {

    const [name] = React.useState('tom');

    return (
        <div style={ {background: 'deepskyblue', width: '500px', padding: '8px'} }>
            <h1>我是父组件 Demo</h1>
            <h3>我的名字是：{name}</h3>
            {/* 向下提供 context（可修改值） */}
            <NameContext.Provider value={name}>
                <Child/>
            </NameContext.Provider>
        </div>
    )
}

class Child extends Component {
    // 类式组件中声明接收 context 后即可直接在 this 上使用，必须使用静态属性 contextType
    static contextType = NameContext;

    render() {
        return (
            <div style={ {background: 'lightskyblue', width: '100%', padding: '8px'} }>
                <h1>我是子组件 Child</h1>
                <h3>我从父组件接收到的名字是：{this.context}</h3>
                <Grandson/>
            </div>
        )
    }
}

function Grandson() {
    return (
        <div style={ {background: 'lightsteelblue', width: '100%', padding: '8px'} }>
            <h1>我是孙组件 Grandson</h1>
            <h3>我从父组件接收到的名字是：
                {/* 函数式组件没有 static 和 this，所以得使用 Consumer 接收 context，
                    Consumer  标签中需要写一个函数，其参数就是接收到的 context */}
                <NameContext.Consumer>
                    {
                        name => <span>{name}</span>
                    }
                </NameContext.Consumer>
            </h3>
        </div>
    )
}

export default Demo;
```

### 4.3 Context Hook

通过 Consumer 使用 Context 实在是不够优雅，所以 React 还为我们提供了一个钩子函数 `useContext()`，我们只需要将 Context 对象作为参数传递给钩子函数，它就会直接给我们返回 Context 对象中存储的数据。

`store/theme-context.js`

```jsx
const NameContext = React.createContext('defaultName');

// ......

function Grandson2() {

    // 使用 useContest Hook 接收 context，更优雅
    const name = React.useContext(NameContext);

    return (
        <div style={ {background: 'lightsteelblue', width: '100%', padding: '8px'} }>
            <h1>我是孙组件 Grandson2</h1>
            <h3>我从父组件接收到的名字是：{name}</h3>
        </div>
    )
}
```

一般我们会将 Context 单独写在一个文件中然后 export，在需要使用的组件中 import 即可。

## 5. 组件优化 memo（PureComponent）

PureComponent 用于优化组件渲染性能，不过新版本已经废弃，使用 memo 代替，memo 见下面。

官网说明与替换方案：https://zh-hans.react.dev/reference/react/PureComponent

## 6. memo

## 7. Render Props

我们自己定义的组件，标签体的内容是不会展示到页面的。

```jsx
<Header>标签体...</Header>
```

回想一下 Router 的知识，在 NavLink 标签中，**标签体的内容其实是放在了标签属性中名为 children 的变量上**，Header 组件中通过 `this.props.children` 即可拿到。

看下面这个例子：

```jsx
function Parent() {
    return (
        <div style={ {background: 'deepskyblue', width: '500px', padding: '8px'} }>
            <h1>我是父组件 Parent</h1>
            <Child>
                {/* 把 Grandson 作为 Child 的标签体 */}
                <Grandson/>
            </Child>
        </div>
    )
}

function Child() {
    return (
        <div style={ {background: 'lightskyblue', width: '100%', padding: '8px'} }>
            <h1>我是子组件 Child</h1>
        </div>
    )
}

function Grandson() {
    return (
        <div style={ {background: 'lightsteelblue', width: '100%', padding: '8px'} }>
            <h1>我是孙组件 Grandson</h1>
        </div>
    )
}
```

Grandson 在 Parent 中 Child 的标签体中，这样是不能把 Grandson 渲染出来的：

![image-20231110185941245](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311101859143.png)

想要渲染出来，得在 Child 中访问到 Grandson：

```jsx
function Child(props) {
    return (
        <div style={ {background: 'lightskyblue', width: '100%', padding: '8px'} }>
            <h1>我是子组件 Child</h1>
            {/* 访问传入的标签体（Grandson），才能把其渲染到页面 */}
            {props.children}	
        </div>
    )
}
```

![image-20231110190230220](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311101902169.png)

这样如果我们只看 Child 和 Grandson，会觉得他们是兄弟组件，但实际上是父子组件。

此时，如果我们想让 Child 组件中的数据传给 Grandson，应该怎么传呢？因为实际开发中确实有这种组件标签包裹组件标签的存在。

其核心思想还是回调，步骤如下：

1. 在 Child 标签中，传递一个函数，该函数中 return Grandson 标签，并且接收参数（即为 Child 想传给 Grandson 的数据）：

    ```jsx
    {/* render 回调函数，从 Child 中接收参数，返回 Grandson 时传递过去 */}
    <Child render={ (data) => <Grandson data={data} /> }/>
    ```

2. 在 Child 中调用 render 回调，传入 Grandson 需要的数据：

    ```jsx
    {/* 调用 render 回调，将数据传递出去 */}
    {props.render(data)}
    ```

3. 在 Grandson 中接收数据：

    ```jsx
    {/* 接收 Child 组件传递过来的数据 */}
    <p>从 Child 组件接收到的数据：{props.data}</p>
    ```

完整代码：

```jsx
function Parent() {
    return (
        <div style={ {background: 'deepskyblue', width: '500px', padding: '8px'} }>
            <h1>我是父组件 Parent</h1>
            {/*<Child>*/}
            {/*    /!* 把 Grandson 作为 Child 的标签体 *!/*/}
            {/*    <Grandson/>*/}
            {/*</Child>*/}

            {/* render 回调函数，从 Child 中接收参数，返回 Grandson 时传递过去 */}
            <Child render={ (data) => <Grandson data={data} /> }/>
        </div>
    )
}

function Child(props) {
    const [data] = React.useState('Child Data');

    return (
        <div style={ {background: 'lightskyblue', width: '100%', padding: '8px'} }>
            <h1>我是子组件 Child</h1>
            {/* 访问传入的标签体（Grandson），才能把其渲染到页面 */}
            {/* 调用 render 回调，将数据传递出去 */}
            {props.render(data)}
        </div>
    )
}

function Grandson(props) {
    return (
        <div style={ {background: 'lightsteelblue', width: '100%', padding: '8px'} }>
            <h1>我是孙组件 Grandson</h1>
            {/* 接收 Child 组件传递过来的数据 */}
            <p>从 Child 组件接收到的数据：{props.data}</p>
        </div>
    )
}
```

![image-20231110191948143](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311101919042.png)

这样写相比直接在 Child 中写 Grandson 有什么好处呢？

- **相当于在 Child 组件中预留了一个位置**，也就是 `{props.render(data)}`，这个位置具体放什么不确定，取决与在 Parent 组件中 render 回调返回的是哪个组件，即 `<Child render={ (data) => <Grandson data={data} /> }/>`。

此时 Parent 中给 Child 传递的 props 就不是一般的 props，而是一个 **render props**。为啥叫 render（规范），因为它 **返回的是一个组件**，这个组件可以随意替换，插到 Child 的指定位置。

其实这就是 Vue 中的 slot 插槽技术，只是 React 没有插槽的概念。

## 8. 组件通信方式总结

**组件间的关系**：

- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）

**几种通信方式**：

1. props：
    - children props
    - render props
2. 消息订阅-发布：pubs-sub、event 等等
3. 集中式管理：redux、dva、hox 等第三方库
4. conText：生产者-消费者模式

**比较好的搭配方式**：

- 父子组件：props
- 兄弟组件：消息订阅-发布、集中式管理
- 祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)



## 9. 错误边界

错误边界(Error boundary)：**用来捕获后代组件错误**，渲染出备用页面。

特点：**只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误**。

由于新版本 React 已经推荐使用函数式组件了，**错误边界目前只能在类式组件中使用**可以使用 [`react-error-boundary`](https://github.com/bvaughn/react-error-boundary) 包来代替。因此可以简单看一下官方文档的使用方法。

在这里需要使用到两个函数：

- `getDerivedStateFromError(error?)`：**子组件** 出现任何错误时，都会先调用此函数。此函数需要 **返回一个状态对象**，用于更新错误状态标识，以便根据此状态响应错误。
- `componentDidCatch(error, errorInfo)`：**子组件** 抛出错误后被调用，一般用来做错误统计，发送给后台服务器。

下面用官网的例子来看看：

```jsx
import React, {Component} from 'react';

function Demo() {
    return (
        <div>
            {/* 自定义子组件渲染错误时的 UI（一般是放置一个 error 组件） */}
            <ErrorBoundary errorFallback={<p>Something went wrong</p>}>
                <Child/>
            </ErrorBoundary>
        </div>
    );
}

class ErrorBoundary extends Component {
    state = {hasError: false};

    // 当前组件的子组件出现任何错误时，都会先调用此函数，而且会传入错误信息。
    // 此函数需要返回一个状态对象，用于更新错误状态标识，以便根据此状态响应错误。
    static getDerivedStateFromError(error) {
        console.log(error);
        // 更新状态，以便下一次渲染将显示后备 UI。
        return {hasError: true};
    }

    componentDidCatch(error, errorInfo) {
        console.log(error, errorInfo);
    }

    render() {
        return (
            <div>
                <h2>我是 ErrorBoundary 组件</h2>
                {
                    this.state.hasError
                        ? this.props.errorFallback  // 可以渲染任何自定义后备 UI
                        : this.props.children   // 正常渲染子组件
                }
            </div>
        )
    }
}

function Child() {
    const arr = [1, 2, 3];
    return (
        <div>
            <h3>我是 Child 组件</h3>
            {
                 arr.map((item, index) => {
                    // 制造错误
                    if (index === 2) {
                        throw new Error('出错了');
                    }
                    return item;
                })
            }
        </div>
    );
}

export default Demo;
```

其实很简单，就是在包裹某个子组件时，在父组件中传入一个兜底的 error 组件，这样父组件就可以决定渲染子组件，还是渲染兜底 error 组件了。

可以看见，当发送错误时，直接不展示 Child 组件，而是我们自定义的错误 UI：

![image-20231110230610039](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311102306110.png)

你 **不需要将每个组件包装到单独的错误边界中**。当你考虑 [错误边界的布置](https://aweary.dev/fault-tolerance-react/) 时，请考虑在哪里显示错误消息才有意义。例如，在消息传递应用程序中，在对话列表周围放置错误边界是有意义的。在每条单独的消息周围放置一个也是有意义的。然而，在每个头像周围设置边界是没有意义的。

> **注意**
>
> 目前还没有办法将错误边界编写为函数式组件。但是你不必自己编写错误边界类。例如，你可以使用 [`react-error-boundary`](https://github.com/bvaughn/react-error-boundary) 包来代替。

## 10. Fragment 标签

我们编写组件的时候每次都需要采用一个 `div` 标签包裹，才能让它正常的编译，但是这样会引发什么问题呢？我们打开控制台看看它的层级

![image-20221027100328758](https://i0.hdslb.com/bfs/album/2971587bd1dbd9d07893218a1c5a86381692b3bf.png)

它 **包裹了几层无意义的 div 标签**，我们可以 **采用 `Fragment` 来解决这个问题**。

React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

首先，我们需要从 react 中暴露出 `Fragment` ，将我们所写的内容采用 `Fragment` 标签进行包裹，**当它解析到 `Fragment` 标签的时候，就会把它去掉**：

```jsx
render() {
  return (
    <React.Fragment key={id}>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

这样我们的内容就直接挂在了 `root` 标签下，而不是一层层的 div。

在 React 中为我们提供了一种更加便捷的方式，直接使用`<></>`代替 Fragment 更加简单：

> 同时采用空标签，也能实现，但是它不能接收任何值，而 `Fragment` 能够接收 1 个值 `key`

```jsx
render() {
  return (
        <>
          <ChildA />
          <ChildB />
          <ChildC />
        </>
  );
}
```







































## 11. Portal

Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。

> 李立超老师的博客
>
> 这篇博客对于Portal的引出我觉得写的很好
>
> [portal – 李立超 | lilichao.com](https://www.lilichao.com/index.php/2022/03/22/portal/)

### 3.1 问题的引出

在React中，父组件引入子组件后，子组件会直接在父组件内部渲染。换句话说，React元素中的子组件，在DOM中，也会是其父组件对应DOM的后代元素。

但是，在有些场景下如果将子组件直接渲染为父组件的后代，在网页显示时会出现一些问题。比如，需要在React中添加一个会盖住其他元素的Backdrop组件，Backdrop显示后，页面中所有的元素都会被遮盖。很显然这里需要用到定位，但是如果将遮罩层直接在当前组件中渲染的话，遮罩层会成为当前组件的后代元素。如果此时，当前元素后边的兄弟元素中有开启定位的情况出现，且层级不低于当前元素时，便会出现盖住遮罩层的情况。

```jsx
const Backdrop = () => {
    
  return <div
           style={
                  {
                    position:'fixed',
                    top:0,
                    bottom:0,
                    left:0,
                    right:0,
                    background:'rgba(0,0,0,.3)',
                    zIndex:9999
                  }
                }
           >
  </div>
};

const Box = props => {
    
  return <div
          style={
              {
                width:100,
                height:100,
                background:props.bgColor
              }
            }
           >
             {props.children}
           </div>
};

const App = () => {
    
  return (
  		<div>
            <Box bgColor='yellowgreen'>
            <Backdrop/>
            </Box>
            <Box bgColor='orange' />
  		</div>;
  )
};
```

上例代码中，App组件中引入了两个Box组件，一个绿色，一个橙色。绿色组件中引入了Backdrop组件，Backdrop组件是一个遮罩层，可以在覆盖住整个网页。

现在三个组件的关系是，绿色Box是橙色Box的兄弟元素，Backdrop是绿色Box的子元素。如果Box组件没有开启定位，遮罩层可以正常显示覆盖整个页面。

![image-20221029232123087](https://i0.hdslb.com/bfs/album/24f29c3777968ed6c976c7144938653880f7b9fc.png)

Backdrop能够盖住页面

但是如果为Box开启定位，并设置层级会出现什么情况呢？

```jsx
const Box = props => {
    
  return <div
           style={
                  {
                    width:100,
                    height:100,
                    background:props.bgColor,
                    position:'relative',
                    zIndex:1
                  }
                }
            >
             {props.children}
           </div>
};
```

现在修改Box组件，开启相对定位，并设置了z-index为1，结果页面变成了这个样子：

![image-20221029232209420](https://i0.hdslb.com/bfs/album/949222a13cb1e7048d3e5982e8631431b3922ab2.png)

和上图对比，显然橙色的box没有被盖住，这是为什么呢？首先我们来看看他们的结构：

```jsx
<App>
    <绿色Box>
            <遮罩/>
    </绿色Box>
    <橙色Box/>
</App>
```

绿色Box和橙色Box都开启了定位，且z-index相同都为1，但是由于橙色在后边，所以实际层级是高于绿色的。由于绿色是遮罩层的父元素，所以即使遮罩的层级是9999也依然盖不住橙色。

问题出在了哪？遮罩层的作用，是用来盖住其他元素的，它本就不该作为Box的子元素出现，作为子元素了，就难免会出现类似问题。所以我们需要在Box中使用遮罩，但是又不能使他成为Box的子元素。怎么办呢？React为我们提供了一个“传送门”可以将元素传送到指定的位置上。

通过ReactDOM中的createPortal()方法，可以在渲染元素时将元素渲染到网页中的指定位置。这个方法就和他的名字一样，给React元素开启了一个传送门，让它可以去到它应该去的地方。

### 3.2 Portal的用法

1. 在index.html中添加一个新的元素
2. 在组件中中通过ReactDOM.createPortal()将元素渲染到新建的元素中

在index.html中添加新元素：

```html
<div id="backdrop"></div>
```

修改Backdrop组件：

```jsx
const backdropDOM = document.getElementById('backdrop');

const Backdrop = () => {
  return ReactDOM.createPortal(
  		<div
           style={
                  {
                    position:'fixed',
                    top:0,
                    bottom:0,
                    left:0,
                    right:0,
                    zIndex:9999,
                    background:'rgba(0,0,0,.3)'
                  }
                }
           >
 		 </div>,
      backdropDOM
  );
};
```

如此一来，我们虽然是在Box中引入了Backdrop，但是由于在Backdrop中开启了“传送门”，Backdrop就会直接渲染到网页中id为backdrop的div中，这样一来上边的问题就解决了

### 3.3 通过 Portal 进行事件冒泡

尽管 portal 可以被放置在 DOM 树中的任何地方，但在任何其他方面，其行为和普通的 React 子节点行为一致。由于 portal 仍存在于 *React 树*， 且与 *DOM 树* 中的位置无关，那么无论其子节点是否是 portal，像 context 这样的功能特性都是不变的。

这包含事件冒泡。一个从 portal 内部触发的事件会一直冒泡至包含 *React 树*的祖先，即便这些元素并不是 *DOM 树* 中的祖先。假设存在如下 HTML 结构：

```html
<html>
  <body>
    <div id="app-root"></div>
    <div id="modal-root"></div>
  </body>
</html>
```

在 `#app-root` 里的 `Parent` 组件能够捕获到未被捕获的从兄弟节点 `#modal-root` 冒泡上来的事件。

```jsx
// 在 DOM 中有两个容器是兄弟级 （siblings）
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
  constructor(props) {
    super(props);
    this.el = document.createElement('div');
  }

  componentDidMount() {
    // 在 Modal 的所有子元素被挂载后，
    // 这个 portal 元素会被嵌入到 DOM 树中，
    // 这意味着子元素将被挂载到一个分离的 DOM 节点中。
    // 如果要求子组件在挂载时可以立刻接入 DOM 树，
    // 例如衡量一个 DOM 节点，
    // 或者在后代节点中使用 ‘autoFocus’，
    // 则需添加 state 到 Modal 中，
    // 仅当 Modal 被插入 DOM 树中才能渲染子元素。
    modalRoot.appendChild(this.el);
  }

  componentWillUnmount() {
    modalRoot.removeChild(this.el);
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el
    );
  }
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {clicks: 0};
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    // 当子元素里的按钮被点击时，
    // 这个将会被触发更新父元素的 state，
    // 即使这个按钮在 DOM 中不是直接关联的后代
    this.setState(state => ({
      clicks: state.clicks + 1
    }));
  }

  render() {
    return (
      <div onClick={this.handleClick}>
        <p>Number of clicks: {this.state.clicks}</p>
        <p>
          Open up the browser DevTools
          to observe that the button
          is not a child of the div
          with the onClick handler.
        </p>
        <Modal>
          <Child />
        </Modal>
      </div>
    );
  }
}

function Child() {
  // 这个按钮的点击事件会冒泡到父元素
  // 因为这里没有定义 'onClick' 属性
  return (
    <div className="modal">
      <button>Click</button>
    </div>
  );
}

const root = ReactDOM.createRoot(appRoot);
root.render(<Parent />);
```

![image-20221029233009114](https://i0.hdslb.com/bfs/album/05f650b10fafb40e1a7a4d1ac26072afa414afa2.png)

点击click后,可以发现数字从0变成1了

![image-20221029233124852](https://i0.hdslb.com/bfs/album/a64a35bc5b691c67fc361986e4770c75fd9bf4df.png)

子组件`Child`的点击事件能冒泡到父组件`Parent `,触发父元素的点击事件

[**在 CodePen 上尝试**](https://codepen.io/gaearon/pen/jGBWpE)

在父组件里捕获一个来自 portal 冒泡上来的事件，使之能够在开发时具有不完全依赖于 portal 的更为灵活的抽象。例如，如果你在渲染一个 `<Modal />` 组件，无论其是否采用 portal 实现，父组件都能够捕获其事件。
