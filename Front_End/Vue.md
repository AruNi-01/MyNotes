# Vue

## 1. 概述

- Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的**渐进式JavaScript框架**，发布于2014年2月。
- 与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。
- **Vue 的核心库只关注视图层**，方便与第三方库（如：`vue-router:跳转`，`vue-resource:通信`，`vuex:管理`)或既有项目整合。
- 官网：[https://cn.vuejs.org/v2/guide/](https://gitee.com/link?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2F)

- https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FTencent%2Fweui)





### 1.1 了解前后分离的演变史

为什么需要前后分离

#### 1.1.1 后端为主的MVC时代

为了降低开发的复杂度， 以后端为出发点， 比如：Struts、Spring MVC等框架的使用， 就是后端的MVC时代； 以`SpringMVC`流程为例：

![1595237159379](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e8d0c886d1aaded28a10884331c28e4f.png)

- 发起请求到前端控制器(`Dispatcher Servlet`)
- 前端控制器请求`HandlerMapping`查找`Handler`，可以根据`xml`配置、注解进行查找
- 处理器映射器`HandlerMapping`向前端控制器返回`Handler`
- 前端控制器调用处理器适配器去执行`Handler`
- 处理器适配器去执行`Handler`
- `Handler`执行完成给适配器返回`ModelAndView`
- 处理器适配器向前端控制器返回`ModelAndView`，`ModelAndView`是`SpringMvc`框架的一个底层对象，包括`Model`和`View`
- 前端控制器请求视图解析器去进行视图解析，根据逻辑视图名解析成真正的视图(`JSP`)
- 视图解析器向前端控制器返回`View`
- 前端控制器进行视图渲染，视图渲染将模型数据(在`ModelAndView`对象中)填充到`request`域
- 前端控制器向用户响应结果

**优点:**

- MVC是一个非常好的协作模式， 能够有效降低代码的耦合度从架构上能够让开发者明白代码应该写在哪里。为了让View更纯粹， 还可以使用Thyme leaf、Frree marker等模板引擎， 使模板里无法写入Java代码， 让前后端分工更加清晰。

**缺点:**

- 前端开发重度依赖开发环境，开发效率低，这种架构下，前后端协作有两种模式：
  - 第一种是前端写DEMO， 写好后， 让后端去套模板。好处是DEMO可以本地开发， 很高效。不足是还需要后端套模板，有可能套错，套完后还需要前端确定，来回沟通调整的成本比较大；
  - 另一种协作模式是前端负责浏览器端的所有开发和服务器端的View层模板开发。好处是UI相关的代码都是前端去写就好，后端不用太关注，不足就是前端开发重度绑定后端环境，环境成为影响前端开发效率的重要因素。
- 前后端职责纠缠不清：模板引擎功能强大，依旧可以通过拿到的上下文变量来实现各种业务逻辑。这样，只要前端弱势一点，往往就会被后端要求在模板层写出不少业务代码，还有一个很大的灰色地带是`Controller`， 页面路由等功能本应该是前端最关注的， 但却是由后端来实现。`Controller`本身与`Model`往往也会纠缠不清，看了让人咬牙的业务代码经常会出现在`Controller`层。这些问题不能全归结于程序员的素养， 否则JSP就够了。
- 对前端发挥的局限性：性能优化如果只在前端做空间非常有限，于是我们经常需要后端合作，但由于后端框架限制，我们很难使用**【Comet】**、**【Big Pipe】**等技术方案来优化性能。

**注：在这期间(2005年以前) ， 包括早期的JSP、PHP可以称之为Web 1.0时代。在这里想说一句，  如果你是一名Java初学者， 请你不要再把一些陈旧的技术当回事了， 比如JSP，  因为时代在变、技术在变、什么都在变(引用扎克伯格的一句话：唯一不变的是变化本身)；当我们去给大学做实训时，有些同学会认为我们没有讲什么干货，其实不然，只能说是你认知里的干货对于市场来说早就过时了而已。**

#### 1.1.2 基于AJAX带来的SPA时代

时间回到2005年`AJAX`(Asynchronous JavaScript And XML， 异步JavaScript和XML，老技术新用法)被正式提出并开始使用`CDN`作为静态资源存储， 于是出现了JavaScript王者归来(在这之前JS都是用来在网页上贴狗皮膏药广告的) 的SPA(Single Page Application) 单页面应用时代。

![1595237421062](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/59f00cdad856524d082cd48673830cd8.png)

**优点:** 这种模式下， **前后端的分工非常清晰， 前后端的关键协作点是AJAX接口。**看起来是如此美妙， 但回过头来看看的话， 这与JSP时代区别不大。复杂度从服务端的JSP里移到了浏览器的JavaScript，浏览器端变得很复杂。类似Spring MVC， **这个时代开始出现浏览器端的分层架构**：

![1595237457081](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/652cca11591d988ecdfa25b800c79539.png)

**缺点:**

- **前后端接口的约定**：如果后端的接口一塌糊涂，如果后端的业务模型不够稳定，那么前端开发会很痛苦；不少团队也有类似尝试，通过接口规则、接口平台等方式来做。**有了和后端一起沉淀的接口规则，还可以用来模拟数据，使得前后端可以在约定接口后实现高效并行开发。**
- 前端开发的复杂度控制：SPA应用大多以功能交互型为主，JavaScript代码过十万行很正常。大量JS代码的组织，与View层的绑定等，都不是容易的事情。

#### 1.1.3 前端为主的MV*时代

此处的MVC模式如下：

- MVC(同步通信为主) ：Model、View、Controller
- MVP(异步通信为主) ：Model、View、Presenter
- MVVM(异步通信为主)：Model、View、View Model为了降低前端开发复杂度，涌现了大量的前端框架，比如：`Angular JS`、`React`、`Vue.js`、`Ember JS`等， 这些框架总的原则是先按类型分层， 比如Templates、Controllers、Models， 然后再在层内做切分，如下图：

![1595237632332](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d913ed74d9ea1ccdee5d2bcef8f110d7.png)

**优点:**

- **前后端职责很清晰**：前端工作在浏览器端，后端工作在服务端。清晰的分工，可以让开发并行，测试数据的模拟不难， 前端可以本地开发。后端则可以专注于业务逻辑的处理， 输出RESTful等接口。
- **前端开发的复杂度可控**：前端代码很重，但合理的分层，让前端代码能各司其职。这一块蛮有意思的，简单如模板特性的选择，就有很多很多讲究。并非越强大越好，限制什么，留下哪些自由，代码应该如何组织，所有这一切设计，得花一本书的厚度去说明。
- **部署相对独立**：可以快速改进产品体验

**缺点:**

- 代码不能复用。比如后端依旧需要对数据做各种校验，校验逻辑无法复用浏览器端的代码。如果可以复用，那么后端的数据校验可以相对简单化。
- 全异步， 对SEO不利。往往还需要服务端做同步渲染的降级方案。
- 性能并非最佳，特别是移动互联网环境下。
- SPA不能满足所有需求， 依旧存在大量多页面应用。URL Design需要后端配合， 前端无法完全掌控。

#### 1.1.4 Node JS带来的全栈时代

前端为主的MV*模式解决了很多很多问题， 但如上所述， 依旧存在不少不足之处。随着Node JS的兴起， JavaScript开始有能力运行在服务端。这意味着可以有一种新的研发模式：

![1595237863311](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7d1b0a2b8a1ef0e7657d959aa71323fd.png)

在这种研发模式下，前后端的职责很清晰。对前端来说，两个UI层各司其职：

- Front-end Ul layer处理浏览器层的展现逻辑。通过CSS渲染样式， 通过JavaScript添加交互功能， HTML的生成也可以放在这层， 具体看应用场景。
- Back-end Ul layer处理路由、模板、数据获取、Cookie等。通过路由， 前端终于可以自主把控URL Design， 这样无论是单页面应用还是多页面应用， 前端都可以自由调控。后端也终于可以摆脱对展现的强关注，转而可以专心于业务逻辑层的开发。

通过Node， WebServer层也是JavaScript代码， 这意味着部分代码可前后复用， 需要SEO的场景可以在服务端同步渲染，由于异步请求太多导致的性能问题也可以通过服务端来缓解。前一种模式的不足，通过这种模式几乎都能完美解决掉。

与JSP模式相比， 全栈模式看起来是一种回归， 也的确是一种向原始开发模式的回归， 不过是一种螺旋上升式的回归。

基于Node JS的全栈模式， 依旧面临很多挑战：

- 需要前端对服务端编程有更进一步的认识。比如TCP/IP等网络知识的掌握。
- Node JS层与Java层的高效通信。Node JS模式下， 都在服务器端， RESTful HTTP通信未必高效， 通过SOAP等方式通信更高效。一切需要在验证中前行。
- 对部著、运维层面的熟练了解，需要更多知识点和实操经验。
- 大量历史遗留问题如何过渡。这可能是最大最大的阻力。

