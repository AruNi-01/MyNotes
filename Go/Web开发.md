# Gin

## Gin 介绍

Gin 是一个用 Go (Golang) 编写的 HTTP Web 框架。 它具有类似 Martini 的 API，但性能比 Martini 快 40 倍。如果你需要极好的性能，使用 Gin 吧。



## 快速开始

### 安装

下载并安装`Gin`:

```bash
go get -u github.com/gin-gonic/gin
```

### 第一个Gin示例：

```golang
package main

import "github.com/gin-gonic/gin"

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 请求方式：GET；请求路径：/hello
	r.GET("/hello", func(c *gin.Context) {
		// c.JSON：返回JSON格式的数据
		// H 是 map[string]interface{} 的快捷方式
		c.JSON(200, gin.H{
			"message": "hello golang",
		})
	})
	r.Run()
}
```

运行代码，在浏览器输入`localhost:8080/hello`，可以看到返回的JSON字符串：

![image-20220508200149545](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3ac97ae4c30cc3f535e1f0fbc9263fc2.png)

控制台也会有对应的信息：

![image-20220508200225650](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/141a679999375910e17789fda7c27999.png)

## Gin 渲染

### JSON 渲染

```go
func main() {
	r := gin.Default()

	// gin.H 是map[string]interface{}的缩写
	r.GET("/someJSON", func(c *gin.Context) {
		// 方式一：自己拼接JSON
		c.JSON(http.StatusOK, gin.H{"message": "Hello world!"})
	})
	r.GET("/moreJSON", func(c *gin.Context) {
		// 方法二：使用结构体
		var msg struct {
			Name    string `json:"user"`
			Message string
			Age     int
		}
		msg.Name = "小王子"
		msg.Message = "Hello world!"
		msg.Age = 18
		c.JSON(http.StatusOK, msg)
	})
	r.Run(":8080")
}
```



### HTML 渲染

我们首先定义一个存放模板文件的`templates`文件夹，然后在其内部按照业务分别定义一个`posts`文件夹和一个`users`文件夹。

 `posts/index.html`文件的内容如下，注意添加 define 和 end 来指定页面的位置：

```html
{{define "posts/index.html"}}
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>posts/index</title>
</head>
<body>
    {{.title}}
</body>
</html>
{{end}}
```

`users/index.html`文件的内容如下：

```html
{{define "users/index.html"}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>users/index</title>
</head>
<body>
    {{.title}}
</body>
</html>
{{end}}
```

Gin框架中使用`LoadHTMLGlob()`或者`LoadHTMLFiles()`方法进行HTML模板解析。

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	r := gin.Default()

	// 对HTML模板进行解析
	r.LoadHTMLGlob("templates/**/*")
	//r.LoadHTMLFiles("templates/posts/index.html", "templates/users/index.html")

	// GET 请求/posts/index  就返回posts/index.html这个页面并渲染数据
	r.GET("/posts/index", func(c *gin.Context) {
		// HTTP请求
		c.HTML(http.StatusOK, "posts/index.html", gin.H{ // 模板渲染
			"title": "posts/index",
		})
	})

	r.GET("/users/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "users/index.html", gin.H{
			"title": "users/index",
		})
	})

	r.Run(":8080")
}
```

启动项目，访问对应的路径：

![image-20220509185554444](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0b0ab95817c8b26b91c21760c9de34fa.png)



### 自定义模板函数

如果我们返回的数据是HTML标签相关的代码，HTML渲染模板会直接将代码转义成数据直接输出。但是我们想让它在模板中不被转义成数据，而是将HTML标签直接交给浏览器渲染，此时就可以自定义模板函数。

在上面的示例中进行修改，让路径 `/users/index` 返回一段HTML标签，再定义自定义模板：

```go
func main() {
	r := gin.Default()

	// 自定义模板函数，加载到模板里
	// FuncMap是装函数的容器，safe是函数在模板里的名字
	r.SetFuncMap(template.FuncMap{
		"safe": func(str string) template.HTML {
			return template.HTML(str)
		},
	})

	// 对HTML模板进行解析
	r.LoadHTMLGlob("templates/**/*")
	//r.LoadHTMLFiles("templates/posts/index.html", "templates/users/index.html")

	// GET 请求/posts/index  就返回posts/index.html这个页面并渲染数据
	r.GET("/posts/index", func(c *gin.Context) {
		// HTTP请求
		c.HTML(http.StatusOK, "posts/index.html", gin.H{ // 模板渲染
			"title": "posts/index",
		})
	})

	r.GET("/users/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "users/index.html", gin.H{
			"title": "<a href='https://baidu.com'>百度一下</a>",
		})
	})

	r.Run(":8080")
}
```

在`users/index.html`中使用定义好的`safe`模板函数：

```html
{{ define "users/index.html" }}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>users/index</title>
</head>
<body>
    {{ .title | safe }}
</body>
</html>
{{ end }}
```

启动项目测试：

![image-20220509191950905](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b3f0a55cd0f9fc8bbce181556a0206e8.png)



### 静态文件处理

当我们渲染的HTML文件中引用了静态文件时，我们只需要按照以下方式在渲染页面前调用`gin.Static`方法即可。

在当前项目新建 static 目录，创建 `index.css` ：

```css
body {
    background: green;
}
```

将上面示例添加Static方法：

```go
func main() {
	r := gin.Default()
        // ...
    
	// 静态文件处理，/static为引用时使用的文件名， ./static为存放css文件的目录
	r.Static("/static", "./static")
    
        // ...
	r.Run(":8080")
}
```

在html页面中引入对应的css文件即可：

```html
{{ define "users/index.html" }}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>users/index</title>
    <!-- 引入css -->
    <link rel="stylesheet" href="/static/index.css">
</head>
<body>
    {{ .title | safe }}
</body>
</html>
{{ end }}
```

启动项目，发现css样式成功加载出来了。



### 使用模板继承

Gin框架默认都是使用单模板，如果需要使用`block template`功能，可以通过`"github.com/gin-contrib/multitemplate"`库实现，具体示例如下：

首先，假设我们项目目录下的templates文件夹下有以下模板文件，其中`home.tmpl`和`index.tmpl`继承了`base.tmpl`：

```bash
templates
├── includes
│   ├── home.tmpl
│   └── index.tmpl
├── layouts
│   └── base.tmpl
└── scripts.tmpl
```

然后我们定义一个`loadTemplates`函数如下：

```go
func loadTemplates(templatesDir string) multitemplate.Renderer {
	r := multitemplate.NewRenderer()
	layouts, err := filepath.Glob(templatesDir + "/layouts/*.tmpl")
	if err != nil {
		panic(err.Error())
	}
	includes, err := filepath.Glob(templatesDir + "/includes/*.tmpl")
	if err != nil {
		panic(err.Error())
	}
	// 为layouts/和includes/目录生成 templates map
	for _, include := range includes {
		layoutCopy := make([]string, len(layouts))
		copy(layoutCopy, layouts)
		files := append(layoutCopy, include)
		r.AddFromFiles(filepath.Base(include), files...)
	}
	return r
}
```

我们在`main`函数中

```go
func indexFunc(c *gin.Context){
	c.HTML(http.StatusOK, "index.tmpl", nil)
}

func homeFunc(c *gin.Context){
	c.HTML(http.StatusOK, "home.tmpl", nil)
}

func main(){
	r := gin.Default()
	r.HTMLRender = loadTemplates("./templates")
	r.GET("/index", indexFunc)
	r.GET("/home", homeFunc)
	r.Run()
}
```



## 获取参数

### querystring 参数

`querystring`指的是URL中`?`后面携带的参数，例如：`/user/search?username=小王子&address=沙河`。 

获取请求的querystring参数的方法有：`gin.Query()`、`gin.DefaultQuery()`、`gin.GetQuery()`。

示例：

```go
func main() {
	r := gin.Default()
	r.GET("/user/search", func(c *gin.Context) {
		// 获取浏览器请求中携带的 querystring 参数

		// 1.Query: 参数为key，返回用户输入的值
		//username := c.Query("username")
		address := c.Query("address")

		// 2.DefaultQuery：若querystring的key不是username，则返回默认值“游客”
		//username := c.DefaultQuery("username", "游客")

		// 3. GetQuery：返回两个值，若querystring的key不是username，则第二个返回值为false
		username, ok := c.GetQuery("username")
		if !ok {
			username = "游客"
		}

		// 获取到参数后使用JSON格式输出参数
		c.JSON(http.StatusOK, gin.H{
			"username": username,
			"address":  address,
		})
	})
	r.Run(":8080")
}
```

测试：

![image-20220509200714666](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6c79cc0095c261e08dd12956c41048b1.png)



### form 参数

当前端请求的数据通过form表单提交时，获取请求数据的方法：

- `gin.PostForm()`
- `gin.DefaultPostForm()`

示例：

`login.html` 页面：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>form</title>
</head>
<body>

<form action="/use/login" method="post">

    <div>
        <label for="username">username:</label>
        <input type="text" name="username" id="username">
    </div>
    <div>
        <label for="password">password:</label>
        <input type="text" name="password" id="password">
    </div>
    <div>
        <input type="submit" value="Login">
    </div>
</form>

</body>
</html>
```

