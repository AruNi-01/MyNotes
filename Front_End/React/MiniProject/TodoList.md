# TodoList

学习完了 React 脚手架后，我们趁热打铁写一个 TodoList 小项目。

项目地址：https://github.com/AruNi-01/React-Study/tree/main/2_react-staging/hello-react/02_todolist

## 1. 组件拆分

我们把 TodoList 拆分成四个小组件：

![image-20231106141124124](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311061411213.png)

## 2. 静态页面准备

下面先准备好一个简单的 HTML 静态页面：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>React App</title>

  <link rel="stylesheet" href="index.css">
</head>
<body>
<div id="root">
  <div class="todo-container">
    <div class="todo-wrap">
      <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认"/>
      </div>
      <ul class="todo-main">
        <li>
          <label>
            <input type="checkbox"/>
            <span>xxxxx</span>
          </label>
          <button class="btn btn-danger" style="display:none">删除</button>
        </li>
        <li>
          <label>
            <input type="checkbox"/>
            <span>yyyy</span>
          </label>
          <button class="btn btn-danger" style="display:none">删除</button>
        </li>
      </ul>
      <div class="todo-footer">
        <label>
          <input type="checkbox"/>
        </label>
        <span>
          <span>已完成0</span> / 全部2
        </span>
        <button class="btn btn-danger">清除已完成任务</button>
      </div>
    </div>
  </div>
</div>

</body>
</html>
```

css：

```css
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}

/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
```

## 3. React 项目搭建

使用 cra 创建好脚手架后，先把静态页面 `root` 容器中的内容放到 `App.jsx` 中。

复制 css 到 `App.css`，然后在 `App.jsx` 中引入。需要注意 `jsx` 的语法规范：

- 标签的 class 改为 className；
- style 用 `{{}}` 包裹。

```jsx
import React, {Component} from "react";
import './App.css'

export default class App extends Component {
    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <div className="todo-header">
                        <input type="text" placeholder="请输入你的任务名称，按回车键确认"/>
                    </div>
                    <ul className="todo-main">
                        <li>
                            <label>
                                <input type="checkbox"/>
                                <span>xxxxx</span>
                            </label>
                            <button className="btn btn-danger" style={{display: 'none'}}>删除</button>
                        </li>
                        <li>
                            <label>
                                <input type="checkbox"/>
                                <span>yyyy</span>
                            </label>
                            <button className="btn btn-danger" style={{display: 'none'}}>删除</button>
                        </li>
                    </ul>
                    <div className="todo-footer">
                        <label>
                            <input type="checkbox"/>
                        </label>
                        <span>
                            <span>已完成0</span> / 全部2
                        </span>
                        <button className="btn btn-danger">清除已完成任务</button>
                    </div>
                </div>
            </div>
        )
    }
}
```

执行 `yarn start` 启动项目：

![image-20231106151311523](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311061513456.png)

没问题之后再开始拆分到不同的组件：

```shell
+---src
|   |   App.css
|   |   App.jsx
|   |   index.js
|   |
|   \---components
|       +---Footer
|       |       index.jsx
|       |       index.css
|       |
|       +---Header
|       |       index.jsx
|       |       index.css
|       |
|       +---Item
|       |       index.jsx
|       |       index.css
|       |
|       \---List
|               index.jsx
|               index.css
```

## 4. state 放置位置

这个小项目比较简单，要展示的数据就只有 TodoList 的数据，这些数据是驱动 react 渲染的依据，因此需要放入 state 中。

那 state 应该放在哪个组件中呢？按照常规思维，数据是属于 List 组件的，但是，当在输入栏中输入数据时，需要将数据展示在 List 组件中，但 Header 和 List 是兄弟组件，以我们目前掌握的知识来说，他们是无法传递数据的。

因此，最合适的是将 state 放在 App 组件中，因为它是 List 和 Header 公共的父组件，通过它，就可以实现数据互通：

- Header 通过某种方式将数据传给 App；
- App 通过 props 将数组传给 List。

## 5. 功能实现

### 5.1 动态初始化列表展示

App 组件：

```jsx
import React, {Component} from "react";
import './App.css'

import Header from "./components/Header";
import List from "./components/List";
import Footer from "./components/Footer";

export default class App extends Component {

    // 初始化 state
    state = {
        todos: [
            {id: '001', name: '吃饭', done: true},
            {id: '002', name: '睡觉', done: true},
            {id: '003', name: '敲代码', done: false},
        ]
    }

    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <Header/>
                    <List todos={this.state.todos}/>
                    <Footer/>
                </div>
            </div>
        )
    }
}

```

List 组件：

```jsx
import React, {Component} from 'react';
import Item from "../Item";
import './index.css';

