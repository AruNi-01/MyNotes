# HTML初识

## 1. 骨架格式

**「HTML骨架格式」**

```html
<!-- 页面中最大的标签 根标签 -->
<html>
    <!-- 头部标签 -->
    <head>     
        <!-- 标题标签 -->
        <title></title> 
    </head>
    <!-- 文档的主体 -->
    <body>
    </body>
</html>
```



## 2. 常用标签

**「1. 排版标签」**主要和css搭配使用，显示网页结构的标签，是网页布局最常用的标签。

- 标题标签h(h1~h6)
- 段落标签p,可以把 HTML 文档分割为若干段落
- 水平线标签hr
- 换行标签br
- div和span标签:是没有语义的,是我们网页布局最主要的2个盒子。

**「2. 排版标签」**

- b和strong 文字以粗体显示
- i和em 文字以斜体显示
- s和del 文字以加删除线显示
- u和ins 文字以加下划线显示

**「3. 标签属性(行内式)」**

使用HTML制作网页时，如果想让HTML标签提供更多的信息，可以使用HTML标签的属性加以设置。

```
<标签名 属性1="属性值1" 属性2="属性值2" …> 内容 </标签名>
<手机 颜色="红色" 大小="5寸">  </手机>
```



**「4. 图像标签img」**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823123.png)

**注意：**

- 标签可以拥有多个属性，必须写在开始标签中，位于标签名后面。
- 属性之间不分先后顺序，标签名与属性、属性与属性之间均以空格分开。
- 采取  键值对 的格式  key="value"  的格式

```
<img src="cz.jpg" width="300" height="300" border="3" title="这是个小蒲公英" />
```



**「5. 链接标签(重点)」**

```
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
target="_self"  默认窗口弹出方式
target="_blank" 新窗口弹出
```

| 属性   | 作用                                                         |
| :----- | :----------------------------------------------------------- |
| href   | 用于指定链接目标的url地址，（必须属性）当为标签应用href属性时，它就具有了超链接的功能 |
| target | 用于指定链接页面的打开方式，其取值有_self和_blank两种，其中_self为默认值，_blank为在新窗口中打开方式。 |



**src 和 href 的区别**

一句话概括:**src 是引入资源的 href 是跳转url的**

1. src用于替换当前元素，href用于在当前文档和引用资源之间确立联系。
2. src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素。当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部。
3. href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接。如果我们在文档中添加那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用link方式来加载css，而不是使用@import方式。

**注意：**

1. 外部链接 需要添加 http://www.baidu.com
2. 内部链接 直接链接内部页面名称即可 比如 < a href="index.html"> 首页< /a>
3. 如果当时没有确定链接目标时，通常将链接标签的href属性值定义为“#”(即href="#")，表示该链接暂时为一个空链接。
4. 不仅可以创建文本超链接，在网页中各种网页元素，如图像、表格、音频、视频等都可以添加超链接。

**锚点定位：通过创建锚点链接，用户能够快速定位到目标内容。**

```
1. 使用<a href="#id名">链接文本</a>    创建链接文本（被点击的） 
  <a href="#two">   
  
2. 使用相应的id名标注跳转目标的位置。 (找目标)
<h3 id="two">第2集</h3> 
```

**「6. 注释标签」**

```
 <!-- 注释语句 -->     
  快捷键是：    ctrl + /       
  或者 ctrl +shift + / 
```

**团队约定：**注释内容前后各一个空格字符，注释位于要注释代码的上面，单独占一行



**空格**

- &nbsp ;    半角的不断行的空白格（推荐使用）
- &ensp ;    半角的空格 
- &emsp ;   全角的空格

详细的含义：

&nbsp ;    ：这是我们使用最多的空格，也就是按下space键产生的空格。在HTML中，如果你用空格键产生此空格，空格是不会累加的（只算1个）。要使用html实体表示才可累加。该空格占据宽度受字体影响明显而强烈。在inline-block布局中会搞些小破坏，在两端对齐布局中又是不可少的元素。

&ensp ;    ：此空格有个相当稳健的特性，就是其**占据的宽度正好是1/2个中文宽度**，而且基本上不受字体影响。

&emsp ;    ：此空格也有个相当稳健的特性，就是**其占据的宽度正好是1个中文宽度**，而且基本上不受字体影响。



**示例：**

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML常用标签</title>
</head>
<body>
    <h1 id="top"></h1>

    <h1>我是一级标题</h1>
    我换行啦！！！ <br />
    <img src="images/tree01.png" alt="二叉树" title="二叉树" width="500px"> <br />
    
    <strong>我是加粗的字体</strong>
    <em>我是倾斜的字体</em>
    <del>我是删除线</del>
    <ins>我是下划线</ins>

    <p>我自己是一个段落哦！！</p>
    
    <div>我是一个垂直盒子</div>
    <div>我是一个垂直盒子</div>
    <div>我是一个垂直盒子</div>
    <span>我是一个水平盒子</span>
    <span>我是一个水平盒子</span>
    <span>我是一个水平盒子</span>

    <h3>1.外部链接标签</h3>
    <a href="http://www.bilibili.com" target="_blank">点我打开B站官网</a> <br />
    <h3>下面这张图片可以点击跳转到相应的界面哦！！</h3> 
    <a href="https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%A0%91/1602879" target="_blank">
        <img src="images/tree01.png" alt="二叉树" title="二叉树" height="200px">
    </a> <br />

    <h3>2.内部链接标签</h3>
    <a href="测试网页.html" target="_blank">我是一个内部链接</a> <br />
    
    <h3>3.空链接</h3>
    <a href="#">空链接</a> <br />

    <h3>4.下载链接</h3>
    <a href="images/tree03.zip">点我下载</a> <br />

    <h3>5.锚点定位链接</h3>
    <a href="#maodian">点击转到锚点位置</a> <br />
    我 <br />
    只 <br />
    是 <br />
    占 <br />
    行 <br />
    用 <br />
    的 <br />
    ! <br />
    ! <br />
    <h5 id="maodian">我就是锚点位置！！！</h5>
    <p> &emsp;&emsp;锚点是网页制作中超级链接的一种，又叫命名锚记，像一个迅速定位器一样，是一种页面内的超级链接，
        运用相当普遍。 使用命名锚记可以在文档中设置标记，这些标记通常放在文档的特定主题处或顶部。然
        后可以创建到这些命名锚记的链接，这些链接可快速将访问者带到指定位置。
    </p>
    <a href="#top">点我返回顶部</a> <br />



</body>
</html>
```



## 3. 表格

**「1. 表格」**

现在还是较为常用的一种标签，但不是用来布局，常见显示、展示表格式数据。因为它可以让数据显示的非常的规整，可读性非常好。特别是后台展示数据的时候表格运用是否熟练就显得很重要，一个清爽简约的表格能够把繁杂的数据表现得很有条理。

**「2. 创建表格」**

```
<table>
  <tr>
    <td>单元格内的文字</td>
    ...
  </tr>
  ...
</table>
```

table、tr、td，他们是创建表格的基本标签，缺一不可

- table用于定义一个表格标签。

- tr标签 用于定义表格中的行，必须嵌套在 table标签中。

- td 用于定义表格中的单元格，必须嵌套在<tr></tr>标签中。

- 字母 td 指表格数据（table data），即数据单元格的内容，现在我们明白，表格最合适的地方就是用来存储数据的。td像一个容器，可以容纳所有的元素

  ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251824962.webp)

**表头单元格标签th**:一般表头单元格位于表格的第一行或第一列，并且文本加粗居中,只需用表头标签<th></th>替代相应的单元格标签<td></td>即可。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251824165.webp)

**表格标题caption**通常这个标题会被居中且显示于表格之上。caption 标签必须紧随 table 标签之后。这个标签只存在 表格里面才有意义。你是风儿我是沙

```
<table>
   <caption>我是表格标题</caption>
</table>
```

**「3. 表格属性」**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251824798.webp)三参为0，平时开发的我们这三个参数   border  cellpadding  cellspacing 为  0

**「4. 合并单元格」**,合并的顺序我们按照  先上 后下   先左  后右 的顺序 ,合并完之后需要删除多余的单元格。

- **跨行合并**：rowspan="合并单元格的个数"

- **跨列合并**：colspan="合并单元格的个数"

  ```html
  <!DOCTYPE html>
  <html>
  <head> 
  <meta charset="utf-8"> 
  <title>菜鸟教程(runoob.com)</title> 
  </head>
  <body>
  
  <h4>单元格跨两列:</h4>
  <table border="1">
  <tr>
    <th>Name</th>
    <th colspan="2">Telephone</th>
  </tr>
  <tr>
    <td>Bill Gates</td>
    <td>555 77 854</td>
    <td>555 77 855</td>
  </tr>
  </table>
  
  <h4>单元格跨两行:</h4>
  <table border="1">
  <tr>
    <th>First Name:</th>
    <td>Bill Gates</td>
  </tr>
  <tr>
    <th rowspan="2">Telephone:</th>
    <td>555 77 854</td>
  </tr>
  <tr>
    <td>555 77 855</td>
  </tr>
  </table>
  
  </body>
  </html>
  ```

  ![image-20220114155232622](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823124.png)



**「5. 总结表格」**

| 标签名              | 定义           | 说明                                         |
| :------------------ | :------------- | :------------------------------------------- |
| <table></table>     | 表格标签       | 就是一个四方的盒子                           |
| <tr></tr>           | 表格行标签     | 行标签要再table标签内部才有意义              |
| <td></td>           | 单元格标签     | 单元格标签是个容器级元素，可以放任何东西     |
| <th></th>           | 表头单元格标签 | 它还是一个单元格，但是里面的文字会居中且加粗 |
| <caption></caption> | 表格标题标签   | 表格的标题，跟着表格一起走，和表格居中对齐   |
| clospan 和 rowspan  | 合并属性       | 用来合并单元格的                             |

**「6. 表格划分结构」**

  对于比较复杂的表格，表格的结构也就相对的复杂了，所以又将表格分割成三个部分：题头、正文和脚注。而这三部分分别用:thead,tbody,tfoot来标注， 这样更好的分清表格结构。

**注意：**

1. <thead></thead>：用于定义表格的头部。用来放标题之类的东西。<thead> 内部必须拥有<tr> 标签！
2. <tbody></tbody>：用于定义表格的主体。放数据本体 。
3.  <tfoot></tfoot>放表格的脚注之类。
4. 以上标签都是放到table标签中。



**示例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表格</title>
</head>
<body>
    <!-- alingn-位置；border-边框；cellpadding-字与表格间距；cellspacing-表格与表格间距 -->
    <table align="center" border="1" cellpadding="0" cellspacing="0" width="500" height="250">
        <caption>表格标题</caption>
        <!-- tr：行；td：单元格；th：表头；thead：题头；tbody：正文；tfoot：脚注 -->
        <!-- rowspan: 跨行；clospan: 跨列 -->
        <thead>
            <tr>
                <th>姓名</th> 
                <th>性别</th> 
                <th>年龄</th>
                <th>称号</th>
            </tr>
        </thead>
        <tbody>
            <tr> 
                <td>刘德华</td>
                <td>男</td> 
                <td>51</td>
                <td rowspan="4">四大天王</td>
            </tr>
            <tr> 
                <td>张学友</td>
                <td>男</td> 
                <td>54</td> 
                
            </tr>
            <tr> 
                <td>郭富城</td> 
                <td>男</td> 
                <td>53</td> 
                
            </tr>
            <tr> 
                <td>黎明</td> 
                <td>男</td> 
                <td>57</td> 
                
            </tr>
        </tbody>
    </table>
</body>
</html>
```

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823125.png)