`index.html` 页面：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
<h1>Hello, {{ .username }}</h1>
<p>Your password: {{ .password }}</p>
</body>
</html>
```



go 程序：

```go
func main() {
	r := gin.Default()
	// 解析html
	r.LoadHTMLFiles("./login.html", "./index.html")

	// GET /login 返回login.html页面
	r.GET("/login", func(c *gin.Context) {
		c.HTML(http.StatusOK, "login.html", nil)
	})

	// POST /login 从form表单获取的数据，返回到index.html中渲染
	r.POST("/login", func(c *gin.Context) {

		// 1.PostForm：参数key对应html页面的name值
		username := c.PostForm("username")
		password := c.PostForm("password")

		// DefaultPostForm取不到值时会返回指定的默认值
		//username := c.DefaultPostForm("username", "游客")

		c.HTML(http.StatusOK, "index.html", gin.H{
			"username": username,
			"password": password,
		})
	})
	r.Run(":8080")
}
```

启动项目，访问`/login`：

![image-20220509203308939](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dcd54ecd30df19d8589eec15e0fbf420.png)



### json 参数

当前端请求的数据通过 JSON 提交时，例如向 `/json` 发送一个POST请求，则获取请求参数的方式如下：

```go
r.POST("/json", func(c *gin.Context) {
	// 注意：下面为了举例子方便，暂时忽略了错误处理
	b, _ := c.GetRawData()  // 从c.Request.Body读取请求数据
	// 定义map或结构体
	var m map[string]interface{}
	// 反序列化
	_ = json.Unmarshal(b, &m)

	c.JSON(http.StatusOK, m)
})
```

更便利的获取请求参数的方式，参见下面 **参数绑定** 。



### path 参数

请求的参数通过URL路径传递，例如：`/user/search/杨超越/18`。 获取请求URL路径中的参数的方法：

- `gin.Param()`

需要在GET请求路径中使用 ` :` 表示要传入的参数名。

示例：

```go
func main() {
	r := gin.Default()
	r.GET("/user/search/:username/:age", func(c *gin.Context) {
		username := c.Param("username")
		age := c.Param("age")

		c.JSON(http.StatusOK, gin.H{
			"username": username,
			"age":      age,
		})
	})
	r.Run(":8080")
}
```

测试：

![image-20220509204007816](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dbc4e90fc96b10d4a63737d468c8535c.png)



### 参数绑定

为了能够更方便的获取请求相关参数，提高开发效率，我们可以基于请求的`Content-Type`识别请求数据类型并利用反射机制自动提取请求中`QueryString`、`form表单`、`JSON`、`XML`等参数到结构体中。 

`ShouldBind`会按照下面的顺序解析请求中的数据完成绑定：

1. 如果是 `GET` 请求，只使用 `Form` 绑定引擎（`querystring`）。
2. 如果是 `POST` 请求，首先检查 `content-type` 是否为 `JSON` 或 `XML`，然后再使用 `Form`（`form-data`）。

下面的示例代码演示了`.ShouldBind()`强大的功能，它能够基于请求自动提取`JSON`、`form表单`和`QueryString`类型的数据，并把值绑定到指定的结构体对象。

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

//User Binding from JSON and form
type User struct {
	Username string `json:"username" form:"username"`
	Password string `json:"password" form:"password"`
}

func main() {
	r := gin.Default()

	// 绑定JSON ({"username": "AruNi", "password": "123456"})
	r.POST("/loginJSON", func(c *gin.Context) {
		var user User
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&user); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"username": user.Username,
				"password": user.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// 绑定form表单 (username=AruNi&&password=123456)
	r.POST("/loginForm", func(c *gin.Context) {
		var user User
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&user); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"username": user.Username,
				"password": user.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// 绑定querystring (/loginQuery?username=AruNi&&password=123456)
	r.GET("/loginQuery", func(c *gin.Context) {
		var user User
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&user); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"username": user.Username,
				"password": user.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	r.Run(":8080")
}
```



JSON 测试：

![image-20220510110942941](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/393e8c4f016a7fbcb3b90715c941c6ca.png)

form 表单测试：

![image-20220510111457824](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/de341c00f61de58c7fc1d1384b82e8d8.png)

querystring 测试：

![image-20220510111312145](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f15adafcd8c91afa0649b7cdddb33c8f.png)



## 文件上传

### 单个文件上传

html 代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
  <!--multipart/form-data：不对字符编码。在使用包含文件上传控件的表单时，必须使用该值-->
  <form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="f1">
    <input type="submit" value="上传">
  </form>
</body>
</html>
```

go 代码：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
	"path"
)

func main() {
	r := gin.Default()

        // 处理multipart forms提交文件时默认的内存限制是32 MiB
	// 可以通过下面的方式修改
	// r.MaxMultipartMemory = 8 << 20  // 8 MiB
    
	r.LoadHTMLFiles("./index.html")
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)
	})

	r.POST("/upload", func(c *gin.Context) {
		// 从请求中读取文件
		f, err := c.FormFile("f1")
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		} else {
			// 文件路径：用Sprintf拼接，路径为./Filename
			//filePath := fmt.Sprintf("./%s", f.Filename)
			// 用path包下的Join也可以拼接路径
			filePath := path.Join("./", f.Filename)

			// 将读取到的文件保存到本地
			// SaveUploadedFile：参数1 保存的文件，参数2 目的地
			c.SaveUploadedFile(f, filePath)
			c.JSON(http.StatusOK, gin.H{"status": "OK"})
		}
	})

	r.Run(":8080")
}
```

测试：

![image-20220510114047523](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/83f8166f405f3919db62561af8481e4e.png)

上传成功后，当前项目目录中发现已上传的文件：

![image-20220510114130027](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/03ec9baab508c97b10e3d495bec3cd3d.png)

### 多个文件上传

```go
func main() {
	router := gin.Default()

	router.POST("/upload", func(c *gin.Context) {
		// Multipart form
		form, _ := c.MultipartForm()
        	// 获取html表单中所有name为file的文件，File是一个map
		files := form.File["file"]

        	// 循环去保存files中的文件，因为files是一个map，所以可以获取索引index
		for index, file := range files {
			log.Println(file.Filename)
			dst := fmt.Sprintf("C:/tmp/%s_%d", file.Filename, index)
			// 上传文件到指定的目录
			c.SaveUploadedFile(file, dst)
		}
		c.JSON(http.StatusOK, gin.H{
			"message": fmt.Sprintf("%d files uploaded!", len(files)),
		})
	})
	router.Run()
}
```



## 重定向与转发

### HTTP 重定向

HTTP 重定向很容易。 内部、外部重定向均支持。

```go
func main() {
	r := gin.Default()
	// HTTP重定向：url输入/index 重定向到百度
	r.GET("/index", func(c *gin.Context) {
		c.Redirect(http.StatusMovedPermanently, "https://www.baidu.com")
	})
	r.Run(":8080")
}
```

### 路由转发

路由转发，使用`HandleContext`：

```go
func main() {
	r := gin.Default()

	// 路由转发，url输入/route-1，转发到/route-2路由
	r.GET("/route-1", func(c *gin.Context) {
		// 指定重定向的URL
		c.Request.URL.Path = "/route-2"
		r.HandleContext(c) // 继续后续的处理
	})

	r.GET("/route-2", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"msg": "route-2"})
	})

	r.Run(":8080")
}
```

测试，可以看到URL没有改变，所以只是转发了路由：

![image-20220510120302908](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ed9c768c790623cab7937e53b59b26c7.png)



## Gin 路由

### 普通路由

普通的路由请求方法：

```go
r.GET("/index", func(c *gin.Context) {...})
r.GET("/login", func(c *gin.Context) {...})
r.POST("/login", func(c *gin.Context) {...})
......
```



此外，还有一个可以匹配所有请求方法的`Any`方法，函数内部再用 switch 判断什么方法即可：

```go
r.Any("/test", func(c *gin.Context) {
        switch c.Request.Method {
            case "GET":
            c.JSON(http.StatusOK, gin.H{"method": "GET"})
            case "POST":
            c.JSON(http.StatusOK, gin.H{"method": "POST"})

            case "......":
            ......
	}
})
```



为没有配置处理函数的路由添加处理程序，默认情况下它返回404代码，可以用`r.NoRoute` 为没有匹配到路由的请求都返回`views/404.html`页面。

```go
r.NoRoute(func(c *gin.Context) {
	c.HTML(http.StatusNotFound, "views/404.html", nil)
})
```



### 路由组

我们可以将拥有**共同URL前缀**的路由划分为一个**路由组**。习惯性一对`{}`包裹同组的路由，这只是为了看着清晰，你用不用`{}`包裹功能上没什么区别。

```go
func main() {
	r := gin.Default()
	// 路由组
	userGroup := r.Group("/user")
	{
		userGroup.GET("/index", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"msg": "/user/index"})
		})
		userGroup.GET("/login", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"msg": "/user/login"})
		})
	}

	shopGroup := r.Group("/shop")
	{
		shopGroup.GET("/index", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"msg": "/shop/login"})
		})
		shopGroup.POST("/checkout", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"msg": "/shop/checkout"})
		})
	}
    
        r.Run(":8080")
}
```

路由组也是支持嵌套的，例如：

```go
	shopGroup := r.Group("/shop")
	{
		shopGroup.GET("/index", func(c *gin.Context) {...})
		shopGroup.GET("/cart", func(c *gin.Context) {...})
		shopGroup.POST("/checkout", func(c *gin.Context) {...})
		// 嵌套路由组
		xx := shopGroup.Group("xx")
		xx.GET("/oo", func(c *gin.Context) {...})
	}
```

通常我们将路由分组用在划分业务逻辑或划分API版本时。



### 路由原理