class List extends Component {
    render() {
        const {todos} = this.props;

        return (
            <ul className="todo-main">
                {
                    todos.map( todo => {
                        return <Item key={todo.id} {...todo}/>
                    })
                }
            </ul>
        );
    }
}

export default List;
```

Item 组件：

```jsx
import React, {Component} from 'react';
import './index.css';

class Item extends Component {
    render() {
        const {name, done} = this.props

        return (
            <li>
                <label>
                    {/* defaultChecked 只在初始化时执行一次，checked 没有这个限制，但是必须添加 onChange。类似的还有 defaultValue */}
                    <input type="checkbox" defaultChecked={done}/>
                    <span>{name}</span>
                </label>
                <button className="btn btn-danger" style={{display: 'none'}}>删除</button>
            </li>
        );
    }
}

export default Item;
```

此时就能通过 state 中的 todos 展示列表了。

### 5.2 新增 Todo 项

**一、首先在 Header 组件中编写输入内容的函数，当输入 `Enter` 键时，获取用户输入的内容**：

```jsx
import React, {Component} from 'react';
import './index.css';

class Header extends Component {
    // 按下 Enter 键时，获取输入的内容
    handleKeyUp = (event) => {
        const {key, target} = event;
        if (key === 'Enter') console.log(target.value);
    }

    render() {
        return (
            <div className="todo-header">
                {/* onKeyUp： 用户释放键盘上的任意按键时触发相应的事件处理函数 */}
                <input onKeyUp={this.handleKeyUp} type="text" placeholder="输入任务名，按回车键确认"/>
            </div>
        );
    }
}

export default Header;
```

**二、接着，根据用户输入的数据，需要影响到 state，而 state 在 App 组件中，其是 Header 的父组件，所以我们要把数据传递给 App 组件，然后 App 组件中再修改 state 即可**。

我们这里使用回调来实现，在 App 中通过 props 传一个函数给 Header，在 Header 中调用该函数，在此函数中传递参数（数据）过去，此时就能更新到 state 了。

App 组件：

```jsx
import React, {Component} from "react";
import './App.css'

import Header from "./components/Header";
import List from "./components/List";
import Footer from "./components/Footer";

export default class App extends Component {

    // 初始化 state
    state = {
        todos: [
            {id: '001', name: '吃饭', done: true},
            {id: '002', name: '睡觉', done: true},
            {id: '003', name: '敲代码', done: false},
        ]
    }

    // 添加一个 todo 对象
    addTodo = (todoObj) => {
        const {todos} = this.state;
        // 添加新的 todo 到上方，然后更新 state
        this.setState({todos: [todoObj, ...todos]})
    }

    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <Header addTodo={this.addTodo}/>
                    <List todos={this.state.todos}/>
                    <Footer/>
                </div>
            </div>
        )
    }
}
```

Header 组件：

```jsx
import React, {Component} from 'react';
import './index.css';

class Header extends Component {
    // 按下 Enter 键时，获取输入的内容。再通过回调传递给 App 组件
    handleKeyUp = (event) => {
        const {key, target} = event;

        if (key !== 'Enter') return;

        if (target.value.trim() === '') {
            alert('输入内容不能为空');
            return;
        }

        const todoObj = {id: new Date().getTime(), name: target.value, done: false};
        this.props.addTodo(todoObj);

        // 添加完 todo 后清空输入框
        target.value = '';
    }

    render() {
        return (
            <div className="todo-header">
                {/* onKeyUp： 用户释放键盘上的任意按键时触发相应的事件处理函数 */}
                <input onKeyUp={this.handleKeyUp} type="text" placeholder="输入任务名，按回车键确认"/>
            </div>
        );
    }
}

export default Header;
```

此时就完成了新增 Todo 项的功能。

### 5.3 鼠标悬停 Item 高亮

这里不要用 css 的 hover 来实现，因为还涉及鼠标悬停显示删除按钮。

我们可以使用一个 state 来保存鼠标当前是否在某个 Item 上，若在则高亮并显示删除按钮。

何如判断鼠标悬停的位置呢，可以在标签上使用 `OnMouseEnter` 和 `OnMouseLeave` 事件来判断鼠标是否移入/移除该标签，它们的事件函数用来更新 state 即可：

- 当鼠标移入时：将 state 更新为 true；
- 当鼠标移出时，将 state 更新为 false；

Item 组件：
```jsx
import React, {Component} from 'react';
import './index.css';

class Item extends Component {

    state = {mouse: false}

    handleMouse = (flag) => {
        // 注意：标签上的事件必须接收一个函数，由于要传参，所以会触发函数的执行，因此使用函数柯里化
        return () => {
            this.setState({mouse: flag})
        }
    }

