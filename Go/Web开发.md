# Gin 框架

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

为了能够更方便的获取请求相关参数，提高开发效率，我们可以基于请求的`Content-Type`识别请求数据类型并利用反射机制自动提取请求中`QueryString`、`form表单`、`JSON`、`XML`等参数到结构体中。 下面的示例代码演示了`.ShouldBind()`强大的功能，它能够基于请求自动提取`JSON`、`form表单`和`QueryString`类型的数据，并把值绑定到指定的结构体对象。

```go
// Binding from JSON
type Login struct {
	User     string `form:"user" json:"user" binding:"required"`
	Password string `form:"password" json:"password" binding:"required"`
}

func main() {
	router := gin.Default()

	// 绑定JSON的示例 ({"user": "q1mi", "password": "123456"})
	router.POST("/loginJSON", func(c *gin.Context) {
		var login Login

		if err := c.ShouldBind(&login); err == nil {
			fmt.Printf("login info:%#v\n", login)
			c.JSON(http.StatusOK, gin.H{
				"user":     login.User,
				"password": login.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// 绑定form表单示例 (user=q1mi&password=123456)
	router.POST("/loginForm", func(c *gin.Context) {
		var login Login
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&login); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"user":     login.User,
				"password": login.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// 绑定QueryString示例 (/loginQuery?user=q1mi&password=123456)
	router.GET("/loginForm", func(c *gin.Context) {
		var login Login
		// ShouldBind()会根据请求的Content-Type自行选择绑定器
		if err := c.ShouldBind(&login); err == nil {
			c.JSON(http.StatusOK, gin.H{
				"user":     login.User,
				"password": login.Password,
			})
		} else {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	// Listen and serve on 0.0.0.0:8080
	router.Run(":8080")
}
```

`ShouldBind`会按照下面的顺序解析请求中的数据完成绑定：

1. 如果是 `GET` 请求，只使用 `Form` 绑定引擎（`query`）。
2. 如果是 `POST` 请求，首先检查 `content-type` 是否为 `JSON` 或 `XML`，然后再使用 `Form`（`form-data`）。