Gin框架中的路由使用的是[httprouter](https://github.com/julienschmidt/httprouter)这个库。

其基本原理就是构造一个路由地址的前缀树。

httprouter是一个高性能路由分发器，它负责将不同方法的多个路径分别注册到各个handle函数，当收到请求时，负责快速查找请求的路径是否有相对应的处理函数，并且进行下一步业务逻辑处理。golang的gin框架采用了httprouter进行路由匹配，httprouter 是通过radix tree来进行高效的路径查找；同时路径还支持两种通配符匹配。

 

## Gin 中间件

Gin 框架允许开发者在处理请求的过程中，加入用户自己的钩子（Hook）函数。这个钩子函数就叫中间件，中间件适合处理一些公共的业务逻辑，比如登录认证、权限校验、数据分页、记录日志、耗时统计等。



### 定义中间件

Gin 中的中间件必须是一个 `gin.HandlerFunc` 类型。例如我们像下面的代码一样定义一个统计请求耗时的中间件。

```go
// indexHandler 函数
func indexHandler(c *gin.Context) {
	fmt.Println("执行index...")
	// 获取前面中间件设置的值
	name, ok := c.Get("name")
	if !ok {
		name = "游客"
	}

	c.JSON(http.StatusOK, gin.H{
		"msg":  "indexHandler",
		"name": name,
	})
}

// StatCost 自定义中间件：统计请求耗时
func StatCost(c *gin.Context) {
	fmt.Println("StatCost 计时开始")
	start := time.Now()

	// 可以通过c.Set在请求上下文中设置值，后续的处理函数能够取到该值
	c.Set("name", "AruNi")

	// 调用该请求后续的处理函数（此处为indexHandler函数）
	c.Next()
	// c.Abort()	// 不调用该请求的剩余处理函数

	cost := time.Since(start)
	fmt.Println("StatCost 计时结束")
	fmt.Println("cost time: ", cost)
}

func main() {
	r := gin.Default()

	// GET函数的参数：GET(relativePath string, handlers ...HandlerFunc)
	// relativePath：路径， handlers ...HandlerFunc：可变的处理函数
	r.GET("/index", StatCost, indexHandler) // 先走StatCost，再走indexHandler

	r.Run(":9000")
}
```

测试，由于indexHandler函数处理时间太短，所以显示0s ：

![image-20220510203049986](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b981942879e43ad61e42bf935be5a52f.png)



### 注册中间件

在gin框架中，我们可以为每个路由添加任意数量的中间件。



#### 为某个路由单独注册

```go
        // 给/test2路由单独注册中间件
	r.GET("/test2", m1, m2, func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "Hello world!",
		})
	})
```



#### 为全局路由注册

```go
func index(c *gin.Context) {
	fmt.Println("index...")
	c.JSON(http.StatusOK, gin.H{
		"method": "GET",
	})
}

func m1(c *gin.Context) {
	fmt.Println("m1 in...")
	c.Next()
	fmt.Println("m1 out...")
}

func m2(c *gin.Context) {
	fmt.Println("m2 in...")
	c.Next()
	fmt.Println("m2 out...")
}

func main() {
	// 新建一个没有任何默认中间件的路由
	r := gin.New()

	// 注册一个全局中间件，每个请求都先走Use中的函数
	r.Use(m1, m2)

	r.GET("/index", index)

	r.Run(":9000")
}
```

测试：

![image-20220510205212668](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5773611d5c2dadcaa704f34a64e49c96.png)

执行过程：

![image-20220510205253265](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/87e4fbb2920051174f91a0b67a50d009.png)

当中间件中有 `c.Abort()` 或者 `return` 的情况：

![image-20220510205949377](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/431db695560e75c6c6a2fbc2193d59da.png)



#### 为路由组注册中间件

为路由组注册中间件有以下两种写法。

写法1：

```go
shopGroup := r.Group("/shop", StatCost())
{
    shopGroup.GET("/index", func(c *gin.Context) {...})
    ...
}
```

写法2：

```go
shopGroup := r.Group("/shop")
shopGroup.Use(StatCost())
{
    shopGroup.GET("/index", func(c *gin.Context) {...})
    ...
}
```



### 中间件的常用格式

我们在项目中定义中间件一般不会把 `c *gin.Context` 作为参数，而是会写成返回值为 `gin.HandlerFunc` 类型的函数，如下：

```go
func home() gin.HandlerFunc {
	return func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)
	}
}

func authMiddleware(openAuth bool) gin.HandlerFunc {
	// 此处可以做一些其他准备工作...

	return func(c *gin.Context) {
		// 如果打开了认证，则走认证流程，否则直接放行
		if openAuth {
			// 具体认证逻辑
			// if 是登录的用户
			// c.Next()
			// else
			// c.Abort()
		} else {
			c.Next()
		}
	}
}

func main() {
	r := gin.Default()
	// 走/index请求，首先经过认证中间件，通过了才能走home主页
	r.GET("/index", authMiddleware(true), home())
}
```



### 中间件注意事项

#### 默认中间件

`gin.Default()`默认使用了`Logger`和`Recovery`中间件，其中：

- `Logger`中间件将日志写入`gin.DefaultWriter`，即使配置了`GIN_MODE=release`。

- `Recovery`中间件会recover任何`panic`。如果有panic的话，会写入500响应码。

- 源码：

  ```go
  // Default returns an Engine instance with the Logger and Recovery middleware already attached.
  func Default() *Engine {
  	debugPrintWARNINGDefault()
  	engine := New()
  	engine.Use(Logger(), Recovery())
  	return engine
  }
  ```

  

如果不想使用上面两个默认的中间件，可以使用`gin.New()`新建一个没有任何默认中间件的路由。



#### 中间件中使用 goroutine

当在中间件或 `handler` 中启动新的 `goroutine` 时，**不能使用**原始的上下文（`c *gin.Context`），必须使用其只读副本（`c.Copy()`）。防止在另一个 `goroutine` 中修改了 `c`  的值，这是并发不安全的操作，必须要避免！

示例：

```go
func main() {
	r := gin.Default()

	r.GET("/long_async", func(c *gin.Context) {
		// 创建在 goroutine 中使用的副本
		cCp := c.Copy()
		go func() {
			// 用 time.Sleep() 模拟一个长任务。
			time.Sleep(5 * time.Second)

			// 请注意您使用的是复制的上下文 "cCp"，这一点很重要
			log.Println("Done! in path " + cCp.Request.URL.Path)
		}()
	})

	r.GET("/long_sync", func(c *gin.Context) {
		// 用 time.Sleep() 模拟一个长任务。
		time.Sleep(5 * time.Second)

		// 因为没有使用 goroutine，不需要拷贝上下文
		log.Println("Done! in path " + c.Request.URL.Path)
	})

	// 监听并在 0.0.0.0:8080 上启动服务
	r.Run(":8080")
}
```



## 运行多个服务

我们可以在多个端口启动服务，例如：

```go
package main

import (
	"log"
	"net/http"
	"time"

	"github.com/gin-gonic/gin"
	"golang.org/x/sync/errgroup"
)

var (
	g errgroup.Group
)

func router01() http.Handler {
	e := gin.New()
	e.Use(gin.Recovery())
	e.GET("/", func(c *gin.Context) {
		c.JSON(
			http.StatusOK,
			gin.H{
				"code":  http.StatusOK,
				"error": "Welcome server 01",
			},
		)
	})

	return e
}

func router02() http.Handler {
	e := gin.New()
	e.Use(gin.Recovery())
	e.GET("/", func(c *gin.Context) {
		c.JSON(
			http.StatusOK,
			gin.H{
				"code":  http.StatusOK,
				"error": "Welcome server 02",
			},
		)
	})

	return e
}

func main() {
	server01 := &http.Server{
		Addr:         ":8080",
		Handler:      router01(),
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
	}

	server02 := &http.Server{
		Addr:         ":8081",
		Handler:      router02(),
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 10 * time.Second,
	}
   // 借助errgroup.Group或者自行开启两个goroutine分别启动两个服务
	g.Go(func() error {
		return server01.ListenAndServe()
	})

	g.Go(func() error {
		return server02.ListenAndServe()
	})

	if err := g.Wait(); err != nil {
		log.Fatal(err)
	}
}
```



# Go MySQL Driver

## 安装驱动

因为Go语言没有提供任何官方数据库驱动，所以需要安装第三方函数库。
终端执行命令：

```bash
go get github.com/go-sql-driver/mysql
```

## 连接数据库

创建测试表：

```rust
mysql> create table stu(
    -> id int primary key,
    -> name varchar(10)
    -> );
Query OK, 0 rows affected (0.02 sec)
```



1、打开GoLand，在src目录下创建 `demo.go` 文件， 导入数据库驱动

```go
import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql"
)

注意：程序在操作数据库的时候只需要用到database/sql，而不需要直接使用数据库驱动，所以程序在导入数据库驱动的时候将这个包的名字设置成下划线。
```

2、通过 `sql.open()` 连接数据库

```go
sql.open(驱动名,数据源dsn)(*DB,err)

数据源语法："用户名:密码@[连接方式](主机名:端口号)/数据库名"

注意：open()在执行时不会真正的与数据库进行连接，只是设置连接数据库需要的参数
ping()方法才是连接数据库
```

3、执行SQL语句

```go
package main

import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql"
    "fmt"
)

func main(){
    //"用户名:密码@[连接方式](主机名:端口号)/数据库名"
    db,_:=sql.Open("mysql","root:root@(127.0.0.1:3306)/testdatabases?charset=utf8mb4") // 设置连接数据库的参数
    defer db.Close()    //关闭数据库
    err:=db.Ping()      //连接数据库
    if err!=nil{
        fmt.Println("数据库连接失败")
        return
    }

    //操作一：执行数据操作语句
    /*
    sql:="insert into stu values (2,'berry')"
    result,_:=db.Exec(sql)      //执行SQL语句
    n,_:=result.RowsAffected(); //获取受影响的记录数
    fmt.Println("受影响的记录数是",n)
    */

    //操作二：执行预处理
    /*
    stu:=[2][2] string{{"3","ketty"},{"4","rose"}}
    stmt,_:=db.Prepare("insert into stu values (?,?)")      //获取预处理语句对象
    for _,s:=range stu{
        stmt.Exec(s[0],s[1])            //调用预处理语句
    }
    */

    //操作三：单行查询
    /*
    var id,name string
    rows:=db.QueryRow("select * from stu where id=4")   //获取一行数据
    rows.Scan(&id,&name)        //将rows中的数据存到id,name中
    fmt.Println(id,"--",name)
    */

    //操作四：多行查询
    rows,_:=db.Query("select * from stu")       //获取所有数据
    var id,name string
    for rows.Next(){        //循环显示所有的数据
        rows.Scan(&id,&name)
        fmt.Println(id,"--",name)
    }
}
```

# GORM

## 介绍

### 什么是 ORM

Object-Relationl Mapping：即对象关系映射，这里的Relationl指的是关系型数据库

作用：

- 在关系型数据库和对象之间作一个映射

这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句语句打交道，只要像平时操作对象一样操作它就可以了 。



### 什么是 GORM

The fantastic ORM library for Golang aims to be developer friendly.

**特性**：

- 全功能 ORM
- 关联 (Has One，Has Many，Belongs To，Many To Many，多态，单表继承)
- Create，Save，Update，Delete，Find 中钩子方法
- 支持 `Preload`、`Joins` 的预加载
- 事务，嵌套事务，Save Point，Rollback To Saved Point
- Context、预编译模式、DryRun 模式
- 批量插入，FindInBatches，Find/Create with Map，使用 SQL 表达式、Context Valuer 进行 CRUD
- SQL 构建器，Upsert，数据库锁，Optimizer/Index/Comment Hint，命名参数，子查询
- 复合主键，索引，约束
- Auto Migration
- 自定义 Logger
- 灵活的可扩展插件 API：Database Resolver（多数据库，读写分离）、Prometheus…
- 每个特性都经过了测试的重重考验
- 开发者友好

## 基本示例

**注意:**

- 以下示例以MySQL数据库为例，学习GORM各项功能的主要使用方法。

### 安装

```go
go get -u github.com/jinzhu/gorm
```



### 创建数据库

在使用GORM前手动创建数据库 `gorm_demo`：

```mysql
CREATE DATABASE gorm_demo;
```



### 连接 MySQL

```go
import (
  "github.com/jinzhu/gorm"
  _ "github.com/jinzhu/gorm/dialects/mysql"
)

func main() {
  db, err := gorm.Open("mysql", "user:password@(localhost)/dbname?charset=utf8mb4&parseTime=True&loc=Local")
  defer db.Close()
}
```



### 操作 MySQL

使用 GORM 连接上面的 `gorm_demo` 进行创建、查询、更新、删除操作。

```go
package main

import (
	"fmt"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

type UserInfo struct {
	Id     uint
	Name   string
	Gender string
	Hobby  string
}

func main() {
	db, err := gorm.Open("mysql", "root:123456@(127.0.0.1:3306)/gorm_demo?charset=utf8mb4&parseTime=True&loc=Local")
	if err != nil {
		panic(err)
	}
	defer db.Close()

	// 自动迁移，发现数据库表和结构体字段不匹配时自动同步
	db.AutoMigrate(&UserInfo{})

	u1 := UserInfo{1, "AruNi", "男", "篮球"}
	u2 := UserInfo{2, "张三", "男", "犯罪"}

	// 插入数据
	db.Create(&u1)
	db.Create(&u2)
        // 批量插入
	//var users = []User{user1, user2, user3}
	//db.Create(&users)

	// 查询
	var u3 = new(UserInfo)
	// 找到符合条件的一条记录，按主键排序
	db.First(u3)
	fmt.Printf("%#v\n", u3)

	var u4 UserInfo
	// 查询hobby=篮球的用户
	db.Find(&u4, "hobby=?", "犯罪")
	fmt.Printf("%#v\n", u4)

	// 更新
	db.Model(&u3).Update("name", "菜菜")
	fmt.Printf("%#v\n", u3)

	// 删除
	db.Delete(&u3)
}
```

输出：

```text
&main.UserInfo{Id:0x1, Name:"AruNi", Gender:"男", Hobby:"篮球"}
main.UserInfo{Id:0x2, Name:"张三", Gender:"男", Hobby:"犯罪"}
&main.UserInfo{Id:0x1, Name:"菜菜", Gender:"男", Hobby:"篮球"}
```



## 模型定义

在使用ORM工具时，通常我们需要在代码中定义模型（Models）与数据库中的数据表进行映射，在GORM中模型（Models）通常是正常定义的结构体、基本的go类型或它们的指针。 同时也支持`sql.Scanner`及`driver.Valuer`接口（interfaces）。

### gorm.Model

为了方便，GORM 定义一个 `gorm.Model` 结构体，其包括字段 `ID`、`CreatedAt`、`UpdatedAt`、`DeletedAt`

```go
// gorm.Model 的定义
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
}
```

你可以将它嵌入到你自己的模型中：

```go
type User struct {
    gorm.Model
    Name string
}

// 等效于
type User struct {
    ID        uint           `gorm:"primaryKey"`
    CreatedAt time.Time
    UpdatedAt time.Time
    DeletedAt gorm.DeletedAt `gorm:"index"`
    Name string
}
```

当然也可以完全自己定义模型：

```go
// 不使用gorm.Model，自行定义模型
type User struct {
  ID   int
  Name string
}
```



### 模型定义示例

```go
type User struct {
  gorm.Model
  Name         string
  Age          sql.NullInt64
  Birthday     *time.Time
  Email        string  `gorm:"type:varchar(100);unique_index"`
  Role         string  `gorm:"size:255"` // 设置字段大小为255
  MemberNumber *string `gorm:"unique;not null"` // 设置会员号（member number）唯一并且不为空
  Num          int     `gorm:"AUTO_INCREMENT"` // 设置 num 为自增类型
  Address      string  `gorm:"index:addr"` // 给address字段创建名为addr的索引
  IgnoreMe     int     `gorm:"-"` // 忽略本字段
}
```

### 结构体标记（tags）

使用结构体声明模型时，标记（tags）是可选项。gorm支持以下标记:

#### 支持的结构体标记（Struct tags）

| 结构体标记（Tag） | 描述                                                     |
| ----------------- | -------------------------------------------------------- |
| Column            | 指定列名                                                 |
| Type              | 指定列数据类型                                           |
| Size              | 指定列大小, 默认值255                                    |
| PRIMARY_KEY       | 将列指定为主键                                           |
| UNIQUE            | 将列指定为唯一                                           |
| DEFAULT           | 指定列默认值                                             |
| PRECISION         | 指定列精度                                               |
| NOT NULL          | 将列指定为非 NULL                                        |
| AUTO_INCREMENT    | 指定列是否为自增类型                                     |
| INDEX             | 创建具有或不带名称的索引, 如果多个索引同名则创建复合索引 |
| UNIQUE_INDEX      | 和 `INDEX` 类似，只不过创建的是唯一索引                  |
| EMBEDDED          | 将结构设置为嵌入                                         |
| EMBEDDED_PREFIX   | 设置嵌入结构的前缀                                       |
| -                 | 忽略此字段                                               |

#### 关联相关标记（tags）

| 结构体标记（Tag）                | 描述                               |
| -------------------------------- | ---------------------------------- |
| MANY2MANY                        | 指定连接表                         |
| FOREIGNKEY                       | 设置外键                           |
| ASSOCIATION_FOREIGNKEY           | 设置关联外键                       |
| POLYMORPHIC                      | 指定多态类型                       |
| POLYMORPHIC_VALUE                | 指定多态值                         |
| JOINTABLE_FOREIGNKEY             | 指定连接表的外键                   |
| ASSOCIATION_JOINTABLE_FOREIGNKEY | 指定连接表的关联外键               |
| SAVE_ASSOCIATIONS                | 是否自动完成 save 的相关操作       |
| ASSOCIATION_AUTOUPDATE           | 是否自动完成 update 的相关操作     |
| ASSOCIATION_AUTOCREATE           | 是否自动完成 create 的相关操作     |
| ASSOCIATION_SAVE_REFERENCE       | 是否自动完成引用的 save 的相关操作 |
| PRELOAD                          | 是否自动完成预加载的相关操作       |



## 约定

### 主键（Primary Key）

GORM 默认会使用名为ID的字段作为表的主键。

```go
type User struct {
  ID   string // 名为`ID`的字段会默认作为表的主键
  Name string
}

// 使用`AnimalID`作为主键
type Animal struct {
  AnimalID int64 `gorm:"primary_key"`
  Name     string
  Age      int64
}
```

### 表名（Table Name）

表名默认就是结构体名称的复数，例如：

```go
type User struct {} // 默认表名是 `users`

// 将 User 的表名设置为 `profiles`
func (User) TableName() string {
  return "profiles"
}

func (u User) TableName() string {
  if u.Role == "admin" {
    return "admin_users"
  } else {
    return "users"
  }
}

// 禁用默认表名的复数形式，如果置为 true，则 `User` 的默认表名是 `user`
db.SingularTable(true)
```

也可以通过`Table()`指定表名：

```go
// 使用User结构体创建名为`deleted_users`的表
db.Table("deleted_users").CreateTable(&User{})

var deleted_users []User
db.Table("deleted_users").Find(&deleted_users)
// SELECT * FROM deleted_users;

db.Table("deleted_users").Where("name = ?", "jinzhu").Delete()
// DELETE FROM deleted_users WHERE name = 'jinzhu';
```

GORM还支持更改默认表名称规则：

```go
gorm.DefaultTableNameHandler = func (db *gorm.DB, defaultTableName string) string  {
  return "prefix_" + defaultTableName;
}
```

### 列名（Column Name）

列名由字段名称进行**下划线分割**来生成

```go
type User struct {
  ID        uint      // column name is `id`
  Name      string    // column name is `name`
  Birthday  time.Time // column name is `birthday`
  CreatedAt time.Time // column name is `created_at`
}
```

可以使用结构体tag指定列名：

```go
type Animal struct {
  AnimalId    int64     `gorm:"column:beast_id"`         // set column name to `beast_id`
  Birthday    time.Time `gorm:"column:day_of_the_beast"` // set column name to `day_of_the_beast`
  Age         int64     `gorm:"column:age_of_the_beast"` // set column name to `age_of_the_beast`
}
```

### 时间戳跟踪

#### CreatedAt

如果模型有 `CreatedAt`字段，该字段的值将会是初次创建记录的时间。

```go
db.Create(&user) // `CreatedAt`将会是当前时间

// 可以使用`Update`方法来改变`CreateAt`的值
db.Model(&user).Update("CreatedAt", time.Now())
```

#### UpdatedAt

如果模型有`UpdatedAt`字段，该字段的值将会是每次更新记录的时间。

```go
db.Save(&user) // `UpdatedAt`将会是当前时间

db.Model(&user).Update("name", "jinzhu") // `UpdatedAt`将会是当前时间
```

#### DeletedAt

如果模型有`DeletedAt`字段，调用`Delete`删除该记录时，将会设置`DeletedAt`字段为当前时间，而不是直接将记录从数据库中删除。



## CRUD

CRUD通常指数据库的增删改查操作，使用GORM实现创建、查询、更新和删除操作。



### Create

```go
package main

import (
	"fmt"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

// User 1.定义模型
type User struct {
	ID   int64
	Name string
	Age  int64 `gorm:"default:'18'"`
}

func main() {
	// 连接MySQL
	db, err := gorm.Open("mysql", "root:123456@(127.0.0.1:3306)/gorm_demo?charset=utf8mb4&parseTime=True&loc=Local")
	if err != nil {
		panic(err)
	}
	defer db.Close()

	// 2.把模型与数据库中的表对应起来(若数据库中无此表，会帮我们创建)
	db.AutoMigrate(&User{})

	// 3.创建
	u := User{Name: "AruNi"} // Age 使用默认值
	// 可以使用NewRecord查询主键是否存在
	fmt.Println(db.NewRecord(u)) // 主键为空返回 true
	db.Create(&u)
        //db.Debug().Create(&u)		// 可以使用Debug().CRUD 来打印出SQL语句
	fmt.Println(db.NewRecord(u)) // 创建 `u` 后主键不为空，返回 false
}
```

运行该程序后，会发现 GORM 帮助我们创建了一个 `users `表：

![image-20220511212410926](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e54d5b2521ed09ba7141adec56c85639.png)



**注意**：通过tag定义字段的默认值，在创建记录时候生成的 SQL 语句会排除没有值或值为 零值 的字段。 在将记录插入到数据库后，Gorm 会从数据库加载那些字段的默认值。

**注意：**所有字段的零值, 比如`0`, `""`,`false`或者其它`零值`，都不会保存到数据库内，但会使用它们的默认值。 

如果想避免这种情况，可以使用**指针 **或 实现`Scanner/Valuer`接口的方式将零值存入数据库表中，比如：

**结构体属性使用指针**：

```go
// User 1.定义模型
type User struct {
	ID   int64
	Name string
        // 使用指针
	Age  *int64 `gorm:"default:'18'"`
}
u := User{Name: "AruNi", Age: new(int64)}	// Age需要使用new
db.Create(&u)  // 此时数据库中该条记录age字段的值就是0
```

**Scanner/Valuer接口**：

```go
// 使用 Scanner/Valuer
type User struct {
	ID int64
	Name string
	Age  sql.NullInt64 `gorm:"default:'18'"`    // sql.NullString 实现了Scanner/Valuer接口
        
}
u := User{Name: "AruNi", Age: sql.NullInt64{Valid: true}}	// Valid: trueb
db.Create(&)  // 此时数据库中该条记录age字段的值就是0

-------------------------------------------
// NullInt64 的结构体
type NullInt64 struct {
	Int64 int64
	Valid bool // Valid is true if Int64 is not NULL
}
```



### Read

当前 `users` 表中的数据：

![image-20220512105311999](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/aeb94d76b2b52fde17d3a5010d568108.png)

#### 一般查询

```go
// 根据主键查询第一条记录
var user User
db.First(&user)
// SELECT * FROM users ORDER BY id LIMIT 1;

// 随机获取一条记录
var user User
db.Take(&user)
// SELECT * FROM users LIMIT 1;

// 根据主键查询最后一条记录
var user User
db.Last(&user)
// SELECT * FROM users ORDER BY id DESC LIMIT 1;

// 查询所有的记录
var users []User
db.Find(&users)
// SELECT * FROM users;


// 查询指定的某条记录(仅当主键为整型时可用)
var user User
db.First(&user, 5)
// SELECT * FROM users WHERE id = 5;
```

注意：`First()` 函数找不到record的时候，会返回 `ErrRecordNotFound` 错误 ， 而 `Find()` 则是返回nil，不会返回错误信息。

所以在项目中，如果想让 record 为0时不报错，则需要提前捕获一下 `ErrRecordNotFound` 错误，将 err 置为 nil。

#### Scan & Raw

Scan 将结果扫描到结构中，类似于使用 Find 的方式。

Raw 执行原生查询 SQL。

```go
type Result struct {
  Name string
  Age  int
}

var result Result
db.Table("users").Select("name", "age").Where("name = ?", "AruNi").Scan(&result)

// Raw SQL
db.Raw("SELECT name, age FROM users WHERE name = ?", "AruNi").Scan(&result)
```



Exec 原生 SQL

```go
db.Exec("DROP TABLE users")
db.Exec("UPDATE orders SET shipped_at = ? WHERE id IN ?", time.Now(), []int64{1, 2, 3})

// Exec with SQL Expression
db.Exec("UPDATE users SET money = ? WHERE name = ?", gorm.Expr("money * ? + ?", 10000, 1), "jinzhu")
```



#### Where 条件

##### 普通SQL查询

```go
// Get first matched record
var user User
db.Debug().Where("name=?", "张三").First(&user)
// SELECT * FROM users WHERE name = '张三' limit 1;

// Get all matched records
var users []User
db.Where("name = ?", "张三").Find(&users)
// SELECT * FROM users WHERE name = '张三';

// <> (相当于!=)
var users []User
db.Debug().Where("age<>?", "18").Find(&users)
// SELECT * FROM users WHERE age <> '18';

// IN
var users []User
db.Debug().Where("age IN(?)", []int64{18, 20, 22}).Find(&users)
// SELECT * FROM `users`  WHERE (age IN(18,20,22));

// LIKE
var users []User
db.Debug().Where("name LIKE ?", "%五").Find(&users)
// SELECT * FROM `users`  WHERE (name LIKE '%五');

// AND
var users []User
db.Debug().Where("name = ? AND age >= ?", "AruNi", "18").Find(&users)
// SELECT * FROM `users`  WHERE (name = 'AruNi' AND age >= '18');

// Time
db.Where("updated_at > ?", lastWeek).Find(&users)
// SELECT * FROM users WHERE updated_at > '2000-01-01 00:00:00';

// BETWEEN
db.Where("created_at BETWEEN ? AND ?", lastWeek, today).Find(&users)
// SELECT * FROM users WHERE created_at BETWEEN '2000-01-01 00:00:00' AND '2000-01-08 00:00:00';
```



##### Struct & Map查询

```go
// Struct
var user User
db.Where(&User{Name: "AruNi", Age: 18}).First(&user)
// SELECT * FROM users WHERE name = "AruNi" AND age = 18 LIMIT 1;

// Map
var users []User
db.Where(map[string]interface{}{"name": "AruNi", "age": 18}).Find(&users)
//// SELECT * FROM users WHERE name = "AruNi" AND age = 18;

// 主键的切片
db.Where([]int64{2, 4, 5}).Find(&users)
// SELECT * FROM users WHERE id IN (2, 4, 5);
```



**提示**：

当通过结构体进行查询时，GORM 将会只通过**非零值字段查询**，这意味着如果你的字段值为`0`，`''`，`false`或者其他`零值`时，将不会被用于构建查询条件，例如：

```go
var users []User
db.Where(&User{Name: "AruNi", Age: 0}).Find(&users)
// SELECT * FROM users WHERE name = "AruNi";
```

你可以使用指针或实现 `Scanner/Valuer` 接口来避免这个问题，当前使用的就是第二种：

```go
// 使用指针
type User struct {
  gorm.Model
  Name string
  Age  *int
}

// 使用 Scanner/Valuer（当前使用的结构体）
type User struct {
  gorm.Model
  Name string
  Age  sql.NullInt64  // sql.NullInt64 实现了 Scanner/Valuer 接口
}
```



#### Not 条件

作用与 Where 类似的情形如下：

```go
db.Not("name", "AruNi").First(&user)
// SELECT * FROM users WHERE name <> "AruNi" LIMIT 1;

// Not In
db.Not("age", []int64{18, 20}).Find(&users)
// SELECT * FROM users WHERE age NOT IN (18, 20);

// Not In slice of primary keys
db.Not([]int64{1,2,3}).First(&user)
// SELECT * FROM users WHERE id NOT IN (1,2,3);

// Plain SQL
db.Not("name = ?", "AruNi").First(&user)
// SELECT * FROM users WHERE NOT(name = "AruNi");

// Struct
db.Not(User{Name: "AruNi"}).First(&user)
// SELECT * FROM users WHERE name <> "AruNi";
```



#### Or条件

```go
db.Where("name = ?", "AruNi").Or("age = ?", 20).Find(&users)
// SELECT * FROM users WHERE name = 'AruNi' OR age = 20;

// Struct
db.Where("name = 'AruNi'").Or(User{Age: 20}).Find(&users)
// SELECT * FROM users WHERE name = 'AruNi' OR age = 20;

// Map
db.Where("name = 'AruNi'").Or(map[string]interface{}{"Age": 20}).Find(&users)
// SELECT * FROM users WHERE name = 'AruNi' OR age = 20;
```

#### 内联条件

查询条件可以内联到 First 和 Find 等方法中，功能与 WHERE 类似

```go
// 根据主键获取记录 (只适用于整形主键)
db.First(&user, 1)
// SELECT * FROM users WHERE id = 1 LIMIT 1;

// 根据主键获取记录, 如果它是一个非整形主键
db.First(&user, "id = ?", "string_primary_key")
// SELECT * FROM users WHERE id = 'string_primary_key' LIMIT 1;

// Plain SQL
db.Find(&user, "name = ?", "AruNi")
// SELECT * FROM users WHERE name = "AruNi";

db.Find(&users, "name <> ? AND age > ?", "AruNi", 18)
// SELECT * FROM users WHERE name <> "AruNi" AND age > 18;

// Struct
db.Find(&users, User{Age: 20})
// SELECT * FROM users WHERE age = 20;

// Map
db.Find(&users, map[string]interface{}{"age": 20})
// SELECT * FROM users WHERE age = 20;
```

注意：

当内联条件与多个立即执行方法一起使用时, 内联条件不会传递给后面的立即执行方法。



#### 额外查询选项

```go
// 为查询 SQL 添加额外的 SQL 操作
db.Set("gorm:query_option", "FOR UPDATE").First(&user, 1)
// SELECT * FROM users WHERE id = 1 FOR UPDATE;
```

`FOR UPDATE`：锁定查出来的这些行，别的会话不能修改，只有当前会话提交（[回滚](https://so.csdn.net/so/search?q=回滚&spm=1001.2101.3001.7020)）后，或当前会话结束事，别的会话才可以修改，一般会造成线程等待，死锁的发生；



#### FirstOrInit

获取匹配的第一条记录，否则根据给定的条件**初始化一个新的对象** (仅支持 struct 和 map 条件)，此对象不会插入到表中！

```go
// 未找到
db.Debug().FirstOrInit(&user, User{Name: "王子"})
// user -> User{{0 王子 {0 false}}

// 找到
db.Where(User{Name: "AruNi"}).FirstOrInit(&user)
// user -> User{Id: 1, Name: "AruNi", Age: 18}

db.FirstOrInit(&user, map[string]interface{}{"name": "AruNi"})
// user -> User{Id: 111, Name: "AruNi", Age: 18}
```



##### Attrs

如果记录未找到，将使用参数初始化 struct

```go
// 未找到
db.Where(User{Name: "non_existing"}).Attrs(User{Age: 20}).FirstOrInit(&user)
// SELECT * FROM USERS WHERE name = 'non_existing';
// user -> User{Name: "non_existing", Age: 20}

db.Where(User{Name: "non_existing"}).Attrs("age", 20).FirstOrInit(&user)
// SELECT * FROM USERS WHERE name = 'non_existing';
// user -> User{Name: "non_existing", Age: 20}

// 找到
db.Where(User{Name: "AruNi"}).Attrs(User{Age: 18}).FirstOrInit(&user)
//// SELECT * FROM users WHERE name = 'AruNi';
//// user -> User{Id: 1, Name: "AruNi", Age: 18}
```



##### Assign

不管记录是否找到，都将参数赋值给 struct

```go
// 未找到
db.Where(User{Name: "non_existing"}).Assign(User{Age: 20}).FirstOrInit(&user)
// user -> User{Name: "non_existing", Age: 20}

// 找到
db.Where(User{Name: "AruNi"}).Assign(User{Age: 22}).FirstOrInit(&user)
//// SELECT * FROM users WHERE name = 'AruNi';
// user -> User{Id: 111, Name: "Jinzhu", Age: 22}
```



#### FirstOrCreate

获取匹配的第一条记录, 否则根据给定的条件创建一个新的记录 (仅支持 struct 和 map 条件)，此对象会插入到表中！

```go
// 未找到
db.FirstOrCreate(&user, User{Name: "non_existing"})
// INSERT INTO "users" (name) VALUES ("non_existing");
// user -> User{Id: 7, Name: "non_existing"}

// 找到
db.Where(User{Name: "AruNi"}).FirstOrCreate(&user)
//// user -> User{Id: 111, Name: "AruNi"}
```



##### Attrs

如果记录未找到，将使用参数创建 struct 和记录

```go
 // 未找到
db.Where(User{Name: "non_existing"}).Attrs(User{Age: 20}).FirstOrCreate(&user)
// SELECT * FROM users WHERE name = 'non_existing';
// INSERT INTO "users" (name, age) VALUES ("non_existing", 20);
// user -> User{Id: 7, Name: "non_existing", Age: 20}

// 找到，不使用Attrs参数
db.Where(User{Name: "AruNi"}).Attrs(User{Age: 30}).FirstOrCreate(&user)
// SELECT * FROM users WHERE name = 'AruNi';
// user -> User{Id: 1, Name: "AruNi", Age: 18}
```



##### Assign

不管记录是否找到，都将参数赋值给 struct 并保存至数据库

```go
// 未找到
db.Where(User{Name: "non_existing"}).Assign(User{Age: 20}).FirstOrCreate(&user)
// SELECT * FROM users WHERE name = 'non_existing';
// INSERT INTO "users" (name, age) VALUES ("non_existing", 20);
// user -> User{Id: 7, Name: "non_existing", Age: 20}

// 找到
db.Where(User{Name: "AruNi"}).Assign(User{Age: 30}).FirstOrCreate(&user)
// SELECT * FROM users WHERE name = 'AruNi';
// UPDATE users SET age=30 WHERE id = 1;
// user -> User{Id: 111, Name: "AruNi", Age: 30}
```



#### 高级查询

##### 子查询

子查询可以嵌套在查询中，GORM 允许在使用 `*gorm.DB` 对象作为参数时生成子查询

```go
db.Where("amount > (?)", db.Table("orders").Select("AVG(amount)")).Find(&orders)
// SELECT * FROM "orders" WHERE amount > (SELECT AVG(amount) FROM "orders");

subQuery := db.Select("AVG(age)").Where("name LIKE ?", "name%").Table("users")
db.Select("AVG(age) as avgage").Group("name").Having("AVG(age) > (?)", subQuery).Find(&results)
// SELECT AVG(age) as avgage FROM `users` GROUP BY `name` HAVING AVG(age) > (SELECT AVG(age) FROM `users` WHERE name LIKE "name%")
```



**From 子查询**：

GORM 允许您在 `Table` 方法中通过 FROM 子句使用子查询，例如：

```go
db.Table("(?) as u", db.Model(&User{}).Select("name", "age")).Where("age = ?", 18}).Find(&User{})
// SELECT * FROM (SELECT `name`,`age` FROM `users`) as u WHERE `age` = 18

subQuery1 := db.Model(&User{}).Select("name")
subQuery2 := db.Model(&Pet{}).Select("name")
db.Table("(?) as u, (?) as p", subQuery1, subQuery2).Find(&User{})
// SELECT * FROM (SELECT `name` FROM `users`) as u, (SELECT `name` FROM `pets`) as p
```



##### 选择字段

GORM中查询会默认会选择全部字段，使用 Select 指定你想从数据库中检索出的字段。

```go
db.Select("name, age").Find(&users)
// SELECT name, age FROM users;

db.Select([]string{"name", "age"}).Find(&users)
// SELECT name, age FROM users;

// COALESCE：若age有零值，则用后面的值代替
db.Table("users").Select("COALESCE(age,?)", 20).Rows()
// SELECT COALESCE(age,'42') FROM users;
```



##### Order

Order，指定从数据库中检索出记录的顺序。设置第二个参数 reorder 为 `true` ，可以覆盖前面定义的排序条件。 

```go
db.Order("age desc, name").Find(&users)
// SELECT * FROM users ORDER BY age desc, name;

// 多字段排序
db.Order("age desc").Order("name").Find(&users)
// SELECT * FROM users ORDER BY age desc, name;

// 覆盖排序
var users1 []User
var users2 []User
db.Debug().Order("age desc").Find(&users1).Order("age", true).Find(&users2)
// SELECT * FROM users ORDER BY age desc; (users1)
// SELECT * FROM users ORDER BY age; (users2)
// users 是按照age desc排序的结果；users2是按照age asc 排序的结果
```



##### Limit

Limit，指定从数据库检索出指定的记录数。

```go
db.Limit(3).Find(&users)
//// SELECT * FROM users LIMIT 3;

// -1 取消 Limit 条件
db.Limit(10).Find(&users1).Limit(-1).Find(&users2)
// SELECT * FROM users LIMIT 10; (users1)
// SELECT * FROM users; (users2)
```



##### Offset

Offset，指定开始返回记录前要跳过的记录数。

```go
db.Offset(3).Find(&users)
// SELECT * FROM users OFFSET 3;

// -1 取消 Offset 条件
db.Offset(10).Find(&users1).Offset(-1).Find(&users2)
// SELECT * FROM users OFFSET 10; (users1)
// SELECT * FROM users; (users2)
```



##### Count & Distinct

Count，该 model 能获取的记录总数。

```go
// 把符合条件的user和count都查询出来
db.Where("name = ?", "AruNi").Or("name = ?", "张三").Find(&users).Count(&count)
// SELECT * from users WHERE name = 'AruNi' OR name = '张三'; (users)
// SELECT count(*) FROM users WHERE name = 'AruNi' OR name = '张三'; (count)

// 只查询count
db.Model(&User{}).Where("name = ?", "AruNi").Count(&count)
// SELECT count(*) FROM users WHERE name = 'AruNi'; (count)

// 获取表的记录行数
db.Table("users").Count(&count)
// SELECT count(*) FROM users;

// 去重查询count
db.Table("users").Select("count(distinct(age))").Count(&count)
// SELECT count( distinct(age) ) FROM users; (count)
```

**注意**：

`Count` 必须是链式查询的**最后一个操作** ，因为它会覆盖前面的 `SELECT`，但如果里面使用了 `Count` 时不会覆盖。



##### Group & Having

```go
type result struct {
  Date  time.Time
  Total int
}

db.Model(&User{}).Select("name, sum(age) as total").Where("name LIKE ?", "group%").Group("name").First(&result)
// SELECT name, sum(age) as total FROM `users` WHERE name LIKE "group%" GROUP BY `name` LIMIT 1


db.Model(&User{}).Select("name, sum(age) as total").Group("name").Having("name = ?", "group").Find(&result)
// SELECT name, sum(age) as total FROM `users` GROUP BY `name` HAVING name = "group"

// Rows() 返回一个迭代器，需要使用 .Next() 进行遍历，遍历的时候使用 .Scan() s
rows, err := db.Table("orders").Select("date(created_at) as date, sum(amount) as total").Group("date(created_at)").Rows()
defer rows.Close()
for rows.Next() {
  ...
}

rows, err := db.Table("orders").Select("date(created_at) as date, sum(amount) as total").Group("date(created_at)").Having("sum(amount) > ?", 100).Rows()
defer rows.Close()
for rows.Next() {
  ...
}

type Result struct {
  Date  time.Time
  Total int64
}
db.Table("orders").Select("date(created_at) as date, sum(amount) as total").Group("date(created_at)").Having("sum(amount) > ?", 100).Scan(&results)
```



##### Joins

Joins，指定连接条件。

```go
type result struct {
  Name  string
  Email string
}

db.Model(&User{}).Select("users.name, emails.email").Joins("left join emails on emails.user_id = users.id").Scan(&result{})
// SELECT users.name, emails.email FROM `users` left join `emails` on emails.user_id = users.id

rows, err := db.Table("users").Select("users.name, emails.email").Joins("left join emails on emails.user_id = users.id").Rows()
for rows.Next() {
  ...
}

// 将结果扫描到result结构体中
var results result
db.Table("users").Select("users.name, emails.email").Joins("left join emails on emails.user_id = users.id").Scan(&results)

// 多连接及参数
db.Joins("JOIN emails ON emails.user_id = users.id AND emails.email = ?", "jinzhu@example.org").Joins("JOIN credit_cards ON credit_cards.user_id = users.id").Where("credit_cards.number = ?", "411111111111").Find(&user)
```



#### Pluck

Pluck，查询 model 中的一个列作为切片，如果您想要查询多个列，应该使用 `Select` 和 `Scan`

```go
var ages []int64
db.Model(&users).Pluck("age", &ages)

var names []string
db.Model(&User{}).Pluck("name", &names)

// Distinct Pluck
db.Model(&User{}).Distinct().Pluck("Name", &names)
// SELECT DISTINCT `name` FROM `users`

// 超过一列的查询，应该使用 `Scan` 或者 `Find`，例如：
var users []User
db.Select("name", "age").Scan(&users)
db.Select("name", "age").Find(&users)
```



### 链式操作相关

#### 链式操作

Method Chaining，Gorm 实现了链式操作接口，所以你可以把代码写成这样，相当于动态SQL：

```go
// 创建一个查询
tx := db.Where("name = ?", "AruNi")

// 添加更多条件
if someCondition {
  tx = tx.Where("age = ?", 20)
} else {
  tx = tx.Where("age = ?", 30)
}

if yetAnotherCondition {
  tx = tx.Where("active = ?", 1)
}
```

在调用立即执行方法前不会生成`Query`语句，借助这个特性你可以创建一个函数来处理一些通用逻辑。



#### 立即执行方法

Immediate methods ，立即执行方法是指那些会立即生成`SQL`语句并发送到数据库的方法, 他们一般是`CRUD`方法，比如：

`Create`, `First`, `Find`, `Take`, `Save`, `UpdateXXX`, `Delete`, `Scan`, `Row`, `Rows`…

这有一个基于上面链式方法代码的立即执行方法的例子：

```go
tx.Find(&user)
```

生成的SQL语句如下：

```sql
SELECT * FROM users where name = 'AruNi' AND age = 30 AND active = 1;
```



#### 范围

`Scopes`，Scope是建立在链式操作的基础之上的。

基于它，你可以抽取一些通用逻辑，写出更多可重用的函数库。

```go
func AmountGreaterThan1000(db *gorm.DB) *gorm.DB {
  return db.Where("amount > ?", 1000)
}

func PaidWithCreditCard(db *gorm.DB) *gorm.DB {
  return db.Where("pay_mode_sign = ?", "C")
}

func PaidWithCod(db *gorm.DB) *gorm.DB {
  return db.Where("pay_mode_sign = ?", "C")
}

func OrderStatus(status []string) func (db *gorm.DB) *gorm.DB {
  return func (db *gorm.DB) *gorm.DB {
    return db.Scopes(AmountGreaterThan1000).Where("status IN (?)", status)
  }
}

db.Scopes(AmountGreaterThan1000, PaidWithCreditCard).Find(&orders)
// 查找所有金额大于 1000 的信用卡订单

db.Scopes(AmountGreaterThan1000, PaidWithCod).Find(&orders)
// 查找所有金额大于 1000 的 COD 订单

db.Scopes(AmountGreaterThan1000, OrderStatus([]string{"paid", "shipped"})).Find(&orders)
// 查找所有金额大于 1000 且已付款或者已发货的订单
```

#### 多个立即执行方法

Multiple Immediate Methods，在 GORM 中使用多个立即执行方法时，后一个立即执行方法会复用前一个**立即执行方法**的条件 (不包括内联条件) 。

```go
var users User
var count int
db.Where("name LIKE ?", "Aru%").Find(&users, "id IN (?)", []int{1, 2, 3}).Count(&count)
```

生成的 SQL：

```sql
SELECT * FROM users WHERE name LIKE 'Aru%' AND id IN (1, 2, 3)

// 不复用前面的Find立即执行方法，因为它是内联条件
SELECT count(*) FROM users WHERE name LIKE 'Aru%'
```



### Update

#### 更新所有字段

`Save()`默认会更新该对象的所有字段，即使你没有赋值，字段是零值。

```go
var user User
db.Where("name = ?", "AruNi").Find(&user)
user.Age = sql.NullInt64{Int64: 99, Valid: true}
db.Debug().Save(&user)
// UPDATE users SET `name` = 'AruNi', `age` = '99'  WHERE users.id = 1
```



#### 更新修改字段

如果你只希望更新指定字段，可以使用`Update`或者`Updates`

```go
// 更新单个属性，如果它有变化
db.Model(&user).Update("name", "hello")
// UPDATE users SET name='hello', updated_at='2013-11-17 21:34:10' WHERE id=111;

// 根据给定的条件更新单个属性
db.Model(&user).Where("active = ?", true).Update("name", "hello")
// UPDATE users SET name='hello', updated_at='2013-11-17 21:34:10' WHERE id=111 AND active=true;

// 使用 map 更新多个属性，只会更新其中有变化的属性
db.Model(&user).Updates(map[string]interface{}{"name": "hello", "age": 18, "active": false})
// UPDATE users SET name='hello', age=18, active=false, updated_at='2013-11-17 21:34:10' WHERE id=111;

// 使用 struct 更新多个属性，只会更新其中有变化且为非零值的字段
db.Model(&user).Updates(User{Name: "hello", Age: 18})
//// UPDATE users SET name='hello', age=18, updated_at = '2013-11-17 21:34:10' WHERE id = 111;

// 警告：当使用 struct 更新时，GORM只会更新那些非零值的字段
// 对于下面的操作，不会发生任何更新，"", 0, false 都是其类型的零值
db.Model(&user).Updates(User{Name: "", Age: 0, Active: false})
```



#### 更新选定字段

如果你想更新或忽略某些字段，你可以使用 `Select`，`Omit`

```go
// 更新name字段
db.Model(&user).Select("name").Updates(map[string]interface{}{"name": "hello", "age": 18, "active": false})
// UPDATE users SET name='hello', updated_at='2013-11-17 21:34:10' WHERE id=111;

// 更新时忽略name字段
db.Model(&user).Omit("name").Updates(map[string]interface{}{"name": "hello", "age": 18, "active": false})
// UPDATE users SET age=18, active=false, updated_at='2013-11-17 21:34:10' WHERE id=111;
```



#### 无Hooks更新

上面的更新操作会自动运行 model 的 `BeforeUpdate`, `AfterUpdate` 方法，更新 `UpdatedAt` 时间戳, 在更新时保存其 `Associations`, 如果你不想调用这些方法，你可以使用 `UpdateColumn`， `UpdateColumns`

```go
// 更新单个属性，类似于 `Update`
db.Model(&user).UpdateColumn("name", "hello")
// UPDATE users SET name='hello' WHERE id = 111;

// 更新多个属性，类似于 `Updates`
db.Model(&user).UpdateColumns(User{Name: "hello", Age: 18})
// UPDATE users SET name='hello', age=18 WHERE id = 111;
```



#### 批量更新

批量更新时`Hooks（钩子函数）`不会运行。

```go
db.Table("users").Where("id IN (?)", []int{1, 2}).Updates(map[string]interface{}{"name": "hello", "age": 18})
// UPDATE users SET name='hello', age=18 WHERE id IN (1, 2);

// 使用 struct 更新时，只会更新非零值字段，若想更新所有字段，使用map[string]interface{}
db.Model(User{}).Updates(User{Name: "hello", Age: 18})
// UPDATE users SET name='hello', age=18;

// 使用 `RowsAffected` 获取更新记录总数
db.Model(User{}).Updates(User{Name: "hello", Age: 18}).RowsAffected
```



#### 使用SQL表达式更新

先查询表中的第一条数据保存至user变量，使用 `gorm.Expr()` 填写表达式

```go
var user User
db.First(&user)
db.Model(&user).Update("age", gorm.Expr("age * ? + ?", 2, 100))
// UPDATE `users` SET `age` = age * 2 + 100, `updated_at` = '2020-02-16 13:10:20'  WHERE `users`.`id` = 1;

db.Model(&user).Updates(map[string]interface{}{"age": gorm.Expr("age * ? + ?", 2, 100)})
// UPDATE "users" SET "age" = age * '2' + '100', "updated_at" = '2020-02-16 13:05:51' WHERE `users`.`id` = 1;

db.Model(&user).UpdateColumn("age", gorm.Expr("age - ?", 1))
// UPDATE "users" SET "age" = age - 1 WHERE "id" = '1';

db.Model(&user).Where("age > 10").UpdateColumn("age", gorm.Expr("age - ?", 1))
// UPDATE "users" SET "age" = age - 1 WHERE "id" = '1' AND quantity > 10;
```



#### 修改Hooks中的值

如果你想修改 `BeforeUpdate`, `BeforeSave` 等 Hooks 中更新的值，可以使用 `scope.SetColumn`, 例如：

```go
func (user *User) BeforeSave(scope *gorm.Scope) (err error) {
  if pw, err := bcrypt.GenerateFromPassword(user.Password, 0); err == nil {
    scope.SetColumn("EncryptedPassword", pw)
  }
}
```



#### 其它更新选项

```go
// 为 update SQL 添加其它的 SQL
db.Model(&user).Set("gorm:update_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Update("name", "hello")
// UPDATE users SET name='hello', updated_at = '2013-11-17 21:34:10' WHERE id=111 OPTION (OPTIMIZE FOR UNKNOWN);
```



### Delete

#### 删除记录

**警告**：删除记录时，请确保主键字段有值，GORM 会通过主键去删除记录，如果主键为空，GORM 会删除该 model 的所有记录。

```go
// 删除现有记录
db.Delete(&email)
// DELETE from emails where id=10;

// 为删除 SQL 添加额外的 SQL 操作
db.Set("gorm:delete_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Delete(&email)
// DELETE from emails where id=10 OPTION (OPTIMIZE FOR UNKNOWN);
```

#### 批量删除

删除全部匹配的记录

```go
db.Where("email LIKE ?", "%jinzhu%").Delete(Email{})
// DELETE from emails where email LIKE "%jinzhu%";

db.Delete(Email{}, "email LIKE ?", "%jinzhu%")
// DELETE from emails where email LIKE "%jinzhu%";
```

#### 软删除

如果一个 model 有 `DeletedAt` 字段，他将自动获得软删除的功能！ 

当调用 `Delete` 方法时， 记录不会真正的从数据库中被删除， 只会将`DeletedAt` 字段的值会被设置为当前时间。

```go
db.Delete(&user)
// UPDATE users SET deleted_at="2013-10-29 10:23" WHERE id = 111;

// 批量删除
db.Where("age = ?", 20).Delete(&User{})
// UPDATE users SET deleted_at="2013-10-29 10:23" WHERE age = 20;

// 查询记录时会忽略被软删除的记录
db.Where("age = 20").Find(&user)
// SELECT * FROM users WHERE age = 20 AND deleted_at IS NULL;

// Unscoped 方法可以查询被软删除的记录
db.Unscoped().Where("age = 20").Find(&users)
// SELECT * FROM users WHERE age = 20;
```

#### 物理删除

```go
// Unscoped 方法可以物理删除记录
db.Unscoped().Delete(&order)
// DELETE FROM orders WHERE id=10;
```





# Todo List

## 前端

在GitHub中拉取前端的代码：https://github.com/Q1mi/bubble_frontend

在本地IDE中打开，构建项目：

```bash
npm install
npm run build
```

构建完后将dist目录中的文件拷贝到后端项目中，供后端使用。



## 后端

初始目录结构：

![image-20220512173603618](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/992c63b69a9177567c7b4ca3f4bfbdee.png)



下载安装Gin：

```bash
go get -u github.com/gin-gonic/gin
```

编写`main.go`：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	route := gin.Default()
	// 告诉gin 模板文件引用的静态文件在哪儿
	route.Static("/static", "static")
	// 告诉gin 模板文件在哪儿
	route.LoadHTMLGlob("templates/*")

	// 主页
	route.GET("/", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)
	})

	err = route.Run()
	if err != nil {
		return 
	}
}
```

运行项目，访问`localhost:8080/`：

![image-20220512174242212](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7ebe2b324f7841a8c29d15e0e715da42.png)

项目运行成功，下面开始编写核心代码。



## 框架搭建

创建一个名为`goweb_todolist`的MySQL数据库，然后引入GORM框架进行初始化：

```go
package main