    render() {
        const {name, done} = this.props
        const {mouse} = this.state

        return (
            <li style={{backgroundColor: mouse ? '#ddd' : 'white'}} onMouseEnter={this.handleMouse(true)} onMouseLeave={this.handleMouse(false)}>
                <label>
                    {/* defaultChecked 只在初始化时执行一次，checked 没有这个限制，但是必须添加 onChange。类似的还有 defaultValue */}
                    <input type="checkbox" defaultChecked={done}/>
                    <span>{name}</span>
                </label>
                <button className="btn btn-danger" style={{display: mouse ? 'block' : 'none'}}>删除</button>
            </li>
        );
    }
}

export default Item;
```

效果：

![image-20231106172830003](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202311061728953.png)

### 5.4 打勾已做功能

此功能聚焦到 Item 组件的 input 标签，需要在 checkbox 改变时，获取到它的状态，所以使用 onChange 事件，对应的回调函数做处理，需要把 id 传过去，以便知道是在勾选哪个 item。

item（todoObj）的 done 属性在父组件 App 中，所以使用之前类似的回调方式，App 传递一个函数给 List，List 再传给 Item，以此来间接传递 done 属性给 App，在 App 中更新 state。

App 组件：

```jsx
    // 更新一个 todo 对象
    updateTodo = (id, done) => {
        const {todos} = this.state;
        const newTodos = todos.map((todo) => {
            if (todo.id === id) return {...todo, done};
            else return todo;
        })
        this.setState({todos: newTodos});
    }

    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <Header addTodo={this.addTodo}/>
                    <List todos={this.state.todos} updateTodo={this.updateTodo}/>
                    <Footer/>
                </div>
            </div>
        )
    }
```

List 组件：

```jsx
class List extends Component {
    render() {
        const {todos, updateTodo} = this.props;

        return (
            <ul className="todo-main">
                {
                    todos.map( todo => {
                        return <Item key={todo.id} {...todo} updateTodo={updateTodo}/>
                    })
                }
            </ul>
        );
    }
}
```

Item 组件：

```jsx
    // checkbox 勾选/取消的回调
    handleCheck = (id) => {
        return (event) => {
            this.props.updateTodo(id, event.target.checked)
        }
    }

    render() {
        const {id, name, done} = this.props
        const {mouse} = this.state

        return (
            <li style={{backgroundColor: mouse ? '#ddd' : 'white'}} onMouseEnter={this.handleMouse(true)} onMouseLeave={this.handleMouse(false)}>
                <label>
                    {/* defaultChecked 只在初始化时执行一次，checked 没有这个限制，但是必须添加 onChange。类似的还有 defaultValue */}
                    <input type="checkbox" defaultChecked={done} onChange={this.handleCheck(id)}/>
                    <span>{name}</span>
                </label>
                <button className="btn btn-danger" style={{display: mouse ? 'block' : 'none'}}>删除</button>
            </li>
        );
    }
```

### 5.5 对 props 进行限制

App 组件传递了很多 props 给其他子组件，因此，有必要在子组件中对 props 进行一些限制。

首先要引入 props-types 库：`yarn add props-types`。

在 Header 中限制：

```jsx
import React, {Component} from 'react';
import PropTypes from 'prop-types';
import './index.css';

class Header extends Component {

    static propTypes = {
        addTodo: PropTypes.func.isRequired
    }
    
    // ......
}
```

List 中：

```jsx
import React, {Component} from 'react';
import PropTypes from 'prop-types';
import Item from "../Item";
import './index.css';

class List extends Component {

    static propTypes = {
        todos: PropTypes.array.isRequired,
        updateTodo: PropTypes.func.isRequired
    }
    
    // ......
}
```

### 5.6 删除 Todo

与更新类似，这里就不详细说了，具体看源码即可。

### 5.7 底部进度全选功能

**一、进度信息**

底部功能很简单，只需要 App 组件把 state.todos 全都传给 Footer 组件，通过简单的计算即可展示出进度信息。

Footer 组件：

```jsx
    render() {
        const {todos} = this.props;

        // 使用 array.reduce 统计：reduce 第一个参数为一个函数，其中 pre 为前一次调用时的值（初始为 0，
        // reduce 的第二个参数指定），todo 为 array 中的 item。
        const doneCount = todos.reduce((pre, todo) => {
            return pre + (todo.done ? 1 : 0);
        }, 0)
        const total = todos.length;

        return (
            <div className="todo-footer">
                <label>
                    <input type="checkbox"/>
                </label>
                <span>
                    进度：{doneCount}/{total}
                </span>
                <button className="btn btn-danger">清除已完成任务</button>
            </div>
        );
    }