### 1.2 总结

综上所述，模式也好，技术也罢，没有好坏优劣之分，只有适合不适合；前后分离的开发思想主要是基于`Soc`(关注度分离原则)，上面种种模式，都是让前后端的职责更清晰，分工更合理高效。





## 2. 第一个Vue程序

### 2.1 MVVM

#### 2.1.1 什么是MVVM

MVVM（Model-View-ViewModel）是一种软件设计模式，由微软WPF（用于替代WinForm，以前就是用这个技术开发桌面应用程序的）和Silverlight（类似于Java Applet，简单点说就是在浏览器上运行WPF）的架构师Ken Cooper和Ted Peters开发，是一种简化用户界面的**事件驱动编程方式**。由John Gossman（同样也是WPF和Sliverlight的架构师）与2005年在他的博客上发表。

MVVM源自于经典的MVC（Model-View-Controller）模式。MVVM的核心是ViewModel层，负责转换Model中的数据对象来让数据变得更容易管理和使用。其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与Model层通过接口请求进行数据交互

MVVM已经相当成熟了，主要运用但不仅仅在网络应用程序开发中。当下流行的MVVM框架有`Vue.js`，`Anfular JS`

#### 2.1.2 为什么要使用MVVM

MVVM模式和MVC模式一样，主要目的是分离视图（View）和模型（Model），有几大好处

- **低耦合**：视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的View上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
- **可复用**：你可以把一些视图逻辑放在一个ViewModel里面，让很多View重用这段视图逻辑。
- **独立开发**：开发人员可以专注于业务逻辑和数据的开发（ViewMode），设计人员可以专注于页面设计。
- **可测试**：界面素来是比较难以测试的，而现在测试可以针对ViewModel来写。

![1595238643753](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/be22c0f3767713beeaee4bfb96f622ba.png)

**View:**

  View是视图层， 也就是用户界面。前端主要由HTH L和csS来构建， 为了更方便地展现vi eu to  del或者Hodel层的数据， 已经产生了各种各样的前后端模板语言， 比如FreeMarker，Thyme leaf等等， 各大MV  VM框架如Vue.js.Angular JS， EJS等也都有自己用来构建用户界面的内置模板语言。

**Model:**

  Model是指数据模型， 泛指后端进行的各种业务逻辑处理和数据操控， 主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的接口规则

**ViewModel:**

  ViewModel是由前端开发人员组织生成和维护的视图数据层。在这一层， 前端开发者对从后端获取的Model数据进行转换处理， 做二次封装， 以生成符合View层使用预期的视图数据模型。  需要注意的是View Model所封装出来的数据模型包括视图的状态和行为两部分， 而Model层的数据模型是只包含状态的

- 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态(展示)
- 页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为(交互)

  视图状态和行为都封装在了View Model里。这样的封装使得View Model可以完整地去描述View层。由于实现了双向绑定，  View Model的内容会实时展现在View层， 这是激动人心的， 因为前端开发者再也不必低效又麻烦地通过操纵DOM去更新视图。  MVVM框架已经把最脏最累的一块做好了， 我们开发者只需要处理和维护View Model， 更新数据视图就会自动得到相应更新，真正实现`事件驱动编程`。  View层展现的不是`Model`层的数据， 而是`ViewModel`的数据， 由`ViewModel`负责与`Model`层交互， 这就**完全解耦了View层和Model层， 这个解耦是至关重要的， 它是前后端分离方案实施的重要一环。**

#### 2.2.3 MVVM模式的实现者

- Model：模型层， 在这里表示JavaScript对象
- View：视图层， 在这里表示DOM(HTML操作的元素)
- ViewModel：连接视图和数据的中间件， `Vue.js`就是MVVM中的View Model层的实现者

在MVVM架构中， 是**不允许数据和视图直接通信**的， 只能通过ViewModel来通信， 而View Model就是定义了一个Observer观察者

- ViewModel能够观察到数据的变化， 并对视图对应的内容进行更新
- ViewModel能够监听到视图的变化， 并能够通知数据发生改变

至此， 我们就明白了， `Vue.js`就是一个MV VM的实现者， 他的核心就是实现了DOM监听与数据绑定

### 2.3 第一个Vue程序

【说明】IDEA可以安装Vue的插件!  注意：Vue不支持IE 8及以下版本， 因为Vue使用了IE 8无法模拟的ECMAScript 5特性。但它支持所有兼容ECMAScript 5的浏览器。

#### 2.3.1 下载地址

- 开发版本
  - 包含完整的警告和调试模式：https://yuejs.org/js/vue.js
  - 删除了警告， 30.96KBmin+gzip：https://vuejs.org/js/vue.min.js
- CDN
  - `<script src=“https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js”></script>`
  - `<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>`

#### 2.3.2 代码编写

`Vue.js`的核心是实现了MVVM模式， 她扮演的角色就是View Model层， 那么所谓的第一个应用程序就是展示她的数据绑定功能，操作流程如下：

**1、创建一个HTML文件**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

</body>
</html>
```

**2、引入Vue.js**

```javascript
 <!--1.导入Vue.js-->
 <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
```

**3、创建一个Vue实例**

```vue
 <script type="text/javascript">
     var vm = new Vue({
         el:"#app",
         /*Model：数据*/
         data:{
             message:"hello,vue!"
         }
     });
 </script>
```

说明：

 - `el: '#vue'`：绑定元素的ID
 - `data:{message:'Hello Vue!'}`：数据对象中有一个名为message的属性，并设置了初始值 Hello Vue！

**4、将数据绑定到页面元素**

```vue
<!--view层，模板-->
 <div id="app">
     {{message}}
 </div>
```

说明：只需要在绑定的元素中使用双花括号将Vue创建的名为message属性包裹起来， 即可实现数据绑定功能， 也就实现了`View Model`层所需的效果， 是不是和EL表达式非常像?

### 2.3 完整的HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>

<!--view层，模板-->
<div id="app">
    {{message}}
</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: "#app",
        /*Model：数据*/
        data: {
            message: "hello,vue!"
        }
    });
</script>
</body>
</html>
```

### 2.4 测试

为了能够更直观的体验Vue带来的数据绑定功能， 我们需要在浏览器测试一番， 操作流程如下：

1. 在浏览器上运行第一个Vue应用程序， 进入开发者工具
2. 在控制台输入`vm.message=‘HelloWorld’`， 然后回车， 你会发现浏览器中显示的内容会直接变成HelloWorld
3. 此时就可以在控制台直接输入`vm.message`来修改值， 中间是可以**省略data**的， 在这个操作中，并**没有主动操作DOM**， 就让页面的内容发生了变化，这就是借助了Vue的数据绑定功能实现的；MV VM模式中要求View  Model层就是使用观察者模式来实现数据的监听与绑定，以做到数据与视图的快速响应。





## 3. Vue基础语法

### 3.1 v-bind指令

- 我们已经成功创建了第一个Vue应用！看起来这跟渲染一个字符串模板非常类似，但是Vue在背后做了大量工作。现在数据和DOM已经被建立了关联，所有东西都是响应式的。我们在控制台操作对象属性，界面可以实时更新!

- 我们还可以使用`v-bind`来绑定元素特性

  ```html
  <!DOCTYPE html>
  <html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  
  </head>
  <body>
  
  <!--view层，模板-->
  <div id="app">
      <span v-bind:title="message">
      鼠标悬停几秒钟查看此处动态绑定的提示信息！
    </span>
  </div>
  
  <!--1.导入Vue.js-->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
  <script type="text/javascript">
      var vm = new Vue({
          el:"#app",
          /*Model：数据*/
          data:{
              message: '页面加载于 ' + new Date().toLocaleString()
          }
      });
  </script>
  </body>
  </html>
  ```





- 你看到的v-bind等被称为指令。指令带有前缀v以表示它们是Vue提供的特殊特性。可能你已经猜到了， 它们会在渲染的DOM上应用特殊的响应式行为在这里，该指令的意思是：“将这个元素节点的title特性和Vue实例的message属性保持一致”。

  ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1bfafbc187e51d0ee9ed734095126b0b.png)

- 如果你再次打开浏览器的JavaScript控制台， 输入`app.message=‘新消息’`，就会再一次看到这个绑定了title特性的HTML已经进行了更新。

### 3.2 v-if，v-else

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--view层，模板-->
<div id="app">
    <h1 v-if="ok">Yes</h1>
    <h1 v-else>No</h1>

</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        /*Model：数据*/
        data:{
            ok: true
        }
    });