import (
	"github.com/gin-gonic/gin"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
	"net/http"
)

// DB 定义全局变量DB
var (
	DB *gorm.DB
)

// Todo Model：代办清单结构体
type Todo struct {
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Status bool   `json:"status"`
}

// 连接数据库，成功返回nil，失败返回error
func initMySQL() (err error) {
	dsn := "root:123456@tcp(127.0.0.1:3306)/goweb_todolist?charset=utf8mb4&parseTime=True&loc=Local"

	// 初始化全局变量DB
	DB, err = gorm.Open("mysql", dsn)
	if err != nil {
		return
	}
	return DB.DB().Ping() // 测试连通性再返回,ping得通返回nil，否则返回error
}

func main() {
	// 创建数据库
	// SQL: CREATE DATABASE goweb_todolist

	// GORM框架---------------------
	// 连接数据库
	err := initMySQL()
	// 连接数据库失败属于不可逆错误，直接panic
	if err != nil {
		panic(err)
	}
	defer DB.Close()

	// Model绑定
	DB.AutoMigrate(&Todo{})

	// Gin框架---------------------
	route := gin.Default()
	// 告诉gin 模板文件引用的静态文件在哪儿
	route.Static("/static", "static")
	// 告诉gin 模板文件在哪儿
	route.LoadHTMLGlob("templates/*")

	// 主页
	route.GET("/", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", nil)
	})

	// v1 路由组
	v1Group := route.Group("v1")
	{
		// 代办事项
		// 添加
		v1Group.POST("/todo", func(c *gin.Context) {

		})

		// 查看
		// 查看所有待办事项
		v1Group.GET("/todo", func(c *gin.Context) {

		})
		// 查看某个待办事项
		v1Group.GET("/todo/:id", func(c *gin.Context) {

		})

		// 修改
		v1Group.PUT("todo/:id", func(c *gin.Context) {

		})

		// 删除
		v1Group.DELETE("todo/:id", func(c *gin.Context) {

		})
	}

	err = route.Run()
	if err != nil {
		return 
	}
}
```

框架搭建好了，下面的具体实现就简单了。



## 功能实现

### 添加待办

```go
		// 添加
		v1Group.POST("/todo", func(c *gin.Context) {
			// 前面页面填写代办事项，点击提交，会发请求到这里
			// 1. 从请求中把数据拿出来
			var todo Todo
			c.BindJSON(&todo)

			// 2. 插入数据库
			err = DB.Create(&todo).Error

			// 3. 返回响应
			// 插入数据库失败，状态码为200表示请求成功，把插入失败的error返回
			if err != nil {
				c.JSON(http.StatusOK, gin.H{"error": err.Error()})
			} else {
				c.JSON(http.StatusOK, todo) // 插入成功将该数据返回
				/*
					企业中返回信息可能如下：
					c.JSON(http.StatusOK, gin.H{
						"code": 200,
						"msg": "success",
						"data": todo,
					})
				*/
			}
		})