## 4. 列表

**「列表ul」**容器里面装载着结构，样式一致的文字或图表的一种形式，叫列表。

列表最大的特点就是整齐 、整洁、 有序，跟表格类似，但是它可组合自由度会更高。

**「1. 无序列表 ul」**

- <ul></ul>中只能嵌套<li></li>，直接在<ul></ul>标签中输入其他标签或者文字的做法是不被允许的。
- <li>与</li>之间相当于一个容器，可以容纳所有元素。

```
<ul>
  <li>列表项1</li>
  <li>列表项2</li>
  <li>列表项3</li>
  ......
</ul>
```

**「2. 有序列表 ol」**

- <ol>标签中的type属性值为排序的序列号，不添加type属性时，有序列表默认从数字1开始排序。
- 常用的type属性值分别为是1，a，A，i，I
- <ol reversed="reversed">中的reversed属性能够让有序列表中的序列倒序排列。
- <ol start="3">中的start属性值为3，有序列表中的第一个序列号将从3开始排列。

```
<ol type="A"> 
  <li>列表项1</li>
  <li>列表二</li>
  <li>列表三</li>
</ol>
```

**「2. 自定义列表 dl」**

- 定义列表常用于对术语或名词进行解释和描述，定义列表的列表项前没有任何项目符号。

```
<dl>
  <dt>名词</dt>
  <dd>名词1解释1</dd>
  <dd>名词1解释2</dd>
  ...
  <dt>名词2</dt>
  <dd>名词2解释1</dd>
  <dd>名词2解释2</dd>
  ...
</dl>
```

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251825264.webp)



**示例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>列表标签</title>
</head>
<body>
    <h2>无序列表</h2>
    <ul>
        <li>无序列表1</li>
        <li>无序列表2</li>
        <li>无序列表3</li>
    </ul>

    <h2>有序列表</h2>
    <ol>
        <li>有序列表1</li>
        <li>有序列表2</li>
        <li>有序列表3</li>
    </ol>

    <h2>自定义列表</h2>
    <dl>
        <dt>关注我们</dt>
        <dd>官方微信</dd>
        <dd>新浪微博</dd>
        <dd>联系我们</dd>
    </dl>
</body>
</html>
```



## 5. 表单

在HTML中，一个完整的表单通常由表单控件（也称为表单元素）、提示信息和表单域3个部分构成。表单目的是为了收集用户信息。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251825818.webp)**表单控件：**
 包含了具体的表单功能项，如单行文本输入框、密码输入框、复选框、提交按钮、重置按钮等。
**提示信息：**
 一个表单中通常还需要包含一些说明性的文字，提示用户进行填写和操作。
**表单域：** 
 它相当于一个容器，用来容纳所有的表单控件和提示信息，可以通过他定义处理表单数据所用程序的url地址，以及数据提交到服务器的方法。如果不定义表单域，表单中的数据就无法传送到后台服务器。

### 5.1 表单域

- 收集的用户信息怎么传递给服务器？

- - 通过form表单域

- 目的：

- - 在HTML中，form标签被用于定义表单域，以实现用户信息的收集和传递，form中的所有内容都会被提交给服务器。

```
<form action="url地址" method="提交方式" name="表单名称">
  各种表单控件
</form>
```

**常用属性：**

- 每个表单都应该有自己表单域。后面学 ajax 后台交互的时候，必须需要form表单域。

| 属性   | 属性值   | 作用                                               |
| :----- | :------- | :------------------------------------------------- |
| action | url地址  | 用于指定接收并处理表单数据的服务器程序的url地址。  |
| method | get/post | 用于设置表单数据的提交方式，其取值为get或post。    |
| name   | 名称     | 用于指定表单的名称，以区分同一个页面中的多个表单。 |

**GET 和 POST 的区别**

- GET在浏览器回退时是无害的，而POST会再次提交请求。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET请求大小一般是(1024字节)，http协议并没有限制，而与服务器，操作系统有关，POST理论上来说没有大小限制，http协议规范也没有进行大小限制，但实际上post所能传递的数据量根据取决于服务器的设置和内存大小。
- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

**团队约定：**

- 元素属性值使用双引号语法
- 元素属性值可以写上的都写上

```
推荐
<input type="text" /> 
<input type="radio" name="name" checked="checked" />
```



### 5.2 表单控件(表单元素)

```
<input type="属性值" value="你好">
```

- input 输入的意思
- <input />标签为单标签
- type属性设置不同的属性值用来指定不同的控件类型
- 除了type属性还有别的属性

**常用属性：**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251825420.webp)

```
用户名: <input type="text" /> 
密  码：<input type="password" />
```

**value属性**

- value 默认的文本值。有些表单想刚打开页面就默认显示几个文字，就可以通过这个value 来设置。

```
用户名:<input type="text"  name="username" value="请输入用户名"> 
```

**name属性**

- name表单的名字， 这样，后台可以通过这个name属性找到这个表单。 页面中的表单很多，name主要作用就是用于区别不同的表单。

- - name属性后面的值，是我们自己定义的。
  - radio  如果是一组，我们必须给他们命名相同的名字 name  这样就可以多个选其中的一个啦
  - name属性，我们现在用的较少，但是，当我们学ajax 和后台的时候，是必须的。

```
<input type="radio" name="sex"  />男
<input type="radio" name="sex" />女
```

**checked属性**

- 表示默认选中状态。 较常见于 单选按钮和复选按钮。

```
性    别:
<input type="radio" name="sex" value="男" checked="checked" />男
<input type="radio" name="sex" value="女" />女 
```

**input 属性小结**

| 属性    | 说明     | 作用                                                   |
| :------ | :------- | :----------------------------------------------------- |
| type    | 表单类型 | 用来指定不同的控件类型                                 |
| value   | 表单值   | 表单里面默认显示的文本                                 |
| name    | 表单名字 | 页面中的表单很多，name主要作用就是用于区别不同的表单。 |
| checked | 默认选中 | 表示那个单选或者复选按钮一开始就被选中了               |

**「1.  label标签」**

- label 标签为 input 元素定义标注（标签）。
- label标签主要目的是为了提高用户体验。为用户提高最优秀的服务。

**作用：**用于绑定一个表单元素, 当点击label标签的时候, 被绑定的表单元素就会获得输入焦点。

**如何绑定元素呢**

- 第一种用法就是用label标签直接包含input表单， 适合单个表单选择
- 第二种用法 for 属性规定 label 与哪个表单元素绑定(通过id)。

```
  第一种
  <label> 用户名： 
    <input type="radio" name="usename" value="请输入用户名">   
  </label>
  
  第二种for -- id
  <label for="sex">男</label>
  <input type="radio" name="sex"  id="sex">
```

**「2.  textarea控件(文本域)」**

- 通过textarea控件可以轻松地创建多行文本输入框.
- cols="每行中的字符数" rows="显示的行数"  我们实际开发不用

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251825700.webp)

```
  <textarea >
    文本内容
  </textarea>
```

**文本框和文本域区别**

| 表单              |  名称  |       区别       |                  默认值显示 |             用于场景 |
| :---------------- | :----: | :--------------: | --------------------------: | -------------------: |
| input type="text" | 文本框 | 只能显示一行文本 | 单标签，通过value显示默认值 | 用户名、昵称、密码等 |
| textarea          | 文本域 | 可以显示多行文本 |  双标签，默认值写到标签中间 |               留言板 |

**「3.  select下拉列表」**

- 如果有多个选项让用户选择，为了节约空间，我们可以使用select控件定义下拉列表。
- 在option 中定义selected =" selected "时，当前项即为默认选中项。
- 我们实际开发会用的比较少

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251825121.webp)

```html
<select>
  
  <option>选项1</option>
  <option>选项2</option>
  <option>选项3</option>
  ...
</select>
```



**示例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单</title>
</head>
<body>
    <h1>表单</h1>
    <form>

        用户名：<input type="text" name="suername" value="请输入用户名" maxlength="10"> <br>
        
        密&emsp;码：<input type="password" name="password" maxlength="15"> <br>
        
        性&emsp;别：<input type="radio" name="sex" value="男"> 男 <input type="radio" name="sex" value="女"> 女 <br>
        
        爱&emsp;好：<input type="checkbox" name="hobby" value="吃饭" checked="checked"> 吃饭  <input type="checkbox" name="hobby" value="睡觉" checked="checked"> 睡觉 <input type="checkbox" name="hobby" value="打游戏" > 打游戏 <br>
        
        满18岁：<input type="radio" name="是否满18" value="是" checked="checked"> 是 <input type="radio" name="是否满18" value="否"> 否 <br>

        <input type="submit" value="免费注册" >

        <input type="reset" value="重置选项" >

        <input type="button" value="获取短信验证码"> <br>

        上传文件：<input type="file" > <br>

        <!-- label标签    点击字体就可以选择相应的控件-->
        <input type="radio" name="sex2" id="boy"> <label for="boy">男孩</label>
        <input type="radio" name="sex2" id="girl"> <label for="girl">女孩</label>
        <br>

        <!-- select下拉列表 -->
        籍贯：<select>
            <option>山东</option>
            <option>北京</option>
            <option selected="selected">贵州</option>
            <option>湖南</option>
            <option>湖北</option>
        </select> <br>

        <!-- textarea文本域    当输入的内容过多时，不能使用text，要用textarea-->
        个人介绍：
        <textarea cols="50" rows="5"></textarea> <br>
        详细地址：
        <textarea cols="75" rows="7">请输出您的详细地址</textarea> <br>
        
    </form>
</body>
</html>
```