</script>
</body>
</html>
```

测试： 

1. 在浏览器上运行，显示Yes；打开控制台 
2. 在控制台输入`vm.ok=false`然后回车，你会发现浏览器中显示的内容会直接变成NO。

**注意:**使用`v-*`形式绑定数据是**不需要双花括号**包裹的

### 3.3 v-else-if

- v-if
- v-else-if
- v-else
- 注：`===`三个等号在JS中表示绝对等于(就是数据与类型都要相等)

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--view层，模板-->
<div id="app">
    <h1 v-if="type==='A'">A</h1>
    <h1 v-else-if="type==='B'">B</h1>
    <h1 v-else-if="type==='D'">D</h1>
    <h1 v-else>C</h1>

</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        /*Model：数据*/
        data:{
            type: 'A'
        }
    });
</script>
</body>
</html>
```

在控制台通过`vm.type='X'`同样可以改变浏览器对应的显示

### 3.4 v-for

**注意:**`v-for="(item, index) in items`的()中参数的遍历顺序是**倒序**，靠近`in`的先遍历出来

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--view层，模板-->
<!--注：`items`是数组，`item`是数组元素迭代的别名，`index`是下标位置-->
<div id="app">
    <ul>
        <li v-for="(item,index) in items">
            {{item.message}}---{{index}}
        </li>
    </ul>
</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        /*Model：数据*/
        data:{
            items:[
                {message:'狂神说Java'},
                {message:'狂神说前端'}
            ]
        }
    });
</script>
</body>
</html>
```

测试：

![image-20220327124606829](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c26d4b63a08120b5c465fd31838d9a5e.png)

- 在控制台输入`vm.items.push({message：'狂神说运维'})`，尝试追加一条数据，你会发现浏览器中显示的内容会增加一条`狂神说运维---2`



### 3.5 v-on

- `v-on`监听事件

- 事件有Vue的事件、和前端页面本身的一些事件!我们这里的`click`是vue的事件， 可以绑定到Vue中的`methods`中的方法事件

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
  <div id="app">
      <button v-on:click="sayHi">clickMe</button>
  </div>
  
  
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
  <script>
      let vm = new Vue({
          el: "#app",
          data: {
              message: "你好"
          },
          methods:{
              sayHi: function () {
                  alert(this.message);
              }
          }
      });
  </script>
  </body>
  </html>
  ```





## 4. 表单双绑

**什么是双向数据绑定:**

`Vue.js`是一个MV VM框架， 即数据双向绑定， 即**当数据发生变化的时候， 视图也就发生变化**， **当视图发生变化的时候，数据也会跟着同步变化**。这也算是`Vue.js`的精髓之处了。  

值得注意的是，我们所说的数据双向绑定，一定是对于UI控件来说的非UI控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提。如果我们使用vue x那么数据流也是单项的，这时就会和双向数据绑定有冲突。



**为什么要实现数据的双向绑定:**

  在`Vue.js`中，如果使用`vuex`， 实际上数据还是单向的， 之所以说是数据双向绑定，这是用的UI控件来说， 对于我们处理表单， `Vue.js`的双向数据绑定用起来就特别舒服了。即两者并不互斥，在全局性数据流使用单项，方便跟踪；局部性数据流使用双向，简单易操作。





### 4.1 在表单中使用双向数据绑定

  你可以用`v-model`指令在表单`<input>`、`<textarea>`及`<select>`元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇， 但`v-model`本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。  

注意：`v-model`会忽略所有表单元素的`value`、`checked`、`selected`特性的初始值而总是将`Vue`实例的数据作为数据来源。你应该通过`JavaScript`在组件的`data`选项中声明初始值!

#### 4.1.1 单行文本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    输入的文本：<input type="text" v-model="message" value="hello">{{message}}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: "#app",
        data:{
            message: ""
        }
    });
</script>
</body>
</html>
```

#### 4.1.2 多行文本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
   多行文本：<textarea v-model="message"></textarea>&nbsp;&nbsp;多行文本是：{{message}}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: "#app",
        data:{
            message: "Hello hello!"
        }
    });
</script>
</body>
</html>
```

#### 4.1.3 单复选框

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    单复选框：
    <input type="checkbox" id="checkbox" v-model="checked">
    &nbsp;&nbsp;
    <label for="checkbox">{{checked}}</label>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            checked:false
        }
    });
</script>
</body>
</html>
```

#### 4.1.4 多复选框

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    多复选框：
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    &nbsp;&nbsp;
    <label for="jack">Jack</label>
    <input type="checkbox" id="join" value="Join" v-model="checkedNames">
    &nbsp;&nbsp;
    <label for="join">Join</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    &nbsp;&nbsp;
    <label for="mike">Mike</label>  <br/>
    <span>选中的值：{{checkedNames}}</span>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: "#app",
        data:{
            checkedNames: []
        }
    });
</script>
</body>
</html>
```

#### 4.1.5 单选按钮

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    单选框按钮
    <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <span>选中的值：{{picked}}</span>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            picked:'Two'
        }
    });
</script>
</body>
</html>
```

#### 4.1.6 下拉框

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    下拉框:
    <select v-model="pan">
        <option value="" disabled>---请选择---</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
        <option>D</option>
    </select>
    <span>value:{{pan}}</span>



</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            pan: ""
        }
    });
</script>
</body>
</html>
```

  注意：`v-model`表达式的初始值未能匹配任何选项，元系将被渲染为“未选中”状态。 在iOS中， 这会使用户无法选择第一个选项，因为这样的情况下，iOS不会触发`change`事件。因此，更推荐像上面这样提供一个值为空的禁用选项。

## 5. 组件

- 组件是可复用的`Vue`实例， 说白了就是一组可以重复使用的模板， 跟`JSTL`的自定义标签、`Thymeleal`的`th:fragment`等框架有着异曲同工之妙，通常一个应用会以一棵嵌套的组件树的形式来组织：

  ![1595251605760](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/52d8acf801667f725b26b07d6e5da5c9.png)

- 例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

### 5.1 定义Vue组件

注意：在实际开发中，我们并不会用以下方式开发组件，而是采用`vue-cli`创建，`vue`模板文件的方式开发，以下方法只是为了让大家理解什么是组件。  

**使用`Vue.component()`方法注册组件，格式如下：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <my-component></my-component>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    //先注册组件
    Vue.component("my-component",{
        template:'<li>Hello</li>'
    });
    //再实例化Vue
    var vm = new Vue({
        el:"#app",
    });
</script>
</body>
</html>
```

  说明：

- `Vue.component()`：注册组件
- `my-component`：自定义组件的名字
- `template`：组件的模板

#### 5.1.1 使用props属性传递参数

像上面那样用组件没有任何意义，所以我们是需要传递参数到组件的，此时就需要使用`props`属性了！  **注意：默认规则下props属性里的值不能为大写**

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>

<div id="app">
    <!--组件，传递给组件中的值：props-->
    <!--item为items中的每一个元素值，将item绑定到itemchild，将每一个元素值传给props数组，template再从props里取值，遍历输出-->
    <my-component v-for="item in items" v-bind:itemchild="item"></my-component>
</div>


<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    // 定义一个vue组件component组件
    Vue.component("my-component", {
        props: ['itemchild'],
        template: `<li>{{itemchild}}</li>`
    });
    var vm = new Vue({
        el: '#app',
        data: {
            items: ['Java','Linux','前端']
        }
    });
</script>
</body>
</html>
```

**说明**：

- `v-for="item in items"`：遍历`Vue`实例中定义的名为`items`的数组，并创建同等数量的组件
- `v-bind:itemchild="item"`：将遍历的`item`项绑定到组件中`props`定义名为`item`属性上；= 号左边的`itemchild`为`props`定义的属性名，右边的为`item in items` 中遍历的item项的值





## 6. Axios异步通信

### 6.1 什么是Axios

Axios是一个开源的可以用在浏览器端和`Node JS`的异步通信框架， 她的主要作用就是实现AJAX异步通信，其功能特点如下：

- 从浏览器中创建`XMLHttpRequests`
- 从node.js创建http请求
- 支持Promise API[JS中链式编程]
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF(跨站请求伪造)