```



### 查看代办

#### 查看所有代办

```go
		// 查看所有待办事项
		v1Group.GET("/todo", func(c *gin.Context) {
			// 1. 从数据库中查询出所有todolist
			var todoList []Todo
			err = DB.Find(&todoList).Error
			// 2. 返回响应
			if err != nil {
				c.JSON(http.StatusOK, gin.H{"error": err.Error()})
			} else {
				c.JSON(http.StatusOK, todoList)
			}
		})
```



#### 查看某个代办

```go
		// 查看某个待办事项
		v1Group.GET("/todo/:id", func(c *gin.Context) {
			// 1. 获取前端传来的参数
			id, ok := c.Params.Get("id")
			if !ok {
				c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
				return
			}
			// 2. 去数据库中查数据
			var todo Todo
			err = DB.First(&todo, id).Error
			// 3. 返回响应
			if err != nil {
				c.JSON(http.StatusOK, gin.H{"error": err.Error()})
			} else {
				c.JSON(http.StatusOK, todo)
			}
		})
```



### 修改代办

```go
		// 修改
		v1Group.PUT("todo/:id", func(c *gin.Context) {
			// 获取前端传来的id
			id, ok := c.Params.Get("id")
			if !ok {
				c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
				return
			}
			// 先查询出当前的todo事项，才知道修改成什么
			var todo Todo
			err = DB.Where("id = ?", id).First(&todo).Error

			// 再根据当前todo的status去修改status
			if err != nil {
				c.JSON(http.StatusOK, gin.H{"error": err.Error()})
			} else {
				if todo.Status {
					DB.Model(&todo).Select("status").Update(map[string]interface{}{"status": false})
				} else {
					DB.Model(&todo).Select("status").Update(map[string]interface{}{"status": true})
				}
				// 修改完后返回响应
				c.JSON(http.StatusOK, todo)
			}
		})