```

**二、全选**

全选和之前的 Item 单选类似，也需要一个 onChange 事件，当点击全选框时，要影响到 App 中 state 的 todos 所有 todo 的 done 属性，因此处理方式也一样。此外，还要加一个 onChecked 事件来动态的判断全选框是否要勾选（当所有的单个 Item 都勾选上时）。

App 组件：

- 用一个 done 参数来标识是全选还是全不选；

```jsx
    // 全选/取消全选
    checkAllTodo = (done) => {
        const {todos} = this.state;
        const newTodos = todos.map((todo) => {
            return {...todo, done};
        })
        this.setState({todos: newTodos})
    }

    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <Header addTodo={this.addTodo}/>
                    <List todos={this.state.todos} updateTodo={this.updateTodo} deleteTodo={this.deleteTodo}/>
                    <Footer todos={this.state.todos} checkAllTodo={this.checkAllTodo}/>
                </div>
            </div>
        )
    }
```

Footer 组件：

- 需要注意，使用 `checked` 必须要有 `onChange`，不然就写死了，因为当 `doneCount === total` 时，该 checkbox 就不能自己取消勾选了；
- 还有一个 bug 需要修改：Item 组件中要把 checkbox 的 `defaultChecked` 改为 `checked`，否则 Footer 中全选时，Item 的 checkbox 只会使用默认是否勾选，不会随之改变。

```jsx
    // 处理全选/取消全选
    handleCheckAll = (event) => {
        this.props.checkAllTodo(event.target.checked);
    }

    render() {
        const {todos} = this.props;

        // 使用 array.reduce 统计：reduce 第一个参数为一个函数，其中 pre 为前一次调用时的值（初始为 0，
        // reduce 的第二个参数指定），todo 为 array 中的 item。
        const doneCount = todos.reduce((pre, todo) => {
            return pre + (todo.done ? 1 : 0);
        }, 0)
        const total = todos.length;

        return (
            <div className="todo-footer">
                <label>
                    <input type="checkbox" checked={doneCount === total && total !== 0} onChange={this.handleCheckAll}/>
                </label>
                <span>
                    进度：{doneCount}/{total}
                </span>
                <button className="btn btn-danger">清除已完成任务</button>
            </div>
        );
    }
```

**三、清除已完成任务**

写法与之前类似，给该按钮添加个 onClick 事件即可。

App：

```jsx
    // 清除所有已经完成的任务
    clearAllDone = () => {
        const {todos} = this.state;
        const newTodos = todos.filter((todo) => {
            return todo.done === false;
        })
        this.setState({todos: newTodos})
    }

    render() {
        return (
            <div className="todo-container">
                <div className="todo-wrap">
                    <Header addTodo={this.addTodo}/>
                    <List todos={this.state.todos} updateTodo={this.updateTodo} deleteTodo={this.deleteTodo}/>
                    <Footer todos={this.state.todos} checkAllTodo={this.checkAllTodo} clearAllDone={this.clearAllDone}/>
                </div>
            </div>
        )
    }
```

Footer：

```jsx
    // 清除所有已经完成的 todo
    handleClearAllDone = () => {
        this.props.clearAllDone();
    }

    render() {
        const {todos} = this.props;

        // 使用 array.reduce 统计：reduce 第一个参数为一个函数，其中 pre 为前一次调用时的值（初始为 0，
        // reduce 的第二个参数指定），todo 为 array 中的 item。
        const doneCount = todos.reduce((pre, todo) => {
            return pre + (todo.done ? 1 : 0);
        }, 0)
        const total = todos.length;

        return (
            <div className="todo-footer">
                <label>
                    <input type="checkbox" checked={doneCount === total && total !== 0} onChange={this.handleCheckAll}/>
                </label>
                <span>
                    进度：{doneCount}/{total}
                </span>
                <button onClick={this.handleClearAllDone} className="btn btn-danger">清除已完成任务</button>
            </div>
        );
    }
```

至此，TodoList 案例就完成了。



## 6. 总结

通过该小案例，有以下几点需要注意：

1. 拆分组件、实现静态组件。注意 `className`、`style` 的写法。
2. 动态初始化列表，如何确定将数据放在哪个组件的 `state` 中？

    - 某个组件使用：放在自身的 `state` 中；

    - 某些组件使用：放在他们共同的父组件中（状态提升）。
3. 关于父子组件之间的通信：

    - 父组件给子组件传递数据：通过 `props` 传递；

    - 子组件给父组件传递数据：通过 `props` 传递，要求父组件提前给子组件传递一个函数。
4. 注意 `defaultChecked` 和 `checked` 区别：
    - `defalutChecked` 只是在初始化的时候执行一次；
    - `checked` 没有这个限制，但是必须添加 `onChange` 方法；
    - 类似的还有：`defaultValue` 和 `value`。
5. 状态在哪里，操作状态的方法就在哪里（对 `state.todos` 的所有操作都在 App 组件中）。