GitHub：[https://github.com/axios/axios](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Faxios%2Faxios)  中文文档：[http://www.axios-js.com/](https://gitee.com/link?target=http%3A%2F%2Fwww.axios-js.com%2F)

### 6.2 为什么要使用Axios

由于`Vue.js`是一个视图层框架并且作者(尤雨溪) 严格准守SoC(关注度分离原则)所以`Vue.js`并不包含AJAX的通信功能， 为了解决通信问题， 作者单独开发了一个名为`vue-resource`的插件， 不过在进入2.0版本以后停止了对该插件的维护并推荐了`Axios`框架。少用`jQuery`， 因为它操作Dom太频繁!

> 简单的Axios使用

```html
<body>

    <div id="app">
        <input type="button" value="获取笑话" @click="getJoke">
        <p>{{ joke }}</p>
    </div>

    <!-- 官网提供的 axios 在线地址 -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    
    <script>
        /*
            接口：随机获取一条笑话
            请求地址：https://autumnfish.cn/api/joke
            请求方式：get
        */
        var app = new Vue({
            el: "#app",
            data: {
                joke: "我是一个笑话"
            },
            methods: {
                getJoke: function() {
                    console.log(this.joke);     // 我是一个笑话
                    // 需要在axios外面定义一个变量来引用传递到this(也可以用箭头函数=>)
                    var that = this;
                    axios
                    .get("https://autumnfish.cn/api/joke")
                    .then(
                        function(response) {
                            console.log(this.joke);     // undefined 说明this.joke的值已经改变了
                            // 将数据响应回去
                            that.joke = response.data;
                        },
                        function(err) {
                            
                        }
                    )
                    // 箭头函数写法：
                    axios
                    .get("https://autumnfish.cn/api/joke")
                    .then(response=>(this.joke=response.data));
                }
            }
        })

    </script>
</body>
```

![image-20220416222903521](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d913830583fbf0783dc68dbe27f8b557.png)

### 6.3 第一个Axios应用程序

咱们开发的接口大部分都是采用JSON格式， 可以先在项目里模拟一段JSON数据

创建一个名为`data.json`的文件并填入上面的内容， 放在项目的根目录下

```json
{
  "name": "狂神说Java",
  "url": "https://www.baidu.com",
  "page": 1,
  "isNonProfit": true,
  "address": {
    "street": "含光门",
    "city": "陕西西安",
    "country": "中国"
  },
  "links": [
    {
      "name": "bilibili",
      "url": "https://space.bilibili.com/95256449"
    },
    {
      "name": "狂神说Java",
      "url": "https://blog.kuangstudy.com"
    },
    {
      "name": "百度",
      "url": "https://www.baidu.com/"
    }
  ]
}
```

**测试代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--v-cloak 解决闪烁问题，没加载之前先白屏-->
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>

<div id="vue" v-cloak>
    <div>姓名：{{info.name}}</div>
    <div>地址：{{info.address.country}} -- {{info.address.city}} -- {{info.address.street}}</div>
    <div>
        链接：
        <!--href绑定到json数据中的url-->
        <a v-bind:href="info.url" target>百度一下</a>
    </div>
</div>

<!--引入js文件-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    let vm = new Vue({
        el: "#vue",
        // 这里的data() 是方法；而data: 是属性
        data() {
            return {
                info: {
                    name: null,
                    address:{
                        country:null,
                        city:null,
                        street:null
                    },
                    url: null
                }
            }
        },

        //钩子函数
        mounted() {
            axios
                // 去json文件里面取数据
                .get('../data.json')
                // 将取到的数据响应回
                .then(response=>(this.info=response.data));
        }
    });
</script>
</body>
</html>
```

![image-20220416224404059](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a998e420eb55ef7a95f99d07bdca4414.png)

说明：

1. `v-cloak`解决页面闪烁问题（刷新网页时不会首先出现`{{message}}`这样的信息）
2. 在这里使用了`v-bind`将`a:href`的属性值与Vue实例中的数据进行绑定
3. 使用axios框架的get方法请求Ajax并自动将数据封装进了Vue实例的数据对象中
4. 我们在data中的数据结构必须和`Ajax`响应回来的数据格式匹配

### 6.4 Vue的生命周期

官方文档：[https://cn.vuejs.org/v2/guide/instance.html#生命周期图示](https://gitee.com/link?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Finstance.html%23%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA) ，Vue实例有一个完整的生命周期，也就是从开始创建初女台化数据、编译模板、挂载DOM、渲染一更新一渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。

在Vue的整个生命周期中，它提供了一系列的事件，可以让我们在事件触发时注册JS方法，可以让我们用自己注册的JS方法控制整个大局，在这些事件响应方法中的this直接指向的是Vue的实例。

![1595253373596](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bd23627e82b9dba8e749768471f9e36a.png)





### 6.5 Axios数据交互

在实际开发中，大多数都是前后端分离的项目，前端只需要通过接口向后端寻要数据，渲染到页面就可以了。所以需要学习使用Axios实现前后端的数据交互！











## 7. 计算属性、内容分发、自定义事件

### 7.1 计算属性

计算属性的重点突出在`属性`两个字上(属性是名词)，首先它是个`属性`其次这个属性有`计算`的能力(计算是动词)，这里的`计算`就是个函数：简单点说，它就是一个能够将计算结果缓存起来的属性(将行为转化成了静态的属性)，仅此而已；可以想象为缓存!

**示例:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    <!--methods中的对象是方法，要加()，通过方法来调用-->
    <p>currentTime1: {{currentTime1()}}</p>

    <!--computed中的对象是属性，不加()，通过属性直接查看-->
    <p>currentTime2: {{currentTime2}}</p>
</div>

<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: "Hello"
        },
        methods: {
            currentTime1: function () {
                return Date.now();      //返回当前时间戳
            }
        },

        //计算属性；methods和computed方法名不能重名，重名只会调用methods的方法
        computed: {
            currentTime2: function () {
                this.message;
                return Date.now();
            }
        }
    });
</script>
</body>
</html>
```

注意：methods和computed里的东西不能重名 

说明：

- methods：定义方法， 调用方法使用currentTime1()， 需要带括号
- computed：定义计算属性， 调用属性使用currentTime2， 不需要带括号：`this.message`是为了能够让currentTime2观察到数据的变化而变化，改变了`this.message`的值（相当于刷新缓存），缓存中的currentTime2的数值才会变化。
- 如何在方法中的值发生了变化，则缓存就会刷新!可以在控制台使用`vm.message="vue"`， 改变下数据的值，再次测试观察效果

![image-20220327170845483](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1de69dab228708805ef94cd44b4102d4.png)



**结论：**调用方法时，每次都需要讲行计算，既然有计算过程则必定产生系统开销，那如果这个结果是不经常变化的呢?此时就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这点，**计算属性的主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销**





### 7.2 内容分发

在`Vue.js`中我们使用`<slot>`元素作为承载分发内容的出口，作者称其为插槽，可以应用在组合组件的场景中，实现动态的拔插。



**示例:**

比如准备制作一个待办事项组件(`todo`) ， 该组件由待办标题(`todo-title`) 和待办内容(`todo-items`)组成，但这三个组件又是相互独立的，该如何操作呢?

**定义一个待办事项的组件:**

```html
<div id="app">
    <todo></todo>
</div>
<!--1.导入Vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script type="text/javascript">
    Vue.component('todo',{
        template:'<div>\
                <div>代办事项</div>\
                <ul>\
                    <li>学习狂神说Java</li>\
                </ul>\
            </div>'
    })
</script>
```

这样写template的话，那么数据就是被写死了的，无法实现动态拔插！！！



**我们需要让代办事项的标题和值实现动态绑定，怎么做呢？我们可以留一个插槽！**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--将数据通过插槽插入-->
<div id="vue">
    <todo>
        <!--v-bind:可以简写为:-->
        <todo-title slot="todo-title" :title="studyTitle"></todo-title>

        <todo-items slot="todo-items" v-for="(item, index) in todoItems" :index="index" :item="item"></todo-items>
    </todo>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    //定义todo组件，预留插槽slot，实现动态的插入数据
    Vue.component("todo", {
        template: '<div>\
                   <slot name="todo-title"></slot>\
                   <ul>\
                       <slot name="todo-items"></slot> \
                   </ul>\
                   </div>'
    });

    //定义todo-title代办标题组件和todo-items代办内容组件
    Vue.component("todo-title", {
        props: ['title'],
        template: '<div>{{title}}</div>'
    });
    Vue.component("todo-items", {
       props: ['index', 'item'],
       template: '<li>{{index + 1}}. {{item}}</li>'
    });

    //实例化Vue，初始化数据
    let vm = new Vue({
        el: "#vue",
        data: {
            studyTitle: '学习任务',
            todoItems: ['MySQL', 'Spring', 'Vue']
        }
    });

</script>
</body>
</html>
```

测试：

![image-20220328204549924](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a906e3af9d69afd291a9a68d9f7b7fa9.png)

说明：我们的`todo-title`和`todo-items`组件分别被分发到了`todo`组件的`todo-title`和`todo-items`插槽中

### 7.3 自定义事件

通以上代码不难发现，数据项在Vue的实例中，但删除操作要在组件中完成，**组件不能直接操作Vue实例**。那么组件如何才能删除Vue实例中的数据呢？

此时就涉及到参数传递与事件分发了，Vue为我们提供了自定义事件的功能很好的帮助我们解决了这个问题；使用`this.$emit(‘自定义事件名’， 参数)` ，操作过程如下：

基于上面的示例增加一个删除功能：

1. 在vue的实例`vm`中增加了methods对象并定义了一个名为`removeTodoltems`的方法

   ```javascript
       //实例化Vue，初始化数据
       let vm = new Vue({
           el: "#vue",
           data: {
               studyTitle: '学习任务',
               todoItems: ['MySQL', 'Spring', 'Vue']
           },
           methods: {
               removeItems(index) {
                   console.log("成功删除任务: " + this.todoItems[index]);
                   this.todoItems.splice(index, 1);     //从index位置开始，一次删除1个元素
               }
           }
       });
   ```

2. 修改`todo-items`待办内容组件的代码，增加一个删除按钮，并且绑定事件！

   ```javascript
   Vue.component("todo-items", {
           props: ['index', 'item'],
           template: '<li>{{index + 1}}. {{item}} <button @click="remove">删除</button></li>',
           methods: {
               remove(index) {
                   //自定义事件分发，前端用v-on绑定该remove后再将事件给removeItems处理
                   this.$emit('remove', index);
               }
           }
       });
   ```

3. 修改`todo-items`待办内容组件的HTML代码，增加一个自定义事件，比如叫remove，可以和组件的方法绑定，然后绑定到vue的方法！

   ```javascript
   <todo-items slot="todo-items" v-for="(item, index) in todoItems" :index="index"
                       :item="item" @remove="removeItems(index)"></todo-items>
   ```
   



完整代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--将数据通过插槽插入-->
<div id="vue">
    <todo>
        <!-- "v-bind:"可以简写为":"   "v-on"可以简写为"@" -->
        <todo-title slot="todo-title" :title="studyTitle"></todo-title>
        <todo-items slot="todo-items" v-for="(item, index) in todoItems" :index="index"
                    :item="item" @remove="removeItems(index)"></todo-items>
    </todo>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    //定义todo组件，预留插槽slot，实现动态的插入数据
    Vue.component("todo", {
        template: '<div>\
                   <slot name="todo-title"></slot>\
                   <ul>\
                       <slot name="todo-items"></slot> \
                   </ul>\
                   </div>'
    });

    //定义todo-title代办标题组件和todo-items代办内容组件
    Vue.component("todo-title", {
        props: ['title'],
        template: '<div>{{title}}</div>'
    });

    Vue.component("todo-items", {
        props: ['index', 'item'],
        template: '<li>{{index + 1}}. {{item}} <button @click="remove">删除</button></li>',
        methods: {
            remove(index) {
                //自定义事件分发，前端用v-on绑定该remove后再将事件给removeItems处理
                this.$emit('remove', index);
            }
        }
    });

    //实例化Vue，初始化数据
    let vm = new Vue({
        el: "#vue",
        data: {
            studyTitle: '学习任务',
            todoItems: ['MySQL', 'Spring', 'Vue']
        },
        methods: {
            removeItems(index) {
                console.log("成功删除任务: " + this.todoItems[index]);
                this.todoItems.splice(index, 1);     //从index位置开始，一次删除1个元素
            }
        }
    });

</script>
</body>
</html>
```

测试：

![image-20220328220304968](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/00c90b070e4956bd356331d676bba9a4.png)

**逻辑理解:**

- 由于要从自定义组件`todo-items`中删除实例对象`vm`中的数据，显然是直接删除不到的，只有实例对象中的方法才可以做到。
- 使用自定义事件(方法)分发`this.$emit()`：
  1. 前端绑定(`v-on:`)一个自定义组件(`todo-items`)中的remove事件
  2. remove事件给实例对象中removeItems事件处理，即前端中的：`@remove="removeItems(index)`
  3. removeItems事件再对实例对象进行删除操作

![1595295779847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7284931727b09a42f8f7578685df55e5.png)

### 7.5 Vue入门小结

核心：数据驱动，组件化

优点：借鉴了AngularJS的模块化开发和React的虚拟Dom，虚拟Dom就是把Demo操作放到内存中执行；

常用的属性：

- `v-if`
- `v-else-if`
- `v-else`
- `v-for`
- `v-on`绑定事件，简写`@`
- `v-model`数据双向绑定
- `v-bind`给巨剑绑定参数，简写`:`

组件化：

- 组合组件`slot`插槽
- 组件内部绑定事件需要使用到`this.$emit("事件名",参数);`
- 计算属性的特色，缓存计算数据

遵循SoC关注度分离原则，Vue是纯粹的视图框架，并不包含，比如Ajax之类的通信功能，为了解决通信问题，我们需要使用Axios框架做异步通信；



**说明:**Vue的开发都是要基于NodeJS，实际开发采用Vue-cli脚手架开发，vue-router路由，vuex做状态管理；Vue UI，界面我们一般使用ElementUI（饿了么出品），或者ICE（阿里巴巴出品）来快速搭建前端项目

官网：

- [https://element.eleme.cn/#/zh-CN](https://gitee.com/link?target=https%3A%2F%2Felement.eleme.cn%2F%23%2Fzh-CN)
- [https://ice.work/](https://gitee.com/link?target=https%3A%2F%2Fice.work%2F)







## 8. 第一个Vue-cli项目

### 8.1 什么是vue-cli

- vue-cli官方提供的一个脚手架，用于快速生成一个vue的项目模板
- 预先定义好的目录结构及基础代码，就好比咱们在创建Maven项目时可以选择创建一个骨架项目，这个估计项目就是脚手架，我们的开发更加的快速；

**主要的功能**

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

### 8.2 需要的环境

**安装nodejs：**

- Node.js：[http://nodejs.cn/download/](https://gitee.com/link?target=http%3A%2F%2Fnodejs.cn%2Fdownload%2F)  安装就是无脑的下一步就好，安装在自己的环境目录下
- Git：[https://git-scm.com/doenloads](https://gitee.com/link?target=https%3A%2F%2Fgit-scm.com%2Fdoenloads)
- 镜像：[https://npm.taobao.org/mirrors/git-for-windows/](https://gitee.com/link?target=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fgit-for-windows%2F)
- cmd下输入`node -v`，查看是否能够正确打印出版本号即可！
- cmd下输入`npm -v`，查看是否能够正确打印出版本号即可！
  - 这个npm，就是一个软件包管理工具，就和linux下的apt软件安装差不多！

**安装Node.js淘宝镜像加速器（cnpm）**

```shell
# -g 就是全局安装
npm install cnpm -g

# 或使用如下语句解决npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```

虽然安装了cnpm，但是尽量少用，有些项目打包后不能使用！



**安装vue-cli**

```shell
cnpm install vue-cli -g
#测试是否安装成功#查看可以基于哪些模板创建vue应用程序，通常我们选择webpack
vue list
```

### 8.3 第一个vue-cli应用程序

1.创建一个Vue项目，我们随便建立一个空的文件夹在电脑上，我这里在D盘下新建一个目录

```shell
D:\Vue\Project\vue-study
```

2.创建一个基于webpack模板的vue应用程序

```shell
#1、首先需要进入到对应的目录 cd D:\Project\vue-study
#2、这里的myvue是顶日名称，可以根据自己的需求起名
vue init webpack myvue
```

一路都选择no即可； **说明：**

- Project name：项目名称，默认回车即可
- Project description：项目描述，默认回车即可
- Author：项目作者，默认回车即可
- Install vue-router：是否安装vue-router，选择n不安装（后期需要再手动添加）
- Use ESLint to lint your code:是否使用ESLint做代码检查，选择n不安装（后期需要再手动添加)
- Set up unit tests:单元测试相关，选择n不安装（后期需要再手动添加）
- Setupe2etests with Nightwatch：单元测试相关，选择n不安装（后期需要再手动添加）
- Should we run npm install for you after the,project has been created:创建完成后直接初始化，选择n，我们手动执行；运行结果！

![1595303458111](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d6afd36c1bb98929ec5722444079f26f.png)

#### 8.3.1 初始化

若报错可以尝试使用管理员身份再次进行安装

```shell
#进入myvue项目
cd myvue
#安装npm
npm install
```

![1595303888573](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/20b4de66f7604fa03dc84514f8964e82.png)

#### 8.3.2 运行

```shell
npm run dev
```

![1595304238421](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/576ab41613c5d67086441b83d60a4219.png)

![1595304400777](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7984c3606dd4a222b3d4b552ecfc27dc.png)

执行完成后，目录多了很多依赖

![1595305147701](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2682abd87a95a23c307064474e318f2e.png)

> Vue-Cli项目结构

![在这里插入图片描述](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/14d58fa25088256cd18c1439782db167.png)





## 9. webpack使用

### 9.1 什么是Webpack

- 本质上， webpack是一个现代JavaScript应用程序的静态模块打包器(module bundler)  。当webpack处理应用程序时， 它会递归地构建一个依赖关系图(dependency graph) ， 其中包含应用程序需要的每个模块，  然后将所有这些模块打包成一个或多个bundle.
- Webpack是当下最热门的前端资源模块化管理和打包工具，  它可以将许多松散耦合的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分离，等到实际需要时再异步加载。通过loader转换， 任何形式的资源都可以当做模块， 比如Commons JS、AMD、ES 6、CSS、JSON、Coffee Script、LESS等；
- 伴随着移动互联网的大潮， 当今越来越多的网站已经从网页模式进化到了WebApp模式。它们运行在现代浏览器里， 使用HTML 5、CSS  3、ES 6等新的技术来开发丰富的功能， 网页已经不仅仅是完成浏览器的基本需求； WebApp通常是一个SPA(单页面应用) ，  每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的JS代码，这给前端的开发流程和资源组织带来了巨大挑战。
- 前端开发和其他开发工作的主要区别，首先是前端基于多语言、多层次的编码和组织工作，其次前端产品的交付是基于浏览器的，这些资源是通过增量加载的方式运行到浏览器端，如何在开发环境组织好这些碎片化的代码和资源，并且保证他们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统，这个理想中的模块化系统是前端工程师多年来一直探索的难题。

### 9.2 模块化的演进

> Script标签

```javascript
	<script src = "module1.js"></script>
	<script src = "module2.js"></script>
	<script src = "module3.js"></script>
```

这是最原始的JavaScript文件加载方式，如果把每一个文件看做是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在window对象中，不同模块的调用都是一个作用域。 这种原始的加载方式暴露了一些显而易见的弊端：

- 全局作用域下容易造成变量冲突
- 文件只能按照`<script>`的书写顺序进行加载
- 开发人员必须主观解决模块和代码库的依赖关系
- 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪



> CommonsJS

------

服务器端的NodeJS遵循CommonsJS规范，该规范核心思想是允许模块通过require方法来同步加载所需依赖的其它模块，然后通过exports或module.exports来导出需要暴露的接口。

```javascript
require("module");
require("../module.js");
export.doStuff = function(){};
module.exports = someValue;
```

**优点：**

- 服务器端模块便于重用
- NPM中已经有超过45万个可以使用的模块包
- 简单易用

**缺点：**

- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
- 不能非阻塞的并行加载多个模块

**实现：**

- 服务端的NodeJS
- Browserify，浏览器端的CommonsJS实现，可以使用NPM的模块，但是编译打包后的文件体积较大
- modules-webmake，类似Browserify，但不如Browserify灵活
- wreq，Browserify的前身



> AMD

------

Asynchronous Module  Definition规范其实主要一个主要接口define(id?,dependencies?,factory);它要在声明模块的时候指定所有的依赖dependencies，并且还要当做形参传到factory中，对于依赖的模块提前执行。

```js
define("module",["dep1","dep2"],functian(d1,d2){
	return someExportedValue;
});
require（["module","../file.js"],function(module，file){});
```

**优点**

- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

**缺点**

- 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现

- RequireJS
- curl



> CMD

------

Commons Module Definition规范和AMD很相似，尽保持简单，并与CommonsJS和NodeJS的Modules规范保持了很大的兼容性。

```js
define(function(require,exports,module){
	var $=require("jquery");
	var Spinning = require("./spinning");
	exports.doSomething = ...;
	module.exports=...;
});
```

**优点：**

- 依赖就近，延迟执行
- 可以很容易在NodeJS中运行缺点
- 依赖SPM打包，模块的加载逻辑偏重

**实现**

- Sea.js
- coolie



> ES6模块

------

  EcmaScript 6标准增加了JavaScript语言层面的模块体系定义。ES 6模块的设计思想， 是尽量静态化， 使编译时就能确定模块的依赖关系， 以及输入和输出的变量。Commons JS和AMD模块，都只能在运行时确定这些东西。

```js
import "jquery"
export function doStuff(){}
module "localModule"{}
```

**优点**

- 容易进行静态分析
- 面向未来的EcmaScript标准

**缺点**

- 原生浏览器端还没有实现该标准
- 全新的命令，新版的Node JS才支持

**实现**

- Babel

**大家期望的模块**  系统可以兼容多种模块风格， 尽量可以利用已有的代码， 不仅仅只是JavaScript模块化， 还有CSS、图片、字体等资源也需要模块化。





### 9.3 安装Webpack

WebPack是一款模块加载器兼打包工具， 它能把各种资源， 如JS、JSX、ES 6、SASS、LESS、图片等都作为模块来处理和使用。  **安装：**

```shell
npm install webpack -g
npm install webpack-cli -g
```

测试安装是否成功：

- `webpack -v`
- `webpack-cli -v`

![image-20220328232330889](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/86264937efc798826c891dc0f8b99b79.png)



> 配置

 `webpack.config.js`配置文件说明

- entry：入口文件， 指定Web Pack用哪个文件作为项目的入口
- output：输出， 指定WebPack把处理完成的文件放置到指定路径
- module：模块， 用于处理各种类型的文件
- plugins：插件， 如：热更新、代码重用等
- resolve：设置路径指向
- watch：监听， 用于设置文件改动后直接打包

```js
module.exports = {
	entry:"",
	output:{
		path:"",
		filename:""
	},
	module:{
		loaders:[
			{test:/\.js$/,;\loade:""}
		]
	},
	plugins:{},
	resolve:{},
	watch:true
}
```

`webpack`: 打包命令

### 9.4 使用webpack

1. 创建项目（创建一个空文件夹，用idea打开）

2. 创建一个名为modules的目录，用于放置JS模块等资源文件

3. 在modules下创建模块文件，如hello.js，用于编写JS模块相关代码

   ```js
   //暴露一个方法
   exports.sayHi = function() {
       document.write("<h1>狂神说ES6</h1>")
   }
   ```

4. 在modules下创建一个名为main.js的入口文件，用于打包时设置entry属性

   ```js
   //require 导入一个模块，就可以调用这个模块中的方法了
   var hello = require("./hello");
   hello.sayHi();
   ```

5. 在项目目录下创建`webpack.config.js`配置文件，使用webpack命令打包(管理员方式)

   ```js
   module.exports = {
       entry: './modules/main.js',
       output: {
           filename: "./js/bundle.js"
       }
   }
   ```

6. 在项目目录下创建HTML页面，如index.html，导入webpack打包后的JS文件

   ```html
   <!doctype html>
   <html lang="en">
       <head>
           <meta charset="UTF-8">
           <title>狂神说Java</title>
       </head>
       <body>
           <script src="dist/js/bundle.js"></script>
       </body>
   </html>
   ```

7. 在IDEA控制台中直接执行webpack；如果失败的话，就使用管理员权限运行即可！

8. 运行HTML看效果，发现将`hello.js`中的内容显示了出来。

**说明**

```shell
# 参数--watch 用于监听变化，可以实现热部署
webpack --watch
```





## 10. vue-router路由

`Vue Router`是`Vue.js`官方的路由管理器。它和`Vue.js`的核心深度集成， 让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于Vue js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的CSS class的链接
- HTML5 历史模式或hash模式， 在IE 9中自动降级
- 自定义的滚动行为





> 安装

**基于第一个vue-cli进行测试学习；先查看node modules中是否存在vue-router** vue-router是一个插件包， 所以我们还是需要用npm/cnpm来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令。

```shell
npm install vue-router --save-dev
#若安装失败，可能是版本问题，在后面加上 --legacy-peer-deps
```

如果在一个模块化工程中使用它，必须要通过`Vue.use()`明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter);
```





> 测试

1、先删除没有用的东西

2、在`components` 目录下存放我们自己编写的组件 

3、定义一个`Content.vue` 的组件

```vue
<template>
  <div>
    <h1>内容页</h1>
  </div>
</template>

<script>
export default {
  name: "Content"
}
</script>

<style scoped>

</style>
```

`Main.vue`组件

```vue
<template>
  <div>
    <h1>首页</h1>
  </div>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped>

</style>
```

4、安装路由，在src目录下，新建一个文件夹：`router`，专门存放路由，配置路由`index.js`，如下

```js
import Vue from "vue";
//导入路由插件
import VueRouter from "vue-router";

//导入组件
import Content from "../components/Content";
import Main from "../components/Main";

//安装路由
Vue.use(VueRouter);

//配置导出路由
export default new VueRouter({
  routes: [
    {
      //路由路径(访问路径，相当于@RequestMapping("/xxx"))
      path: '/content',

      //路由的名字
      name: 'content',

      //跳转的组件
      component: Content
    },

    {
      path: '/main',
      name: 'main',
      component: Main
    }
  ]
});
```

5、在`main.js`中配置路由，整个前端工程只有这一个入口

```js
import Vue from 'vue'
//导入组件
import App from './App'

//导入路由配置目录
import router from "./router";    //自动扫描里面的路由配置

//关闭生产模式下给出的提示
Vue.config.productionTip = false

new Vue({
  el: '#app',
  //配置路由
  router,
  components: { App },
  template: '<App/>'
})
```

6、在`App.vue`中使用路由

```vue
<template>
  <div id="app">
    <h1>Vue-Router</h1>
    <!--
		router-link：默认会被渲染成一个<a>标签，to属性为指定链接
		router-view：用于渲染路由匹配到的组件
    -->
    <router-link to="/main">首页</router-link>
    <router-link to="/content">内容页</router-link>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App'
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

注意：vue项目中各项的版本号必须匹配，这里使用的是Vue2的版本，vue-router应该使用3版本，使用4版本会不兼容；

修改成指定版本：`npm i vue-router@3.2.0 --save`

7、在控制台输入`npm run dev`，运行项目

![image-20220329122526430](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/04bff0d7612abab94726e8e5bf2f1ff5.png)

**总结:**

- `index.js`：路由配置，位于router目录下，在此配置具体的路由

- `main.js`：程序的主入口，在这里导入路由配置的目录，该目录下的所有路由配置都可以被扫描到，项目中一般只需要配置一次，以后都不用再动了
- `App.vue`：`main.js`中绑定了该vue，用于展示主页面，在`App.vue`中可以引用路由配置中`routes`里的路由路径
- components目录下的`Content.vue`和`Main.vue`：就是路由组件展示的真正内容

![image-20220329125455424](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/556bfec13a4f46d81fef83956d9d4318.png)



**有了`vue-router`后，就实现了模块化开发，十分便捷。如果想要添加页面，只需三步：**

1. 在components目录下创建相应的页面组件
2. 在router目录下的`index.js`路由配置中导入该页面组件，并配置该组件的导出路由
3. 最后在`App.vue`中引用该路由即可





## 11. vue实战

### 11.1 创建工程

注意：命令行都要使用管理员模式运行 

1、创建一个名为hello-vue的工程`vue init webpack hello-vue` 

2、安装依赖， 我们需要安装vue-router、element-ui、sass-loader和node-sass四个插件

```shell
#进入工程目录
cd hello-vue
#安装vue-routern，指定版本
npm install vue-router@3.2.0 --save-dev
#安装element-ui
npm i element-ui -S
#安装依赖
npm install
# 安装SASS加载器
cnpm install sass-loader node-sass --save-dev
#启功测试
npm run dev
```

3、npm命令解释：

- `npm install moduleName`：安装模块到项目目录下
- `npm install -g moduleName`：-g的意思是将模块安装到全局，具体安装到磁盘哪个位置要看npm config prefix的位置
- `npm install --save moduleName`：--save的意思是将模块安装到项目目录下， 并在package文件的dependencies节点写入依赖，-S为该命令的缩写
- `npm install --save-dev moduleName`:--save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写

### 11.2 创建登录页面

  把没有用的初始化东西删掉！  在源码目录中创建如下结构：

- assets：用于存放资源文件
- components：用于存放Vue功能组件
- views：用于存放Vue视图组件
- router：用于存放vue-router配置

![image-20220329135140863](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6540470ae270cd7d9b056883d762c3d0.png)

创建首页视图，在views目录下创建一个名为`Main.vue`的视图组件：

```vue
<template>
<h1>首页</h1>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped>

</style>
```

登录页视图在views目录下创建名为`Login.vue`的视图组件，其中`el-form`的元素为ElementUI组件：

```vue
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        form: {
          username: '',
          password: ''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },

        // 对话框显示和隐藏
        dialogVisible: false
      }
    },
    methods: {
      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$router.push("/main");
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
```

创建路由，在router目录下创建一个名为`index.js`的vue-router路由配置文件

```js
import Vue from "vue";
import VueRouter from "vue-router";
import Login from "../views/Login";
import Main from "../views/Main";


Vue.use(VueRouter);

export default new VueRouter({
  routes: [
    {
      path: '/login',
      component: Login
    },
    {
      path: '/main',
      component: Main
    }
  ]
});
```

APP.vue

```vue
<template>
  <div id="app">
    <h1>Hello Vue</h1>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',
}
</script>

<style>

</style>
```

main.js

```js
import Vue from 'vue'
import App from './App'

import router from "./router"

import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(router)
Vue.use(ElementUI)

new Vue({
  el: '#app',
  router,
  render: h=>h(App)
});
```

### 11.3 运行项目

1. 输入命令

   ```shell
   npm run dev
   ```

2. 如果报错如下图：Module build failed: TypeError: loaderContext.getResolve is not a function（sass-loader版本太高）

   解决:

   修改配置文件，重新安装

   ```shell
   //1.修改sass-loader的版本为^4.0.0
   //2.重新安装配置环境
   npm install
   ```

   或者 cnpm install

3. 再次执行终于成功

   ```shell
   npm run dev
   ```

   ![1595338033562](https://gitee.com/lzh_gitee/Vue/raw/master/Vue%E8%AF%BE%E5%A0%82%E7%AC%94%E8%AE%B0/10%E5%AE%9E%E6%88%98%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B.assets/1595338033562.png)

### 11.4 路由嵌套

嵌套路由又称子路由，在实际应用中，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

![image-20220329151715521](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9490d3c4a682c96a6b9c48bf313a0c8f.png)

1. 用户信息组件，在 views/user 目录下创建一个名为 `Profile.vue` 的视图组件；

   ```vue
   <template>
       <div>
         <h1>个人信息</h1>
       </div>
   </template>
   
   <script>
       export default {
           name: "UserProfile"
       }
   </script>
   
   <style scoped>
   
   </style>
   ```

2. 用户列表组件在 views/user 目录下创建一个名为 `List.vue` 的视图组件；

   ```vue
   <template>
       <div>
         <h1>用户列表</h1>
       </div>
   </template>
   
   <script>
       export default {
           name: "UserList"
       }
   </script>
   
   <style scoped>
   
   </style>
   ```

3. 配置嵌套路由修改 router 目录下的 `index.js` 路由配置文件，代码如

   ```js
   import Vue from "vue"
   import VueRouter from "vue-router"
   import Login from "../views/Login"
   import Main from "../views/Main"
   
   import UserList from '../views/user/List'
   import UserProfile from '../views/user/Profile'
   
   Vue.use(VueRouter);
   
   export default new VueRouter({
     routes: [
       {
         path: '/login',
         component: Login
       },
       {
         path: '/main',
         component: Main,
         //嵌套路由
         children: [
           {path: '/user/profile', component: UserProfile},
           {path: '/user/list', component: UserList}
         ]
       }
     ]
   });
   ```

4. 修改首页视图，我们修改 `Main.vue` 视图组件，此处使用了 ElementUI 布局容器组件，代码如下：

   ```vue
   <template>
     <div>
       <el-container>
         <el-aside width="200px">
           <el-menu :default-openeds="['1']">
             <el-submenu index="1">
               <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
               <el-menu-item-group>
                 <el-menu-item index="1-1">
                   <router-link to="/user/profile">个人信息</router-link>
                 </el-menu-item>
                 <el-menu-item index="1-2">
                   <router-link to="/user/list">用户列表</router-link>
                 </el-menu-item>
               </el-menu-item-group>
             </el-submenu>
             <el-submenu index="2">
               <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
               <el-menu-item-group>
                 <el-menu-item index="2-1">分类管理</el-menu-item>
                 <el-menu-item index="2-2">内容列表</el-menu-item>
               </el-menu-item-group>
             </el-submenu>
           </el-menu>
         </el-aside>
   
         <el-container>
           <el-header style="text-align: right; font-size: 12px">
             <el-dropdown>
               <i class="el-icon-setting" style="margin-right: 15px"></i>
               <el-dropdown-menu slot="dropdown">
                 <el-dropdown-item>个人信息</el-dropdown-item>
                 <el-dropdown-item>退出登录</el-dropdown-item>
               </el-dropdown-menu>
             </el-dropdown>
           </el-header>
   
           <el-main>
             <router-view />
           </el-main>
         </el-container>
       </el-container>
     </div>
   </template>
   
   <script>
   export default {
     name: "Main"
   }
   </script>
   
   <style scoped lang="scss">
   .el-header {
     background-color: #24a153;
     color: #333;
     line-height: 60px;
   }
   
   .el-aside {
     color: #333;
   }
   </style>
   ```

说明：

在元素中配置了用于展示嵌套路由,主要使用个人信息展示嵌套路由内容

### 11.5 参数传递

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。此时我们就需要传递参数了；

### 11.6 组件转发

#### $route方式

1. 修改路由配置,

   - 主要是在`index.js`的 path 属性中增加了 :id，:name 这样的占位符

     ```js
         {
           path: '/main',
           component: Main,
           //嵌套路由
           children: [
             {
               path: '/user/profile/:id/:name', 
               name: 'UserProfile',
               component: UserProfile,
             },
             {
               path: '/user/list',
               component: UserList
             }
           ]
         }
     ```

2. 传递参数

   - 此时我们将 `to` 改为了 `:to`，是为了将这一属性当成对象使用，

   - 去`Main.vue`主页中，在点击个人信息的时候传递2个参数过去；注意 `router-link` 中的 name 属性名称一定要和路由中的name 属性名称 匹配，因为这样 Vue 才能找到对应的路由路径；

     ```vue
     <!--name:传组件名， params：传递参数， 需要对象： v-bind-->
     <router-link :to="{name:'UserProfile',params:{id:1,name:'狂神'}}">个人信息</router-link>
     ```

3. 在`Profile`中接收参数, 在目标组件中，注意：所有东西都要用div标签包起来，不能直接放在根节点上，否则会报错

   ```vue
   <div>
       <h1>个人信息</h1>
       {{$route.params.id}}
       {{$route.params.name}}
   </div>
   ```

#### props方式

1. 修改路由配置 , 主要在routes中增加了 `props: true` 属性

   ```js
       routes: [
       {
         path: '/login',
         component: Login
       },
       {
         path: '/main',
         component: Main,
         //嵌套路由
         children: [
           {
             path: '/user/profile/:id/:name',
             name: 'UserProfile',
             component: UserProfile,
             props: true       //支持props方式
           },
           {
             path: '/user/list',
             component: UserList
           }
         ]
       }
     ]
   ```

2. 传递参数和之前一样

3. `Profile.vue`中接收参数为目标组件增加 props 属性

   ```vue
   <template>
     <div>
       <h1>个人信息</h1>
       {{id}}
       {{name}}
     </div>
   </template>
   
   <script>
   export default {
     props: ['id', 'name'],
     name: "UserProfile"
   }
   </script>
   
   <style scoped>
   
   </style>
   ```



**登录表单传递信息:**

在`Login.vue`中的onSubmit方法中修改：

```js
    onSubmit(formName) {
      // 为表单绑定验证功能
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
          this.$router.push("/main/" + this.form.username);   //提交表单的时候把username传过去
        } else {
          this.dialogVisible = true;
          return false;
        }
      });
    }
```

在`index.js`的path中带上传递过来的username，利用props传递给前端页面组件：

```js
      path: '/main/:username',
      component: Main,
      props: true,
```

在`Main.vue`中利用props接收username：

```vue
<script>
export default {
  props: ['username'],
  name: "Main"
}
</script>
```

让username显示在页面：

```vue
<span>{{username}}</span>
```

从登录页面跳转到Main页面：

![image-20220329162811922](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/afc949852d1f649f189a3fabe9374cb0.png)



### 11.7 组件重定向

- 重定向的意思大家都明白，但 Vue 中的重定向是作用在路径不同但组件相同的情况下，比如：在`index.js`的子路由中增加一个

  ```js
        //嵌套路由
        children: [
          {
            path: '/user/profile/:id/:name',
            name: 'UserProfile',
            component: UserProfile,
            props: true       //支持props方式
          },
          {
            path: '/user/list',
            component: UserList
          },
          {
            path: '/goHome',
            redirect: '/main'
          }
        ]
  ```

- 说明：这里定义了两个路径，一个是 `/main` ，一个是`/goHome`

  - 其中 `/goHome` 重定向到了 `/main` 路径，由此可以看出重定向不需要定义组件；

- 使用的话，只需要设置对应路径即可；

  ```vue
  	    <el-menu-item-group>
                <el-menu-item index="1-1">
                  <!--name传组件名，params传参数-->
                  <router-link :to="{name: 'UserProfile', params: {id: 1, name: 'NiceVue'}}">个人信息</router-link>
                </el-menu-item>
                <el-menu-item index="1-2">
                  <router-link to="/user/list">用户列表</router-link>
                </el-menu-item>
                <el-menu-item index="1-3">
                  <router-link to="/goHome">回到首页</router-link>
                </el-menu-item>
              </el-menu-item-group>
  ```

## 12. 路由模式与404

### 路由模式有两种

- hash：路径带 # 符号，如 [http://localhost/#/login](https://gitee.com/link?target=http%3A%2F%2Flocalhost%2F%23%2Flogin)

  - 默认为hash路由模式

    ```js
    export default new Router({
      routes: []
    })
    ```

- history：路径不带 # 符号，如 [http://localhost/login](https://gitee.com/link?target=http%3A%2F%2Flocalhost%2Flogin)

  - history路由模式

    ```js
    export default new Router({
      mode: 'history',
      routes: []
    })
    ```

### 404

1. 在views目录下创建一个名为 `NotFound.vue` 的视图组件，代码如下：

   ```vue
   <template>
       <div>
         <h1>404,你的页面走丢了</h1>
       </div>
   </template>
   
   <script>
       export default {
           name: "NotFound"
       }
   </script>
   
   <style scoped>
   
   </style>
   ```

2. 在`index.js`的routes中增加路由配置，代码如下：

   ```js
   import NotFound from '../views/NotFound'
   
       {
         //*代表有对应的请求就走它，没有就走此NotFound
         path: '*',
         component: NotFound
       }
   ```

![image-20220329163553530](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/591c0a156f338495831c75a4a10c6a1f.png)



## 13. 路由钩子与异步请求

### 13.1 路由钩子

和过滤器，拦截器相似

- `beforeRouteEnter`：在进入路由前执行
- `beforeRouteLeave`：在离开路由前执行

我们在`Profile.vue`中增加路由钩子：

```js
export default {
      name: "UserProFile",
      props: ['id','name'],
      beforeRouteEnter: (to,from,next) => {
        console.log('进入路由之前')
        next()		//继续往下走
      },
      beforeRouteLeave: (to,from,next) => {
        console.log('进入路由之后')
        next()
      }
    }
```

可以发现，我们在点击个人信息时，会打印出“进入路由之前”，在点击其他链接时，会打印出“进入路由之后”：

![image-20220329165519246](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ac1465e5a60f1e9824029b3ead4f65a3.png)

参数说明：

- to：路由将要跳转的路径信息
- from：路径跳转前的路径信息
- next：路由的控制参数
- next() 跳入下一个页面
- next(’/path’) 改变路由的跳转方向，使其跳到另一个路由
- next(false) 返回原来的页面
- next((vm)=>{}) 仅在 beforeRouteEnter 中可用，vm 是组件实例

### 13.2 异步请求

1. 安装 Axios和vue-axios: `cnpm install axios` `cnpm install --save vue-axios`

2. `main.js`引用 Axios

   ```js
   import axios from 'axios'
   import VueAxios from 'vue-axios'
   
   Vue.use(VueAxios, axios)
   ```

3. 准备数据 ： 只有我们的 static 目录下的文件是可以被访问到的，所以我们就把静态文件放入该目录下`static/mock/data.json`

   ```json
   {
     "name": "狂神说Java",
     "url": "https://blog.kuangstudy.com",
     "page": 1,
     "isNonProfit": true,
     "address": {
       "street": "含光门",
       "city": "陕西西安",
       "country": "中国"
     },
     "links": [
       {
         "name": "bilibili",
         "url": "https://space.bilibili.com/95256449"
       },
       {
         "name": "狂神说Java",
         "url": "https://blog.kuangstudy.com"
       },
       {
         "name": "百度",
         "url": "https://www.baidu.com/"
       }
     ]
   }
   ```

4. 运行项目`npm run dev`看是否正常

   

5. 在 beforeRouteEnter 中进行异步请求

   ```js
   export default {
       name: "UserProFile",
       props: ['id','name'],
       beforeRouteEnter: (to,from,next) => {
           console.log('进入路由之前');	//加载数据
           next(vm => {
               vm.getData();	//进入路由之前执行getData()
           })
       },
       beforeRouteLeave: (to,from,next) => {
           console.log('进入路由之后');
           next()
       },
       methods: {
           getData() {
               this.axios({
                   method: 'get',
                   url: 'http://localhost:8080/static/mock/data.json'
               }).then((response) => {
                   console.log(response)
               })
           }
       }
   }
   ```

测试：在点击个人信息的时候，json中的数据获取成功

![image-20220329170114787](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/04c7b3318d506ad14139a7bfe8dd406a.png)