```



### 删除代办

```go
		// 删除
		v1Group.DELETE("todo/:id", func(c *gin.Context) {
			// 获取前端传来的id
			id, ok := c.Params.Get("id")
			if !ok {
				c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
				return
			}

			// 删除
			err = DB.Where("id = ?", id).Delete(Todo{}).Error
			if err != nil {
				c.JSON(http.StatusOK, gin.H{"error": err.Error()})
			} else {
				c.JSON(http.StatusOK, gin.H{id: "delete成功"})
			}
		})
```



到此，所有基本功能编写完毕！



## 项目结构拆分

在企业级开发中，我们所有的代码肯定不是都放在一个程序里面的，我们需要对它进行结构拆分，让项目更加清晰，方便后续维护！

整体目录结构：

![image-20220513200927966](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/02783de8f03abd1016255c11c2c07188.png)



### dao

`mysql.go`，MySQL数据库的连接、关闭，初始化DB的操作。

```go
package dao

import "github.com/jinzhu/gorm"
import _ "github.com/jinzhu/gorm/dialects/mysql"

// DB 定义全局变量DB
var (
	DB *gorm.DB
)

// InitMySQL 连接数据库，成功返回nil，失败返回error
func InitMySQL() (err error) {
	dsn := "root:123456@tcp(127.0.0.1:3306)/goweb_todolist?charset=utf8mb4&parseTime=True&loc=Local"

	// 初始化全局变量DB
	DB, err = gorm.Open("mysql", dsn)
	if err != nil {
		return
	}
	return DB.DB().Ping() // 测试连通性再返回,ping得通返回nil，否则返回error
}