## 相关案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>注册界面</title>
</head>
<body>
    <h3>青春不常在，抓紧谈恋爱</h3>
    <table width="500" >
        <tr>
            <td>性别：</td>
            <td>
                <form>
                    <input type="radio" name="sex" value="男" id="nan"> <label for="nan">男</label>
                    <input type="radio" name="sex" value="女" id="nv"> <label for="nv">女</label>
                </form>
            </td>
        </tr>
        <tr>
            <td>生日：</td>
            <td>
                <form>
                    <select>
                        <option>--请选择年--</option>
                        <option>1983</option>
                        <option>1984</option>
                        <option>1985</option>
                        <option>1986</option>
                        <option>1987</option>
                        <option>1988</option>
                        <option>1989</option>
                        <option>1990</option>
                        <option>1991</option>
                        <option>1992</option>
                        <option>1993</option>
                        <option>1994</option>
                    </select>
                    <select>
                        <option>--请选择月--</option>
                        <option>1</option>
                        <option>2</option>
                        <option>3</option>
                        <option>4</option>
                        <option>5</option>
                        <option>6</option>
                        <option>7</option>
                    </select>
                </form>
            </td>
        </tr>
        <tr>
            <td>所在地区：</td>
            <td>
                <form>
                    <input type="text" name="area" value="请输入所在地">
                </form>
            </td>
        </tr>
        <tr>
            <td>婚姻状况：</td>
            <td>
                <form>
                    <input type="radio" name= "marry" value="未婚" id="weihun"> <label for="weihun">未婚</label>
                    <input type="radio" name="marry" value="已婚" id="yihun"> <label for="yihun">已婚</label>
                    <input type="radio" name="marry" value="离婚" id="lihun"> <label for="lihun">离婚</label>
                </form>
            </td>
        </tr>
        <tr>
            <td>学历：</td>
            <td>
                <form>
                    <input type="text" name="education" value="请输入您的学历">
                </form>
            </td>
        </tr>
        <tr>
            <td>喜欢的类型：</td>
            <td>
                <form>
                    <input type="checkbox" name="liketype" value="妩媚的" id="wumei"> <label for="wumei">妩媚的</label>
                    <input type="checkbox" name="liketype" value="可爱的" id="keai"> <label for="keai">可爱的</label>
                    <input type="checkbox" name="liketype" value="小鲜肉" id="xianrou"> <label for="xianrou">小鲜肉</label>
                    <input type="checkbox" name="liketype" value="都喜欢" id="all" checked="checked"> <label for="all">都喜欢</label>
                </form>
            </td>
        </tr>
        <tr >
            <td >自我介绍：</td>
            <td>
                <form>
                    <textarea cols="50" rows="7">简单介绍一下自己叭</textarea>
                </form>
            </td>
        </tr>
        <tr>
            <td rowspan="4"></td>
            <td>
                <form>
                    <input type="submit" name="enroll" value="免费注册">
                </form>
            </td>
        </tr>
        <tr>
            <td>
                <form>
                    <input type="checkbox" name="rule" checked="checked">我同意注册条款和会员加入标准
                </form>
            </td>
        </tr>
        <tr>
            <td> <a href="#">我是会员，立即登录</a></td>
        </tr>
        <tr>
            <td>
                <h2>我承诺</h2>
                <ur>
                    <li>年满18，单身</li>
                    <li>抱着严肃的态度</li>
                    <li>真诚寻找另一半</li>
                </ur>
            </td>
        </tr>
    </table>
</body>
</html>
```

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823126.png)





# CSS

## 1. CSS构造块

**「1. HTML的局限性」**

- HTML满足不了设计者的需求，可以将网页结构与样式相分离，这样就可以在不更改网页结构的前提下，更换网站的样式。
- 操作html属性不方便
- HTML里面添加样式带来的是无尽的臃肿和繁琐

**「2. CSS网页的美容师」**

- 让我们的网页更加丰富多彩，布局更加灵活自如。
- CSS最大的贡献：让HTML从样式中脱离，实现了HTML专注去做结构呈现，样式交给CSS

**「3. CSS」**CSS(Cascading Style Sheets)通常称为CSS样式表或层叠样式表(级联样式表)。

- **作用**

- - 主要用于设置HTML页面中的文本内容(字体、大小、对齐方式等)\图片的外形(宽高、边框样式、边距等)以及版面的布局和外观显示样式。
  - CSS以HTML为基础，提供了丰富的功能，如字体、样式、背景的控制及整体排版等，而且可以针对不同的浏览器设置不同的样式。

**「4. CSS注释」**

```
/* 这是注释 */
```



### 1.1 引入CSS样式表

**「1.行内式(内联样式)」**

通过标签的style属性来设置元素的样式

- style其实就是标签的属性
- 样式属性和值中间是:
- 多组属性值直接用;隔开
- 只能控制当前的标签和以及嵌套在其中的字标签，造成代码冗余。
- **缺点:**没有实现样式和结构相分离。

```html
<标签名 style="属性1:属性值1; 属性2:属性值2; 属性3:属性值3;"> 内容 </标签名>
例如：
<div style="color: red; font-size: 12px;">青春不常在，抓紧谈恋爱</div>
```

**「2.内部样式表(内嵌样式表)」**

也称为内嵌式，将CSS代码集中写在HTML文档的head头部标签中，并且用style标签定义。

- style标签一般位于head标签中，当然理论上他可以放在HTML文档的任何地方。
- type="text/css"  在html5中可以省略。
- 只能控制当前的页面
- **缺点:**没有彻底分离结构与样式

```html
<head>
<style type="text/CSS">
    选择器（选择的标签） { 
      属性1: 属性值1;
      属性2: 属性值2; 
      属性3: 属性值3;
    }
</style>
</head>
```

**「3.外部样式表(外链式)」**

也称链入式，是将所有的样式放在一个或多个以.css为扩展名的外部样式表文件中，通过link标签将外部样式表文件链接到HTML文档中。

- `rel`:定义当前文档与被链接文档之间的关系，在这里需要指定为“stylesheet”，表示被链接的文档是一个样式表文件。
- `href`:定义所链接外部样式表文件的URL，可以是相对路径，也可以是绝对路径。

```html
<link rel="stylesheet" href="index.css">
```

**「4.团队约定-代码风格」**

```css
/*1.紧凑格式 (Compact)*/
h3 { color: deeppink;font-size: 20px;}
// 2.一种是展开格式（推荐）
h3 {
 color: deeppink;
    font-size: 20px;    
}

/* 团队约定-代码大小写*/
/* 样式选择器，属性名，属性值关键字全部使用小写字母书写，属性字符串允许使用大小写。*/
/* 推荐 */
h3{
 color: pink;
}
 
/* 不推荐 */
H3{
 COLOR: PINK;
}
```



## 2. CSS基础选择器

**CSS选择器作用**

找到指定的HTML页面元素，选择标签。

### 2.1 CSS基础选择器

**「1. 标签选择器」**

- 标签选择器（元素选择器）是指用HTML标签名称作为选择器，按标签名称分类，为页面中某一类标签指定统一的CSS样式。
- 作用：可以把某一类标签全部选择出来。
- 优点：快速为网页中同类型的标签统一样式
- 缺点：不能设计差异化样式。

```
标签名{属性1:属性值1; 属性2:属性值2; 属性3:属性值3; } 
```

**「2. 类选择器」**

- 类选择器使用"."(英文点号)进行标识，后面紧跟类名。
- 语法：类名选择器

```html
.类名  {   
    属性1:属性值1; 
    属性2:属性值2; 
    属性3:属性值3;     
}
<p class='类名'></p>
```

- `优点`：可以为元素对象定义单独或相同的样式。可以选择一个或者多个标签。

- `注意`：类选择器使用“.”（英文点号）进行标识，后面紧跟类名(自定义，我们自己命名的)

- - 长名称或词组可以使用中横线来为选择器命名。
  - 不要纯数字、中文等命名， 尽量使用英文字母来表示。
  - 多类名选择器：各个类名中间用空格隔开。

**「3. id选择器」**id选择器使用`#`进行标识，后面紧跟id名

- 元素的id值是唯一的，只能对应于文档中某一个具体的元素。

```html
#id名 {属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }
<p id="id名"></p>
```

**「4. 通配符选择器」**

通配符选择器用`*`号表示，`*` 就是选择所有的标签。它是所有选择器中作用范围最广的，能匹配页面中所有的元素。

- `注意`：会匹配页面所有的元素，降低页面响应速度，不建议随便使用

```
* { 属性1:属性值1; 属性2:属性值2; 属性3:属性值3; }
```

例如下面代码，使用通配符选择器定义CSS样式，清除所有HTML标记的默认边距。

```html
* {
  margin: 0;                    /* 定义外边距*/
  padding: 0;                   /* 定义内边距*/
}
```

**「5. 基础选择器总结」**

| 选择器       | 作用                          | 缺点                     | 使用情况   | 用法                 |
| :----------- | :---------------------------- | :----------------------- | :--------- | :------------------- |
| 标签选择器   | 可以选出所有相同的标签，比如p | 不能差异化选择           | 较多       | p { color：red;}     |
| 类选择器     | 可以选出1个或者多个标签       | 可以根据需求选择         | 非常多     | .nav { color: red; } |
| id选择器     | 一次只能选择器1个标签         | 只能使用一次             | 不推荐使用 | #nav {color: red;}   |
| 通配符选择器 | 选择所有的标签                | 选择的太多，有部分不需要 | 不推荐使用 | * {color: red;}      |

**「6. 团队约定-选择器」**

1. 尽量少用通配符选择器 `*`。
2. 尽量少用ID选择器
3. 不使用无具体语义定义的标签选择器。

```
/* 推荐 */
.jdc {}
li {}
p{}

/* 不推荐 */
*{}
#jdc {}
div{}   因为div 没有语义，我们尽量少用
```



