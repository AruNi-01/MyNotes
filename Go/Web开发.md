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