func Close() {
	DB.Close()
}
```





### models

`todo.go`，models中的结构体对象和数据库的表对应，有关todo的所有操作。

```go
package models

import (
	"go-web-project-todolist/dao"
)

// Todo Model：代办清单结构体
type Todo struct {
	ID     int    `json:"id"`
	Title  string `json:"title"`
	Status bool   `json:"status"`
}

/*
	Todo这个Model的增删改查操作都放在这里
*/
func AddTodo(todo *Todo) (err error) {
	err = dao.DB.Create(&todo).Error
	return
}

func QueryAllTodos() (todoList []*Todo, err error) {
	if err = dao.DB.Find(&todoList).Error; err != nil {
		return nil, err
	}
	return
}

func QueryTodoById(id string) (todo *Todo, err error) {
	todo = new(Todo)
	if err = dao.DB.Debug().Where("id = ?", id).First(todo).Error; err != nil {
		return nil, err
	}
	return
}

func UpdateTodoStatusById(id string) (err error) {
	// 先查询出当前的todo事项，才知道修改成什么
	var todo Todo
	err = dao.DB.Where("id = ?", id).First(&todo).Error

	// 再根据当前todo的status去修改status
	if todo.Status {
		dao.DB.Model(&todo).Select("status").Update(map[string]interface{}{"status": false})
	} else {
		dao.DB.Model(&todo).Select("status").Update(map[string]interface{}{"status": true})
	}
	return
}