**示例：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>基础选择器</title>
    <style>
	
        /* 1、标签选择器，改变所有该标签的属性 */
        p {
            color: green;
            
        }
        div {
            color: red;
        }

        /* 2、类选择器，谁要用调用即可 */
        .font_blue {
            color: blue;
        }
        .font_white {
            color: white;
        }
        .box {
            width: 100px;
            height: 100px;
        }
        .back_red {
            background-color: red;
        }
        .back_green {
            background-color: green;
        }
        .font35 {
            font-size: 35px;
        }

        /* 3、id选择器，id选择器只能调用一次，不能被重复调用 */
        #pink {
            color: pink;
        }
		
		/* 4、通配符选择器，选取页面中所有元素，不会选取有样式的内容 */
		* {
			color: dodgerblue;
		}
		
    </style>
</head>
<body>
    <!-- 1、标签选择器 -->
    <p>男生</p>
    <p>男生</p>
    <div>女生</div>
    <div>女生</div>

    <!-- 2、类选择器 -->
    <p class="blue">男生</p>

    <!-- 类选择器的多类名 -->
    <div class="box back_red font_white">红色</div>
    <div class="box back_green">绿色</div>
    <div class="box back_red font_white">红色</div>

    <!-- 3、id选择器 -->
    <p id="pink">粉红色</p>
    
	<!-- 4、通配符选择器 -->
	<h1>通配符</h1>
	<strong>选择器</strong>
	<h3>测试</h3>
	
	
</body>
</html>
```



### 2.2 CSS复合选择器

复合选择器是由两个或多个基础选择器，通过不同的方式组合而成的

**「1. 后代选择器」**又称为包含选择器

- 用来选择元素或元素组的子孙后代
- 其写法就是把外层标签写在前面，内层标签写在后面，中间用**「空格」**分隔，先写父亲爷爷，再写儿子孙子。
- 子孙后代都可以这么选择。或者说，它能选择任何包含在内 的标签。

```
父级 子级{属性:属性值;属性:属性值;}

.class h3 {color:red;font-size:16px;}
```

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826257.webp)

- 当标签发生嵌套时，内层标签就成为外层标签的后代。
- 子孙后代都可以这么选择。或者说，它能选择任何包含在内的标签。

**「2. 子元素选择器」**

- 子元素选择器只能选择作为某元素子元素(亲儿子)的元素。
- 其写法就是把父级标签写在前面，子级标签写在后面，中间跟一个 `>` 进行连接
- 这里的子,指的是亲儿子。不包含孙子 重孙子之类。

```
.class>h3 {color:red;font-size:14px;}
```

**「3. 交集选择器」**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826893.webp)

- 其中第一个为标签选择器，第二个为class选择器，两个选择器之间`不能有空格`，如h3.special。

```
交集选择器是并且的意思,即...又...的意思
比如：   p.one   选择的是： 类名为 .one 的段落标签。 
/*用的相对来说比较少，不建议使用。*/
```

**「4. 并集选择器」**如果某些选择器定义的相同样式，就可以利用并集选择器，可以让代码更简洁。并集选择器（CSS选择器分组）是各个选择器通过`,`连接而成的，通常用于集体声明。

- 任何形式的选择器（包括标签选择器、class类选择器 id选择器等），都可以作为并集选择器的一部分。
- 并集选择器通常用于集体声明 ，逗号隔开的，所有选择器都会执行后面样式，逗号可以理解为和的意思。

```
比如  
.one, 
p , 
#test {color: #F00;}  
表示   .one 和 p  和 #test 这三个选择器都会执行颜色为红色。 
通常用于集体声明。  
```

**「5. 链接伪类选择器」**

用于向某些选择器添加特殊的效果。写的时候，他们的**顺序尽量不要颠倒,按照lvha的顺序**。否则可能引起错误。

链接伪类，是利用交集选择器.

- `a:link` 未访问过的链接
- `a:visited` 已访问过的链接
- `a:hover` 鼠标移动到链接上
- `a:active` 选定的链接（鼠标点击住的时候）

**实际工作中，很少写全四个状态，一般写法如下：**

```
a {   /* a是标签选择器  所有的链接 */
   font-weight: 700;
   font-size: 16px;
   color: gray;
      text-decoration: none; /* 清除链接默认的下划线*/
}
a:hover {   /* :hover 是链接伪类选择器 鼠标经过 */
   color: red; /*  鼠标经过的时候，由原来的 灰色 变成了红色 */
}
```

**「6. 复合选择器总结」**

| 选择器            | 作用                     | 特征                 | 使用情况 | 隔开符号及用法                          |
| :---------------- | :----------------------- | :------------------- | :------- | :-------------------------------------- |
| 后代选择器        | 用来选择元素后代         | 是选择所有的子孙后代 | 较多     | 符号是`空格` .nav a                     |
| 子代选择器        | 选择 最近一级元素        | 只选亲儿子           | 较少     | 符号是`>`  .nav>p                       |
| 交集选择器        | 选择两个标签交集的部分   | 既是 又是            | 较少     | `没有符号` p.one                        |
| 并集选择器        | 选择某些相同样式的选择器 | 可以用于集体声明     | 较多     | 符号是`逗号` .nav, .header              |
| 链接伪类选择器    | 给链接更改状态           | 跟链接相关           | 较多     | 重点记住 a{} 和 a:hover  实际开发的写法 |
| input:focus选择器 | 选择获取光标的表单       | 跟表单有关           | 较少     | input:focus                             |



**示例：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>符合选择器</title>
		<style>
			/* 1、后代选择器 */
			ol li {
				color: #FF0000;
			}

			ul li a {
				color: #008000;
				text-decoration: none;
			}

			.nav li a {
				color: #0000FF;
				text-decoration: none;
			}

			/* 2、子元素选择器（重要） */
			.nav2>a {
				color: #FFC0CB;
			}
			
			/* 3、并集选择器（重要） */
			div,
			p,
			.pig li {
				color: #FFC0CB;
			}
			
			/* 4、伪类选择器 */
			a:link {
				color: black;
				text-decoration: none;
			}
			a:visited {
				color: #888888;
			}
			a:hover {
				color: #FF0000;
			}
			a:active {
				color: #008000;
			}
			
			/* focus选择器，把获得光标的input表单元素选取出来 */
			input:focus {
				background-color: #10ff60;
				
			}
		</style>
	</head>
	<body>
		<!-- 1、后代选择器 -->
		<ol class="nav">
			<li>我是ol的孩子</li>
			<li>我是ol的孩子</li>
			<li>我是ol的孩子</li>
			<li>我是ol的孩子</li>
			<li><a href="#">我是nav类下li的孩子a</a></li>
		</ol>
		<ul>
			<li>我是ul的孩子</li>
			<li>我是ul的孩子</li>
			<li>我是ul的孩子</li>
			<li><a href="#" target="_blank">我是ul里li的孩子a</a></li>
		</ul>

		<!-- 2、子元素选择器（重要） -->
		<div class="nav2">
			<a href="#">我是儿子，子元素选择器只选择我，因为我离nav2近</a>
			<p>
				<a href="#">我是孙子，不选择我</a>
			</p>
		</div>
		
		<!-- 3、并集选择器（重要） -->
		<div>熊大 -- 粉红色</div>
		<p>熊二 -- 粉红色</p>
		<span>光头强</span>
		<ul class="pig">
			<li>小猪佩奇 -- 粉红色</li>
			<li>猪妈妈 -- 粉红色</li>
			<li>猪妈妈 -- 粉红色</li>
		</ul>

		<!-- 4、伪类选择器 -->
		<a href="http:\\www.baidu.com" target="_blank">百度首页</a>
		<a href="http:\\www.bilibili.com" target="_blank">B站首页</a>
		<br />
		<!-- focus选择器 -->
		<input type="text" />
		<input type="text" />
		<input type="text" />
	</body>
</html>
```



## 3. CSS字体样式

### 3.1 font字体

**「1. font-size」**

- font-size属性用于设置字号(字体大小)
- `谷歌浏览器`默认的文字大小为16px
- 不同浏览器可能默认显示的字号大小不一致，我们尽量给一个明确值大小，不要默认大小。一般给body指定整个页面文字的大小。

```
p { font-size:20px; }
```

#### 单位

- 相对长度单位、绝对长度单位

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826723.webp)**「2. font-family」**

- font-family属性用于设置哪一种字体。

```
p { font-family:"微软雅黑";}
```

- 指定多个字体，如果浏览器不支持第一个字体就会尝试下一个直到找到合适的字体，如果都没有，以电脑默认字体为准。

```
p {font-family: Arial,"Microsoft Yahei", "微软雅黑";}
```

- CSS Unicode字体

- - 在 CSS 中设置字体名称，直接写中文是可以的。但是在文件编码（GB2312、UTF-8 等）不匹配时会产生乱码的错误。
  - xp 系统不支持 类似微软雅黑的中文。
  - 解决方案：英文来替代。比如`font-family:"Microsoft Yahei"`。在 CSS 直接使用 Unicode 编码来写字体名称可以避免这些错误。使用 Unicode 写中文字体名称，浏览器是可以正确的解析的。

```
font-family: "\5FAE\8F6F\96C5\9ED1";   表示设置字体为“微软雅黑”。
```

**「3. font-weight」**

| 属性值  | 描述                                                        |
| :------ | :---------------------------------------------------------- |
| normal  | 默认值（不加粗的）                                          |
| bold    | 定义粗体（加粗的）                                          |
| 100~900 | 400 等同于 normal，而 700 等同于 bold  (数字表示粗细用的多) |

**「4. font-weight」**

font-style属性用于定义字体风格，如设置斜体、倾斜或正常字体，其可用属性值如下：

| 属性   | 作用                                                    |
| :----- | :------------------------------------------------------ |
| normal | 默认值，浏览器会显示标准的字体样式  font-style: normal; |
| italic | 浏览器会显示斜体的字体样式。                            |

**「5. font:综合设置字体样式」**

```
选择器 { font: font-style  font-weight  font-size/line-height  font-family;}
```

- 注意：使用font属性时，**必须按上面语法格式中的顺序书写，不能更换顺序**，各个属性以`空格`隔开

- - 其中不需要设置的属性可以省略(取默认值),但必须保留`font-size`和`font-family`属性，否则font属性将不起作用。

**「6. font总结」**

| 属性        | 表示     | 注意点                                                       |
| :---------- | :------- | :----------------------------------------------------------- |
| font-size   | 字号     | 我们通常用的单位是px 像素，一定要跟上单位                    |
| font-family | 字体     | 实际工作中按照团队约定来写字体                               |
| font-weight | 字体粗细 | 记住加粗是 700 或者 bold  不加粗 是 normal 或者  400  记住数字不要跟单位 |
| font-style  | 字体样式 | 记住倾斜是 italic   不倾斜 是 normal  工作中我们最常用 normal |
| font        | 字体连写 | 1. 字体连写是有顺序的  不能随意换位置 2. 其中字号 和 字体 必须同时出现 |



