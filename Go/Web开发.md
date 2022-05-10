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

![image-20220508200149545](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220508200149545.png)

控制台也会有对应的信息：

![image-20220508200225650](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220508200225650.png)

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

![image-20220509185554444](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220509185554444.png)



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

![image-20220509191950905](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220509191950905.png)



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

![image-20220509200714666](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220509200714666.png)



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

![image-20220509203308939](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220509203308939.png)



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

![image-20220509204007816](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220509204007816.png)



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

![image-20220510110942941](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510110942941.png)

form 表单测试：

![image-20220510111457824](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510111457824.png)

querystring 测试：

![image-20220510111312145](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510111312145.png)



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

![image-20220510114047523](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510114047523.png)

上传成功后，当前项目目录中发现已上传的文件：

![image-20220510114130027](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510114130027.png)

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

![image-20220510120302908](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510120302908.png)



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

![image-20220510203049986](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510203049986.png)



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

![image-20220510205212668](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510205212668.png)

执行过程：

![image-20220510205253265](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510205253265.png)

当中间件中有 `c.Abort()` 或者 `return` 的情况：

![image-20220510205949377](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220510205949377.png)



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



# GORM

## 什么是 ORM

Object-Relationl Mapping：即对象关系映射，这里的Relationl指的是关系型数据库

作用：

- 在关系型数据库和对象之间作一个映射

这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句语句打交道，只要像平时操作对象一样操作它就可以了 。

## GORM 介绍

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

## 安装

```bash
go get -u github.com/jinzhu/gorm
```

## 连接数据库

连接不同的数据库都需要导入对应数据的驱动程序，`GORM`已经贴心的为我们包装了一些驱动程序，只需要按如下方式导入需要的数据库驱动即可：

```go
import _ "github.com/jinzhu/gorm/dialects/mysql"
// import _ "github.com/jinzhu/gorm/dialects/postgres"
// import _ "github.com/jinzhu/gorm/dialects/sqlite"
// import _ "github.com/jinzhu/gorm/dialects/mssql"
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



## GORM 基本示例

**注意:**

- 以下示例以MySQL数据库为例，学习GORM各项功能的主要使用方法。



### 创建数据库

在使用GORM前手动创建数据库`db1`：

```mysql
CREATE DATABASE db1;
```



### GORM 操作 MySQL

使用GORM连接上面的`db1`进行创建、查询、更新、删除操作。

```go
package main

import (
	"fmt"
	"github.com/jinzhu/gorm"
	_ "github.com/jinzhu/gorm/dialects/mysql"
)

// UserInfo 用户信息
type UserInfo struct {
	ID uint
	Name string
	Gender string
	Hobby string
}


func main() {
	db, err := gorm.Open("mysql", "root:root1234@(127.0.0.1:13306)/db1?charset=utf8mb4&parseTime=True&loc=Local")
	if err!= nil{
		panic(err)
	}
	defer db.Close()

	// 自动迁移
	db.AutoMigrate(&UserInfo{})

	u1 := UserInfo{1, "七米", "男", "篮球"}
	u2 := UserInfo{2, "沙河娜扎", "女", "足球"}
	// 创建记录
	db.Create(&u1)
	db.Create(&u2)
	// 查询
	var u = new(UserInfo)
	db.First(u)
	fmt.Printf("%#v\n", u)

	var uu UserInfo
	db.Find(&uu, "hobby=?", "足球")
	fmt.Printf("%#v\n", uu)

	// 更新
	db.Model(&u).Update("hobby", "双色球")
	// 删除
	db.Delete(&u)
}
```

## GORM Model定义

在使用ORM工具时，通常我们需要在代码中定义模型（Models）与数据库中的数据表进行映射，在GORM中模型（Models）通常是正常定义的结构体、基本的go类型或它们的指针。 同时也支持`sql.Scanner`及`driver.Valuer`接口（interfaces）。

### gorm.Model

为了方便模型定义，GORM内置了一个`gorm.Model`结构体。`gorm.Model`是一个包含了`ID`, `CreatedAt`, `UpdatedAt`, `DeletedAt`四个字段的Golang结构体。

```go
// gorm.Model 定义
type Model struct {
  ID        uint `gorm:"primary_key"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt *time.Time
}
```

你可以将它嵌入到你自己的模型中：

```go
// 将 `ID`, `CreatedAt`, `UpdatedAt`, `DeletedAt`字段注入到`User`模型中
type User struct {
  gorm.Model
  Name string
}
```

当然你也可以完全自己定义模型：

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

## 主键、表名、列名的约定

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
//// SELECT * FROM deleted_users;

db.Table("deleted_users").Where("name = ?", "jinzhu").Delete()
//// DELETE FROM deleted_users WHERE name = 'jinzhu';
```

GORM还支持更改默认表名称规则：

```go
gorm.DefaultTableNameHandler = func (db *gorm.DB, defaultTableName string) string  {
  return "prefix_" + defaultTableName;
}
```

### 列名（Column Name）

列名由字段名称进行下划线分割来生成

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



