func DeleteTodoById(id string) (err error) {
	err = dao.DB.Where("id = ?", id).Delete(Todo{}).Error
	return
}
```



### controller

`controller.go`，主要与前端对接，负责接收参数和返回响应。

```go
package controller

import (
	"github.com/gin-gonic/gin"
	"go-web-project-todolist/models"
	"net/http"
)

// IndexHandler 主页
func IndexHandler(c *gin.Context) {
	c.HTML(http.StatusOK, "index.html", nil)
}

// AddTodo 添加代办事项
func AddTodo(c *gin.Context) {
	// 前面页面填写代办事项，点击提交，会发请求到这里
	// 1. 从请求中把数据拿出来
	var todo models.Todo
	c.BindJSON(&todo)

	// 2.插入数据库，返回响应
	// 插入数据库失败，状态码为200表示请求成功，把插入失败的error返回
	if err := models.AddTodo(&todo); err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, todo) // 插入成功将该数据返回
		
		/*
			企业中返回信息可能如下：
			c.JSON(http.StatusOK, gin.H{
				"code": 200,
				"msg": "success",
				"data": todo,
			})
		*/
	}
}

// QueryAllTodos 查看所有待办事项
func QueryAllTodos(c *gin.Context) {
	// 从数据库中查询出所有todolist，返回响应
	if todoList, err := models.QueryAllTodos(); err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, todoList)
	}
}

// QueryTodoById 根据id查看某个待办事项
func QueryTodoById(c *gin.Context) {
	// 1. 获取前端传来的参数
	id, ok := c.Params.Get("id")
	if !ok {
		c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
		return
	}

	// 2. 去数据库中查数据，返回响应
	if todo, err := models.QueryTodoById(id); err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, todo)
	}
}

// UpdateTodoStatusById 根据id修改待办事项的状态
func UpdateTodoStatusById(c *gin.Context) {
	// 获取前端传来的id
	id, ok := c.Params.Get("id")
	if !ok {
		c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
		return
	}

	// 再根据当前todo的status去修改status
	if err := models.UpdateTodoStatusById(id); err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		// 修改成功返回响应
		c.JSON(http.StatusOK, gin.H{id: "修改成功"})
	}
}

// DeleteTodoById 根据id删除代办事项
func DeleteTodoById(c *gin.Context) {
	// 获取前端传来的id
	id, ok := c.Params.Get("id")
	if !ok {
		c.JSON(http.StatusOK, gin.H{"error": "接收id失败"})
		return
	}

	// 删除
	if err := models.DeleteTodoById(id); err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, gin.H{id: "delete成功"})
	}
}
```



### routers

`routers.go`，路由层，主要负责请求转发。

```go
package routers

import (
	"github.com/gin-gonic/gin"
	"go-web-project-todolist/controller"
)

func SetupRouter() *gin.Engine {
	route := gin.Default()
	// 告诉gin 模板文件引用的静态文件在哪儿
	route.Static("/static", "static")
	// 告诉gin 模板文件在哪儿
	route.LoadHTMLGlob("templates/*")

	// 主页
	route.GET("/", controller.IndexHandler)

	// v1 路由组
	v1Group := route.Group("v1")
	{
		// 添加
		v1Group.POST("/todo", controller.AddTodo)

		// 查看
		// 查看所有待办事项
		v1Group.GET("/todo", controller.QueryAllTodos)

		// 根据id查看某个待办事项
		v1Group.GET("/todo/:id", controller.QueryTodoById)

		// 根据id修改待办事项的状态
		v1Group.PUT("todo/:id", controller.UpdateTodoStatusById)

		// 根据id删除代办事项
		v1Group.DELETE("todo/:id", controller.DeleteTodoById)
	}
	return route
}
```



在实际开发中，很多业务逻辑是十分复杂的，此时需要另外分出一个 `logic` 逻辑层来处理具体的逻辑服务。