### 3.2 CSS外观属性

**「1. color」**

color属性用于定义**文本的颜色**
其取值方式有以下3种：

- 实际工作中，用16进制的写法是最多的，且我们更喜欢简写方式比如#f0代表红色。

| 表示表示       | 属性值                        |
| :------------- | :---------------------------- |
| 预定义的颜色值 | red，green，blue，pink        |
| 十六进制       | #FF0000，#FF6600，#29D794     |
| RGB代码        | rgb(255,0,0)或rgb(100%,0%,0%) |

**「2.text-align」**

text-align属性用于设置文本内容的**水平对齐方式**，相当于html中的align对齐属性。

- 注意：是让盒子里面的文本内容水平居中， 而不是让盒子居中对齐

其可用属性值如下：

| 属性   |       解释       |
| :----- | :--------------: |
| left   | 左对齐（默认值） |
| right  |      右对齐      |
| center |     居中对齐     |

**「3. line-height」**line-height属性用于**设置行间距**，就是行与行之间的距离，即字符的垂直间距，一般称为行高。

- line-height常用的属性值单位有三种，分别为像素px，相对值em和百分比%，实际工作中使用最多的是像素px

```
一般情况下，行距比字号大7--8像素左右就可以了。
line-height: 24px;
```

#### 3.2.1 行高测量

行高测量方法：![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826050.webp)

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826883.webp)`行高测量方法`行高我们利用最多的一个地方是：可以让单行文本在盒子中垂直居中对齐。

> **文字的行高等于盒子的高度。**行高  =  上距离 +  内容高度  + 下距离
> 上距离和下距离总是相等的，因此文字看上去是垂直居中的。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251826418.webp)

#### 3.2.2 行高与高度的三种关系

- 如果 行高 等 高度  文字会 垂直居中
- 如果行高 大于 高度  文字会 偏下
- 如果行高小于高度  文字会  偏上

```
  /*line-height 要设置在font属性下面，否则无效，例如：*/
  height: 80px;
  text-align: center;
  font: normal bold 30px "宋体";
  line-height: 80px;
```

可以使用display:flex;布局方式让文字水平垂直居中

```
  display: flex;
  align-items: center;     /* 侧轴对齐方式*/
  justify-content: center; /* 主轴对齐方式 */
```

**「4. text-indent」**

text-indent属性用于**设置首行文本的缩进**

- 其属性值可为不同单位的数值、em字符宽度的倍数、或相对于浏览器窗口宽度的百分比%，允许使用负值。
- 建议使用em作为设置单位。
- 1em 就是一个字的宽度。如果是汉字的段落，1em 就是一个汉字的宽度

```
p {
      /*行间距*/
      line-height: 25px;
      /*首行缩进2个字  em  1个em 就是1个字的大小*/
      text-indent: 2em;  
 }
```

**「5. text-decoration」**文本的装饰

text-decoration,通常我们用于**给链接修改装饰效果**

| 值           | 描述                                                  |
| :----------- | :---------------------------------------------------- |
| none         | 默认。定义标准的文本。取消下划线（最常用）            |
| underline    | 定义文本下的一条线。下划线 也是我们链接自带的（常用） |
| overline     | 定义文本上的一条线。（不用）                          |
| line-through | 定义穿过文本下的一条线。（不常用）                    |

**「6. CSS外观属性总结」**

| 属性            | 表示     | 注意点                                                 |
| :-------------- | :------- | :----------------------------------------------------- |
| color           | 颜色     | 我们通常用  十六进制  比如 而且是简写形式 #fff         |
| line-height     | 行高     | 控制行与行之间的距离                                   |
| text-align      | 水平对齐 | 可以设定文字水平的对齐方式                             |
| text-indent     | 首行缩进 | 通常我们用于段落首行缩进2个字的距离  text-indent: 2em; |
| text-decoration | 文本修饰 | 记住 添加 下划线  underline  取消下划线  none          |



**示例：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>文本文字样式</title>
		<style>
			/* 链接修饰 */
			.no_underline{
				text-decoration: none;
			}
			
			/* 文本缩进 */
			p{
				text-indent: 2em;
			}
			
		</style>
	</head>
	<body>
		<a class="no_underline" href="https://www.w3.org/" target="_blank">我是没有下划线的连接</a>
		
		<p>我是一个段落！！！</p>
	</body>
</html>
```



## 4. 标签显示模式(display)

`标签显示模式`是标签以什么方式进行显示。HTML标签一般分为块标签和行内标签两种类型，它们也称为块元素和行内元素。

#### 4.1 标签显示模式转换 display

- 块转行内：display:inline;
- 行内转块：display:block;
- 块、行内元素转换为行内块：display: inline-block;

**「1. 块级元素(block-level)」**

> 常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素。

**块级元素的特点**

- 独占一行
- 高度，宽度，外边距以及内边距都可以控制。
- 宽度默认是容器(父级宽度)的100%
- 是一个容器及盒子，里面可以放行内或者块级元素
- **注意**：只有文字才能组成段落，因此p标签里面不能放块级元素，特别是p不能放div。同理，还有h1~h6，dt,它们都是文字类块级标签，里面不能放其他块级元素。

**「2. 行内元素(inline-level)」**

> 有的地方也称为`内联元素`
>
> 常见的行内元素有<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>标签最典型的行内元素。

**行内元素的特点**

1. 相邻行内元素在一行上，一行可以显示多个。
2. 高度、宽度直接设置是无效的。
3. 默认高度就是它本身内容的宽度。
4. 行内元素只能容纳文本或其他行内元素。

**注意**

- 链接里面不能再放链接
- 特殊情况a里面可以放块级元素，但是给a转换一下块级模式最安全。

**「3. 行内块元素(inline-block)」**

> 在行内元素中有几个特殊的标签——<img>、<input >、<td>，可以对它们设置宽高和对齐属性，有些资料可能会称它们为行内块元素。

- **行内块元素的特点**
  1. 和相邻行内元素(行内块)在一行上，但是之间会有空白风险。一行可以显示多个
  2. 默认宽度就是它本身内容的宽度。
  3. 高度，行高，外边距以及内边距都可以控制。

**三种模式总结**

| 元素模式   | 元素排列               | 设置样式               | 默认宽度         | 包含                     |
| :--------- | :--------------------- | :--------------------- | :--------------- | :----------------------- |
| 块级元素   | 一行只能放一个块级元素 | 可以设置宽度高度       | 容器的100%       | 容器级可以包含任何标签   |
| 行内元素   | 一行可以放多个行内元素 | 不可以直接设置宽度高度 | 它本身内容的宽度 | 容纳文本或则其他行内元素 |
| 行内块元素 | 一行放多个行内块元素   | 可以设置宽度和高度     | 它本身内容的宽度 |                          |



**示例：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>标签显示模式的转换</title>
		<style>
			/* 1、行内元素转块级元素 */
			a {
				width: 300px;
				height: 150px;
				background-color: #10FF60;
				display: block;
			}
			
			/* 2、块级元素转行内元素 */
			div {
				width: 300px;
				height: 100px;
				background-color: #FFC0CB;
				display: inline;
			}
			
			/* 3、其他元素转为行内块元素 */
			span {
				width: 400px;
				height: 100px;
				background-color: #FF0000;
				display: inline-block;
			}
		</style>
	</head>
	<body>
		<a href="#">行内元素转块级元素，现在可以对我设置长宽，而且我现在独占一行！</a>
		<a href="#">行内元素转块级元素，现在可以对我设置长宽，而且我现在独占一行！</a>

		<div>块级元素转行内元素，现在对我设置长宽不管用哦，而且一行可以显示多个我</div>
		<div>块级元素转行内元素，现在对我设置长款不管用哦，而且一行可以显示多个我</div>
		<br />
		
		<span>
			span水平盒子(行内元素)转为行内块元素，我既可以设置长宽，一行也可以显示多个哦！
		</span>
		<span>
			span水平盒子(行内元素)转为行内块元素，我既可以设置长宽，一行也可以显示多个哦！
		</span>
	</body>
</html>

```



------

## 5. CSS背景(background)

**「1. 背景颜色」**

```
background-color: 颜色值;   默认的值是 transparent  透明的
```

**「2. 背景图片(image)」**

```
语法：
background-image : none | url (url) ;
例如:
background-image: url(images/1.png);
```

**「3. 背景平铺（repeat）」**

```
background-repeat : repeat | no-repeat | repeat-x | repeat-y 
```

| 参数      |                 作用                 |
| :-------- | :----------------------------------: |
| repeat    | 背景图像在纵向和横向上平铺（默认的） |
| no-repeat |            背景图像不平铺            |
| repeat-x  |         背景图像在横向上平铺         |
| repeat-y  |          背景图像在纵向平铺          |

**「4. 背景位置(position)」**

```
background-position : length || length
background-position : position || position 
```

| 参数     |                              值                              |
| :------- | :----------------------------------------------------------: |
| length   |         百分数 \| 由浮点数字和单位标识符组成的长度值         |
| position | top \| center \| bottom \| left \| center \| right  方位名词 |



**注意：**

- 必须先指定background-image属性
- position 后面是x坐标和y坐标。可以使用方位名词或者 精确单位。
- 如果指定两个值，两个值都是方位名字，则两个值前后顺序无关，比如left  top和top  left效果一致
- 如果只指定了一个方位名词，另一个值默认居中对齐。
- 如果position 后面是精确坐标， 那么第一个，肯定是 x 第二个一定是y
- 如果只指定一个数值,那该数值一定是x坐标，另一个默认垂直居中
- 如果指定的两个值是 精确单位和方位名字混合使用，则第一个值是x坐标，第二个值是y坐标



**背景简写：**

- background：属性的值的书写顺序官方没有强制的标准。为了可读性，建议如下写：
- background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;

```
/* 有背景图片背景颜色可以不用写*/
background: transparent url(image.jpg) repeat-y  scroll center top ;
```

**「5. 背景半透明(CSS3)」**

```
background: rgba(0, 0, 0, 0.3);
background: rgba(0, 0, 0, .3);
```

- 等同于background-color: rgba(0, 0, 0, .3)
- 最后一个参数是alpha 透明度 取值范围 0~1之间
- 我们习惯把0.3 的 0 省略掉 这样写 background: rgba(0, 0, 0, .3);
- 注意：背景半透明是指盒子背景半透明，盒子里面的内容不受影响
- 低于IE 9的版本不支持

### 5.1 盒子半透明 opacity

- 设置opacity元素的所有后代元素会随着一起具有透明性，一般用于调整图片或者模块的整体不透明度

```
opacity: .2;
```

**「6. 背景总结」**

| 属性                  | 作用             | 值                                                           |
| :-------------------- | :--------------- | :----------------------------------------------------------- |
| background-color      | 背景颜色         | 预定义的颜色值/十六进制/RGB代码                              |
| background-image      | 背景图片         | url(图片路径)                                                |
| background-repeat     | 是否平铺         | repeat/no-repeat/repeat-x/repeat-y                           |
| background-position   | 背景位置         | length/position   分别是x  和 y坐标， 切记 如果有 精确数值单位，则必须按照先X 后Y 的写法 |
| background-attachment | 背景固定还是滚动 | scroll/fixed                                                 |
| 背景简写              | 更简单           | 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;  他们没有顺序 |
| 背景透明              | 让盒子半透明     | background: rgba(0,0,0,0.3);  后面必须是 4个值               |



**示例：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>css背景</title>
		<style>
			div {
				width: 1200px;
				height: 700px;
				
				/* 
				background-image: url(../images/css.webp);
				background-repeat: no-repeat;
				background-color: cornsilk;
				background-position: center top;
				background-attachment: fixed; 		/* 背景图片固定位置，内容滚动！！ */ 
				
				
				/* 简写（顺序不做要求) */
				background: cornsilk url(../images/css.webp) no-repeat center top fixed;	
			}
			
			/* 背景半透明 */
			span {
				width: 1000px;
				height: 500px;
				background: rgba(0, 0, 0, .6) url(../images/tree03.png) no-repeat 20px center scroll;
				display: block;
			}
		</style>
	</head>
	<body>
		<div>
			
		</div>
		
		<span>
			半透明盒子
		</span>
	</body>
</html>

```



## 6. CSS三大特性

**「1. CSS 层叠性」**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827422.webp)

-`概念`：

- 所谓层叠性是指多种CSS样式的叠加
- 是浏览器处理冲突的一个能力,如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉

-`原则`：

- 样式冲突，遵循的原则是就近原则。 哪个样式离着结构近，就执行哪个样式。
- 样式不冲突，不会层叠。



**「2. CSS 继承性」**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827173.webp)-`概念`：

- 子标签会继承父标签的某些样式，如文本颜色和字号。
- 想要设置一个可继承的属性，只需将它应用于父元素即可。

-`注意`：

- 恰当地使用继承可以简化代码，降低CSS样式的复杂性。比如有很多子级孩子都需要某个样式，可以给父级指定一个，这些孩子继承过来就好了。
- 子元素可以继承父元素的样式（**text-，font-，line-这些元素开头的可以继承，以及color属性**）



**「3. CSS 优先级(CSS特殊性)」**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827343.webp)-`概念`：定义CSS样式时，经常出现两个或更多规则应用在同一元素上，此时，

- 选择器相同，则执行层叠性
- 选择器不同，就会出现优先级的问题。

-`权重计算公式`：

| 标签选择器               | 计算权重公式 |
| :----------------------- | :----------- |
| 继承或者 *               | 0,0,0,0      |
| 每个元素（标签选择器）   | 0,0,0,1      |
| 每个类，伪类             | 0,0,1,0      |
| 每个ID                   | 0,1,0,0      |
| 每个行内样式 style=""    | 1,0,0,0      |
| 每个!important  最重要的 | ∞ 无穷大     |

- 值从左到右，左面的最大，一级大于一级，数位之间没有进制，级别之间不可超越。
- 关于CSS权重，我们需要一套计算公式来去计算，这个就是 CSS Specificity（特殊性）
- div { color: pink !important; }

-`权重叠加`：

```
 div ul  li   ------>      0,0,0,3
 .nav ul li   ------>      0,0,1,2
 a:hover      -----—>      0,0,1,1
 .nav a       ------>      0,0,1,1
```

-`继承的权重是0`：

- 我们修改样式，一定要看该标签有没有被选中
- 如果选中了，那么以上面的公式来计权重。谁大听谁的。
- 如果没有选中，那么权重是0，因为继承的权重为0.



------

## 7. 盒子模型

css学习三大重点： css 盒子模型 、 浮动 、 定位  

**网页布局的本质**

- 首先利用CSS设置好盒子的大小，然后摆放盒子的位置。
- 最后把网页元素比如文字图片等等，放入盒子里面。

### 7.1 盒子模型(Box Model)

- 盒子模型就是把HTML页面中的布局元素看作是一个矩形的盒子，也就是一个盛装内容的容器。
- 盒子模型由元素的内容、边框（border）、内边距（padding）、和外边距（margin）组成。
- 盒子里面的文字和图片等元素是 内容区域
- 盒子的厚度 我们称为为盒子的边框
- 盒子内容与边框的距离是内边距
- 盒子与盒子之间的距离是外边距

**W3c标准盒子模型**

标准 w3c 盒子模型的范围包括 margin、border、padding、content

当设置为box-sizing: content-box;时，将采用标准模式解析计算，也是默认模式；

```
内盒尺寸计算(元素实际大小)
```

- 宽度：Element Height = content height + padding + border （Height为内容高度）
- 高度：Element  Width = content width + padding + border （Width为内容宽度）
- 盒子的实际大小：**内容的宽度和高度 +  内边距  +  边框**  ![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827162.webp)**IE盒子模型**

IE 盒子模型的 content 部分包含了 border 和 pading

当设置为box-sizing: border-box时，将采用怪异模式解析计算；

### 7.2 盒子边框(border)

| 属性         |          作用          |
| :----------- | :--------------------: |
| border-width | 定义边框粗细，单位是px |
| border-style |       边框的样式       |
| border-color |        边框颜色        |

**边框的样式：**

- none：没有边框即忽略所有边框的宽度（默认值）
- solid：边框为单实线(最为常用的)
- dashed：边框为虚线
- dotted：边框为点线

```
边框综合设置
border : border-width || border-style || border-color 

border: 1px solid red;  没有顺序要求  
```

**盒子边框写法总结表：**

很多情况下，我们不需要指定4个边框，我们是可以单独给4个边框分别指定的。

| 上边框                     | 下边框                        | 左边框                      | 右边框                       |
| :------------------------- | :---------------------------- | :-------------------------- | :--------------------------- |
| border-top-style:样式;     | border-bottom-style:样式;     | border-left-style:样式;     | border-right-style:样式;     |
| border-top-width:宽度;     | border- bottom-width:宽度;    | border-left-width:宽度;     | border-right-width:宽度;     |
| border-top-color:颜色;     | border- bottom-color:颜色;    | border-left-color:颜色;     | border-right-color:颜色;     |
| border-top:宽度 样式 颜色; | border-bottom:宽度 样式 颜色; | border-left:宽度 样式 颜色; | border-right:宽度 样式 颜色; |

**表格的细线边框：**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827514.webp)

- 通过表格的`cellspacing="0"`,将单元格与单元格之间的距离设置为0，

- 但是两个单元格之间的边框会出现重叠，从而使边框变粗

- 通过css属性：table{ border-collapse:collapse; }  

- - `collapse` 单词是合并的意思,`border-collapse: collapse;`表示相邻边框合并在一起。

```
<style>
 table {
  width: 500px;
  height: 300px;
  border: 1px solid red;
 }
 td {
  border: 1px solid red;
  text-align: center;
 }
 table, td {
  border-collapse: collapse;  /*合并相邻边框*/
 }
</style>
```

### 7.3 内边距(padding)

padding属性用于设置内边距。是指**边框与内容**之间的距离。

**设置**

| 属性           | 作用     |
| :------------- | :------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

**padding简写**

| 值的个数 | 表达意思                                        |
| :------- | :---------------------------------------------- |
| 1个值    | padding：上下左右内边距;                        |
| 2个值    | padding: 上下内边距   左右内边距 ；             |
| 3个值    | padding：上内边距  左右内边距  下内边距；       |
| 4个值    | padding: 上内边距 右内边距 下内边距 左内边距 ； |

当我们给盒子指定padding值之后， 发生了2件事情：

1. 内容和边框 有了距离，添加了内边距。
2. 盒子会变大

**解决措施：**通过给设置了宽高的盒子，减去相应的内边距的值，维持盒子原有的大小。

**padding不影响盒子大小情况：👉**如果没有给一个盒子指定宽度， 此时，如果给这个盒子指定padding， 则不会撑开盒子。

### 7.4 外边距（margin）

margin属性用于设置外边距。margin就是控制`盒子和盒子之间的距离`

**设置**

| 属性          | 作用     |
| :------------ | :------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

margin值的简写 （复合写法）代表意思  跟 padding 完全相同。

**块级盒子水平居中**

- 盒子必须指定宽度（width）
- 然后就给左右的外边距都设置为auto

实际工作中常用这种方式进行网页布局，示例代码如下：

```
.header  { width: 960px; margin: 0 auto;}
```

常见的写法，以下下三种都可以👇👇。

- margin-left: auto;  margin-right: auto;
- margin: auto;
- margin: 0 auto;

**文字居中和盒子居中区别👇👇**

1. 盒子内的文字水平居中是 text-align: center; 而且还可以让 行内元素和行内块居中对齐
2. 块级盒子水平居中  左右margin 改为 auto

**插入图片和背景图片区别👇👇**

1. `插入图片`我们用的最多 比如产品展示类  移动位置只能靠盒模型 padding margin
2. `背景图片`我们一般用于小图标背景或者超大背景图片、背景图片，移动位置只能通过  background-position

**清除元素的默认内外边距👇👇**

- 行内元素为了照顾兼容性,尽量只设置左右内外边距，不要设置上下内外边距。

```
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```

### 7.5 外边距合并

使用margin定义块元素的**「垂直外边距」**时，可能会出现外边距的合并。

**(1). 相邻块元素垂直外边距的合并**

- 当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom
- 下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和
- **「取两个值中的较大者」**这种现象被称为相邻块元素垂直外边距的合并（也称外边距塌陷）。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827515.webp)**「解决方案：尽量给只给一个盒子添加margin值」**。

**(2). 嵌套块元素垂直外边距的合并（塌陷）**

- 对于两个嵌套关系的块元素，如果父元素没有上内边距及边框
- 父元素的上外边距会与子元素的上外边距发生合并
- 合并后的外边距为两者中的较大者

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827453.webp)**「解决方案：」**

1. 可以为父元素定义上边框。
2. 可以为父元素定义上内边距
3. 可以为父元素添加overflow: hidden。

还有其他方法，比如浮动、固定、绝对定位的盒子不会有问题，后面咱们再总结。。。

**盒子模型布局稳定性**

优先使用  宽度 （width）  其次 使用内边距（padding）   再次  外边距（margin）

```
width >  padding  >   margin   
```

**原因：**

- margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。
- padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。
- width  没有问题（嗨皮）我们经常使用宽度剩余法 高度剩余法来做。



### 7.6 CSS3 新增

```
圆角边框：
border-radius:length;

border-top-left-radius   定义了左上角的弧度
border-top-right-radius   定义了右上角的弧度
border-bottom-right-radius   定义了右下角的弧度
border-bottom-left-radius   定义了左下角的弧度
```

- 其中每一个值可以为 数值或百分比的形式。
- 技巧：让一个正方形 变成圆圈

```
border-radius: 50%;
```

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251827806.webp)如果要在四个角上一一指定，可以使用以下规则👇👇：

```
border-radius: 左上角 右上角  右下角  左下角;
```

1. 四个值: 第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角。
2. 三个值: 第一个值为左上角, 第二个值为右上角和左下角，第三个值为右下角
3. 两个值: 第一个值为左上角与右下角，第二个值为右上角与左下角
4. 一个值：四个圆角值相同



**盒子阴影**

```
盒子阴影(box-shadow)：
box-shadow: offset-x offset-y [blur [spread]] [color] [inset]
```

| 值       | 描述                                           |
| :------- | :--------------------------------------------- |
| offset-x | 阴影的水平偏移量。正数向右偏移，负数向左偏移。 |
| offset-y | 阴影的垂直偏移量。正数向下偏移，负数向上偏移。 |
| blur     | 可选。阴影模糊距离，不能取负数。               |
| spread   | 可选。阴影大小                                 |
| color    | 可选。阴影的颜色                               |
| inset    | 可选。表示添加内阴影，默认为外阴影             |

```css
div {
   width: 200px;
   height: 200px;
   border: 10px solid red;
   /* box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);  */
   /* box-shadow:水平位置 垂直位置 模糊距离 阴影尺寸（影子大小） 阴影颜色  内/外阴影； */
   box-shadow: 0 15px 30px  rgba(0, 0, 0, .4);   
}
```



**示例：**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>圆角边框 阴影</title>
		<style>
			div {
				width: 200px;
				height: 120px;
				background-color: #FF7F50;
				border-radius: 10px;
				
				/* 正方形改原型：border-radius: 50%; */
			}
			div:hover {
				box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);
			}
		</style>
	</head>
	<body>
		<div></div>
	</body>
</html>
```



## 相关案例

1 新闻页面

![image-20220119163648949](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823127.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>新闻页面</title>
		<style>
            		body {
                                    font: 16px/28px "microsoft yahei";
                                }
                                h1 {
                                    text-align: center;
                                    font-weight: normal;
                                }
                                p {
                                    text-indent: 2em;
                                }
                                a {
                                    text-decoration: none;
                                }
                                .gray {
                                    color: #888888;
                                    font-size: 12px;
                                    text-align: center;
                                }
                                .search {
                                    width: 173px;
                                    color: #666666;
                                }
                                .btn {
                                    font-weight: 700;
                                }
                                .pic {
                                    text-align: center;
                                }
                                .foot {
                                    color: #888888;
                                    font-size: 12px;
                                }
        	</style>
	</head>
	<body>
		<h1>北方高温明日达鼎盛 京津冀多地地表温度将超60℃</h1>
		    <div class="gray">2019-07-03 16:31:47 来源: <a href="http://www.baidu.com" target="_blank" width="400">中国天气网 </a><input type="text" value="请输入查询条件..." class="search"> <button class="btn">搜索</button>
		    </div>
		    <hr>
		    <p>中国天气网讯 今天（3日），华北、黄淮多地出现高温天气，截至下午2点，北京、天津、郑州等地气温突破35℃。预报显示，今后三天（3-5日），这一带的高温天气将继续发酵，高温范围以及强度将在4日达到鼎盛，预计北京、天津、石家庄、济南等地明天的最高气温有望突破38℃，其中北京和石家庄的最高气温还有望创今年以来的新高。</p>
		    <h4>气温41.4℃！地温66.5！北京强势迎七月首个高温日</h4>
		    <p class="pic"><img src="images/pic.jpeg" height="300"></p>
		    <p>今天，华北、黄淮一带的高温持续发酵，截至今天下午2点，陕西北部、山西西南部、河北南部、北京、天津、山东西部、河南北部最高气温已普遍超过35℃。大城市中，北京、天津、郑州均迎来高温日。</p>
		
		    <p>在阳光暴晒下，地表温度也逐渐走高。今天下午2点，华北黄淮大部地区的地表温度都在50℃以上，部分地区地表温度甚至超过60℃。其中，河北衡水地表温度高达68.3℃，天津站和北京站附近的地表温度分别高达66.6℃和66.5℃。</p>
		    <h4>明日热度再升级！京津冀携手冲击38℃+</h4>
		    <p>中国天气网气象分析师王伟跃介绍，明天（4日），华北、黄淮地区35℃以上的高温天气还将继续升级，并进入鼎盛阶段，高温强度和范围都将发展到最强。 明天，北京南部、天津大部、河北中部和南部、山东中部和西部、山西南部局地、河南北部、东北部分地区的最高气温都将达到或超过35℃。</p>
		    <p>不过，专家提醒，济南被雨水天气完美绕开，因此未来一周，当地的高温还会天天上岗。在此提醒当地居民注意防暑降温，防范持续高温带来的各种不利影响。（文/张慧 数据支持/王伟跃 胡啸 审核/刘文静 张方丽）</p>
		    <p class="foot">本文来源：中国天气网 责任编辑：刘京_NO5631</p>
	</body>
</html>

```





2 小米商城侧边栏

![image-20220119163843783](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823128.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>小米商城侧边栏</title>
		<style>
			a {
				width: 200px;
				height: 40px;
				line-height: 40px;		/* 文字行高等于盒子高度，即可实现文字居中 */
				font-family: "microsoft yahei";
				color: #FFFFFF;
				background-color: #666666;
				display: block;
				text-decoration: none;
				padding-left: 30px;
			}
			a:hover {
				background-color: coral;
			}
		</style>
	</head>
	<body>
		<a href="#">手机 电话卡</a>
		<a href="#">电视 盒子</a>
		<a href="#">笔记本 平板</a>
		<a href="#">出行 穿戴</a>
		<a href="#">智能 路由器</a>
		<a href="#">健康 儿童</a>
		<a href="#">耳机 音响</a>
	</body>
</html>

```



3 产品模块

![image-20220119163939519](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823129.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>产品模块</title>
		<style>
			* {
				margin: 0;
				padding: 0;
			}
			body {
				background-color: #f5f5f5;
			}
			.box {
				width: 330px;
				height: 590px;
				background-color: #fff;
				margin: 100px auto;
			}
			.box img {
				/* 图片的长宽和父亲一样 */
				width: 100%;
			}
			.review {
				height: 70px;
				font-size: 14px;
				/* 因为这个段落没有width属性，所以padding不会撑开盒子的宽度 */
				padding: 0 28px;
				margin-top: 30px;
			}
			.appraise {
				font-size: 12px;
				color: #b0b0b0;
				padding: 0 28px;
				margin-top: 5px;
			}
			.info {
				font-size: 14px;
				margin-top: 15px;
				padding: 0 28px;
			}
			.info h4{
				display: inline-block;
				font-weight: 400;
			}
			.info span {
				color: #ff6700;
			}
			.info em {
				font-style: normal;
				color: #ebe4e0;
				margin: 0 6px 0 15px;
			}
			.info a {
				text-decoration: none;
				color: #000000;
			}
			.info a:hover{
				color: #FF7F50;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<img src="../images/xiaomi.webp" alt="小米Sound"  >
			<p class="review">快递牛，整体不错，音质非常棒。小米给力！</p>
			<div class="appraise">来自于 177832921 的评价</div>
			<div class="info">
				<h4><a href="#" target="_blank">Sound殿堂级调音大师调教...</a></h4>
				<em>|</em>
				<span> 459.99元</span>
			</div>
		</div>
	</body>
</html>

```



4 五彩导航栏

![image-20220119164036228](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823130.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>五彩导航栏</title>
		<style>
			.nav a {
				display: inline-block;
				width: 120px;
				height: 58px;
				background-color: pink;
				text-align: center;
				line-height: 58px;
				text-decoration: none;
				color: #FFF8DC;
			}

			.nav .bg1 {
				background: url(../images/bg1.png) no-repeat;
			}

			.nav .bg1:hover {
				background-image: url(../images/bg11.png);
			}

			.nav .bg2 {
				background: url(../images/bg3.png) no-repeat;
			}

			.nav .bg2:hover {
				background-image: url(../images/bg33.png);
			}
		</style>
	</head>
	<body>
		<div class="nav">
			<a href="#" class="bg1">五彩导航</a>
			<a href="#" class="bg2">五彩导航</a>
			<a href="#" class="bg3">五彩导航</a>
			<a href="#" class="bg4">五彩导航</a>
		</div>
	</body>
</html>

```



5 商品快报

![image-20220119164125830](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251823131.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>商品快报</title>
		<style>
			* {
				margin: 0;
				padding: 0;
			}
			.box {
				width: 248px;
				height: 163px;
				border: 2px solid #B0B0B0;
				margin: 100px auto;
			}
			.one {
				height: 32px;
				border-bottom: 2px dotted #B0B0B0;
				font-size: 14px;
				font-weight: 400;
				line-height: 32px;
				padding-left: 15px;
			}
			li {
				list-style: none;
			}
			.box ul li a {
				font-size: 12px;
				color: #666;
				text-decoration: none;
			}
			.box ul li a:hover {
				text-decoration: underline;
			}
			.box ul li {
				height: 23px;
				line-height: 23px;
				padding-left: 20px;
			}
			.box ul {
				margin-top: 7px;
			}
		</style>
	</head>
	<body>
		<div class="box">
			<h3 class="one">品优购快报</h3>
			<ul>
				<li><a href="#" >【特惠】爆款耳机5折秒杀！</li>
				<li><a href="#" >【特惠】9.9元100张照片！</li>
				<li><a href="#" >【特惠】爆款耳机、音响5折秒杀！</li>
				<li><a href="#" >【特惠】长虹智能空调立省1000！</li>
				<li><a href="#" >【特惠】爆款耳机5折秒杀！</li>
			</ul>
		</div>
	</body>
</html>

```



## 8. 浮动

### 8.1 浮动

**「1. CSS布局的三种机制」**

> 网页布局的核心——就是**用CSS来摆放盒子**。

CSS 提供了3种机制来设置盒子的摆放位置，分别是普通流（标准流）、浮动和定位，其中：

**A. 普通流（标准流）**

- 块级元素会独占一行，从上向下顺序排列；

- - 常用元素：div、hr、p、h1~h6、ul、ol、dl、form、table

- 行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行；

- - 常用元素：span、a、i、em等

**B. 浮动**

- 让盒子从普通流中浮起来,主要作用让多个块级盒子一行显示。

**C. 定位**

- 将盒子定在浏览器的某一个位置——CSS 离不开定位，特别是后面的 js 特效。

**「2. 什么是浮动」**元素的浮动是指设置了浮动属性的元素会

- 脱离标准普通流的控制,不占位置，脱标
- 移动到指定位置。

**作用**

1. 让多个盒子(div)水平排列成一行，使得浮动称为布局的重要手段。
2. 可以实现盒子的左右对齐等等。
3. 浮动最早是用来控制图片，实现文字环绕图片效果。
4. float属性会改变元素的display属性，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。生成的块级框和我们前面的行内块极其相似。

**语法**

```
选择器 { float: 属性值; }
```

| 属性值 | 描述                 |
| :----- | :------------------- |
| none   | 元素不浮动（默认值） |
| left   | 元素向左浮动         |
| right  | 元素向右浮动         |



> 浮动只会影响当前的或者是后面的标准流盒子，不会影响前面的标准流。
> **建议:**如果一个盒子里面有多个子盒子，如果其中一个盒子浮动了，其他兄弟也应该浮动。防止引起问题

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828302.webp)

**浮动(float)小结**

| 特点 | 说明                                                         |
| :--- | :----------------------------------------------------------- |
| 浮   | 加了浮动的盒子**「是浮起来」**的，漂浮在其他标准流盒子的上面。 |
| 漏   | 加了浮动的盒子**「是不占位置的」**，它原来的位置**「漏给了标准流的盒子」**。 |
| 特   | **「特别注意」**：浮动元素会改变display属性， 类似转换为了行内块，但是元素之间没有空白缝隙 |

### 8.2 清除浮动

因为父级盒子很多情况下，不方便给高度，但是子盒子浮动就不占有位置，最后父级盒子高度为0，就影响了下面的标准流盒子。![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828388.webp)![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828283.webp)**总结：**

- 由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响
- 准确地说，并不是清除浮动，而是清除浮动后造成的影响

**清除浮动本质**清除浮动主要为了解决父级元素因为子级浮动引起内部高度为0 的问题。清除浮动之后， 父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

#### 8.2.1 清除浮动的方法

```
选择器 { clear: 属性值; }   clear 清除  
```

| 属性值 | 描述                                       |
| :----- | :----------------------------------------- |
| left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
| right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
| both   | 同时清除左右两侧浮动的影响                 |

实际工作中,几乎只用clear: both

**1).额外标签法(隔墙法)**

是W3C推荐的做法是通过在浮动元素末尾添加一个空的标签例如 <div style=”clear:both”></div>，或则其他标签br等亦可。

- 优点：通俗易懂，书写方便
- 缺点：添加许多无意义的标签，结构化较差。

**2).父级添加overflow属性方法**

```
可以给父级添加： overflow为 hidden| auto| scroll  都可以实现。
```

- 优点： 代码简洁
- 缺点： 内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。

**3).使用after伪元素清除浮动**:after 方式为空元素额外标签法的升级版，好处是不用单独加标签了

```
    .clearfix:after {
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
  
    /* IE6、7 专有 */
    .clearfix {
        *zoom: 1;
    }
        
```

- 优点：符合闭合浮动思想  结构语义化正确
- 缺点：由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

**4).使用双伪元素清除浮动**

```
    .clearfix:before,
    .clearfix:after {
        content: "";
        display: table;
    }

    .clearfix:after {
        clear: both;
    }

    .clearfix {
       *zoom: 1;
    }
```

- 优点： 代码更简洁
- 缺点： 由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

**清除浮动总结**

```
什么时候用清除浮动呢？
```

1. 父级没高度
2. 子盒子浮动了
3. 影响下面布局了，我们就应该清除浮动了。



| 清除浮动的方式       | 优点               | 缺点                               |
| :------------------- | :----------------- | :--------------------------------- |
| 额外标签法（隔墙法） | 通俗易懂，书写方便 | 添加许多无意义的标签，结构化较差。 |
| 父级overflow:hidden; | 书写简单           | 溢出隐藏                           |
| 父级after伪元素      | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |
| 父级双伪元素         | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |

### 8.3 CSS属性书写顺序

建议遵循以下顺序：

1. 布局定位属性：display / position / float / clear / visibility / overflow（建议 display 第一个写，毕竟关系到模式）
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

```
.jdc {
    display: block;
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    margin: 0 10px;
    padding: 20px 0;
    font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
    color: #333;
    background: rgba(0,0,0,.5);
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```



------

## 9. 定位(position)

**「1. 定位详解」**

将盒子**「定」**在某一个**「位」**置  自由的漂浮在其他盒子(包括标准流和浮动)的上面。

所以，我们脑海应该有三种布局机制的上下顺序👇👇
标准流在最底层 (海底)  -------   浮动 的盒子 在 中间层  (海面)  -------  定位的盒子 在 最上层  （天空）

**定位**是用来布局的，它有两部分组成：定位 = 定位模式 + 边偏移在 CSS 中，通过 `top`、`bottom`、`left` 和 `right` 属性定义元素的**「边偏移」**：（方位名词）

| 边偏移属性 | 示例           | 描述                                                         |
| :--------- | :------------- | :----------------------------------------------------------- |
| `top`      | `top: 80px`    | **「顶端」**偏移量，定义元素相对于其父元素**「上边线的距离」**。 |
| `bottom`   | `bottom: 80px` | **「底部」**偏移量，定义元素相对于其父元素**「下边线的距离」**。 |
| `left`     | `left: 80px`   | **「左侧」**偏移量，定义元素相对于其父元素**「左边线的距离」**。 |
| `right`    | `right: 80px`  | **「右侧」**偏移量，定义元素相对于其父元素**「右边线的距离」** |

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828366.webp)

**「2. 定位模式(position)」**在 CSS 中，通过 `position` 属性定义元素的**「定位模式」**，语法如下：

```
选择器 { position: 属性值; }
```

| 值         |       语义       |
| :--------- | :--------------: |
| `static`   | **「静态」**定位 |
| `relative` | **「相对」**定位 |
| `absolute` | **「绝对」**定位 |
| `fixed`    | **「固定」**定位 |

**「3. 静态定位(static)」**

- 静态定位是元素的默认定位方式，无定位的意思。它相当于border里面的none，不要定位的时候用。
- 静态定位 按照标准流特性摆放位置。它没有边偏移。
- 静态定位在布局时几乎不用

**「4. 相对定位(relative)」**

- 相对定位是元素相对于它原来在标准流中的位置来说的。![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828569.webp)
- 相对于自己原来在标准流中位置来移动的
- 原来在标准流的区域继续占有，后面的盒子仍然以标准流的方式对待它。

**「5. 绝对定位(absolute)」**

绝对定位是元素以带有定位的父级元素来移动位置

- 完全脱表--完全不占位置；
- 父元素没有定位，则以浏览器为准定位(Document文档)。

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828774.webp)

**父元素有定位**

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828755.webp)

**定位口诀--子绝父相**

**「6. 固定定位(fixed)」**

固定定位是绝对定位的一种特殊形式;

- 完全脱标--完全不占位置；

- 只认**浏览器的可视窗口**--浏览器可视窗口+边偏移属性来设置元素的位置

- - 跟父元素没有任何关系；单独使用
  - 不随滚动条滚动

### 9.1 定位(position)的扩展

#### 9.1.1 绝对定位的盒子居中

> 绝对定位/固定定位的盒子不能通过设置margin: auto设置水平居中 在使用绝对定位时要向实现水平居中，可以按照下面的方法：

![图片](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202203251828199.webp)

1. left : 50%:让盒子的左侧移动到父级元素的水平中心位置；
2. margin-left: -100px;让盒子向左移动自身宽度的一半。
3. 同理垂直居中。

#### 9.1.2 堆叠顺序（z-index）

在使用**「定位」**布局时，可能会**「出现盒子重叠的情况」**。

加了定位的盒子，默认**「后来者居上」**， 后面的盒子会压住前面的盒子。

应用 `z-index` 层叠等级属性可以**「调整盒子的堆叠顺序」**。如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8beCsHfDxy5VBkWU3ET4s4qtBpHm9iajaajxo2GeWWb81a04PfG5rmH0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)z-index的特性如下:

1. **属性值**：正整数、负整数或 0，默认值是 0，数值越大，盒子越靠上；
2. 如果属性值相同，则按照书写顺序，后来居上；
3. 数字后面不能加单位
4. z-index只能用于相对定位、绝对定位和固定定位的元素，其他标准流、浮动和静态定位无效。

#### 9.1.3 定位改变display属性

前面提过， display 是 显示模式， 可以通过以下方式改变显示模式:

- 可以用inline-block  转换为行内块
- 可以用浮动 float 默认转换为行内块（类似，并不完全一样，因为浮动是脱标的）
- 绝对定位和固定定位也和浮动类似， 默认转换的特性 转换为行内块。

所以说， 一个行内的盒子，如果加了**「浮动」**、**「固定定位」**和**「绝对定位」**，不用转换，就可以给这个盒子直接设置宽度和高度等。

#### 9.1.4 定位小结

| 定位模式         | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| :--------------- | :------------------- | :--------------------- | :----------------- | :----------------------- |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |

**注意：**

1. `边偏移` 需要和 `定位模式` 联合使用，`单独使用无效`；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。
