# 初识 Go

![img](https://www.runoob.com/wp-content/uploads/2015/06/go128.png)

Go 是一个开源的编程语言，它能让构造简单、可靠且高效的软件变得容易。

Go是从2007年末由Robert Griesemer, Rob Pike, Ken Thompson主持开发，后来还加入了Ian  Lance Taylor, Russ Cox等人，并最终于2009年11月开源，在2012年早些时候发布了Go  1稳定版本。现在Go的开发已经是完全开放的，并且拥有一个活跃的社区。

------

## 1. Go 语言特色

- 简洁、快速、安全
- 并行、有趣、开源
- 内存管理、数组安全、编译迅速

------

## 2. Go 语言用途

Go 语言被设计成一门应用于搭载 Web 服务器，存储集群或类似用途的巨型中央服务器的系统编程语言。

对于高性能分布式系统领域而言，Go 语言无疑比大多数其它语言有着更高的开发效率。它提供了海量并行的支持，这对于游戏服务端的开发而言是再好不过了。



# 开发环境准备

Go 语言支持以下系统：

- Linux
- FreeBSD
- Mac OS X（也称为 Darwin）
- Windows

安装包下载地址：https://golang.org/dl/

国内地址：https://golang.google.cn/dl/

> 安装目录清单

Go 安装目录（`$GOROOT`）的文件夹结构应该如下所示：

README.md, AUTHORS, CONTRIBUTORS, LICENSE

- `/bin`：包含可执行文件，如：编译器，Go 工具
- `/doc`：包含文档模版
- `/lib`：包含示例程序，代码工具，本地文档等
- `/misc`：包含与支持 Go 编辑器有关的配置文件以及 cgo 的示例
- `/os_arch`：包含标准库的包的对象文件（`.a`）
- `/src`：包含源代码构建脚本和标准库的包的完整源代码（Go 是一门开源语言）
- `/src/cmd`：包含 Go 和 C 的编译器和命令行脚本

## 1. Windows 环境

下载安装包到指定目录下，配置环境变量：

1. 系统变量 GOROOT ，变量值为Go目录

   ![image-20220430110143079](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a6bdc6420aa21a706b92ab082f45a378.png)

2. Go的工程目录，新建系统变量 GOPATH，变量值为工程目录：

   ![image-20220430111204206](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/63fca31b32ab8c0315f37bd85d6a2b7a.png)

3. 在用户变量里也修改相应的 GOPATH：

   ![image-20220430111449566](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f76e18422a415349e90c2661a391cead.png)

4. 在 Path 变量中添加Go的 bin 目录

5. 在终端输入 `go env` 命令，查看 GOPATH 和 GOROOT 路径是否正确和 Go 的版本号：

   ![image-20220430112241689](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6a73e1a409214991faec7f34e3db4d92.png)

## 2. GOROOT和GOPATH

`GOROOT`和`GOPATH`都是环境变量，其中`GOROOT`是我们安装go开发包的路径，在Go1.14 及之后的版本中启用了 Go Module 模式之后，不一定非要将代码写到 GOPATH 目录下，所以也就**不需要我们再自己配置 GOPATH** 了，使用默认的即可。

> GOPROXY（非常重要）

Go1.14 版本之后，都推荐使用`go mod`模式来管理依赖环境了，也不再强制我们把代码必须写在`GOPATH`下面的src目录了，你可以在你电脑的任意位置编写go代码。

默认GoPROXY配置是：`GOPROXY=https://proxy.golang.org,direct`，由于国内访问不到`https://proxy.golang.org`，所以我们需要换一个PROXY，这里推荐使用`https://goproxy.io`或`https://goproxy.cn`。

可以执行下面的命令修改 GOPROXY：

```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

## 3. 第一个Go程序

用VS Code编辑器安装`Go`扩展插件，让它支持Go语言开发。

![image-20220430122822728](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/179f2f50350e248464652cbd33e38eb1.png)

现在我们来创建第一个Go项目—`hello`，创建一个`hello`目录。





> go mod init

使用 `go module` 模式新建项目时，我们需要通过 `go mod init 项目名 `命令对项目进行初始化，该命令会在项目根目录下生成`go.mod`文件。例如，我们使用`hello`作为我们第一个Go项目的名称，执行如下命令：

```bash
go mod init hello
```

![image-20220430141109052](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8966771712523d016b97f9250ffa64e2.png)

接下来在该目录中创建一个`main.go`文件：

```go
package main  // 声明 main 包，表明当前是一个可执行程序

import "fmt"  // 导入内置 fmt 包

func main(){  // main函数，是程序执行的入口
	fmt.Println("Hello World!")  // 在终端打印 Hello World!
}
```

如果此时VS Code右下角弹出提示让你安装插件，务必点 **install all** 进行安装。



### 命令

> go build

`go build`命令表示将源代码编译成可执行文件。

在hello目录下执行：

```bash
go build
```

或者在其他目录执行以下命令：

```bash
go build hello
```

go编译器会去 `GOPATH`的src目录下查找你要编译的`hello`项目

编译得到的可执行文件会保存在执行编译命令的当前目录下，如果是windows平台会在当前目录下找到`hello.exe`可执行文件。

可在终端直接执行该`.\hello.exe`文件：

```bash
PS D:\Go\study\hello> .\hello.exe
Hello World!
```

我们还可以使用`-o`参数来指定编译后得到的可执行文件的名字。

```bash
go build -o myhello.exe
```





> go run

`go run hello.go`也可以执行程序，该命令本质上也是先编译再执行。

![image-20220430142211489](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9766536c7e184499c7be322ddbf8e403.png)





> go install

`go install`表示安装的意思，它先编译源代码得到可执行文件，然后将可执行文件移动到`GOPATH`的bin目录下。因为我们的环境变量中配置了`GOPATH`下的bin目录，所以我们就可以在任意地方直接执行可执行文件了。

![image-20220430142355022](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/747b88e35306321cc1a2b29845764049.png)

可以看到 hello 的同级目录下，自动生成了 bin 和 pkg 目录，并且 bin 目录下有 hello.exe可执行文件：

![image-20220430142453035](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0517275262dbbfa10c2567dcbbfcb212.png)

### 目录结构

**bin**

存放编译后可执行的文件。

**pkg**

存放编译后的应用包。

**src**

存放应用源代码。

例如：

```tex
├─ code  -- 代码根目录
│  ├─ bin
│  ├─ pkg
│  ├─ src
│     ├── hello
│         ├── hello.go
```

![image-20220501140644801](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/fc59d1503e3643da54a3615e28e2fdd9.png)

## 4. VS Code配置代码片段快捷键

按`Ctrl+Shift+P`，输入`snippets`，选择命令并执行，然后再输入go：

![image-20220430143304398](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8857ff2909f87fa19eda242451cd7c3f.png)

在空白位置就可以自定义代码片段了，其中`$0`表示最终光标提留的位置：

```js
“这里放个名字”:{
    "prefix": "这个是快捷键",
    "body": "这里是按快捷键插入的代码片段",
    "description": "这里放提示信息的描述"
}
```

示例：创建了两个快捷方式，一个是输入`pln`就会在编辑器中插入`fmt.Println()`代码；输入`plf`，就会插入`fmt.Printf("")`代码。

![image-20220430143944404](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/13780f2b0992293d913ef537bbcd1310.png)

测试：

![image-20220430144036544](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/941768f78b8f3d067dbd8df197b92437.png)

# 语言结构

在我们开始学习 Go 编程语言的基础构建模块前，让我们先来了解 Go 语言最简单程序的结构。

### Hello World 实例

Go 语言的基础组成有以下几个部分：

- 包声明
- 引入包
- 函数
- 变量
- 语句 & 表达式
- 注释

接下来让我们来看下简单的代码，该代码输出了 "Hello World" ：

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World!")
}
```

让我们来看下以上程序的各个部分：

- `package main`：声明了 main.go 所在的包，Go 语言中使用包来组织代码。一般一个文件夹即一个包，包内可以暴露类型或方法供其他包使用
- `import “fmt”`：fmt 是 Go 语言的一个标准库/包，用来处理标准输入输出
- `func main`：main 函数是整个程序的入口，main 函数所在的包名也必须为 `main`
- `fmt.Println(“Hello World!”)`：调用 fmt 包的 Println 方法，打印出 “Hello World!”



> 注意：`{` 不能单独放在一行





# 数据类型

### 字符串

`string`：只能用一对双引号（""）或反引号（``）括起来定义，**不能用单引号**（''）定义

### 布尔类型

`bool`：只有 true 和 false，默认为 false。

### 数字类型

Go 也有基于架构的类型，例如：int、uint 和 uintptr。

| 序号 | 类型和描述                                                   |
| ---- | ------------------------------------------------------------ |
| 1    | **uint8** 无符号 8 位整型 (0 到 255)                         |
| 2    | **uint16** 无符号 16 位整型 (0 到 65535)                     |
| 3    | **uint32** 无符号 32 位整型 (0 到 4294967295)                |
| 4    | **uint64** 无符号 64 位整型 (0 到 18446744073709551615)      |
| 5    | **int8** 有符号 8 位整型 (-128 到 127)                       |
| 6    | **int16** 有符号 16 位整型 (-32768 到 32767)                 |
| 7    | **int32** 有符号 32 位整型 (-2147483648 到 2147483647)       |
| 8    | **int64** 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

`int` `uint`：未指定长度则其长度取决于 CPU 位数。

### 浮点型

| 序号 | 类型和描述                        |
| ---- | --------------------------------- |
| 1    | **float32** IEEE-754 32位浮点型数 |
| 2    | **float64** IEEE-754 64位浮点型数 |
| 3    | **complex64** 32 位实数和虚数     |
| 4    | **complex128** 64 位实数和虚数    |

------

### 其他数字类型

以下列出了其他更多的数字类型：

| 序号 | 类型和描述                               |
| ---- | ---------------------------------------- |
| 1    | **byte** 类似 uint8                      |
| 2    | **rune** 类似 int32                      |
| 3    | **uint** 32 或 64 位                     |
| 4    | **int** 与 uint 一样大小                 |
| 5    | **uintptr** 无符号整型，用于存放一个指针 |



# 常量与变量声明

### 常量声明

**常量**：在程序**编译阶段**就确定下来的值，而程序在**运行时无法改变**该值。

**单个常量声明**：

第一种：`const 变量名称 数据类型 = 变量值`

如果不赋值，使用的是该数据类型的默认值。

第二种：`const 变量名称 = 变量值`

根据变量值，自行判断数据类型。

**多个常量声明**：

第一种：`const 变量名称,变量名称 ... ,数据类型 = 变量值,变量值 ...`

第二种：`const 变量名称,变量名称 ...  = 变量值,变量值 ...`

**测试代码**：

```go
//demo_1.go
package main

import "fmt"

func main() {
	const name string = "Tom"
	fmt.Println(name)

	const age = 30
	fmt.Println(age)

	const name_1, name_2 string = "Tom", "Jay"
	fmt.Println(name_1, name_2)

	const name_3, age_1 = "Tom", 30
	fmt.Println(name_3, age_1)
}
```

运行结果：

```text
Tom
30
Tom Jay
Tom 30
```

### iota

`iota`是`go`语言的常量计数器，只能在常量的表达式中使用。 `iota`在`const`关键字出现时将被重置为`0`。`const`中每新增一行常量声明将使`iota`计数一次(`iota`可理解为`const`语句块中的行索引)。 使用`iota`能简化定义，在定义枚举时很有用。

举个例子：

```go
const (
    n1 = iota //0
    n2        //1
    n3        //2
    n4        //3
)
```





> 几个常见的iota示例

使用`_`跳过某些值：

```go
const (
    n1 = iota // 0
    n2        // 1
    _
    n4        // 3
)
```

`iota`声明中间插队：

```go
const (
    n1 = iota // 0
    n2 = 100  // 独立值100，iota += 1
    n3		// 100，iota +=1
    n4 = iota // 3
    n5        // 4
)
const n6 = iota //0
```

定义数量级： （这里的`<<`表示左移操作，`1<<10`表示将`1`的二进制表示向左移`10`位，也就是由`1`变成了`10000000000`，也就是十进制的`1024`。同理`2<<2`表示将`2`的二进制表示向左移`2`位，也就是由`10`变成了`1000`，也就是十进制的`8`。）

注：`x << n == x*(2^n)`

```go
const (
    _  = iota
    KB = 1 << (10 * iota)
    MB = 1 << (10 * iota)
    GB = 1 << (10 * iota)
    TB = 1 << (10 * iota)
    PB = 1 << (10 * iota)
)
```

多个`iota`定义在一行：

```go
const (
    a, b = iota + 1, iota + 2  // 1,2
    c, d                      // 2,3
    e, f                      // 3,4
)
```



### 变量声明

**单个变量声明**：

第一种：`var 变量名称 数据类型 = 变量值`

如果不赋值，使用的是该数据类型的默认值。

第二种：`var 变量名称 = 变量值`

根据变量值，自行判断数据类型。

第三种：`变量名称 := 变量值`

省略了 var 和数据类型，变量名称一定要是未声明过的。

**多个变量声明**

第一种：`var 变量名称,变量名称 ... ,数据类型 = 变量值,变量值 ...`

第二种：`var 变量名称,变量名称 ...  = 变量值,变量值 ...`

第三种：`变量名称,变量名称 ... := 变量值,变量值 ...`

**测试代码**：

```go
//demo_2.go
package main

import "fmt"

func main() {
	var age_1 uint8 = 31
	var age_2 = 32
	age_3 := 33
	fmt.Println(age_1, age_2, age_3)

	var age_4, age_5, age_6 int = 31, 32, 33
	fmt.Println(age_4, age_5, age_6)

	var name_1, age_7 = "Tom", 30
	fmt.Println(name_1, age_7)

	name_2, is_boy, height := "Jay", true, 180.30
	fmt.Println(name_2, is_boy, height)
}
```

运行结果：

```text
31 32 33
31 32 33
Tom 30
Jay true 180.3
```

### 输出方法

**fmt.Print**：输出到控制台（仅只是输出）。

**fmt.Println**：输出到控制台并换行。

**fmt.Printf**：仅输出格式化的字符串和字符串变量（整型和整型变量不可以）。[详细的格式化参数](https://blog.csdn.net/qq_34777600/article/details/81266453)

**fmt.Sprintf**：格式化并返回一个字符串，不输出。

Go 字符串格式化符号：

| 格  式 | 描  述                                   |
| ------ | ---------------------------------------- |
| %v     | 按值的本来值输出                         |
| %+v    | 在 %v 基础上，对结构体字段名和值进行展开 |
| %#v    | 输出 Go 语言语法格式的值                 |
| %T     | 输出 Go 语言语法格式的类型和值           |
| %%     | 输出 % 本体                              |
| %b     | 整型以二进制方式显示                     |
| %o     | 整型以八进制方式显示                     |
| %d     | 整型以十进制方式显示                     |
| %x     | 整型以十六进制方式显示                   |
| %X     | 整型以十六进制、字母大写方式显示         |
| %U     | Unicode 字符                             |
| %f     | 浮点数                                   |
| %p     | 指针，十六进制方式显示                   |

**测试代码**：

```go
//demo_3.go
package main

import (
	"fmt"
)

func main() {
	fmt.Print("输出到控制台不换行")
	fmt.Println("---")
	fmt.Println("输出到控制台并换行")
	fmt.Printf("name=%s,age=%d\n", "Tom", 30)
	fmt.Printf("name=%s,age=%d,height=%v\n", "Tom", 30, fmt.Sprintf("%.2f", 180.567))
}
```

运行结果：

```text
输出不换行---
输出换行
name=Tom, age=30
name=Tom, age=30, height=180.34
```

# 数组

### 概述

数组是一个由固定长度的特定类型元素组成的序列，一个数组可以由零个或多个元素组成，一旦声明了，数组的长度就固定了，不能动态变化。

在数组中由于长度固定不可变，因此`len(arr)`和`cap(arr)`的输出永远相同。

- `len()`：查看数组或 slice(切片) 的长度
- `cap()`：查看数组或 slice(切片) 的容量

### 声明数组

```go
// demo_4
package main

import "fmt"

func main() {
	// 一维数组
	var arr_1 [5]int
	fmt.Println(arr_1)

	var arr_2 = [5]int{1, 2, 3, 4, 5}
	fmt.Println(arr_2)

	arr_3 := [5]int{1, 2, 3, 4, 5}
	fmt.Println(arr_3)

	// 数组长度不确定，使用... 代替数组的长度，编译器会根据元素个数自行推断数组的长度
	arr_4 := [...]int{1, 2, 3, 4, 5}
	fmt.Println(arr_4)

	// 通过指定下标来初始化元素
	arr_5 := [5]int{0: 3, 1: 5, 4: 6}
	fmt.Println(arr_5)

	// 二维数组
	var arr_6 = [3][5]int{{1, 2, 3, 4, 5}, {9, 8, 7, 6, 5}, {3, 4, 5, 6, 7}}
	fmt.Println(arr_6)

	arr_7 := [3][5]int{{1, 2, 3, 4, 5}, {9, 8, 7, 6, 5}, {3, 4, 5, 6, 7}}
	fmt.Println(arr_7)

	arr_8 := [...][5]int{{1, 2, 3, 4, 5}, {0: 3, 1: 5, 4: 6}}
	fmt.Println(arr_8)
}
```

运行结果：

```text
[0 0 0 0 0]
[1 2 3 4 5]
[1 2 3 4 5]
[1 2 3 4 5]
[3 5 0 0 6]
[[1 2 3 4 5] [9 8 7 6 5] [3 4 5 6 7]]
[[1 2 3 4 5] [9 8 7 6 5] [3 4 5 6 7]]
[[1 2 3 4 5] [3 5 0 0 6]]
```

### 注意事项

一、**数组不可动态变化**问题，一旦声明了，其长度就是固定的。

```go
var arr_1 = [5] int {1, 2, 3, 4, 5}
arr_1[5] = 6	// arr_1数组只有5个，给第6个下标赋值，报错
fmt.Println(arr_1)
```

会报错：`invalid array index 5 (out of bounds for 5-element array)`





二、在**函数中传递的时候是传递的值**时，如果传递数组很大，这对内存是很大开销。

示例：

```go
//demo_5.go
package main

import "fmt"

func main() {
	var arr = [5] int {1,2,3,4,5}
	modifyArr(arr)	// 传递的是值，不会改变原来的值
	fmt.Println(arr)
}

func modifyArr(a [5] int) {
	a[1] = 20
}
```

运行结果：

```text
[1 2 3 4 5]
```

使用引用类型传递地址：

```go
//demo_6.go
package main

import "fmt"

func main() {
	var arr = [5] int {1, 2, 3, 4, 5}
	modifyArr(&arr)		// 引用传递地址，会改变原来的值
	fmt.Println(arr)
}

func modifyArr(a *[5] int) {
	a[1] = 20
}
```

运行结果：

```text
[1 20 3 4 5]
```





三、数组赋值问题，**同样类型的数组**（长度一样且每个元素类型也一样）才可以相互赋值，反之不可以。

```go
var arr =  [5] int {1, 2, 3, 4, 5}
var arr_1 [5] int = arr		// 正确
var arr_2 [6] int = arr		// 错误
```

运行会报错：`Cannot use 'arr1' (type [5]int) as the type [6]int`

# Slice 切片

### 概述

切片是一种**动态数组**，比数组操作灵活，长度不是固定的，可以进行**追加和删除**。

`len()` 和 `cap()` 返回结果可相同和不同。

- `len()` ：查看数组或 slice 的长度
- `cap()`：查看数组或 slice 的容量

### 定义切片

你可以声明一个未指定大小的数组来定义切片：

```go
var identifier []type
```

切片不需要说明长度。

或使用 **make()** 函数来创建切片:

```go
var slice1 []type = make([]type, len)
// 也可以简写为
slice1 := make([]type, len)
```

也可以指定容量，其中 **capacity** 为可选参数。

```go
make([]T, length, capacity)
```



示例：

```go
//demo_7.go
package main

import "fmt"

func main() {
	var sli_1 []int // 默认 nil切片
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_1), cap(sli_1), sli_1)

	var sli_2 = []int{} // 空切片
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_2), cap(sli_2), sli_2)

	var sli_3 = []int{1, 2, 3, 4, 5}
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_3), cap(sli_3), sli_3)

	sli_4 := []int{1, 2, 3, 4, 5}
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_4), cap(sli_4), sli_4)

	// 使用make函数定义切片
	var sli_5 []int = make([]int, 5, 8)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_5), cap(sli_5), sli_5)

	sli_6 := make([]int, 5)
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli_6), cap(sli_6), sli_6)
}
```

运行结果：

```text
len=0 cap=0 slice=[]
len=0 cap=0 slice=[]
len=5 cap=5 slice=[1 2 3 4 5]
len=5 cap=5 slice=[1 2 3 4 5]
len=5 cap=8 slice=[0 0 0 0 0]
len=5 cap=5 slice=[0 0 0 0 0]
```

### 截取切片

可以通过设置下限及上限来设置截取切片 `[lower-bound:upper-bound]`

注意：[] 区间**左闭右开**

示例：

```go
//demo_8.go
package main

import "fmt"

func main() {
	sli := []int{1, 2, 3, 4, 5}
	fmt.Printf("len=%d cap=%d slice=%d\n", len(sli), cap(sli), sli)

	// 切指定位置
	fmt.Println("sli[1] == ", sli[1])
	// 从头切到尾
	fmt.Println("sli[:] == ", sli[:])
	// 从1切到最尾部
	fmt.Println("sli[1:] == ", sli[1:])
	// 从最开头切到4(不包含)
	fmt.Println("sli[:4] == ", sli[:4])

	// 子切片从0(包含)到3(不包含) ——> index 0,1,2
	fmt.Println("sli[0:3] == ", sli[0:3])
	fmt.Printf("len=%d cap=%d slic=%v\n", len(sli[0:3]), cap(sli[0:3]), sli[0:3])

	// 子切片从0到3(不包含)，4控制切片的cap容量(4-1)
	fmt.Println("sli[0:3:4] == ", sli[0:3:4])
	fmt.Printf("len=%d cap=%d slice=%v\n", len(sli[0:3:4]), cap(sli[0:3:4]), sli[0:3:4])
}
```

运行结果：

```text
len=5 cap=5 slice=[1 2 3 4 5]
sli[1] ==  2
sli[:] ==  [1 2 3 4 5]
sli[1:] ==  [2 3 4 5]
sli[:4] ==  [1 2 3 4]
sli[0:3] ==  [1 2 3]
len=3 cap=5 slic=[1 2 3]
sli[0:3:4] ==  [1 2 3]
len=3 cap=4 slice=[1 2 3]
```



### 追加切片

append 追加切片时，容量不够需要扩容时，cap 会翻倍。

```go
//demo_9.go
package main

import "fmt"

func main() {
	sli := []int{4, 5, 6}
	printSlice(sli)

	// cap不够，翻倍扩容
	sli = append(sli, 7)
	printSlice(sli)

	sli = append(sli, 8)
	printSlice(sli)

	sli = append(sli, 9)
	printSlice(sli)

	// cap不够，翻倍扩容
	sli = append(sli, 10)
	printSlice(sli)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d slice=%v\n", len(s), cap(s), s)
}
```

运行结果：

```text
len=3 cap=3 slice=[4 5 6]
len=4 cap=6 slice=[4 5 6 7]
len=5 cap=6 slice=[4 5 6 7 8]
len=6 cap=6 slice=[4 5 6 7 8 9]
len=7 cap=12 slice=[4 5 6 7 8 9 10]
```

### 复制切片

Go语言的内置函数 `copy()` 可以将一个数组切片复制到另一个数组切片中，如果加入的两个数组切片不一样大，就会按照其中较小的那个数组切片的元素个数进行复制。

`copy()` 函数的使用格式：`copy( destSlice, srcSlice []T) int`

- srcSlice 为数据来源切片
- destSlice 为复制的目标（也就是将 srcSlice 复制到  destSlice）
- 目标切片必须分配过空间且足够承载复制的元素个数，并且来源和目标的类型必须一致
- `copy()` 函数的返回值表示**实际发生复制的元素个数**

下面的代码展示了使用 `copy()` 函数将一个切片复制到另一个切片的过程：

```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := []int{5, 4, 3}
copy(slice2, slice1) 		  // 只会复制slice1的前3个元素到slice2中
copy(slice1, slice2)		 // 只会复制slice2的3个元素到slice1的前3个位置
```

虽然通过循环复制切片元素更直接，不过内置的 `copy()` 函数使用起来更加方便，`copy()` 函数的第一个参数是要复制的目标 slice，第二个参数是源 slice，两个 slice 可以共享同一个底层数组，甚至有重叠也没有问题。

### 删除切片

#### 从开头位置删除

删除开头的元素可以直接移动数据指针：

```go
a := []int {1, 2, 3}
a := a[1:] 		// 删除开头1个元素
a := a[N:] 		// 删除开头N个元素
```

也可以不移动数据指针，但是**将后面的数据向开头移动**，可以用 `append()` 原地完成（所谓原地完成是指在原有的切片数据对应的内存区间内完成，不会导致内存空间结构的变化）：

```go
a := []int {1, 2, 3}
a := append(a[:0], a[1:]...)		 // 删除开头1个元素
a := append(a[:0], a[N:]...) 		// 删除开头N个元素
```

还可以用 `copy()` 函数来删除开头的元素，`copy()`的返回值为实际复制的元素个数：

```go
a := []int {1, 2, 3}
// copy(a, a[1:]) 将 a 的第1个元素删除，返回2；a[:1] 刚好是删除完后切片的
a := a[:copy(a, a[1:])] 		// 删除开头1个元素
a := a[:copy(a, a[N:])] 		// 删除开头N个元素
```

#### 从中间位置删除

对于删除中间的元素，需要对剩余的元素进行一次整体挪动，同样可以用 append 或 copy 原地完成：

```go
a := []int {1, 2, 3, 4, 5, 6, 7, 8}	
a := append(a[:3], a[3+2:]...) 		// 删除中间2个元素
a := a[:3+copy(a[3:], a[3+2:])] 	// 删除中间2个元素
```

#### 从尾部删除

```go
a = []int {1, 2, 3}
a = a[:len(a)-1] 		  // 删除尾部1个元素
a = a[:len(a)-N]		 // 删除尾部N个元素
```

删除开头的元素和删除尾部的元素都可以认为是删除中间元素操作的特殊情况。

示例：删除切片指定区间元素

```go
// demo_10
package main

import "fmt"

func main() {
	sli := []string{"a", "b", "c", "d", "e"}

	// 指定删除区间的位置(从哪儿开始删)
	index := 2

	// 查看删除区间之前的元素和之后的元素，index+2：跳过了从index位置开始的2个元素
	fmt.Println(sli[:index], sli[index+2:])

	// 将删除区间前后的元素连接起来
	sli = append(sli[:index], sli[index+2:]...)
	fmt.Println(sli)
}
```

输出：

```text
[a b] [e]
[a b e]
```

代码的删除过程可以使用下图来描述：

![img](http://c.biancheng.net/uploads/allimg/180813/1-1PQ3164544530.jpg)

Go语言中删除切片元素的本质：

- 以被删除元素为分界点，将前后两个部分的内存重新连接起来。



# 指针

### 概念

区别于C/C++中的指针，Go语言中的指针不能进行偏移和运算，是安全指针。

要搞明白Go语言中的指针需要先知道3个概念：指针地址、指针类型和指针取值。

Go语言中的函数传参都是值拷贝，当我们想要修改某个变量的时候，我们可以创建一个指向该变量地址的指针变量。传递数据使用指针，而无须拷贝数据。类型指针不能进行偏移和运算。Go语言中的指针操作非常简单，只需要记住两个符号：`&`（取地址）和`*`（根据地址取值）。

### 指针地址和指针类型

每个变量在运行时都拥有一个地址，这个地址代表变量在内存中的位置。Go语言中使用&字符放在变量前面对变量进行“取地址”操作。 Go语言中的值类型`（int、float、bool、string、array、struct）`都有对应的指针类型，如：`*int、*int64、*string`等。

取变量指针的语法如下：

- `ptr := &T`
- T 为类型，可以是结构体、整型、字符串等。
- ptr：用于接收地址的变量，ptr的类型就为`*T`，称做T的指针类型。`*`代表指针。

举个例子：

```go
func main() {
    a := 10
    b := &a
    fmt.Printf("a:%d ptr:%p\n", a, &a) 	// a:10 ptr:0xc00001a078
    fmt.Printf("b:%p type:%T\n", b, b) 	// b:0xc00001a078 type:*int
    fmt.Println(&b)                    // 0xc00000e018
}
```

`b := &a`的图示：

![image-20220502192740753](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/00d90b4d5c59d7a16ea79e0a58ad2dd5.png)

### 指针取值

在对普通变量使用`&`操作符取地址后会获得这个变量的指针，然后可以对指针使用`*`操作，也就是指针取值，代码如下：

```go
// demo_22
package main

import "fmt"

func main() {
	a := 10
	b := &a // 取变量a的地址，将指针保存到b中
	fmt.Printf("type of b: %T\n", b)
	c := *b // 指针取值（根据指针去内存取值）
	fmt.Printf("type of c: %T\n", c)
	fmt.Printf("value of c: %v\n", c)
}
```

输出：

```text
type of b: *int
type of c: int
value of c: 10
```

总结： 

取地址操作符&和取值操作符`*`是一对互补操作符，`&`取出地址，`*`根据地址取出地址指向的值。

变量、指针地址、指针变量、取地址、取值的相互关系和特性如下：

1. 对变量进行取地址（&）操作，可以获得这个变量的指针变量。
2. 指针变量的值是指针地址。
3. 对指针变量进行取值（*）操作，可以获得指针变量指向的原变量的值。

> 当一个指针被定义后没有分配到任何变量时，它的值为 nil



### new 和 make

在Go语言中对于**引用类型**的变量，我们在使用的时候**不仅要声明它**，还要为它**分配内存空间**，否则我们的值就没办法存储。

而对于值类型的声明不需要分配内存空间，是因为它们在声明的时候已经默认分配好了内存空间。

要分配内存，就要使用`new`和`make`这两个内建函数，主要用来分配内存。

#### new

new是一个内置的函数，它的函数签名如下：

- `func new(Type) *Type`

- `Type`表示类型，new函数只接受一个参数，这个参数是一个类型
- `*Type`表示类型指针，new函数返回一个指向该类型内存地址的指针

new函数不太常用，使用new函数得到的是一个**类型的指针**，并且该指针对应的值为该类型的零值。举个例子：

```go
func main() {
    // 只是声明了一个指针变量a但是没有初始化
    var a *int
    // 指针作为引用类型需要初始化后才会拥有内存空间，才可以给它赋值
    a = new(int)
    *a = 10
    fmt.Println(*a)	// 10
    
    a := new(int)
    b := new(bool)
    fmt.Printf("%T\n", a) // *int
    fmt.Printf("%T\n", b) // *bool
    fmt.Println(*a)       // 0
    fmt.Println(*b)       // false
}
```



#### make

make也是用于内存分配的，区别于new，它只用于slice、map以及chan的内存创建，而且它**返回的类型**就是这三个**类型本身**，而不是他们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。

make函数的函数签名如下：

- `func make(t Type, size ...IntegerType) Type`

make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。

```go
func main() {
    // 只是声明变量b是一个map类型的变量
    var b map[string]int
    // 使用make函数进行初始化操作之后，才能对其进行键值对赋值
    b = make(map[string]int, 10)
    b["测试"] = 100
    fmt.Println(b)	// map[测试:100]
}
```

### new 和 make 的区别

1. 二者都是用来做内存分配的
2. make只用于slice、map以及channel的初始化，返回的还是这三个引用类型本身
3. 而new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针







# Struct 结构体

### 概述

Go语言中没有 “类” 的概念，也不支持 “类” 的继承等面向对象的概念。Go语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。

Go语言可以通过自定义的方式形成新的类型，结构体就是这些类型中的一种复合类型，结构体是由零个或多个任意类型的值聚合成的实体，每个值都可以称为结构体的成员。

### 声明和实例化

结构体的定义格式如下：

```go
type 类型名 struct {
    字段1 字段1类型
    字段2 字段2类型
    …
}
```

基本实例化形式：

```go
var ins T		// T 为结构体类型，ins为结构体的实例
```





> 匿名结构体

匿名结构体没有类型名称，无须通过 type 关键字定义就可以直接使用。



示例：

```go
// demo_11
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func main() {
	var p1 Person
	p1.Name = "Tom"
	p1.Age = 18
	fmt.Println("p1 = ", p1)

	var p2 = Person{Name: "Jerry", Age: 20}
	fmt.Println("p2 = ", p2)

	p3 := Person{Name: "Jack", Age: 22}
	fmt.Println("p3 = ", p3)

	// 匿名结构体
	p4 := struct {
		Name string
		Age  int
		Sex  string
	}{Name: "匿名", Age: 32, Sex: "男"}
	fmt.Println("p4 = ", p4)
}
```

输出：

```text
p1 =  {Tom 18}
p2 =  {Jerry 20}
p3 =  {Jack 22}
p4 =  {匿名 32 男}
```

### 创建指针类型的结构体

Go语言中，还可以使用 new 关键字对类型（包括结构体、整型、浮点数、字符串等）进行实例化，结构体在实例化后会形成指针类型的结构体。

使用 new 的格式如下：

```go
ins := new(T)
```

其中：

- T 为类型，可以是结构体、整型、字符串等。
- ins：T 类型被实例化后保存到 ins 变量中，ins 的类型为 *T，属于指针。

Go语言让我们可以像访问普通结构体一样使用`.`来访问结构体指针的成员。





> 取结构体的地址实例化

在Go语言中，对结构体进行`&`取地址操作时，视为对该类型进行一次 new 的实例化操作，取地址格式如下：

```go
ins := &T{}
```

其中：

- T 表示结构体类型。
- ins 为结构体的实例，类型为 *T，是指针类型。



示例：

定义一个玩家（Player）的结构，玩家拥有名字、生命值和魔法值

```go
// demo_12
package main

import "fmt"

type Player struct {
	Name        string
	HealthPoint int
	MagicPoint  int
}

func main() {
	//tank := new(Player)
	//tank.Name = "YaSe"
	//tank.HealthPoint = 100
	//tank.MagicPoint = 70
	tank := &Player{ // 与上面new等价
		Name:        "YaSe",
		HealthPoint: 100,
		MagicPoint:  70,
	}
	fmt.Println("tank = ", tank)   // 取指针
	fmt.Println("*tank = ", *tank) // 取指针的值
	fmt.Println("&tank = ", &tank) // 取地址
}
```

输出：

```text
tank =  &{YaSe 100 70}
*tank =  {YaSe 100 70}
&tank =  0xc000006028
```





> Go语言和 C/C++

在 C/C++ 语言中，使用 new 实例化类型后，访问其成员变量时必须使用`->`操作符。

在Go语言中，访问结构体指针的成员变量时可以继续使用`.`，这是因为Go语言为了方便开发者访问结构体指针的成员变量，使用了语法糖（Syntactic sugar）技术，将 `ins.Name` 形式转换为 `(*ins).Name`。



### 构造函数

Go语言的结构体没有构造函数，我们可以自己实现。 例如，下方的代码就实现了一个`person`的构造函数。 

因为`struct`是值类型，如果结构体比较复杂的话，值拷贝性能开销会比较大，所以该构造函数返回的是结构体指针类型。

```go
func newPlayer(name string, healthPoint, magicPoint int) *Player {
	return &person{
		Name: name,
        	HealthPoint: healthPoint,
        	MagicPoint: magicPoint,
	}
}
```

调用构造函数

```go
p9 := newPlayer("张三", 100, 90)
fmt.Printf("%#v\n", p9) 	//&main.person{Name:"张三", HealthPoint:100, MagicPoint:90}
```



### 嵌套结构体

一个结构体中可以嵌套包含另一个结构体或结构体指针，就像下面的示例代码那样。

```go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address Address
}

func main() {
	user1 := User{
		Name:   "小王子",
		Gender: "男",
		Address: Address{
			Province: "山东",
			City:     "威海",
		},
	}
	fmt.Printf("user1=%#v\n", user1)
        //user1=main.User{Name:"小王子", Gender:"男", Address:main.Address{Province:"山东", City:"威海"}}
}
```

### 嵌套匿名字段

上面user结构体中嵌套的`Address`结构体也可以采用匿名字段的方式，例如：

```go
//Address 地址结构体
type Address struct {
	Province string
	City     string
}

//User 用户结构体
type User struct {
	Name    string
	Gender  string
	Address //匿名字段
}

func main() {
	var user2 User
	user2.Name = "小王子"
	user2.Gender = "男"
	user2.Address.Province = "山东"    // 匿名字段默认使用类型名作为字段名
	user2.City = "威海"                // 匿名字段可以省略
	fmt.Printf("user2=%#v\n", user2) 
        //user2=main.User{Name:"小王子", Gender:"男", Address:main.Address{Province:"山东", City:"威海"}}
}
```

当访问结构体成员时会先在结构体中查找该字段，找不到再去嵌套的匿名字段中查找。

### 嵌套结构体的字段名冲突

嵌套结构体内部可能存在相同的字段名。在这种情况下为了避免歧义需要通过指定具体的内嵌结构体字段名。

```go
//Address 地址结构体
type Address struct {
	Province   string
	City       string
	CreateTime string
}

//Email 邮箱结构体
type Email struct {
	Account    string
	CreateTime string
}

//User 用户结构体
type User struct {
	Name   string
	Gender string
	Address
	Email
}

func main() {
	var user3 User
	user3.Name = "沙河娜扎"
	user3.Gender = "男"
	// user3.CreateTime = "2019" //ambiguous selector user3.CreateTime
	user3.Address.CreateTime = "2000" //指定Address结构体中的CreateTime
	user3.Email.CreateTime = "2000"   //指定Email结构体中的CreateTime
}
```



### 结构体的 “继承”

Go语言中使用结构体也可以实现其他编程语言中面向对象的继承。

```go
//Animal 动物
type Animal struct {
	name string
}

func (a *Animal) move() {
	fmt.Printf("%s会动！\n", a.name)
}

//Dog 狗
type Dog struct {
	Feet    int8
	*Animal //通过嵌套匿名结构体实现继承
}

func (d *Dog) wang() {
	fmt.Printf("%s会汪汪汪~\n", d.name)
}

func main() {
	d1 := &Dog{
		Feet: 4,
		Animal: &Animal{ //注意嵌套的是结构体指针
			name: "乐乐",
		},
	}
	d1.wang() //乐乐会汪汪汪~
	d1.move() //乐乐会动！
}
```



### 结构体字段的可见性

结构体中字段**大写开头**表示可**公开访问**，**小写**表示**私有**（仅在定义当前结构体的包中可访问）。

# JSON 序列化

### 概述

JSON（JavaScript Object Notation）是一种简单的数据交换格式。在语法上它类似于 JavaScript 的对象和列表。它最常用于 Web 后端和在浏览器中运行的 JavaScript 程序之间的通信，但它也用于许多其他地方。

使用 [json package](https://go.dev/pkg/encoding/json/) 包，可以轻松地从 Go 程序中读取和写入 JSON 数据。

### 序列化

 Go 中使用 `json.Marshal()` 序列化数据结构成 JSON字符串：

- `func Marshal(v interface{}) ([]byte, error)`

下面给出一个`Message`结构体并且实例化该结构体：

```go
type Message struct {
    Name string
    Body string
    Time int64
}

m := Message{"Alice", "Hello", 1294706395881547000}
```

使用 `json.Marshal` 序列化这个m：

```go
b, err := json.Marshal(m)
```

如果一切顺利，err 将为 nil，b 将是包含此 JSON 数据的 []byte ：

```go
b == []byte(`{"Name":"Alice","Body":"Hello","Time":1294706395881547000}`)
```

只有可以表示为有效 JSON 的数据结构才会被序列化：

- JSON 对象只支持字符串作为键；要对 Go 地图类型进行序列化，它必须采用 `map[string]T` 的形式（其中 T 是 json 包支持的任何 Go 类型）

- Channel、complex 和函数类型无法序列化

- 不支持循环数据结构；它们将导致 `Marshal` 进入无限循环

- 指针将被序列化为它们指向的值（如果指针为 `nil`，则为 “null”）

### 反序列化

Go 中使用 `json.Unmarshal()` 反序列化 JSON字符串 成对应的数据结构：

- `func Unmarshal(data []byte, v interface{}) error`

首先必须创建一个存储反序列化数据的地方：

```go
var m Message
```

然后调用 `json.Unmarshal`，将 JSON 数据的 `[]byte` 和指向 m 的**指针**传递给它

```go
err := json.Unmarshal(b, &m)
```

如果 b 包含适合 m 的有效 JSON（反序列化成功），则在调用后 err 将为 nil 并且来自 b 的数据将存储在 struct m 中，就像通过如下赋值：

```go
m = Message{
    Name: "Alice",
    Body: "Hello",
    Time: 1294706395881547000,
}
```

当 JSON 数据的结构与 Go 类型不完全匹配时会发生什么？

```go
b := []byte(`{"Name":"Bob","Food":"Pickle"}`)
var m Message
err := json.Unmarshal(b, &m)
```

`Unmarshal` 将仅反序列化它可以在目标类型中找到的字段。在这种情况下，只会填充 m 的 Name 字段，而忽略 Food 字段。

当希望从大型 JSON blob(大对象) 中仅选择几个特定字段时，此行为特别有用。这也意味着目标结构中任何未导出的字段都不会受到 `Unmarshal` 的影响。

但是，如果您事先不知道 JSON 数据的结构怎么办？

使用 带接口的通用 JSON，具体查看官方文档：https://go.dev/blog/json



> 序列化与反序列化的简单示例

```go
// demo_13
package main

import (
	"encoding/json"
	"fmt"
)

type Result struct {
	Code    int    `json:"code"`
	Message string `json:"message"`
}

func main() {
	var res Result
	res.Code = 200
	res.Message = "success"

	// 序列化
	// Marshal 成功时 errs为nil(错误为空)
	jsons, errs := json.Marshal(res)

	if errs != nil { // 序列化失败
		fmt.Println("json marshal error: ", errs)
	}

	// jsons 是[]byte类型，转化成string类型便于查看
	fmt.Println("json data: ", string(jsons))

	// 反序列化
	var res2 Result
	// Unmarshal的第一个参数是json字符串，第二个参数是接受json解析的数据结构
	// 第二个参数必须是指针，否则无法接收解析的数据
	errs = json.Unmarshal(jsons, &res2)

	if errs != nil { // 反序列化失败
		fmt.Println("json unmarshal error: ", errs)
	}

	fmt.Println("res2: ", res2)
}
```

输出：

```text
json data:  {"code":200,"message":"success"}
res2:  {200 success}
```

### 改变数据

```go
// demo_14
package main

import (
	"encoding/json"
	"fmt"
)

type Result struct {
	Code    int    `json:"code"`
	Message string `json:"message"`
}

func main() {
	var res Result
	res.Code = 200
	res.Message = "success"
	toJson(&res)

	// 改变res的值
	setData(&res)
	toJson(&res)
}

func setData(res *Result) {
	res.Code = 500
	res.Message = "fail"
}

func toJson(res *Result) {
	jsons, errs := json.Marshal(res)
	if errs != nil {
		fmt.Println("json marshal error: ", errs)
	}
	fmt.Println("json data: ", string(jsons))
}
```

输出：

```text
json data:  {"code":200,"message":"success"}
json data:  {"code":500,"message":"fail"}
```



# Map 集合

map是一种无序的基于 key-value 的数据结构，Go语言中的map是引用类型，必须初始化才能使用。

### map 定义

Go 中 map 的定义语法如下：

```go
map[KeyType]ValueType
```

- KeyType：表示键的类型。

- ValueType：表示键对应的值的类型。

map 类型的变量默认初始值为 nil，需要使用 `make()` 函数来分配内存。语法为：

```go
make(map[KeyType]ValueType, [cap])
```

其中 cap 表示 map 的容量，该参数虽然不是必须的，但是我们应该在初始化 map 的时候就为其指定一个合适的容量。

### map 基本使用

map 中的数据都是成对出现的，map 的基本使用示例代码如下：

```go
// demo_15
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int, 8)
	scoreMap["张三"] = 60
	scoreMap["小明"] = 100
	fmt.Println(scoreMap)
	fmt.Println(scoreMap["小明"])
	// 打印该map的类型，%T为对应值的类型
	fmt.Printf("type of scoreMap: %T\n", scoreMap)

	// map 也支持在声明时填充元素
	userInfo := map[string]string{
		"username": "admin",
		"password": "123456",
	}
	fmt.Println(userInfo)
}
```

输出：

```text
map[小明:100 张三:60]
100
type of scoreMap: map[string]int
map[password:123456 username:admin]
```



### key 是否存在

Go 语言中有个判断 map 中键是否存在的特殊写法：

- `value, ok := map[key]`
- key 存在：ok 为 true，value 为对应的值
- key 不存在：ok 为 false，value 为零值

示例：

```go
// demo_16
package main

import "fmt"

func main() {
	hobbyMap := make(map[string]string)
	hobbyMap["张三"] = "犯法"
	hobbyMap["小明"] = "学习"

	v, ok := hobbyMap["张三"]
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("未查到此人")
	}
}
```

输出：

```
犯法
```



### map 的遍历

Go语言中使用 `for range` 遍历 map。

示例：

```go
// demo_17
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int)
	scoreMap["张三"] = 60
	scoreMap["小明"] = 100
	scoreMap["王五"] = 80

	// 遍历 key value
	for k, v := range scoreMap {
		fmt.Println(k, v)
	}

	// 只遍历 key
	for k := range scoreMap {
		fmt.Println(k)
	}
}
```

输出：

```text
张三 60
小明 100
王五 80
小明
王五
张三
```

注意： 遍历 map 时的元素顺序与添加键值对的顺序无关。





> 指定顺序遍历 map

```go
// domo_19
package main

import (
	"fmt"
	"math/rand"
	"sort"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixMilli()) // 初始化随机数种子

	var scoreMap = make(map[string]int, 100)

	// 给 map 赋值
	for i := 0; i < 20; i++ {
		key := fmt.Sprintf("stu%02d", i) // 生成stu开头的字符串
		value := rand.Intn(20)           // 生成 0~99 的随机整数
		scoreMap[key] = value
	}

	// 定义切片keys，将map中所有key存入keys
	var keys = make([]string, 0, 100)
	for key := range scoreMap {
		keys = append(keys, key)
	}

	// 对切片排序
	sort.Strings(keys)

	// 按照排序后的key遍历map
	for _, key := range keys {
		fmt.Println(key, scoreMap[key])
	}
}
```

输出：

```text
stu00 9
stu01 10
stu02 7
stu03 10
stu04 19
stu05 13
stu06 7
stu07 3
stu08 14
stu09 3
stu10 7
stu11 1
stu12 13
stu13 17
stu14 5
stu15 2
stu16 10
stu17 7
stu18 1
stu19 15
```



### 删除键值对

使用 `delete()` 内建函数从 map 中删除一组键值对：

- `delete(map, key)`
- map：要删除键值对的 map
- key：要删除的键值对的键

示例：

```go
// demo_18
package main

import "fmt"

func main() {
	scoreMap := make(map[string]int)
	scoreMap["张三"] = 60
	scoreMap["小明"] = 100
	scoreMap["王五"] = 80
	delete(scoreMap, "张三")
	for k, v := range scoreMap {
		fmt.Println(k, v)
	}
}
```

输出：

```text
小明 100
王五 80
```



### 元素为 map 类型的切片

下面的代码演示了切片中的元素为map类型时的操作：

```go
// demo_20
package main

import "fmt"

func main() {
	var mapSlice = make([]map[string]string, 3)
	for index, value := range mapSlice {
		fmt.Printf("index: %d value: %v\n", index, value)
	}
	fmt.Println("after init")

	// 对切片中的map元素进行初始化
	// 往切片0位置存放map元素
	mapSlice[0] = make(map[string]string, 10)
	mapSlice[0]["name"] = "王五"
	mapSlice[0]["password"] = "123456"
	mapSlice[0]["address"] = "陆家嘴"
	for index, value := range mapSlice {
		fmt.Printf("index: %d value: %v\n", index, value)
	}
}
```

输出：

```text
index: 0 value: map[]
index: 1 value: map[]
index: 2 value: map[]
after init
index: 0 value: map[address:陆家嘴 name:王五 password:123456]
index: 1 value: map[]
index: 2 value: map[]
```



### 值为切片类型的 map

下面的代码演示了 map 中值为切片类型的操作：

```go
// demo_21
package main

import "fmt"

func main() {
	var sliceMap = make(map[string][]string, 3)
	fmt.Println(sliceMap)
	fmt.Println("after init")
    
	key := "中国"
	value, ok := sliceMap[key]
	if !ok { // 若该key里面没有值，先定义切片类型的值
		value = make([]string, 0, 2)
	}
	// 给切片添加2个元素
	value = append(value, "北京", "上海")
	// 将切片值赋给键
	sliceMap[key] = value
	fmt.Println(sliceMap)
}
```

输出：

```text
map[]
after init
map[中国:[北京 上海]]
```



# 流程控制

## if

Go 语言中 if 语句由一种特殊的写法，可以在 if 表达式之前添加一个语句，再根据变量值进行判断，语句作用范围被限制在 if、else 语句组合中。

示例：

```go
func main() {
	x := 0
	// 在if表达式之前添加一个执行语句，x > 0 才是真正的判断语句
	if n := "abc"; x > 0 {
		println(n[2])
	} else if x < 0 {
		println(n[1])
	} else {
		println(n[0])
	}
	// 输出：97
}
```

注意：

- `{` 必须在条件表达式尾部，不能在单独的一行
- 不支持三元操作符(三目运算符) `"a > b ? a : b"`



## switch

Golang switch 分支表达式可以是任意类型，不限于常量。可省略 break，默认自动终止。

示例：

```go
demo_24
package main

import "fmt"

func main() {
	/* 定义局部变量 */
	var grade string = "B"
	var marks int = 90

	switch marks {
	case 90:
		grade = "A"
	case 80:
		grade = "B"
	case 50, 60, 70:
		grade = "C"
	default:
		grade = "D"
	}

	switch {
	case grade == "A":
		fmt.Printf("优秀!\n")
	case grade == "B", grade == "C":
		fmt.Printf("良好\n")
	case grade == "D":
		fmt.Printf("及格\n")
	case grade == "F":
		fmt.Printf("不及格\n")
	default:
		fmt.Printf("差\n")
	}
	fmt.Printf("你的等级是 %s\n", grade)
}
```

输出：

```text
优秀!
你的等级是 A
```



### Type Switch

switch 语句还可以被用于 type-switch 来判断某个 interface 变量中实际存储的变量类型。

Type Switch 语法格式如下：

```go
switch x.(type){
    case type:
       statement(s)      
    case type:
       statement(s)
    /* 你可以定义任意个数的case */
    default:	 /* 可选 */
       statement(s)
}
```

示例：

```go
// demo_25
package main

import "fmt"

func main() {
	var x interface{}

	//写法一：
	switch i := x.(type) { // 带初始化语句
	case nil:
		fmt.Printf(" x 的类型 :%T\r\n", i)
	case int:
		fmt.Printf("x 是 int 型")
	case float64:
		fmt.Printf("x 是 float64 型")
	case func(int) float64:
		fmt.Printf("x 是 func(int) 型")
	case bool, string:
		fmt.Printf("x 是 bool 或 string 型")
	default:
		fmt.Printf("未知型")
	}
	// 输出：x 的类型 :<nil>

	//写法二
	var j = 0
	switch j {
	case 0:
	case 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	default:
		fmt.Println("def")
	}
	// 没有输出

	//写法三
	var k = 0
	switch k {
	case 0:
		println("fallthrough")
		fallthrough
		/*
		   Go的switch非常灵活，表达式不必是常量或整数，执行的过程从上至下，直到找到匹配项；
		   而如果switch没有表达式，它会匹配true。
		   Go里面switch默认相当于每个case最后带有break，
		   匹配成功后不会自动向下执行其他case，而是跳出整个switch,
		   但是可以使用fallthrough强制执行后面的case代码。
		*/
	case 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	default:
		fmt.Println("def")
	}
	// 输出：fallthrough
        //	       1

	//写法三
	var m = 0
	switch m {
	case 0, 1:
		fmt.Println("1")
	case 2:
		fmt.Println("2")
	default:
		fmt.Println("def")
	}
	// 输出：1

	//写法四
	var n = 0
	switch { //省略条件表达式，可当 if...else if...else
	case n > 0 && n < 10:
		fmt.Println("i > 0 and i < 10")
	case n > 10 && n < 20:
		fmt.Println("i > 10 and i < 20")
	default:
		fmt.Println("def")
	}
	// 输出：def
}
```



## select

select 语句类似于 switch 语句，每个case必须是一个**通信操作**，要么是发送要么是接收。 select 随机执行一个可运行的case。**如果没有case可运行，它将阻塞，直到有case可运行**。一个默认的子句应该总是可运行的。

语法：

```sql
select {
    case communication clause  :
       statement(s);      
    case communication clause  :
       statement(s);
    /* 你可以定义任意数量的 case */
    default :	 /* 可选 */
       statement(s);
}
```

select 语句的语法：

- 每个 case 都必须是一个通信
- 所有 channel 表达式都会被求值
- 所有被发送的表达式都会被求值
- 如果任意某个通信可以进行，它就执行；其他被忽略
- 如果有多个 case 都可以运行，Select 会随机公平地选出一个执行。其他不会执行
- 否则：
  - 如果有 default 子句，则执行该语句
  - 如果没有 default 字句，select 将阻塞，直到某个通信可以运行；Go不会重新对channel 或值进行求值



示例：

```go
// demo_26
package main

import "fmt"

func main() {
	var c1, c2, c3 chan int // channel为int类型
	var i1, i2 int
        // channel 中没有数据读写，下面只会触发default
	select {
	// // 从Channel c1中接收数据，并将数据赋值给i1
	case i1 = <-c1:
		fmt.Println("received ", i1, " from c1\n")
	case c2 <- i2:
		fmt.Println("sent ", i2, " to c2\n")
	case i3, ok := <-c3:
		if ok {
			fmt.Println("received ", i3, " from c3\n")
		} else {
			fmt.Println("c3 is closed\n")
		}
	default:
		fmt.Println("no communication\n")
	}
}
```

输出：

```text
no communication
```

select可以监听channel的数据流动

select的用法与switch语法非常类似，由select开始的一个新的选择块，每个选择条件由case语句来描述

与switch语句可以选择任何使用相等比较的条件相比，select由比较多的限制，其中最大的一条限制就是**每个case语句里必须是一个IO操作**

```go
    select { //不停的在这里检测
    case <-chanl : //检测有没有数据可以读
    //如果chanl成功读取到数据，则进行该case处理语句
    case chan2 <- 1 : //检测有没有可以写
    //如果成功向chan2写入数据，则进行该case处理语句


    //假如没有default，那么在以上两个条件都不成立的情况下，就会在此阻塞
    //一般default会不写在里面，select中的default子句总是可运行的，因为会很消耗CPU资源
    default:
    //如果以上都没有符合条件，那么则进行default处理流程
    }
```

在一个select语句中，Go会按顺序从头到尾评估每一个发送和接收的语句。

如果其中的任意一个语句可以继续执行（即没有被阻塞），那么就从那些可以执行的语句中任意选择一条来使用。 如果没有任意一条语句可以执行（即所有的通道都被阻塞），那么有两种可能的情况： ①如果给出了default语句，那么就会执行default的流程，同时程序的执行会从select语句后的语句中恢复。 ②如果没有default语句，那么select语句将被阻塞，直到至少有一个case可以进行下去。



#### 典型用法

1、超时判断

```go
// 比如在下面的场景中，使用全局resChan来接受response，如果时间超过3S, resChan中还没有数据返回，则第二条case将执行
var resChan = make(chan int)
// do request
func test() {
    select {
    case data := <-resChan:
        doData(data)
    case <-time.After(time.Second * 3):
        fmt.Println("request time out")
    }
}

func doData(data int) {
    // ...
}
```

2、退出

```go
// 主线程（协程）中如下：
var shouldQuit=make(chan struct{})
fun main(){
    {
        // loop循环
    }
    // ...out of the loop循环
    select {
        case <-c.shouldQuit:
            cleanUp()
            return
        default:
        }
    // ...
}

// 再另外一个协程中，如果运行遇到非法操作或不可处理的错误，就向shouldQuit发送数据通知程序停止运行
close(shouldQuit)
```

3、判断 channel 是否阻塞

```go
// 在某些情况下是存在不希望channel缓存满了的需求的，可以用如下方法判断
ch := make (chan int, 5)
//...
data：=0
select {
case ch <- data:
default:
    // 做相应操作，比如丢弃data。视需求而定
}
```



## for

Golang for支持三种循环方式，包括类似 while 的语法。

**语法**：

Go语言的For循环有3中形式，只有其中的一种使用分号。

```go
for init; condition; post { }
for condition { }
for { }
```

- init： 一般为赋值表达式，给控制变量赋初值
- condition： 关系表达式或逻辑表达式，循环控制条件
- post： 一般为赋值表达式，给控制变量增量或减量

与C++一致。

示例：

```go
package main

func main() {
	s := "abc"
	// 常见的 for 循环，支持初始化语句。
	for i, n := 0, len(s); i < n; i++ {
		println(s[i])
	}

	n := len(s)
	// 代替 while (n > 0) {}
	for n > 0 {
		println(s[n-1])
		n--
	}

	// 代替 while (true) {}
	for {
        	// 无线循环
		println(s)
	}
}
```

输出：

```text
97
98
99
99
98
97
abc
abc
abc
......
```



## range

Golang range 类似迭代器操作，返回 (索引, 值) 或 (键, 值)。

for 循环的 range 格式可以对 slice、map、数组、字符串等进行迭代循环。格式如下：

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

|             | 1st value | 2nd value |               |
| ----------- | --------- | --------- | ------------- |
| string      | index     | s[index]  | unicode, rune |
| array/slice | index     | s[index]  |               |
| map         | key       | m[key]    |               |
| channel     | element   |           |               |

可忽略不想要的返回值，或 `"_"` 这个特殊变量。

示例：

```go
// demo_28
package main

func main() {
	s := "abc"
	// 忽略 2nd value
	for i := range s {
		println(s[i])
	}

	// 忽略 index
	for _, c := range s {
		println(c)
	}

	// 忽略全部返回值，仅迭代
	for range s {

	}

	m := map[string]int{"a": 1, "b": 2}
	// 返回（key, value）
	for k, v := range m {
		println(k, v)
	}
}
```

输出：

```text
97
98
99
97
98
99
a 1
b 2
```





> range 会**复制对象**

对于非引用类型，range 会复制对象，而不是不是直接在原对象上操作：

```go
// demo_29
package main

import "fmt"

func main() {
	a := [3]int{0, 1, 2}

	for i, v := range a { // index、value 都是从复制品中取出
		// 先修改原数组
		if i == 0 {
			a[1], a[2] = 999, 999
			fmt.Println(a) // 修改有效，原数组的a[1], a[2]都为999，输出：[0, 999, 999]
		}
		// 使用复制品中取出的 value 修改原数组
		a[i] = v + 100
	}
	fmt.Println(a) // 输出：[100, 101, 102]；发现并没有使用原数组的value
}
```

建议改用引用类型，其底层数据不会被复制：

```go
// demo_30
package main

import "fmt"

func main() {
	s := []int{1, 2, 3} // 改成slice
	for i, v := range s {
		if i == 0 {
			s[1], s[2] = 200, 300
			fmt.Println(s) // [1 200 300]
		}
		s[i] = v + 100
	}
	fmt.Println(s) // [101 300 400]
}
```

for range 的k, v变量在整个遍历过程中共用，不能直接进行引用传递，即地址传递

```go
package main

import "fmt"

func main(){
	a := []int {1, 2, 3} 	//改成slice
	for i, v := range a{
		if i == 0{
			a[1], a[2] = 200, 300
			fmt.Println(a)	 //[1 200 300]
		}
		fmt.Printf("i:{addr: %p; value: %v}, v:{addr: %p; value: %v}\n", &i, i, &v, v)
		a[i] = v + 100
	}
	fmt.Println(a)
}
// 注意，循环中i, v的地址是一样的，也就是说这个变量是被复用的！禁止直接使用i或者v的地址！！！
/* result: 
[1 200 300]
i:{addr: 0xc0000aa058; value: 0}, v:{addr: 0xc0000aa070; value: 1}
i:{addr: 0xc0000aa058; value: 1}, v:{addr: 0xc0000aa070; value: 200}
i:{addr: 0xc0000aa058; value: 2}, v:{addr: 0xc0000aa070; value: 300}
[101 300 400]
 */
```

注意：循环中i, v的地址是一样的，也就是说这个变量是被复用的，禁止直接使用i或者v的地址！！！



`for` 和 `for range` 的区别：

- for：

  - 遍历array和slice

  - 遍历key为整型递增的map

  - 遍历string

- for range 能完成所有for能做的事情，也能做到for不能做的，包括：

  - 遍历key为string类型的map并同时获取key和value

  - 遍历channel



# 函数

Go语言中支持函数、匿名函数和闭包，并且函数在Go语言中属于 “一等公民”。

## 函数定义与调用

Go语言中定义函数使用`func`关键字，具体格式如下：

```go
func 函数名(参数)(返回值){
    函数体
}
```

示例：

```go
// demo_31
package main

import "fmt"

func intSum(a int, b int) int {
	return a + b
}

func sayHello() {
	fmt.Println("Hello Golang")
}

func main() {
	sayHello()
	res := intSum(10, 20)
	fmt.Println(res)
}
```

## 参数

### 类型简写

函数的参数中如果相邻变量的类型相同，则可以省略类型，例如：

```go
func intSum(x, y int) int {
	return x + y
}
```



### 可变参数

可变参数是指函数的参数数量不固定，通过在参数名后加`...`来标识。

注意：可变参数通常要作为函数的最后一个参数。

示例：

```go
func intSum2(x ...int) int {
    fmt.Println(x)	// 输出：[x...]  (x是一个slice)
    sum := 0
    for _, v := range x {
        sum = sum + v
    }
    return sum
}
```

调用上面的函数：

```go
res1 := intSum2()
res2 := intSum2(10)
res3 := intSum2(10, 20)
fmt.Println(res1, res2, res3) // 0 10 30
```

固定参数搭配可变参数使用时，可变参数要放在固定参数的后面，示例代码如下：

```go
func intSum3(x int, y ...int) int {
	fmt.Println(x, y) // 100 [y...]
	sum := x
	for _, v := range y {
		sum += v
	}
	return sum
}
```

调用上述函数：

```go
res4 := intSum3(100)
res5 := intSum3(100, 10)
res6 := intSum3(100, 10, 20)
fmt.Println(res4, res5, res6) // 100 110 130
```

本质上，函数的可变参数是通过切片来实现的。



## 返回值

Go语言中通过`return`关键字向外输出返回值。

### 多返回值

Go语言中函数支持多返回值，函数如果有多个返回值时必须用`()`将所有返回值包裹起来。

举个例子：

```go
func calc(x, y int) (int, int) {
	sum := x + y
	sub := x - y
	return sum, sub
}
```



### 返回值命名

函数定义时可以给返回值命名，并在函数体中直接使用这些变量，最后通过`return`关键字返回。

例如：

```go
func calc(x, y int) (sum, sub int) {
	sum = x + y
	sub = x - y
	return
}
```



### 返回值补充

当我们的一个函数返回值类型为slice时，`nil`可以看做是一个有效的slice，没必要显示返回一个长度为0的切片。

```go
func someFunc(x string) []int {
	if x == "" {
		return nil // 没必要返回[]int{}
	}
	...
}
```





## 函数类型与变量

### 定义函数类型

我们可以使用`type`关键字来定义一个函数类型，具体格式如下：

```go
type calculation func(int, int) int
```

上面语句定义了一个`calculation`类型，它是一种函数类型，这种函数接收两个`int`类型的参数并且返回一个`int`类型的返回值。

简单来说，凡是满足这个条件的函数都是calculation类型的函数，例如下面的add和sub是calculation类型。

```go
func add(x, y int) int {
	return x + y
}

func sub(x, y int) int {
	return x - y
}
```

add和sub都能赋值给calculation类型的变量：

```go
var c, d calculation
c = add
d = sub
```



### 函数类型变量

我们可以声明函数类型的变量并且为该变量赋值：

```go
func main() {
	var c calculation               // 声明一个calculation类型的变量c
	c = add                         // 把add赋值给c
	fmt.Printf("type of c: %T\n", c) // type of c: main.calculation
	fmt.Println(c(1, 2))            // 像调用add一样调用c

	f := add                        // 将函数add赋值给变量f
	fmt.Printf("type of f: %T\n", f) // type of f: func(int, int) int
	fmt.Println(f(10, 20))          // 像调用add一样调用f
}
```



## 高阶函数

高阶函数分为 函数作为参数 和 函数作为返回值 两部分。



### 函数作为参数

函数可以作为参数：

```go
func add(x, y int) int {
    return x + y
}

func calc(x, y int, op func(int, int) int) int {
    return op(x, y)
}

func main() {
    res := calc(10, 20, add)
    fmt.Println(res)	// 30
}
```

### 函数作为返回值

函数也可以作为返回值：

```go
// 返回值为一个返回类型为int，error的函数
func do(s string) (func(int, int) int, error) {
	switch s {
	case "+":
		return add, nil
	case "-":
		return sub, nil
	default:
		err := errors.New("无法识别的操作符")
		return nil, err
	}
}

func main() {
    f, err := do("*")
    if err != nil {
        fmt.Println(err)
    } else {
        result := f(1, 2)
        fmt.Println(result)
    }
    // 输出：3
}
```



## 匿名函数和闭包

### 匿名函数

函数当然还可以作为返回值，但是在Go语言中函数内部不能再像之前那样定义函数了，只能定义匿名函数。匿名函数就是没有函数名的函数，匿名函数的定义格式如下：

```go
func(参数) (返回值) {
    函数体
}
```

匿名函数因为没有函数名，所以没办法像普通函数那样调用，所以匿名函数需要保存到某个变量或者作为立即执行函数：

```go
func main() {
    // 将匿名函数保存到变量
    add := func(x, y int) {
        fmt.Println(x + y)
    }
    add(10, 20)	// 通过变量调用匿名函数
    
    // 自执行函数：匿名函数定义完加（）直接执行
    func(x, y int) {
        fmt.Println(x + y)
    }(10, 20)
}
```

匿名函数多用于实现回调函数和闭包。



### 闭包

闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+引用环境`。 

示例：

```go
// demo_33

func adder() func(int) int {
	var x int
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	var f = adder()
	fmt.Println(f(10))	// 10
	fmt.Println(f(20))	// 30
	fmt.Println(f(30))	// 60

	f1 := adder()
	fmt.Println(f1(40))	// 40
	fmt.Println(f1(50))	// 90
}
```

变量`f`是一个函数并且它引用了其**外部作用域**中的`x`变量，此时`f`就是一个闭包。 在`f`的生命周期内，变量`x`也一直有效。

闭包进阶示例1：

```go
// demo_34

// 将x变量定义在参数上
func adder2(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
}

func main() {
	var f = adder2(10)
        // x在参数上，也属于外部作用域
	fmt.Println(f(10))	// 20
	fmt.Println(f(20))	// 40
	fmt.Println(f(30))	// 70

	f1 := adder2(20)
	fmt.Println(f1(40))	// 60
	fmt.Println(f1(50))	// 110
}
```

闭包进阶示例2：

```go
// demo_35

// 将变量suffix定义在参数上
func makeSuffixFunc(suffix string) func(string) string {
	return func(name string) string {
		if !strings.HasSuffix(name, suffix) {
			return name + suffix
		}
		return name
	}
}

func main() {
	// 将.jpg传给suffix，后面jpgFunc调用时该suffix还有效
	jpgFunc := makeSuffixFunc(".jpg")
	fmt.Println(jpgFunc("text")) // text.jpg
	txtFunc := makeSuffixFunc(".txt")
	fmt.Println(txtFunc("text")) // text.txt
}
```

闭包进阶示例3：

```go
// demo_36

func calc(base int) (func(int) int, func(int) int) {
	add := func(i int) int {
		base += i
		return base
	}

	sub := func(i int) int {
		base -= i
		return base
	}

	return add, sub
}

func main() {
	// f1, f2共享一个base，因为f1，f2在同一个闭包内
	f1, f2 := calc(10)
	fmt.Println(f1(1), f2(2))	// 11(10+1) 9(11-2)
	fmt.Println(f1(3), f2(4))	// 12(9+3) 8(12-4)
	fmt.Println(f1(5), f2(6))	// 13(8+5) 7(13-6)
}
```

闭包其实并不复杂，只要牢记`闭包=函数+引用环境`



## defer 语句

Go语言中的`defer`语句会将其后面跟随的语句进行**延迟处理**。

在`defer`归属的函数即将返回时，将延迟处理的语句按`defer`定义的**逆序进行**执行：

先被`defer`的语句最后被执行，最后被`defer`的语句，最先被执行。

示例：

```go
func main() {
    fmt.Println("start")
    defer fmt.Println(1)
    defer fmt.Println(2)
    defer fmt.Println(3)
    fmt.Println("end")
}
```

输出：

```text
start
end
3
2
1
```

由于`defer`语句延迟调用的特性，所以`defer`语句能非常方便的处理**资源释放**问题。比如：资源清理、文件关闭、解锁及记录时间等。



### defer执行时机

在Go语言的函数中`return`语句在底层并**不是原子操作**，它分为给 返回值赋值 和 RET指令  两步。

`defer`语句执行的时机就在返回值赋值操作后，RET指令执行前。具体如下图所示：

![image-20220503112609132](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b09848c5f9a8e496977ad8bda7e36cde.png)



### defer经典案例

```go
// demo_37

func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x //返回值=x=5，再执行defer：x=6，RET 返回值
}

func f2() (x int) {
	defer func() {
		x++
	}()
	return 5 // 返回值x=5，再执行defer：x=6，RET x=6
}

func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x // 返回值y=x=5，再执行defer：x=6，RET y=5
}

func f4() (x int) {
	defer func(x int) {
		x++
	}(x)
	return 5	// 返回值x=5，再执行defer：自执行匿名函数中的x不是返回值的x，互不相干，最后RET x=5
}

func main() {
	fmt.Println(f1()) // 5
	fmt.Println(f2()) // 6
	fmt.Println(f3()) // 5
	fmt.Println(f4()) // 5
}
```



### defer面试题

提示：defer注册要延迟执行的函数时该函数所有的参数都需要确定其值

```go
// demo_38

func calc1(index string, a, b int) int {
	ret := a + b
	fmt.Println(index+": ", a, b, ret)
	return ret
}

func main() {
	x := 1
	y := 2
	defer calc1("AA", x, calc1("A", x, y))
	x = 10
	defer calc1("BB", x, calc1("B", x, y))
	y = 20
}
```

输出：

```text
A:  1 2 3
B:  10 2 12
BB:  10 12 22
AA:  1 3 4
```



## 内置函数介绍

| 内置函数       | 介绍                                                         |
| -------------- | ------------------------------------------------------------ |
| close          | 主要用来关闭channel                                          |
| len            | 用来求长度，比如string、array、slice、map、channel           |
| new            | 用来分配内存，主要用来分配值类型，比如int、struct。返回的是指针 |
| make           | 用来分配内存，主要用来分配引用类型，比如chan、map、slice     |
| append         | 用来追加元素到数组、slice中                                  |
| panic和recover | 用来做错误处理                                               |

### panic/recover

Go语言中目前（Go1.12）是没有异常机制，但是使用`panic/recover`模式来处理错误。 `panic`可以在任何地方引发，但`recover`只有在`defer`调用的函数中有效。 首先来看一个例子：

```go
func funcA() {
	fmt.Println("func A")
}

func funcB() {
	panic("panic in B")
}

func funcC() {
	fmt.Println("func C")
}
func main() {
	funcA()
	funcB()
	funcC()
}
```

输出：

```bash
func A
panic: panic in B

goroutine 1 [running]:
main.funcB(...)
        .../code/func/main.go:12
main.main()
        .../code/func/main.go:20 +0x98
```

程序运行期间`funcB`中引发了`panic`导致程序崩溃，异常退出了。这个时候我们就可以通过`recover`将程序恢复回来，继续往后执行。

```go
func funcA() {
	fmt.Println("func A")
}

func funcB() {
	defer func() {
		err := recover()
		//如果程序出出现了panic错误,可以通过recover恢复过来
		if err != nil {
			fmt.Println("recover in B")
		}
	}()
	panic("panic in B")
}

func funcC() {
	fmt.Println("func C")
}
func main() {
	funcA()
	funcB()
	funcC()
}
```



## 异常处理

Golang 没有结构化异常机制，使用 panic 抛出错误，recover 捕获错误。

异常的使用场景简单描述：Go中可以抛出一个 panic 的异常，然后在 defer 中通过 recover 捕获这个异常，然后正常处理。

**注意**：

1. `panic`可以在任何地方引发。
1. `recover`只有在`defer`调用的函数中有效。
2. `defer`一定要在可能引发`panic`的语句之前定义。
3. 多个 `defer` 会形成 `defer` 栈，后定义的 `defer` 语句会被最先调用。

由于 panic、recover 参数类型为 `interface{}`，因此可抛出任何类型对象：

```go
func panic(v interface{})
func recover() interface{}
```

示例：

```go
// demo_39
package main

func test() {
	defer func() { // 用recover() 来捕获下面的panic，捕获到则err不为nil
		if err := recover(); err != nil {
			println(err.(string))
		}
	}()

	panic("panic error")
}

func main() {
	test()
}
```

输出：

```text
panic error
```





> 向已关闭的通道发送数据会引发panic

```go
// demo_40
package main

import "fmt"

func main() {
	defer func() {
		if err := recover(); err != nil {
			fmt.Println(err)
		}
	}()

	var ch chan int = make(chan int, 10)
	// 关闭channel
	close(ch)
	// 向关闭的channel中发送数据，会引发panic
	ch <- 1
}
```

输出：

```text
send on closed channel
```





> 延迟调用中引发的错误，可被后续延迟调用捕获，但仅最后一个错误可被捕获

```go
// demo_41
package main

import "fmt"

func test1() {
	defer func() {
		fmt.Println(recover())
	}()

	defer func() {
		panic("defer panic 1")
	}()

	defer func() {
		panic("defer panic 2")
	}()

	panic("test panic")
    
        // panic 顺序：test panic	defer panic 2	defer panic 1
        // defer panic 1 是最后一个错误
}

func main() {
	test1()
}
```

输出：

```text
defer panic 1
```





> 捕获函数 recover 只有在延迟调用内直接调用才会终止错误，否则总是返回 nil

任何未捕获的错误都会沿调用堆栈向外传递。

示例：

```go
// demo_42
package main

import "fmt"

func test2() {
	defer func() {
		fmt.Println(recover()) // 有效
	}()
	defer recover()              // 无效；defer 不能直接调用recover
	defer fmt.Println(recover()) // 同上，打印返回的nil
	defer func() {
		func() {
			println("defer inner")
			recover() // 无效；没有在defer内直接调用
		}()
	}()

	panic("test panic")
}

func main() {
	test2()
}
```

输出：

```text
defer inner
<nil>
test panic
```

除了使用延迟匿名函数，使用 defer 然后调用某个函数，在这个函数内执行 recover 也可以：

```go
func except() {
    fmt.Println(recover())
}

func test() {
    defer except()
    panic("test panic")
}

func main() {
    test()
}
```





> 可以将代码块重构成匿名函数，可以确保后续代码被执行

```go
// demo_43
package main

import "fmt"

func test3(x, y int) {
	var z int
	func() {
		defer func() {
			if recover() != nil {
				// 最后执行的代码
				z = 10
			}
		}()
		panic("test panic")
		z = x / y // 不会执行此行代码
		return
	}()
	
	fmt.Printf("x / y = %d\n\n", z)
}

func main() {
	test3(2, 1)
}
```

输出：

```text
x / y = 10
```





> 除用 panic 引发中断性错误外，还可返回 error 类型错误对象来表示函数调用状态

```go
type error interface {
    Error() string
}
```

标准库 `errors.New` 和 `fmt.Errorf` 函数用于创建实现 error 接口的错误对象。通过判断错误对象实例来确定具体错误类型。

```go
// demo_44
package main

import (
	"errors"
	"fmt"
)

var ErrDivByZero = errors.New("division by zero")

func div(x, y int) (int, error) {
	if y == 0 {
		return 0, ErrDivByZero
	}
	return x / y, nil
}

func main() {
	defer func() {
		fmt.Println(recover())
	}()
	switch z, err := div(10, 0); err {
	case nil:
		println(z)
	case ErrDivByZero:
		panic(err)
	}
	
	// 输出：division by zero
}
```





> Go 实现类似 try catch 的异常处理

```go
// demo_45
package main

import "fmt"

func Try(fun func(), handler func(interface{})) {
	defer func() {
		if err := recover(); err != nil {
			handler(err)
		}
	}()
	fun()
}

func main() {
	Try(func() {
		panic("test panic")
	}, func(err interface{}) {
		fmt.Println(err)
	})

	// 输出：test panic
	/* 调用Try后，传两个函数进去，先执行fun函数，发现panic异常；然后执行
	defer中匿名函数的handler函数(因为recover()捕获到异常了)，handler打印err
	*/
}
```



如何区别使用 panic 和 error 两种方式?

惯例是：导致关键流程出现**不可修复性错误**的使用 panic，其他使用 error



# 方法

Go语言中的`方法（Method）`是一种作用于特定类型变量的函数。这种特定类型变量叫做`接收者（Receiver）`。接收者的概念就类似于其他语言中的`this`或者 `self`。

法的定义格式如下：

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

其中：

- 接收者变量：接收者中的参数变量名在命名时，官方建议使用**接收者类型名称首字母的小写**，而不是`self`、`this`之类的命名。例如，`Person`类型的接收者变量应该命名为 `p`，`Connector`类型的接收者变量应该命名为`c`等。
- 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型。
- 方法名、参数列表、返回参数：具体格式与函数定义相同。

方法与函数的区别是，函数不属于任何类型，**方法属于特定的类型**。

例子：

```go
// demo_46
package main

import "fmt"

type Person struct {
	name string
	age  int8
}

// NewPerson 构造函数
func NewPerson(name string, age int8) *Person {
	return &Person{
		name: name,
		age:  age,
	}
}

// Dream Person的方法
func (p Person) Dream() {
	fmt.Printf("%s的梦想是学好Golang!\n", p.name)
}

func main() {
	p1 := NewPerson("张三", 18)
	p1.Dream()
    
        // 输出：张三的梦想是学好Golang!
}
```



## 指针类型的接收者

指针类型的接收者由一个结构体的指针组成，由于指针的特性，调用方法时修改接收者指针的任意成员变量，在方法结束后，修改都是有效的。这种方式就十分接近于其他语言中面向对象中的`this`或者`self`。 

例如我们为`Person`添加一个`SetAge`方法，来修改实例变量的年龄。

```go
// SetAge 设置p的年龄
func (p *Person) SetAge(newAge int8) {
	p.age = newAge
}
```

调用该方法：

```go
func main() {
	p1 := NewPerson("张三", 18)
	//p1.Dream()
	fmt.Println(p1.age)	// 18
	p1.SetAge(28)
	fmt.Println(p1.age)	// 28
}
```



## 值类型的接收者

当方法作用于值类型接收者时，Go语言会在代码运行时将**接收者的值复制一份**。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。

示例：

```go
// SetAge2 设置p的年龄
// 使用值接收者
func (p Person) SetAge2(newAge int8) {
	p.age = newAge
}

func main() {
	p1 := NewPerson("小王子", 18)
	p1.Dream()
	fmt.Println(p1.age) 	// 18
	p1.SetAge2(30) 	// (*p1).SetAge2(30)
	fmt.Println(p1.age) 	// 18
}
```



## 什么时候应该使用指针类型接收者

1. 需要修改接收者中的值
2. 接收者是拷贝代价比较大的大对象
3. 保证一致性，如果有某个方法使用了指针接收者，那么其他的方法也应该使用指针接收者。





## 任意类型添加方法

在Go语言中，接收者的类型可以是任何类型，不仅仅是结构体，任何类型都可以拥有方法。 举个例子，我们基于内置的`int`类型使用type关键字可以定义新的自定义类型，然后为我们的自定义类型添加方法。

```go
//MyInt 将int定义为自定义MyInt类型
type MyInt int

//SayHello 为MyInt添加一个SayHello的方法
func (m MyInt) SayHello() {
	fmt.Println("Hello, 我是一个int。")
}
func main() {
	var m1 MyInt
	m1.SayHello() //Hello, 我是一个int。
	m1 = 100
	fmt.Printf("%#v  %T\n", m1, m1) //100  main.MyInt
}
```

**注意事项**：

- 非本地类型不能定义方法，也就是说我们不能给别的包的类型定义方法。



# 包与依赖管理

在工程化的Go语言开发项目中，Go语言的源码复用是建立在包（package）基础之上的。

接下来学习Go语言中如何定义包、如何导出包的内容及如何引入其他包。同时也将介绍如何在项目中使用go module管理依赖。



## package 包

### 包介绍

Go语言中支持模块化的开发理念，在Go语言中使用`包（package）`来支持代码模块化和代码复用。

一个包是由一个或多个Go源码文件（.go结尾的文件）组成，是一种高级的代码复用方案，Go语言为我们提供了很多内置包，如`fmt`、`os`、`io`等。

例如，在之前的章节中我们频繁使用了`fmt`这个内置包。

```go
package main

import "fmt"

func main(){
  fmt.Println("Hello world!")
}
```

上面短短的几行代码就涉及到了如何定义包以及如何引入其它包两个内容。



### 定义包

我们可以根据自己的需要创建自定义包。一个包可以简单理解为一个存放`.go`文件的文件夹。该文件夹下面的所有`.go`文件都要在非注释的第一行添加如下声明，声明该文件归属的包。

```go
package packagename
```

其中：

- package：声明包的关键字
- packagename：包名，可以不与文件夹的名称一致，不能包含 `-` 符号，最好与其实现的功能相对应。

注意：

- 一**个文件夹下面直接包含的文件只能归属一个包**，同一个包的文件不能在多个文件夹下。
- 包名为`main`的包是应用程序的**入口包**，这种包编译后会得到一个可执行文件，而编译不包含`main`包的源代码则不会得到可执行文件。



### 标识符可见性

在同一个包内部声明的标识符都位于同一个命名空间下，在不同的包内部声明的标识符就属于不同的命名空间。想要在包的外部使用包内部的标识符就需要添加包名前缀，例如`fmt.Println("Hello world!")`，就是指调用`fmt`包中的`Println`函数。

如果想让一个包中的标识符（如变量、常量、类型、函数等）能被外部的包使用，那么标识符必须是对外可见的（public）。在Go语言中是通过标识符的首字母大/小写来控制标识符的对外可见（public）/不可见（private）的。在一个包内部只有首字母大写的标识符才是对外可见的。

例如我们定义一个名为`demo`的包，在其中定义了若干标识符。在另外一个包中并不是所有的标识符都能通过`demo.`前缀访问到，因为只有那些首字母是大写的标识符才是对外可见的。

```go
package demo

import "fmt"

// 包级别标识符的可见性

// num 定义一个全局整型变量
// 首字母小写，对外不可见(只能在当前包内使用)
var num = 100

// Mode 定义一个常量
// 首字母大写，对外可见(可在其它包中使用)
const Mode = 1

// person 定义一个代表人的结构体
// 首字母小写，对外不可见(只能在当前包内使用)
type person struct {
	name string
	Age  int
}

// Add 返回两个整数和的函数
// 首字母大写，对外可见(可在其它包中使用)
func Add(x, y int) int {
	return x + y
}

// sayHi 打招呼的函数
// 首字母小写，对外不可见(只能在当前包内使用)
func sayHi() {
	var myName = "七米" // 函数局部变量，只能在当前函数内使用
	fmt.Println(myName)
}

```

同样的规则也适用于结构体，结构体中可导出字段的字段名称必须首字母大写。

```go
type Student struct {
	Name  string // 可在包外访问的方法
	class string // 仅限包内访问的字段
}
```



### 包的引入

要在当前包中使用另外一个包的内容就需要使用`import`关键字引入这个包，并且import语句通常放在文件的开头，`package`声明语句的下方。完整的引入声明语句格式如下：

```go
import importname "path/to/package"
```

其中：

- importname：引入的包名，通常都省略。默认值为引入包的包名。
- path/to/package：引入包的路径名称，必须使用双引号包裹起来。
- Go语言中禁止循环导入包。



一个Go源码文件中可以同时引入多个包，例如：

```go
import "fmt"
import "net/http"
import "os"
```

当然可以使用批量引入的方式。

```go
import (
    "fmt"
  	"net/http"
    "os"
)
```

当引入的多个包中存在相同的包名或者想自行为某个引入的包设置一个新包名时，都需要通过`importname`指定一个在当前文件中使用的新包名。例如，在引入`fmt`包时为其指定一个新包名`f`。

```go
import f "fmt"
```

这样在当前这个文件中就可以通过使用`f`来调用`fmt`包中的函数了。

```go
f.Println("Hello world!")
```



如果引入一个包的时候为其设置了一个特殊`_`作为包名，那么这个包的引入方式就称为匿名引入。一个包被匿名引入的目的主要是为了加载这个包，从而使得这个包中的资源得以初始化。 被匿名引入的包中的`init`函数将被执行并且仅执行一遍。

```go
import _ "github.com/go-sql-driver/mysql"
```

匿名引入的包与其他方式导入的包一样都会被编译到可执行文件中。

注意：Go语言中不允许引入包却不在代码中使用这个包的内容，如果引入了未使用的包则会触发编译错误。



### init 初始化函数

在每一个Go源文件中，都可以定义任意个如下格式的特殊函数：

```go
func init(){
  // ...
}
```

这种特殊的函数不接收任何参数也没有任何返回值，我们也不能在代码中主动调用它。当程序启动的时候，init函数会按照它们声明的顺序自动执行。



一个包的初始化过程是按照代码中引入的顺序来进行的，所有在该包中声明的`init`函数都将被串行调用并且仅调用执行一次。

每一个包初始化的时候都是先执行依赖的包中声明的`init`函数再执行当前包中声明的`init`函数。确保在程序的`main`函数开始执行时所有的依赖包都已初始化完成。

![](https://www.liwenzhou.com/images/Go/package/package01.png)

每一个包的初始化是先从初始化包级别变量开始的。例如从下面的示例中我们就可以看出包级别变量的初始化会先于`init`初始化函数：

```go
package main

import "fmt"

var x int8 = 10

const pi = 3.14

func init() {
  fmt.Println("x:", x)
	fmt.Println("pi:", pi)
	sayHi()
}

func sayHi() {
	fmt.Println("Hello World!")
}

func main() {
	fmt.Println("你好，世界！")
}
```

输出结果：

```text
x: 10
pi: 3.14
Hello World!
你好，世界！
```



## go module

在Go语言的早期版本中，我们编写Go项目代码时所依赖的所有第三方包都需要保存在GOPATH这个目录下面。这样的依赖管理方式存在一个致命的缺陷，那就是不支持版本管理，同一个依赖包只能存在一个版本的代码。可是我们本地的多个项目完全可能分别依赖同一个第三方包的不同版本。

### go module 介绍

Go module 是 Go1.11 版本发布的依赖管理方案，从 Go1.14 版本开始推荐在生产环境使用，于 Go1.16 版本默认开启。Go module 提供了以下命令供我们使用：

| 命令            | 介绍                                       |
| --------------- | ------------------------------------------ |
| go mod init     | 初始化项目依赖，生成go.mod文件             |
| go mod download | 根据 go.mod 文件下载依赖                   |
| go mod tidy     | 比对项目文件中引入的依赖与 go.mod 进行比对 |
| go mod graph    | 输出依赖关系图                             |
| go mod edit     | 编辑 go.mod 文件                           |
| go mod vendor   | 将项目的所有依赖导出至vendor目录           |
| go mod verify   | 检验一个依赖包是否被篡改过                 |
| go mod why      | 解释为什么需要某个依赖                     |



#### GOPROXY

这个环境变量主要是用于设置 Go 模块代理（Go module proxy），其作用是用于使 Go 在后续拉取模块版本时能够脱离传统的 VCS 方式，直接通过镜像站点来快速拉取。

GOPROXY 的默认值是：`https://proxy.golang.org,direct`，由于某些原因国内无法正常访问该地址，所以我们通常需要配置一个可访问的地址。目前社区使用比较多的有两个`https://goproxy.cn`和`https://goproxy.io`，当然如果你的公司有提供 GOPROXY 地址那么就直接使用。设置GOPAROXY的命令如下：

```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

GOPROXY 允许设置多个代理地址，多个地址之间需使用英文逗号 “,” 分隔。最后的 “direct” 是一个特殊指示符，用于指示 Go 回源到源地址去抓取（比如 GitHub 等）。当配置有多个代理地址时，如果第一个代理地址返回 404 或 410 错误时，Go  会自动尝试下一个代理地址，当遇见 “direct” 时触发回源，也就是回到源地址去抓取。

GOPRIVATE

设置了GOPROXY 之后，go 命令就会从配置的代理地址拉取和校验依赖包。当我们在项目中引入了非公开的包（公司内部git仓库或  github 私有仓库等），此时便无法正常从代理拉取到这些非公开的依赖包，这个时候就需要配置 GOPRIVATE  环境变量。GOPRIVATE用来告诉 go 命令哪些仓库属于私有仓库，不必通过代理服务器拉取和校验。

GOPRIVATE 的值也可以设置多个，多个地址之间使用英文逗号 “,” 分隔。我们通常会把自己公司内部的代码仓库设置到 GOPRIVATE 中，例如：

```bash
$ go env -w GOPRIVATE="git.mycompany.com"
```

这样在拉取以`git.mycompany.com`为路径前缀的依赖包时就能正常拉取了。

此外，如果公司内部自建了 GOPROXY 服务，那么我们可以通过设置 `GONOPROXY=none`，允许通内部代理拉取私有仓库的包。



### 使用 go module 引入包

#### 初始化项目

我们在本地新建一个名为`holiday`项目，按如下方式创建一个名为`holiday`的文件夹并切换到该目录下：

```bash
$ mkdir holiday
$ cd holiday
```

目前我们位于`holiday`文件夹下，接下来执行下面的命令初始化项目。

```bash
$ go mod init holiday
go: creating new go.mod: module holiday
```

该命令会自动在项目目录下创建一个`go.mod`文件，其内容如下。

```go
module holiday

go 1.18
```

其中：

- module holiday：定义当前项目的导入路径
- go 1.18：标识当前项目使用的 Go 版本

`go.mod`文件会记录项目使用的第三方依赖包信息，包括包名和版本，由于我们的`holiday`项目目前还没有使用到第三方依赖包，所以`go.mod`文件暂时还没有记录任何依赖包信息，只有当前项目的一些信息。

接下来，我们在项目目录下新建一个`main.go`文件，其内容如下：

```go
// holiday/main.go

package main

import "fmt"

func main() {
	fmt.Println("现在是假期时间...")
}
```

然后，我们的`holiday`项目现在需要引入一个第三方包`github.com/q1mi/hello`来实现一些必要的功能。类似这样的场景在我们的日常开发中是很常见的。我们需要先将依赖包下载到本地同时在`go.mod`中记录依赖信息，然后才能在我们的代码中引入并使用这个包。

下载依赖包主要有两种方法。



第一种方法：

在项目目录下执行`go get`命令手动下载依赖的包：

```bash
holiday $ go get -u github.com/q1mi/hello
go get: added github.com/q1mi/hello v0.1.1
```

这样默认会下载最新的发布版本，你也可以指定想要下载指定的版本号的。

```bash
holiday $ go get -u github.com/q1mi/hello@v0.1.0
go: downloading github.com/q1mi/hello v0.1.0
go get: downgraded github.com/q1mi/hello v0.1.1 => v0.1.0
```

如果依赖包没有发布任何版本则会拉取最新的提交，最终`go.mod`中的依赖信息会变成类似下面这种由默认v0.0.0的版本号和最新一次commit的时间和hash组成的版本格式：

```go
require github.com/q1mi/hello v0.0.0-20210218074646-139b0bcd549d
```

如果想指定下载某个commit对应的代码，可以直接指定commit hash，不过没有必要写出完整的commit hash，一般前7位即可。例如：

```bash
holiday $ go get github.com/q1mi/hello@2ccfadd
go: downloading github.com/q1mi/hello v0.1.2-0.20210219092711-2ccfaddad6a3
go get: added github.com/q1mi/hello v0.1.2-0.20210219092711-2ccfaddad6a3
```

此时，我们打开`go.mod`文件就可以看到下载的依赖包及版本信息都已经被记录下来了。

```go
module holiday

go 1.18

require github.com/q1mi/hello v0.1.1 // indirect
```

行尾的`indirect`表示该依赖包为间接依赖，说明在当前程序中的所有 import 语句中没有发现引入这个包。

另外在执行`go get`命令下载一个新的依赖包时一般会额外添加`-u`参数，强制更新现有依赖。

若下载的依赖爆红，在Goland中需要勾选如下选项：

![image-20220505143632336](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2743073adf076a1d629427e138903d61.png)



第二种方法：

直接编辑`go.mod`文件，将依赖包和版本信息写入该文件。例如我们修改`holiday/go.mod`文件内容如下：

```go
module holiday

go 1.16

require github.com/q1mi/hello latest
```

表示当前项目需要使用`github.com/q1mi/hello`库的最新版本，然后在项目目录下执行`go mod download`下载依赖包。

```bash
holiday $ go mod download
```

如果不输出其它提示信息就说明依赖已经下载成功，此时`go.mod`文件已经变成如下内容。

```go
module holiday

go 1.16

require github.com/q1mi/hello v0.1.1
```

从中我们可以知道最新的版本号是`v0.1.1`。如果事先知道依赖包的具体版本号，可以直接在`go.mod`中指定需要的版本然后再执行`go mod download`下载。

这种方法同样支持指定想要下载的commit进行下载，例如直接在`go.mod`文件中按如下方式指定commit hash，这里只写出来了commit  hash的前7位。

```go
require github.com/q1mi/hello 2ccfadda
```

执行`go mod download`下载完依赖后，`go.mod`文件中对应的版本信息会自动更新为类似下面的格式。

```go
module holiday

go 1.16

require github.com/q1mi/hello v0.1.2-0.20210219092711-2ccfaddad6a3
```

下载好要使用的依赖包之后，我们现在就可以在`holiday/main.go`文件中使用这个包了。

```go
package main

import (
	"fmt"

	"github.com/q1mi/hello"
)

func main() {
	fmt.Println("现在是假期时间...")

	hello.SayHi() 	// 调用hello包的SayHi函数
}
```

输出：

```text
现在是假期时间...
你好呀~我是七米。很高兴认识你。
```

当我们的项目功能越做越多，代码越来越多的时候，通常会选择在项目内部按功能或业务划分成多个不同包。Go语言支持在一个项目（project）下定义多个包（package）。

例如，我们在`holiday`项目内部创建一个新的package——`summer`，此时新的项目目录结构如下：

```bash
holidy
├── go.mod
├── go.sum
├── main.go
└── summer
    └── summer.go
```

其中`holiday/summer/summer.go`文件内容如下：

```go
package summer

import "fmt"

// Diving 潜水...
func Diving() {
	fmt.Println("夏天去诗巴丹潜水...")
}
```

此时想要在当前项目目录下的其他包或者`main.go`中调用这个`Diving`函数需要如何引入呢？这里以在`main.go`中演示详细的调用过程为例，在项目内其他包的引入方式类似。

```go
package main

import (
	"fmt"

	"holiday/summer" // 导入当前项目下的包

	"github.com/q1mi/hello" // 导入github上第三方包
)

func main() {
	fmt.Println("现在是假期时间...")
	hello.SayHi()

	summer.Diving()
}
```

从上面的示例可以看出，项目中定义的包都会以项目的导入路径为前缀。

如果你想要导入本地的一个包，并且这个包也没有发布到到其他任何代码仓库，这时候你可以在`go.mod`文件中使用`replace`语句将依赖临时替换为本地的代码包。例如在我的电脑上有另外一个名为`liwenzhou.com/overtime`的项目，它位于`holiday`项目同级目录下：

```bash
├── holiday
│   ├── go.mod
│   ├── go.sum
│   ├── main.go
│   └── summer
│       └── summer.go
└── overtime
    ├── go.mod
    └── overtime.go
```

由于`liwenzhou.com/overtime`包只存在于我本地，并不能通过网络获取到这个代码包，这个时候应该如何在`holidy`项目中引入它呢？

我们可以在`holidy/go.mod`文件中正常引入`liwenzhou.com/overtime`包，然后像下面的示例那样使用`replace`语句将这个依赖替换为使用相对路径表示的本地包。

```go
module holiday

go 1.16

require github.com/q1mi/hello v0.1.1
require liwenzhou.com/overtime v0.0.0

replace liwenzhou.com/overtime  => ../overtime
```

这样，我们就可以在`holiday/main.go`下正常引入并使用`overtime`包了。

```go
package main

import (
	"fmt"

	"holiday/summer" // 导入当前项目下的包

	"liwenzhou.com/overtime" // 通过replace导入的本地包

	"github.com/q1mi/hello" // 导入github上第三方包
)

func main() {
	fmt.Println("现在是假期时间...")
	hello.SayHi()

	summer.Diving()

	overtime.Do()
}
```

我们也经常使用`replace`将项目依赖中的某个包，替换为其他版本的代码包或我们自己修改后的代码包。



#### go.mod 文件

`go.mod`文件中记录了当前项目中所有依赖包的相关信息，声明依赖的格式如下：

```bash
require module/path v1.2.3
```

其中：

- require：声明依赖的关键字
- module/path：依赖包的引入路径
- v1.2.3：依赖包的版本号。支持以下几种格式：
  - latest：最新版本
  - v1.0.0：详细版本号
  - commit hash：指定某次commit hash

引入某些没有发布过`tag`版本标识的依赖包时，`go.mod`中记录的依赖版本信息就会出现类似`v0.0.0-20210218074646-139b0bcd549d`的格式，由版本号、commit时间和commit的hash值组成。

![go module生成的版本信息组成示意图](https://www.liwenzhou.com/images/Go/package/module_version_info.png)



#### go.sum 文件

使用go module下载了依赖后，项目目录下还会生成一个`go.sum`文件，这个文件中详细记录了当前项目中引入的依赖包的信息及其hash 值。`go.sum`文件内容通常是以类似下面的格式出现。

```go
<module> <version>/go.mod <hash>
```

或者

```go
<module> <version> <hash>
<module> <version>/go.mod <hash>
```

不同于其他语言提供的基于中心的包管理机制，例如 npm 和 pypi 等，Go并没有提供一个中央仓库来管理所有依赖包，而是采用分布式的方式来管理包。为了防止依赖包被非法篡改，Go module 引入了`go.sum`机制来对依赖包进行校验。



#### 依赖保存位置

Go module 会把下载到本地的依赖包会以类似下面的形式保存在  `$GOPATH/pkg/mod`目录下，每个依赖包都会带有版本号进行区分，这样就允许在本地存在同一个包的多个不同版本。

```bash
mod
├── cache
├── cloud.google.com
├── github.com
    	└──q1mi
          ├── hello@v0.0.0-20210218074646-139b0bcd549d
          ├── hello@v0.1.1
          └── hello@v0.1.0
...
```

如果想清除所有本地已缓存的依赖包数据，可以执行 `go clean -modcache` 命令。



## 使用 go module 发布包

在上面的小节中我们学习了如何在项目中引入别人提供的依赖包，那么当我们想要在社区发布一个自己编写的代码包或者在公司内部编写一个供内部使用的公用组件时，我们该怎么做呢？接下来，我们就一起编写一个代码包并将它发布到`github.com`仓库，让它能够被全球的Go语言开发者使用。

在GitHub仓库新建一个hello仓库，然后将其拉取到本地的项目目录中：

```bash
D:\Go\study\src\go-01-basis>git clone https://github.com/AruNi-01/hello.git
```

在hello目录下执行`go mod init`命令创建`go.mod`文件，定义项目的引入路径为自己的仓库路径：

```bash
D:\Go\study\src\go-01-basis\hello>go mod init github.com/AruNi-01/hello
```

接下来我们在该项目根目录下创建 `hello.go` 文件，添加下面的内容：

```go
package hello

import "fmt"

func SayHi() {
	fmt.Println("Hello, I am Golang")
}
```

然后将该项目的代码 push 到仓库的远端分支，这样就对外发布了一个Go包。其他的开发者可以通过`github.com/AruNi-01/hello`这个引入路径下载并使用这个包了。

![image-20220505151727369](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e584d75ae278d4e20f72460dc7bb2ff5.png)

一个设计完善的包应该包含开源许可证及文档等内容，并且我们还应该尽心维护并适时发布适当的版本。github 上发布版本号使用`git tag`为代码包打上标签后，再push对应的tag即可：

```bash
hello $ git tag -a v0.1.0 -m "release version v0.1.0"
hello $ git push origin v0.1.0
```

经过上面的操作我们就发布了一个版本号为`v0.1.0`的版本：

![image-20220505152741276](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/26f1b29cfa9b567f02799e764a521eed.png)

Go modules中建议使用语义化版本控制，其建议的版本号格式如下：

![语义化版本号示意图](https://www.liwenzhou.com/images/Go/package/version_number.png)

其中：

- 主版本号：发布了不兼容的版本迭代时递增（breaking changes）。
- 次版本号：发布了功能性更新时递增。
- 修订号：发布了bug修复类更新时递增。



#### 发布新的主版本

现在我们的`hello`项目要进行与之前版本不兼容的更新，我们计划让`SayHi`函数支持向指定人发出问候。更新后的`SayHi`函数内容如下：

```go
package hello

import "fmt"

func SayHi(name string) {
	fmt.Printf("Hello %s, I am Golang!\n", name)
}
```

由于这次改动巨大（修改了函数之前的调用规则），对之前使用该包作为依赖的用户影响巨大。因此我们需要发布一个主版本号递增的`v2`版本。

在这种情况下，我们通常会修改当前包的引入路径，像下面的示例一样为引入路径添加版本后缀：

```go
module github.com/AruNi-01/hello/v2

go 1.18
```

把修改后的代码提交：

```bash
D:\Go\study\src\go-01-basis\hello>git add .

D:\Go\study\src\go-01-basis\hello>git commit -m "v2"

D:\Go\study\src\go-01-basis\hello>git push
```

打好 tag 推送到远程仓库:

```bash
D:\Go\study\src\go-01-basis\hello>git tag -a v2.0.0 -m "release version v2.0.0"

D:\Go\study\src\go-01-basis\hello>git push origin v2.0.0
```

这样在不影响使用旧版本的用户的前提下，我们新的版本也发布出去了。想要使用`v2`版本的代码包的用户只需按修改后的引入路径下载即可。

```bash
go get github.com/AruNi-01/hello/v2@v2.0.0
```

接下来我们在holiday包中引入自己远程仓库的hello包，需要注意引入路径要添加 v2 版本后缀。

```bash
D:\Go\study\src\go-01-basis\holiday>go get github.com/AruNi-01/hello/v2@v2.0.0
```

引入自己仓库的包后就可以直接使用了：

![image-20220505155016630](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/eeb03164c944c941b35c1e40f9423328.png)

```go
package main

import (
	"fmt"
	"github.com/AruNi-01/hello/v2"
)

func main() {
	fmt.Println("现在是假期时间...")

	//hello.SayHi()
	//
	//summer.Diving()

	hello.SayHi("AruNi")
}
```

输出：

```text
现在是假期时间...
Hello AruNi, I am Golang!
```



#### 废弃已发布版本

如果某个发布的版本存在致命缺陷不再想让用户使用时，我们可以使用`retract`声明废弃的版本。例如我们在`hello/go.mod`文件中按如下方式声明即可对外废弃`v0.1.0`版本。

```go
module github.com/AruNi-01/hello

go 1.18

retract v0.1.0
```

用户使用go get下载`v0.1.0`版本时就会收到提示，催促其升级到其他版本。



# 接口

接口（interface）定义了一个对象的行为规范，只定义规范不实现，由具体的对象来实现规范的细节。

在Go语言中接口（interface）是一种类型，一种抽象的类型。相较于之前章节中讲到的那些具体类型（字符串、切片、结构体等）更注重“我是谁”，接口类型更注重“我能做什么”的问题。接口类型就像是一种约定——概括了一种类型应该具备哪些方法，在Go语言中提倡使用面向接口的编程方式实现解耦。

## 接口类型

接口是一种由程序员来定义的类型，一个接口类型就是一组方法的集合，它规定了需要实现的所有方法。

相较于使用结构体类型，当我们使用接口类型说明相比于它是什么更关心它能做什么。

### 接口的定义

每个接口类型由任意个方法签名组成，接口的定义格式如下：

```go
type 接口类型名 interface{
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
    …
}
```

其中：

- 接口类型名：Go语言的接口在命名时，一般会在单词后面添加`er`，如写操作的接口叫`Writer`。接口名最好要能突出该接口的类型含义。
- 方法名：当方法名首字母是大写且这个接口类型名首字母也是大写时，这个方法可以被接口所在的包（package）之外的代码访问。
- 参数列表、返回值列表：参数列表和返回值列表中的参数变量名可以省略。

### 实现接口的条件

接口就是规定了一个**需要实现的方法列表**，在 Go 语言中一个类型只要实现了接口中规定的所有方法，那么我们就称它实现了这个接口。

我们定义的`Singer`接口类型，它包含一个`Sing`方法。

```go
// Singer 接口
type Singer interface {
	Sing()
}
```

我们有一个`Bird`结构体类型如下。

```go
type Bird struct {}
```

因为`Singer`接口只包含一个`Sing`方法，所以只需要给`Bird`结构体添加一个`Sing`方法就可以满足`Singer`接口的要求。

```go
// Sing Bird类型的Sing方法
func (b Bird) Sing() {
	fmt.Println("汪汪汪")
}
```

这样就称为`Bird`实现了`Singer`接口。

### 为什么要使用接口？

例子：

```go
package main

import "fmt"

type dog struct{}

func (d dog) say() {
	fmt.Println("汪汪汪~~")
}

type cat struct{}

func (c cat) say() {
	fmt.Println("喵喵喵~~")
}

// 再定义一个人对象
type person struct {
	name string
}

// 人也实现了say方法，就实现了sayer这个接口
func (p person) say() {
	fmt.Println("啊啊啊~~")
}

// 接口不管你是什么类型，只管你实现了什么方法
// 只要实现了say()方法的类型都可以称为sayer类型
type sayer interface {
	say()
}

// 打的函数，接收了1个sayer类型的参数
func beat(arg sayer) {
	arg.say() // 不管传进来的是什么，都要打Ta，就会执行say方法
}

func main() {
	d1 := dog{}
	beat(d1)
	c1 := cat{}
	beat(c1)

	p1 := person{
		name: "张三",
	}
	beat(p1)
}
```

输出：

```text
汪汪汪~~
喵喵喵~~
啊啊啊~~
```



### 接口类型变量

那实现了接口又有什么用呢？一个接口类型的变量能够存储所有实现了该接口的类型变量。

例如在上面的示例中，`person`类型实现了`sayer`接口，此时一个`sayer`类型的变量就能接收`person`类型的变量：

```go
var s sayer
p2 := person{
    name: "张三",
}
s = p2
fmt.Println(s)	// {张三}
```

## 值接收者和指针接收者

在结构体那一章节中，我们介绍了在定义结构体方法时既可以使用值接收者也可以使用指针接收者。那么对于实现接口来说使用值接收者和使用指针接收者有什么区别呢？接下来我们通过一个例子看一下其中的区别。

我们定义一个`Mover`接口，它包含一个`Move`方法。

```go
// Mover 定义一个接口类型
type Mover interface {
	Move()
}
```

### 值接收者实现接口

我们定义一个`Dog`结构体类型，并使用值接收者为其定义一个`Move`方法。

```go
// Dog 狗结构体类型
type Dog struct{}

// Move 使用值接收者定义Move方法实现Mover接口
func (d Dog) Move() {
	fmt.Println("狗会动")
}
```

此时实现`Mover`接口的是`Dog`类型。

```go
var x Mover    // 声明一个Mover类型的变量x

var d1 = Dog{} // d1是Dog类型
x = d1         // 可以将d1赋值给变量x
x.Move()

var d2 = &Dog{} // d2是Dog指针类型
x = d2          // 也可以将d2赋值给变量x
x.Move()
```

使用值接收者实现接口：**不管是结构体类型还是对应的结构体指针类型的变量都可以赋值给该接口变量**。

### 指针接收者实现接口

我们再来测试一下使用指针接收者实现接口有什么区别。

```go
// Cat 猫结构体类型
type Cat struct{}

// Move 使用指针接收者定义Move方法实现Mover接口
func (c *Cat) Move() {
	fmt.Println("猫会动")
}
```

此时实现`Mover`接口的是`*Cat`类型，我们可以将`*Cat`类型的变量直接赋值给`Mover`接口类型的变量`x`。

```go
var x Mover
var c1 = &Cat{} // c1是*Cat类型
x = c1          // 可以将c1当成Mover类型
x.Move()
```

但是不能给将`Cat`类型的变量赋值给`Mover`接口类型的变量`x`。

```go
// 下面的代码无法通过编译
var c2 = Cat{} // c2是Cat类型
x = c2         // 不能将c2当成Mover类型
```

由于Go语言中有对指针求值的语法糖，对于值接收者实现的接口，无论使用值类型还是指针类型都没有问题。但是我们并不总是能对一个值求址，所以对于指针接收者实现的接口要额外注意。

## 类型与接口的关系

### 一个类型实现多个接口

一个类型可以同时实现多个接口，而接口间彼此独立，不知道对方的实现。例如狗不仅可以叫，还可以动。我们完全可以分别定义`Sayer`接口和`Mover`接口，具体代码示例如下。

```go
// Sayer 接口
type Sayer interface {
	Say()
}

// Mover 接口
type Mover interface {
	Move()
}
```

`Dog`既可以实现`Sayer`接口，也可以实现`Mover`接口。

```go
type Dog struct {
	Name string
}

// 实现Sayer接口
func (d Dog) Say() {
	fmt.Printf("%s会叫汪汪汪\n", d.Name)
}

// 实现Mover接口
func (d Dog) Move() {
	fmt.Printf("%s会动\n", d.Name)
}
```

同一个类型实现不同的接口互相不影响使用。

```go
var d = Dog{Name: "旺财"}

var s Sayer = d
var m Mover = d

s.Say()  // 对Sayer类型调用Say方法
m.Move() // 对Mover类型调用Move方法
```

### 多种类型实现同一接口

Go语言中不同的类型还可以实现同一接口。例如在我们的代码世界中不仅狗可以动，汽车也可以动。我们可以使用如下代码体现这个关系。

```go
// 实现Mover接口
func (d Dog) Move() {
	fmt.Printf("%s会动\n", d.Name)
}

// Car 汽车结构体类型
type Car struct {
	Brand string
}

// Move Car类型实现Mover接口
func (c Car) Move() {
	fmt.Printf("%s速度70迈\n", c.Brand)
}
```

这样我们在代码中就可以把狗和汽车当成一个会动的类型来处理，不必关注它们具体是什么，只需要调用它们的`Move`方法就可以了。

```go
var obj Mover

obj = Dog{Name: "旺财"}
obj.Move()

obj = Car{Brand: "宝马"}
obj.Move()
```

上面的代码执行结果如下：

```go
旺财会跑
宝马速度70迈
```

一个接口的所有方法，不一定需要由一个类型完全实现，接口的方法可以通过在类型中嵌入其他类型或者结构体来实现。

```go
// WashingMachine 洗衣机
type WashingMachine interface {
	wash()
	dry()
}

// 甩干器
type dryer struct{}

// 实现WashingMachine接口的dry()方法
func (d dryer) dry() {
	fmt.Println("甩一甩")
}

// 海尔洗衣机
type haier struct {
	dryer //嵌入甩干器
}

// 实现WashingMachine接口的wash()方法
func (h haier) wash() {
	fmt.Println("洗刷刷")
}
```

## 接口组合

接口与接口之间可以通过互相嵌套形成新的接口类型，例如Go标准库`io`源码中就有很多接口之间互相组合的示例。

```go
// src/io/io.go

type Reader interface {
	Read(p []byte) (n int, err error)
}

type Writer interface {
	Write(p []byte) (n int, err error)
}

type Closer interface {
	Close() error
}

// ReadWriter 是组合Reader接口和Writer接口形成的新接口类型
type ReadWriter interface {
	Reader
	Writer
}

// ReadCloser 是组合Reader接口和Closer接口形成的新接口类型
type ReadCloser interface {
	Reader
	Closer
}

// WriteCloser 是组合Writer接口和Closer接口形成的新接口类型
type WriteCloser interface {
	Writer
	Closer
}
```

对于这种由多个接口类型组合形成的新接口类型，同样只需要实现新接口类型中规定的所有方法就算实现了该接口类型。

接口也可以作为结构体的一个字段，我们来看一段Go标准库`sort`源码中的示例。

```go
// src/sort/sort.go

// Interface 定义通过索引对元素排序的接口类型
type Interface interface {
    Len() int
    Less(i, j int) bool
    Swap(i, j int)
}


// reverse 结构体中嵌入了Interface接口
type reverse struct {
    Interface
}
```

通过在结构体中嵌入一个接口类型，从而让该结构体类型实现了该接口类型，并且还可以改写该接口的方法。

```go
// Less 为reverse类型添加Less方法，重写原Interface接口类型的Less方法
func (r reverse) Less(i, j int) bool {
	return r.Interface.Less(j, i)
}
```

`Interface`类型原本的`Less`方法签名为`Less(i, j int) bool`，此处重写为`r.Interface.Less(j, i)`，即通过将索引参数交换位置实现反转。

在这个示例中还有一个需要注意的地方是`reverse`结构体本身是不可导出的（结构体类型名称首字母小写），`sort.go`中通过定义一个可导出的`Reverse`函数来让使用者创建`reverse`结构体实例。

```go
func Reverse(data Interface) Interface {
	return &reverse{data}
}
```

这样做的目的是保证得到的`reverse`结构体中的`Interface`属性一定不为`nil`，否者`r.Interface.Less(j, i)`就会出现空指针panic。

此外在Go内置标准库`database/sql`中也有很多类似的结构体内嵌接口类型的使用示例，各位读者可自行查阅。

## 空接口

### 空接口的定义

空接口是指没有定义任何方法的接口类型。因此任何类型都可以视为实现了空接口。也正是因为空接口类型的这个特性，空接口类型的变量可以存储任意类型的值。

```go
package main

import "fmt"

// 空接口

// Any 不包含任何方法的空接口类型
type Any interface{}

// Dog 狗结构体
type Dog struct{}

func main() {
	var x Any

	x = "你好" // 字符串型
	fmt.Printf("type:%T value:%v\n", x, x)
	x = 100 // int型
	fmt.Printf("type:%T value:%v\n", x, x)
	x = true // 布尔型
	fmt.Printf("type:%T value:%v\n", x, x)
	x = Dog{} // 结构体类型
	fmt.Printf("type:%T value:%v\n", x, x)
}
```

通常我们在使用空接口类型时不必使用`type`关键字声明，可以像下面的代码一样直接使用`interface{}`。

```go
var x interface{}  // 声明一个空接口类型变量x
```

### 空接口的应用

#### 空接口作为函数的参数

使用空接口实现可以接收任意类型的函数参数。

```go
// 空接口作为函数参数
func show(a interface{}) {
	fmt.Printf("type:%T value:%v\n", a, a)
}
```

#### 空接口作为map的值

使用空接口实现可以保存任意值的字典。

```go
// 空接口作为map值
	var studentInfo = make(map[string]interface{})
	studentInfo["name"] = "沙河娜扎"
	studentInfo["age"] = 18
	studentInfo["married"] = false
	fmt.Println(studentInfo)
```

## 接口值

由于接口类型的值可以是任意一个实现了该接口的类型值，所以接口值除了需要记录具体**值**之外，还需要记录这个值属于的**类型**。也就是说接口值由 “类型” 和 “值” 组成，鉴于这两部分会根据存入值的不同而发生变化，我们称之为接口的`动态类型`和`动态值`。

![接口值示例](https://www.liwenzhou.com/images/Go/interface/interface01.png)

我们接下来通过一个示例来加深对接口值的理解。

下面的示例代码中，定义了一个`Mover`接口类型和两个实现了该接口的`Dog`和`Car`结构体类型。

```go
type Mover interface {
	Move()
}

type Dog struct {
	Name string
}

func (d *Dog) Move() {
	fmt.Println("狗在跑~")
}

type Car struct {
	Brand string
}

func (c *Car) Move() {
	fmt.Println("汽车在跑~")
}
```

首先，我们创建一个`Mover`接口类型的变量`m`。

```go
var m Mover
```

此时，接口变量`m`是接口类型的零值，也就是它的类型和值部分都是`nil`，就如下图所示。

![接口值示例](https://www.liwenzhou.com/images/Go/interface/interface02.png)

我们可以使用`m == nil`来判断此时的接口值是否为空。

```go
fmt.Println(m == nil)  // true
```

**注意：**我们不能对一个空接口值调用任何方法，否则会产生panic。

```go
m.Move() // panic: runtime error: invalid memory address or nil pointer dereference
```

接下来，我们将一个`*Dog`结构体指针赋值给变量`m`。

```go
m = &Dog{Name: "旺财"}
```

此时，接口值`m`的动态类型会被设置为`*Dog`，动态值为结构体变量的拷贝。

![接口值示例](https://www.liwenzhou.com/images/Go/interface/interface03.png)

然后，我们给接口变量`m`赋值为一个`*Car`类型的值。

```go
m = new(Car)
```

这一次，接口值的动态类型为`*Car`，动态值为`nil`。

![接口值示例](https://www.liwenzhou.com/images/Go/interface/interface04.png)

**注意：**此时接口变量`m`与`nil`并不相等，因为它只是动态值的部分为`nil`，而动态类型部分保存着对应值的类型。

```go
fmt.Println(m == nil) // false
```

接口值是支持相互比较的，**仅当接口值的动态类型和动态值都相等**时才相等。

```go
var (
	x Mover = new(Dog)
	y Mover = new(Car)
)
fmt.Println(x == y) // false
```

但是有一种特殊情况需要特别注意，如果接口值保存的动态类型相同，但是这个动态类型不支持互相比较（比如切片），那么对它们相互比较时就会引发panic。

```go
var z interface{} = []int{1, 2, 3}
fmt.Println(z == z) // panic: runtime error: comparing uncomparable type []int
```

## 类型断言

接口值可能赋值为任意类型的值，那我们如何从接口值获取其存储的具体数据呢？

我们可以借助标准库`fmt`包的格式化打印获取到接口值的动态类型。

```go
var m Mover

m = &Dog{Name: "旺财"}
fmt.Printf("%T\n", m) // *main.Dog

m = new(Car)
fmt.Printf("%T\n", m) // *main.Car
```

而`fmt`包内部其实是使用反射的机制在程序运行时获取到动态类型的名称。关于反射的内容会在后面详细学习。



而想要从接口值中获取到对应的实际值需要使用类型断言，其语法格式如下。

```go
x.(T)
```

其中：

- x：表示接口类型的变量
- T：表示断言`x`可能是的类型。

该语法返回两个参数，第一个参数是`x`转化为`T`类型后的变量，第二个值是一个布尔值，若为`true`则表示断言成功，为`false`则表示断言失败。

举个例子：

```go
var n Mover = &Dog{Name: "旺财"}
v, ok := n.(*Dog)
if ok {
	fmt.Println("类型断言成功")
	v.Name = "富贵" // 变量v是*Dog类型
} else {
	fmt.Println("类型断言失败")
}
```

如果对一个接口值有多个实际类型需要判断，推荐使用`switch`语句来实现。

```go
// justifyType 对传入的空接口类型变量x进行类型断言
func justifyType(x interface{}) {
	switch v := x.(type) {
	case string:
		fmt.Printf("x is a string，value is %v\n", v)
	case int:
		fmt.Printf("x is a int is %v\n", v)
	case bool:
		fmt.Printf("x is a bool is %v\n", v)
	default:
		fmt.Println("unsupport type！")
	}
}
```

由于接口类型变量能够动态存储不同类型值的特点，所以很多初学者会滥用接口类型（特别是空接口）来实现编码过程中的便捷。只有当有两个或两个以上的具体类型必须以相同的方式进行处理时才需要定义接口。切记不要为了使用接口类型而增加不必要的抽象，导致不必要的运行时损耗。

在 Go 语言中接口是一个非常重要的概念和特性，使用接口类型能够实现代码的抽象和解耦，也可以隐藏某个功能的内部实现，但是缺点就是在查看源码的时候，不太方便查找到具体实现接口的类型。

**小技巧：** 下面的代码可以在程序编译阶段验证某一结构体是否满足特定的接口类型。

```go
// 来自gin框架routergroup.go
type IRouter interface{ ... }

type RouterGroup struct { ... }

var _ IRouter = &RouterGroup{}  // 确保RouterGroup实现了接口IRouter
```

上面的代码中也可以使用`var _ IRouter = (*RouterGroup)(nil)`进行验证。

# Error 接口和错误处理

Go 语言中的错误处理与其他语言不太一样，它把错误当成一种值来处理，更强调判断错误、处理错误，而不是一股脑的 catch 捕获异常。不支持其他语言中使用`try/catch`捕获异常的方式。

## Error 接口

Go 语言中使用一个名为 `error` 接口来表示错误类型。

```go
type error interface {
    Error() string
}
```

`error` 接口只包含一个方法——`Error`，这个函数需要返回一个描述错误信息的字符串。

当一个函数或方法需要返回错误时，我们通常是把错误作为最后一个返回值。例如下面标准库 os 中打开文件的函数。

```go
func Open(name string) (*File, error) {
	return OpenFile(name, O_RDONLY, 0)
}
```

由于 error 是一个接口类型，默认零值为`nil`。所以我们通常将调用函数返回的错误与`nil`进行比较，以此来判断函数是否返回错误。例如你会经常看到类似下面的错误判断代码。

```go
file, err := os.Open("./xx.go")
if err != nil {
	fmt.Println("打开文件失败,err:", err)
	return
}
```

**注意**

当我们使用`fmt`包打印错误时会**自动调用 error 类型的 Error 方法**，也就是会打印出错误的描述信息。



## 创建错误

我们可以根据需求自定义 error，最简单的方式是使用`errors` 包提供的`New`函数创建一个错误。

### errors.New

函数签名如下：

```go
func New(text string) error
```

它接收一个字符串参数返回包含该字符串的错误。我们可以在函数返回时快速创建一个错误。

```go
func queryById(id int64) (*Info, error) {
	if id <= 0 {
		return nil, errors.New("无效的id")
	}

	// ...
}
```

或者用来定义一个错误变量，例如标准库`io.EOF`错误定义如下：

```go
var EOF = errors.New("EOF")
```



## fmt.Errorf

当我们需要传入格式化的错误描述信息时，使用`fmt.Errorf`是个更好的选择。

```go
fmt.Errorf("查询数据库失败，err: %v", err)
```

但是上面的方式会丢失原有的错误类型，只拿到错误描述的文本信息。

为了不丢失函数调用的错误链，使用`fmt.Errorf`时搭配使用特殊的格式化动词`%w`，可以实现基于已有的错误再包装得到一个新的错误。

```go
fmt.Errorf("查询数据库失败，err: %w", err)
```

对于这种二次包装的错误，`errors`包中提供了以下三个方法。

```go
func Unwrap(err error) error                 // 获得err包含下一层错误
func Is(err, target error) bool              // 判断err是否包含target
func As(err error, target interface{}) bool  // 判断err是否为target类型
```



## 错误结构体类型

此外我们还可以自己定义结构体类型，实现`error`接口。

```go
// OpError 自定义结构体类型
type OpError struct {
	Op string
}

// Error OpError 类型实现error接口
func (e *OpError) Error() string {
	return fmt.Sprintf("无权执行%s操作", e.Op)
}
```

# 反射

Go语言中的变量是分为两部分的：

- 类型信息：预先定义好的元信息
- 值信息：程序运行过程中可动态变化的



## 反射介绍

反射是指在程序运行期对程序本身进行访问和修改的能力。程序在编译时，变量被转换为内存地址，变量名不会被编译器写入到可执行部分。在运行程序时，程序无法获取自身的信息。

支持反射的语言可以在程序编译期将变量的反射信息，如字段名称、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期获取类型的反射信息，并且有能力修改它们。

Go程序在运行期使用reflect包访问程序的反射信息。

空接口可以存储任意类型的变量，那我们如何知道这个空接口保存的数据是什么呢？ 反射就是在运行时动态的获取一个变量的类型信息和值信息。



## reflect 包

在Go语言的反射机制中，任何接口值都由是`一个具体类型`和`具体类型的值`两部分组成的。

反射的相关功能由内置的reflect包提供，任意接口值在反射中都可以理解为由`reflect.Type`和`reflect.Value`两部分组成，并且reflect包提供了`reflect.TypeOf`和`reflect.ValueOf`两个函数来获取任意对象的Value和Type。



### TypeOf

`reflect.TypeOf()`函数可以获得任意值的**类型对象**（reflect.Type），程序通过类型对象可以访问任意值的类型信息。

```go
package main

import (
	"fmt"
	"reflect"
)

func reflectType(x interface{}) {
	v := reflect.TypeOf(x)
	fmt.Printf("type:%v\n", v)
}
func main() {
	var a float32 = 3.14
	reflectType(a) // type:float32
	var b int64 = 100
	reflectType(b) // type:int64
}
```

#### type name 和 type kind

在反射中关于类型还划分为两种：`类型（Type）`和`种类（Kind）`。

因为在Go语言中我们可以使用type关键字构造很多自定义类型，而`种类（Kind）`就是指底层的类型，但在反射中，当需要区分指针、结构体等大品种的类型时，就会用到`种类（Kind）`。 举个例子，我们定义了两个指针类型和两个结构体类型，通过反射查看它们的类型和种类。

```go
package main

import (
	"fmt"
	"reflect"
)

type myInt int64

func reflectType(x interface{}) {
	t := reflect.TypeOf(x)
	fmt.Printf("type:%v kind:%v\n", t.Name(), t.Kind())
}

func main() {
	var a *float32 // 指针
	var b myInt    // 自定义类型
	var c rune     // int32类型别名
	reflectType(a) // type: kind:ptr
	reflectType(b) // type:myInt kind:int64
	reflectType(c) // type:int32 kind:int32

	type person struct {
		name string
		age  int
	}
	type book struct{ title string }
	var d = person{
		name: "沙河小王子",
		age:  18,
	}
	var e = book{title: "《跟小王子学Go语言》"}
	reflectType(d) // type:person kind:struct
	reflectType(e) // type:book kind:struct
}
```

Go语言的反射中像数组、切片、Map、指针等类型的变量，它们的`.Name()`都是返回`空`。

在`reflect`包中定义的Kind类型如下：

```go
type Kind uint
const (
    Invalid Kind = iota  // 非法类型
    Bool                 // 布尔型
    Int                  // 有符号整型
    Int8                 // 有符号8位整型
    Int16                // 有符号16位整型
    Int32                // 有符号32位整型
    Int64                // 有符号64位整型
    Uint                 // 无符号整型
    Uint8                // 无符号8位整型
    Uint16               // 无符号16位整型
    Uint32               // 无符号32位整型
    Uint64               // 无符号64位整型
    Uintptr              // 指针
    Float32              // 单精度浮点数
    Float64              // 双精度浮点数
    Complex64            // 64位复数类型
    Complex128           // 128位复数类型
    Array                // 数组
    Chan                 // 通道
    Func                 // 函数
    Interface            // 接口
    Map                  // 映射
    Ptr                  // 指针
    Slice                // 切片
    String               // 字符串
    Struct               // 结构体
    UnsafePointer        // 底层指针
)
```

### ValueOf

`reflect.ValueOf()`返回的是`reflect.Value`类型，其中包含了原始值的值信息。`reflect.Value`与原始值之间可以互相转换。

`reflect.Value`类型提供的获取原始值的方法如下：

| 方法                     | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| Interface() interface {} | 将值以 interface{} 类型返回，可以通过类型断言转换为指定类型  |
| Int() int64              | 将值以 int 类型返回，所有有符号整型均可以此方式返回          |
| Uint() uint64            | 将值以 uint 类型返回，所有无符号整型均可以此方式返回         |
| Float() float64          | 将值以双精度（float64）类型返回，所有浮点数（float32、float64）均可以此方式返回 |
| Bool() bool              | 将值以 bool 类型返回                                         |
| Bytes() []bytes          | 将值以字节数组 []bytes 类型返回                              |
| String() string          | 将值以字符串类型返回                                         |

#### 通过反射获取值

```go
func reflectValue(x interface{}) {
	v := reflect.ValueOf(x)
	k := v.Kind()
	switch k {
	case reflect.Int64:
		// v.Int()从反射中获取整型的原始值，然后通过int64()强制类型转换
		fmt.Printf("type is int64, value is %d\n", int64(v.Int()))
	case reflect.Float32:
		// v.Float()从反射中获取浮点型的原始值，然后通过float32()强制类型转换
		fmt.Printf("type is float32, value is %f\n", float32(v.Float()))
	case reflect.Float64:
		// v.Float()从反射中获取浮点型的原始值，然后通过float64()强制类型转换
		fmt.Printf("type is float64, value is %f\n", float64(v.Float()))
	}
}
func main() {
	var a float32 = 3.14
	var b int64 = 100
	reflectValue(a) // type is float32, value is 3.140000
	reflectValue(b) // type is int64, value is 100
	// 将int类型的原始值转换为reflect.Value类型
	c := reflect.ValueOf(10)
	fmt.Printf("type c :%T\n", c) // type c :reflect.Value
}
```

#### 通过反射设置变量的值

想要在函数中通过反射修改变量的值，需要注意**函数参数传递的是值拷贝**，必须**传递变量的址才能修改变量值**。而反射中使用专有的`Elem()`方法来获取指针对应的值。

```go
package main

import (
	"fmt"
	"reflect"
)

// 传递值会引发panic
func reflectSetValue1(x interface{}) {
	v := reflect.ValueOf(x)
	if v.Kind() == reflect.Int64 {
		v.SetInt(200) //修改的是副本，reflect包会引发panic
	}
}

func reflectSetValue2(x interface{}) {
	v := reflect.ValueOf(x)
	// 反射中使用 Elem()方法获取指针对应的值
	if v.Elem().Kind() == reflect.Int64 {
		v.Elem().SetInt(200)
	}
}
func main() {
	var a int64 = 100
        // 传递值会引发panic
	// reflectSetValue1(a) //panic: reflect: reflect.Value.SetInt using unaddressable value
	
        // 必须传递地址
        reflectSetValue2(&a)
	fmt.Println(a)
}
```



### IsNil() 和 IsValid()

#### isNil()

```go
func (v Value) IsNil() bool
```

`IsNil()` 返回v持有的值是否为nil。v持有的值的分类必须是通道、函数、接口、映射、指针、切片之一；否则 IsNil 函数会导致 panic。

#### isValid()

```go
func (v Value) IsValid() bool
```

`IsValid()` 返回v是否持有一个值。如果v是Value零值会返回假，此时v除了 IsValid、String、Kind 之外的方法都会导致 panic。

#### 举个例子

`IsNil()`常被用于判断指针是否为空；`IsValid()`常被用于判定返回值是否有效。

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	// *int类型空指针
	var a *int
	fmt.Println("var a *int IsNil:", reflect.ValueOf(a).IsNil())
	
	// nil值
	fmt.Println("nil IsValid:", reflect.ValueOf(nil).IsValid())
	
	// 实例化一个匿名结构体
	b := struct{}{}
	// 尝试从结构体中查找"abc"字段
	fmt.Println("不存在的结构体成员:", reflect.ValueOf(b).FieldByName("abc").IsValid())
	
	// 尝试从结构体中查找"abc"方法
	fmt.Println("不存在的结构体方法:", reflect.ValueOf(b).MethodByName("abc").IsValid())
	
	// map
	c := map[string]int{}
	
	// 尝试从map中查找一个不存在的键
	fmt.Println("map中不存在的键：", reflect.ValueOf(c).MapIndex(reflect.ValueOf("娜扎")).IsValid())
}
```

输出：

```text
var a *int IsNil: true
nil IsValid: false
不存在的结构体成员: false
不存在的结构体方法: false
map中不存在的键： false
```



## 结构体反射

### 相关方法

任意值通过`reflect.TypeOf()`获得反射对象信息后，如果它的类型是结构体，可以通过反射值对象（`reflect.Type`）的`NumField()`和`Field()`方法获得结构体成员的详细信息。

`reflect.Type`中与获取结构体成员相关的的方法如下表所示。

| 方法                                                        | 说明                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Field(i int) StructField                                    | 根据索引，返回索引对应的结构体字段的信息。                   |
| NumField() int                                              | 返回结构体成员字段数量。                                     |
| FieldByName(name string) (StructField, bool)                | 根据给定字符串返回字符串对应的结构体字段的信息。             |
| FieldByIndex(index []int) StructField                       | 多层成员访问时，根据 []int 提供的每个结构体的字段索引，返回字段的信息。 |
| FieldByNameFunc(match func(string) bool) (StructField,bool) | 根据传入的匹配函数匹配需要的字段。                           |
| NumMethod() int                                             | 返回该类型的方法集中方法的数目                               |
| Method(int) Method                                          | 返回该类型方法集中的第i个方法                                |
| MethodByName(string)(Method, bool)                          | 根据方法名返回该类型方法集中的方法                           |

### StructField类型

`StructField`类型用来描述结构体中的一个字段的信息。

`StructField`的定义如下：

```go
type StructField struct {
    // Name是字段的名字。PkgPath是非导出字段的包路径，对导出字段该字段为""。
    // 参见http://golang.org/ref/spec#Uniqueness_of_identifiers
    Name    string
    PkgPath string
    Type      Type      // 字段的类型
    Tag       StructTag // 字段的标签
    Offset    uintptr   // 字段在结构体中的字节偏移量
    Index     []int     // 用于Type.FieldByIndex时的索引切片
    Anonymous bool      // 是否匿名字段
}
```

### 示例

当我们使用反射得到一个结构体数据之后可以通过索引依次获取其字段信息，也可以通过字段名去获取指定的字段信息：

```go
package main

import (
	"fmt"
	"reflect"
)

type student struct {
	Name  string `json:"name"`
	Score int    `json:"score"`
}

func main() {
	stu1 := student{
		Name:  "小王子",
		Score: 90,
	}

	t := reflect.TypeOf(stu1)

	// 查看t的类型和种类
	fmt.Println(t.Name(), t.Kind()) // student struct

	// 通过for循环遍历结构体
	for i := 0; i < t.NumField(); i++ {
		// 通过索引获得字段信息
		fieldInfo := t.Field(i)
		// Tag.Get()：根据json的key获取json的tag(json的值)
		fmt.Printf("name:%s index:%d type:%v json tag:%v\n", fieldInfo.Name, fieldInfo.Index, fieldInfo.Type, fieldInfo.Tag.Get("json"))
	}

	// 通过字段名获取指定结构体字段信息
	if scoreField, ok := t.FieldByName("Score"); ok {
		fmt.Printf("name:%s index:%d type:%v json tag:%v\n", scoreField.Name, scoreField.Index, scoreField.Type, scoreField.Tag.Get("json"))
	}
}
```

输出：

```text
student struct
name:Name index:[0] type:string json tag:
name:Score index:[1] type:int json tag:score
name:Score index:[1] type:int json tag:score
```



接下来编写一个函数`printMethod(s interface{})`来遍历打印s包含的方法。

```go
// 给student添加两个方法 Study和Sleep(注意首字母大写)
func (s student) Study() string {
	msg := "好好学习，天天向上。"
	fmt.Println(msg)
	return msg
}

func (s student) Sleep() string {
	msg := "好好睡觉，快快长大。"
	fmt.Println(msg)
	return msg
}

func printMethod(x interface{}) {
	// 获取x的类型
	t := reflect.TypeOf(x)
	// 获取x的值信息
	v := reflect.ValueOf(x)

	fmt.Println(t.NumMethod())

	// 遍历方法
	for i := 0; i < t.NumMethod(); i++ {
		// 方法名使用类型的方法调用
		fmt.Printf("method name:%s\n", t.Method(i).Name)

		// 方法类型使用值信息的方法调用
		fmt.Printf("method type:%s\n", v.Method(i).Type())

		// 通过反射调用方法传递的参数必须是 []reflect.Value 类型
		var args []reflect.Value
		// 调用第i个方法
		v.Method(i).Call(args)
	}
}
```

main方法调用后，输出：

```text
2
method name:Sleep
method type:func() string
好好睡觉，快快长大。
method name:Study
method type:func() string
好好学习，天天向上。
```



## 反射是把双刃剑

反射是一个强大并富有表现力的工具，能让我们写出更灵活的代码。但是反射不应该被滥用，原因有以下三个：

1. 基于反射的代码是极其脆弱的，反射中的类型错误会在真正运行的时候才会引发panic，那很可能是在代码写完的很长时间之后。
2. 大量使用反射的代码通常难以理解。
3. 反射的性能低下，基于反射实现的代码通常比正常代码运行速度慢一到两个数量级。





# 并发编程

并发编程在当前软件领域是一个非常重要的概念，随着CPU等硬件的发展，我们无一例外的想让我们的程序运行的快一点、再快一点。

Go语言在语言层面天生支持并发，充分利用现代CPU的多核优势，这也是Go语言能够大范围流行的一个很重要的原因。



## 基本概念

首先我们先来了解几个与并发编程相关的基本概念。

### 串行、并发与并行

串行：串行是大家排队一个一个来，并行是大家一起上。

并发：同一时间段内执行多个任务。

- 单核处理器；
- 逻辑上同步运行。

并行：同一时刻执行多个任务。

- 多核处理器；
- 物理上同步运行。

![image-20220506185701884](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bf2dad9826f49fe8c8c8f9cdc6ed6be4.png)

### 进程、线程和协程

进程（process）：程序运行以后，会在内存空间里形成一个独立的内存体，这个内存体**有自己独立的地址空间，有自己的堆**，上级挂靠单位是操作系统。**操作系统会以进程为单位，分配系统资源（CPU时间片、内存等资源），进程是资源分配的最小单位**。

![在这里插入图片描述](https://img-blog.csdn.net/20150603133335514)

线程（thread）：操作系统基于进程开启的轻量级进程，是进程的一个执行实体，是操作系统调度执行的最小单位。





> 进程和线程的联系

- **一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程**；

- 资源分配给进程，同一进程的所有线程共享该进程的所有资源；
- 处理机分给线程，即**真正在处理机上运行的是线程**；
- 线程在执行过程中，需要协作同步。不同进程的线程间要利用消息通信的办法实现同步。





协程（coroutine）：比线程更加轻量级，协程不是被操作系统内核所管理，而完全是由程序所控制（也就是在用户态执行）

![在这里插入图片描述](http://5b0988e595225.cdn.sohucs.com/images/20180622/6765e36cc4604fba897976638af03524.jpeg)



### 并发模型

业界将如何实现并发编程总结归纳为各式各样的并发模型，常见的并发模型有以下几种：

- 线程&锁模型
- Actor模型
- CSP模型
- Fork&Join模型

Go语言中的并发程序主要是通过基于CSP（communicating sequential processes）的goroutine和channel来实现，当然也支持使用传统的多线程共享内存的并发方式。



## goroutine

Goroutine 是 Go  语言支持并发的核心，在一个Go程序中同时创建成百上千个goroutine是非常普遍的，一个goroutine会以一个很小的栈开始其生命周期，一般只需要2KB。区别于操作系统线程由系统内核进行调度，  goroutine 是由Go运行时（runtime）负责调度。例如Go运行时会智能地将 m个goroutine  合理地分配给n个操作系统线程，实现类似m:n的调度机制，不再需要Go开发者自行在代码层面维护一个线程池。

Goroutine 是 Go 程序中最基本的并发执行单元。每一个 Go 程序都至少包含一个 goroutine——main goroutine，当 Go 程序启动时它会自动创建。

在Go语言编程中你不需要去自己写进程、线程、协程，你的技能包里只有一个技能——goroutine，当你需要让某个任务并发执行的时候，你只需要把这个任务包装成一个函数，开启一个 goroutine 去执行这个函数就可以了，就是这么简单粗暴。



### go 关键字

Go语言中使用 goroutine 非常简单，只需要**在函数或方法调用前加**上`go`关键字就可以创建一个 goroutine ，从而让该函数或方法在新创建的 goroutine 中执行。

```go
go f()  // 创建一个新的 goroutine 运行函数f
```

匿名函数也支持使用`go`关键字创建 goroutine 去执行。

```go
go func(){
  // ...
}()
```

一个 goroutine 必定对应一个函数/方法，可以创建多个 goroutine 去执行相同的函数/方法。

### 启动单个 goroutine

启动 goroutine 的方式非常简单，只需要在调用函数（普通函数和匿名函数）前加上一个`go`关键字。

我们先来看一个在 main 函数中执行普通函数调用的示例。

```go
package main

import "fmt"

func hello() {
	fmt.Println("hello")
}

func main() {
	hello()
	fmt.Println("你好")
}
```

输出：

```bash
hello
你好
```

代码中 hello 函数和其后面的打印语句是串行的。

![main goroutine](https://www.liwenzhou.com/images/Go/concurrence/goroutine01.png)

接下来我们在调用 hello 函数前面加上关键字`go`：

```go
func main() { // 开启一个主goroutine去执行main函数
	go hello() // 开启另一个goroutine去执行hello函数
	fmt.Println("你好")
}
```

输出：

```bash
你好
```

这一次的执行结果只打印了 ”你好”，并没有打印 `hello`。这是为什么呢？

其实在 Go 程序启动时，Go 程序就会为 main 函数创建一个默认的 goroutine 。在上面的代码中我们在 main 函数中使用 go 关键字创建了另外一个 goroutine 去执行 hello 函数，而此时 main goroutine  还在继续往下执行，我们的程序中此时存在两个并发执行的 goroutine。

当 main 函数结束时整个程序也就结束了，同时 main  goroutine 也结束了，所有由 main goroutine 创建的 goroutine 也会一同退出。也就是说我们的 main  函数退出太快，另外一个 goroutine 中的函数还未执行完程序就退出了，导致未打印出 “hello”。

所以我们要想办法让 main 函数 “等一等”，让在另一个 goroutine 中运行的 hello 函数在 main 函数退出之前执行。

其中最简单粗暴的方式就是在  main 函数中 “time.Sleep” 一秒钟了：

```go
package main

import (
	"fmt"
	"time"
)

func hello() {
	fmt.Println("hello")
}

func main() {
	go hello()
	fmt.Println("你好")
	time.Sleep(time.Second)
}
```

输出如下结果，并且会停顿一会儿程序才退出。

```bash
你好
hello
```

为什么会先打印`你好`呢？

这是因为在程序中创建 goroutine 执行函数需要一定的开销，而与此同时 main 函数所在的 goroutine 是继续执行的。

![main goroutine和hello goorutine](https://www.liwenzhou.com/images/Go/concurrence/goroutine02.png)

在上面的程序中使用`time.Sleep`让 main goroutine 等待 hello goroutine执行结束是不优雅的，当然也是不准确的。

Go 语言中通过 `sync`包 为我们提供了一些常用的并发原语，我们会在后面学习`sync`包中的内容。

先介绍一下 sync 包中的`WaitGroup`。当你并不关心并发操作的结果或者有其它方式收集并发操作的结果时，`WaitGroup`是实现等待一组并发操作完成的好方法。

下面的示例在 main goroutine 中使用`sync.WaitGroup`来等待 hello goroutine 完成后再退出。

```go
package main

import (
	"fmt"
	"sync"
)

// 声明全局等待组变量
var wg sync.WaitGroup

func hello() {
	fmt.Println("hello")
	wg.Done() // 告知当前goroutine完成了，把计数器-1
}

func main() {
	wg.Add(1) // 登记1个goroutine
	go hello()
	fmt.Println("你好")
	wg.Wait() // 阻塞等待登记的goroutine完成(计数器为0)
}
```

输出结果和之前一致，但是这一次程序不再会有多余的停顿，hello goroutine 执行完毕后程序直接退出。

### 启动多个 goroutine

在 Go 语言中实现并发就是这样简单，我们还可以启动多个 goroutine 。让我们再来看一个新的代码示例。这里同样使用了`sync.WaitGroup`来实现 goroutine 的同步。

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func hello(i int) {
	defer wg.Done() // goroutine结束就登记-1
	fmt.Println("hello", i)
}

func main() {
	for i := 0; i < 100; i++ {
		wg.Add(1) // 启动一个goroutine就登记+1
		go hello(i)
	}
	fmt.Println("你好")
	wg.Wait() // 等待所有登记的goroutine都结束
}
```

多次执行上面的代码会发现每次终端上打印数字的顺序都不一致。这是因为100个 goroutine 是并发执行的，而 goroutine 的调度是随机的。

![image-20220506203317207](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1d573accce65c8eaf3a0d6d25f7b8194.png)

### 匿名函数 goroutine

将上面的hello函数改成匿名函数，直接调用：

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func main() {
	wg.Add(100)
	for i := 0; i < 100; i++ {
		go func() {
			fmt.Println("hello", i)
			wg.Done() // 执行完一次就将计数器-1
		}()
	}
	fmt.Println("你好")
	wg.Wait()
}
```

运行输出，发现会有很多相同的结果：

![image-20220506204205207](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/35053138de09be802409854c0b97249d.png)

这是因为这个匿名函数也是一个闭包，匿名函数中包含了一个外部作用域的变量 i，当匿名函数的 goroutine 在执行的时候，还要去外面找 i 的值；可能外面的for循环已经遍历好多次了，匿名函数的 goroutine 才开始执行，所以慢的那些 goroutine 就会打印出相同的值。此例中就是for循环的 i 已经遍历到100了，还有好几个 goroutine 才开始执行，所以都输出了相同的。

解决方法：让匿名函数中的 i 在自己内部去查找，只需要将外部作用域的 i 当成参数传进匿名函数中即可。

```go
func main() {
	wg.Add(100)
	for i := 0; i < 100; i++ {
		go func(i int) {
			fmt.Println("hello", i)
			wg.Done() // 执行完一次就将计数器-1
		}(i)
	}
	fmt.Println("你好")
	wg.Wait()
}
```

此时输出正常，没有出现重复的结果。



### 动态栈

操作系统的线程一般都有固定的栈内存（通常为2MB），而 goroutine 的栈不是固定的，可以根据需要动态地增大或缩小，Go 的 runtime 会自动为 goroutine 分配合适的栈空间。

而且 Go 的 goroutine 非常轻量级，一个 goroutine  的初始栈空间很小（一般为2KB），所以在 Go 语言中一次创建数万个 goroutine 也是可能的。并



### goroutine 调度

操作系统的线程被操作系统内核调度时会挂起当前执行的线程，并将它的寄存器内容保存到内存中，选出下一次要执行的线程并从内存中恢复该线程的寄存器信息，然后恢复执行该线程的现场并开始执行线程。从一个线程切换到另一个线程需要完整的上下文切换。因为可能需要多次内存访问，索引这个切换上下文的操作开销较大，会增加运行的cpu周期。

区别于操作系统内核调度操作系统线程，goroutine 的调度是Go语言运行时（runtime）层面的实现，是完全由 Go  语言本身实现的一套调度系统 — go scheduler。它的作用是按照一定的规则将所有的 goroutine 调度到操作系统线程上执行。

在经历数个版本的迭代之后，目前 Go 语言的调度器采用的是 `GPM` 调度模型。

![gpm](https://www.liwenzhou.com/images/Go/concurrence/gpm.png)

其中：

- G：表示 goroutine，每执行一次`go f()`就创建一个 G，包含要执行的函数和上下文信息。
- 全局队列（Global Queue）：存放等待运行的 G。
- P：表示 goroutine 执行所需的资源，最多有 GOMAXPROCS 个。
- P 的本地队列：同全局队列类似，存放的也是等待运行的G，存的数量有限，不超过256个。新建 G 时，G 优先加入到 P 的本地队列，如果本地队列满了会批量移动部分 G 到全局队列。
- M：线程想运行任务就得获取 P，从 P 的本地队列获取 G，当 P 的本地队列为空时，M 也会尝试从全局队列或其他 P 的本地队列获取 G。M 运行 G，G 执行之后，M 会从 P 获取下一个 G，不断重复下去。
- Goroutine 调度器和操作系统调度器是通过 M 结合起来的，每个 M 都代表了1个内核线程，操作系统调度器负责把内核线程分配到 CPU 的核上执行。

单从线程调度讲，Go语言相比起其他语言的优势在于OS线程是由OS内核来调度的， goroutine  则是由Go运行时（runtime）自己的调度器调度的，完全是在用户态下完成的，  不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池，  不直接调用系统的malloc函数（除非内存池需要改变），成本比调度OS线程低很多。  另一方面充分利用了多核的硬件资源，近似的把若干goroutine均分在物理线程上， 再加上本身 goroutine  的超轻量级，以上种种特性保证了 goroutine 调度方面的性能。

### GOMAXPROCS

Go 运行时的调度器使用 `GOMAXPROCS` 参数来确定需要使用多少个 OS 线程来同时执行 Go 代码，默认值是机器上的 CPU 核心数。

Go语言中可以通过 `runtime.GOMAXPROCS` 函数设置当前程序并发时占用的 CPU逻辑核心数。（Go1.15版本之前，默认使用的是单核心执行。Go1.15 版本之后，默认使用全部的CPU 逻辑核心数。）



## channel

单纯地将函数并发执行是没有意义的。函数与函数间需要交换数据才能体现并发执行函数的意义。

虽然可以使用共享内存进行数据交换，但是共享内存在不同的 goroutine 中容易发生竞态问题。为了保证数据交换的正确性，很多并发模型中必须使用互斥量对内存进行加锁，这种做法势必造成性能问题。

Go语言采用的并发模型是 CSP（Communicating Sequential Processes），提倡**通过通信共享内存**而不是**通过共享内存而实现通信**。

如果说 goroutine 是Go程序并发的执行体，`channel`就是它们之间的连接。`channel`是可以让一个 goroutine 发送特定值到另一个 goroutine 的通信机制。

Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序。每一个通道都是一个具体类型的导管，也就是声明channel的时候需要为其指定元素类型。



### channel类型

`channel`是 Go 语言中一种特有的类型。声明通道类型变量的格式如下：

```go
var 变量名称 chan 元素类型
```

其中：

- chan：是关键字
- 元素类型：是指通道中传递元素的类型

举几个例子：

```go
var ch1 chan int   // 声明一个传递整型的通道
var ch2 chan bool  // 声明一个传递布尔型的通道
var ch3 chan []int // 声明一个传递int切片的通道
```

未初始化的通道类型变量其默认零值是`nil`。

```go
var ch chan int
fmt.Println(ch) // <nil>
```

### 初始化channel

声明的通道类型变量需要使用内置的`make`函数初始化之后才能使用。具体格式如下：

```go
make(chan 元素类型, [缓冲大小])
```

其中：

- channel的缓冲大小是可选的。

举几个例子：

```go
ch4 := make(chan int)
ch5 := make(chan bool, 1)  // 声明一个缓冲区大小为1的通道
```

### channel操作

通道共有发送（send）、接收(receive）和关闭（close）三种操作。而发送和接收操作都使用`<-`符号。

现在我们先使用以下语句定义一个通道：

```go
ch := make(chan int)
```

将一个值发送到通道中。

```go
ch <- 10 // 把10发送到ch中
```

从一个通道中接收值。

```go
x := <- ch // 从ch中接收值并赋值给变量x
<-ch       // 从ch中接收值，忽略结果
```

我们通过调用内置的`close`函数来关闭通道。

```go
close(ch)
```

**注意**：一个通道值是可以被垃圾回收掉的。通道通常由发送方执行关闭操作，并且只有在接收方明确等待通道关闭的信号时才需要执行关闭操作。它和关闭文件不一样，通常在结束操作之后关闭文件是必须要做的，但关闭通道不是必须的。

关闭后的通道有以下特点：

1. 对一个关闭的通道再发送值就会导致 panic。
2. 对一个关闭的通道进行接收会一直获取值直到通道为空。
3. 对一个关闭的并且没有值的通道执行接收操作会得到对应类型的零值。
4. 关闭一个已经关闭的通道会导致 panic。



### 无缓冲的通道

![img](https://www.topgoer.com/static/7.1/3.png)

无缓冲的通道又称为阻塞的通道。示例：

```go
func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
```

上面这段代码能够通过编译，但是执行的时候会出现以下错误：

```bash
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
        .../main.go:8 +0x54
```

`deadlock`表示我们程序中的 goroutine 都被挂起导致程序死锁了。为什么会出现`deadlock`错误呢？

因为我们使用`ch := make(chan int)`创建的是无缓冲的通道，**无缓冲的通道只有在有接收方能够接收值的时候才能发送成功，否则会一直处于等待发送的阶段**。同理，如果对一个无缓冲通道执行接收操作时，没有任何向通道中发送值的操作那么也会导致接收操作阻塞。简单来说就是无缓冲的通道必须有至少一个接收方才能发送成功。

上面的代码会阻塞在`ch <- 10`这一行代码形成死锁，那如何解决这个问题呢？

其中一种可行的方法是创建一个 goroutine 去接收值，例如：

```go
func recv(c chan int) {
	ret := <-c
	fmt.Println("接收成功", ret)
}

func main() {
	ch := make(chan int)
	go recv(ch) // 创建一个 goroutine 从通道接收值
	ch <- 10
	fmt.Println("发送成功")
}
```

首先无缓冲通道`ch`上的发送操作会阻塞，直到另一个 goroutine  在该通道上执行接收操作，这时数字10才能发送成功，两个 goroutine 将继续执行。相反，如果接收操作先执行，接收方所在的  goroutine 将阻塞，直到 main goroutine 中向该通道发送数字10。

使用无缓冲通道进行通信将导致发送和接收的 goroutine 同步化。因此，无缓冲通道也被称为`同步通道`。

### 有缓冲的通道

还有另外一种解决上面死锁问题的方法，那就是使用有缓冲区的通道。

![img](https://www.topgoer.com/static/7.1/4.png)

我们可以在使用 make 函数初始化通道时，可以为其指定通道的容量，例如：

```go
func main() {
	ch := make(chan int, 1) // 创建一个容量为1的有缓冲区通道
	ch <- 10
	fmt.Println("发送成功")
}
```

只要通道的容量大于零，那么该通道就属于有缓冲的通道，通道的容量表示通道中最大能存放的元素数量。当通道内已有元素数达到最大容量后，再向通道执行发送操作就会阻塞，除非有从通道执行接收操作。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

我们可以使用内置的`len`函数获取通道内元素的数量，使用`cap`函数获取通道的容量，虽然我们很少会这么做。

### 从通道中循环取值

当通过通道发送有限的数据时，我们可以通过close函数关闭通道来告知从该通道接收值的goroutine停止等待。当通道被关闭时，往该通道发送值会引发panic，从该通道里接收的值一直都是类型零值。那如何判断一个通道是否被关闭了呢？

下面示例给出两种方法：

```go
package main

import "fmt"

// 方式1：多返回值模式
func f1(ch chan int) {
	for {
		v, ok := <-ch
		if !ok {
			fmt.Println("通道已关闭")
			break
		}
		fmt.Printf("v: %#v ok: %#v\n", v, ok)
	}
}

// 方式2：for range接收（常用）
func f2(ch chan int) {
	for v := range ch {
		fmt.Println(v)
	}
}

func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	close(ch)
	f1(ch)
	f2(ch)
}
```

输出：

```text
v: 1 ok: true
v: 2 ok: true
通道已关闭
```

**注意**：目前Go语言中并没有提供一个不对通道进行读取操作就能判断通道是否被关闭的方法。不能简单的通过`len(ch)`操作来判断通道是否被关闭。



### 单向通道

在某些场景下我们可能会将通道作为参数在多个任务函数间进行传递，通常我们会选择在不同的任务函数中对通道的使用进行限制，比如限制通道在某个函数中只能执行发送或只能执行接收操作。

Go语言中提供了**单向通道**来处理这种需要限制通道只能进行某种操作的情况：

```go
<- chan int 	// 只接收通道，只能接收不能发送
chan <- int 	// 只发送通道，只能发送不能接收
```

其中，箭头`<-`和关键字`chan`的相对位置表明了当前通道允许的操作，这种限制将在编译阶段进行检测。另外对一个只接收通道执行close也是不允许的，因为默认通道的关闭操作应该由发送方来完成。

示例：

```go
package main

import "fmt"

// 生成0-100的数字发送到ch1中
func counter(ch chan<- int) {
	for i := 0; i < 100; i++ {
		ch <- i
	}
	close(ch)
}

// 从ch1中取出数据计算它的平方，把结果发送到ch2中
func squarer(ch1 <-chan int, ch2 chan<- int) {
	for i := range ch1 {
		ch2 <- i * i
	}
	close(ch2)
}

// 打印通道中的值
func printer(ch <-chan int) {
	for i := range ch {
		fmt.Println(i)
	}
}

func main() {
	ch1 := make(chan int, 100)
	ch2 := make(chan int, 100)
	go counter(ch1)
	go squarer(ch1, ch2)
	printer(ch2)
}
```

运行测试，成功输出0-100平方后的数值。



### 总结

![image-20220507194919278](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ab1eb1083f8cada8dfa0d90fcac49c57.png)

**注意：**对已经关闭的通道再执行 close 也会引发 panic。



## goroutine 池

### worker pool（goroutine池）

在工作中我们通常会使用`worker pool`模式，控制`goroutine`的数量，防止`goroutine`泄露和暴涨。

一个简易的`worker pool`示例如下：

```go
package main

import (
	"fmt"
	"time"
)

// 从jobs通道中取出工作，工作结束后将结果发送到results通道
func worker(id int, jobs <-chan int, results chan<- int) {
	for job := range jobs {
		fmt.Printf("worker: %d start job: %d\n", id, job)
		results <- job * 2
		time.Sleep(time.Millisecond * 500) // 模拟执行了5ms
		fmt.Printf("worker: %d stop job: %d\n", id, job)
	}
	// results通道接收完后关闭，避免下面从中取结果输出时一直等待，造成死锁
	close(results)
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 开启3个goroutine去执行下面5个任务，1个goroutine对应一个worker
	for j := 0; j < 3; j++ {
		go worker(j, jobs, results)
	}

	// 向jobs通道中发送5个任务
	for i := 0; i < 5; i++ {
		jobs <- i
	}
	close(jobs)

	// 从results通道中输出结果
	for ret := range results {
		fmt.Println(ret)
	}
}
```

输出如下，可以发现：worker1和2都执行了2个任务，而worker0只执行了1个，这就实现了3个goroutine完成5个工作任务。

```text
worker: 2 start job: 1
worker: 1 start job: 2
0
2
4
worker: 2 stop job: 1
worker: 2 start job: 3
6
worker: 0 stop job: 0
worker: 0 start job: 4
worker: 1 stop job: 2
8
```



## select多路复用

在某些场景下我们可能需要同时从多个通道接收数据。通道在接收数据时，如果没有数据可以被接收那么当前 goroutine 将会发生阻塞。

你也许会写出如下代码尝试使用遍历的方式来实现从多个通道中接收值。

```go
for{
    // 尝试从ch1接收值
    data, ok := <-ch1
    // 尝试从ch2接收值
    data, ok := <-ch2
    …
}
```

这种方式虽然可以实现从多个通道接收值的需求，但是程序的运行性能会差很多。Go 语言内置了`select`关键字，使用它可以同时响应多个通道的操作。

select 的使用方式类似于之前学到的 switch 语句，它也有一系列 case 分支和一个默认的分支。每个 case  分支会对应一个通道的通信（接收或发送）过程。select 会一直等待，直到其中的某个 case 的通信操作完成时，就会执行该 case  分支对应的语句。具体格式如下：

```go
select {
case <-ch1:
	//...
case data := <-ch2:
	//...
case ch3 <- 10:
	//...
default:
	//默认操作
}
```

select 语句具有以下特点：

- 可处理一个或多个 channel 的发送/接收操作。
- 如果多个 case 同时满足，select 会**随机**选择一个执行。
- 对于没有 case 的 select 会一直阻塞，可用于阻塞 main 函数，防止退出。

下面的示例代码能够在终端打印出10以内的奇数，我们借助这个代码片段来看一下 select 的具体使用：

```go
package main

import "fmt"

func main() {
	ch := make(chan int, 1)
	for i := 1; i <= 10; i++ {
		select {
                // 从通道中取值
		case x := <-ch:
			fmt.Println(x)
                // 往通道中发送值
		case ch <- i:
		}
	}
}
```

初始i=1时通道中没有值，执行第2个case；i=2时通道中有一个1，执行第1个case，如此反复，就只会把奇数打印出：

```bash
1
3
5
7
9
```



## 并发安全和锁

有时候我们的代码中可能会存在多个 goroutine 同时操作一个资源（临界区）的情况，这种情况下就会发生`竞态问题`（数据竞态）。这就好比现实生活中十字路口被各个方向的汽车竞争，还有火车上的卫生间被车厢里的人竞争。

演示一个数据竞争的示例：

```go
package main

import (
	"fmt"
	"sync"
)

var (
	x int64

	wg sync.WaitGroup // 等待组
)

// add 对全局变量x执行5000次加1操作
func add() {
	for i := 0; i < 5000; i++ {
		x = x + 1
	}
	wg.Done()
}

func main() {
	wg.Add(2)

        // 用2个goroutine并发的执行add
	go add()
	go add()

	wg.Wait()
	fmt.Println(x)
}
```

我们将上面的代码编译后执行，不出意外每次执行都会输出诸如9537、5865、6527等不同的结果。这是为什么呢？

在上面的示例代码片中，我们开启了两个 goroutine 分别执行 add 函数，这两个 goroutine 在访问和修改全局的`x`变量时就会存在数据竞争，某个 goroutine 中对全局变量`x`的修改可能会覆盖掉另一个 goroutine 中的操作，所以导致最后的结果与预期不符。

### 互斥锁

互斥锁是一种常用的控制共享资源访问的方法，它能够保证同一时间只有一个 goroutine 可以访问共享资源。Go 语言中使用`sync`包中提供的`Mutex`(互斥的意思)类型来实现互斥锁。

`sync.Mutex`提供了两个方法供我们使用：

| 方法名                   | 功能       |
| ------------------------ | ---------- |
| func (m *Mutex) Lock()   | 获取互斥锁 |
| func (m *Mutex) Unlock() | 释放互斥锁 |

我们在下面的示例代码中使用互斥锁限制每次只有一个 goroutine 才能修改全局变量`x`，从而修复上面代码中的问题。

```go
package main

import (
	"fmt"
	"sync"
)

var (
	x  int64
	wg sync.WaitGroup // 等待组
	m  sync.Mutex     // 互斥锁
)

// add 对全局变量x执行5000次加1操作
func add() {
	for i := 0; i < 5000; i++ {
		m.Lock() // 修改x前加锁，防止出现数据竞争
		x += 1
		m.Unlock() // 修改完释放锁，让其他goroutine可以修改此变量
	}
	wg.Done()
}

func main() {
	wg.Add(2)

	go add()
	go add()

	wg.Wait()
	fmt.Println(x)
}
```

将上面的代码编译后多次执行，每一次都会得到预期中的结果—10000。

使用互斥锁能够保证同一时间有且只有一个 goroutine 进入临界区，其他的 goroutine 则在等待锁；当互斥锁释放后，等待的 goroutine 才可以获取锁进入临界区，多个 goroutine 同时等待一个锁时，唤醒的策略是随机的。

### 读写互斥锁

互斥锁是完全互斥的，但是实际上有很多场景是读多写少的，当我们并发的去读取一个资源而不涉及资源修改的时候是没有必要加互斥锁的，这种场景下使用读写锁是更好的一种选择。读写锁在 Go 语言中使用`sync`包中的`RWMutex`类型。

`sync.RWMutex`提供了以下5个方法。

| 方法名                              | 功能                           |
| ----------------------------------- | ------------------------------ |
| func (rw *RWMutex) Lock()           | 获取写锁                       |
| func (rw *RWMutex) Unlock()         | 释放写锁                       |
| func (rw *RWMutex) RLock()          | 获取读锁                       |
| func (rw *RWMutex) RUnlock()        | 释放读锁                       |
| func (rw *RWMutex) RLocker() Locker | 返回一个实现Locker接口的读写锁 |

当一个 goroutine 获取到读锁之后，其他的 goroutine 如果是获取读锁会继续获得锁，如果是获取写锁就会等待。

当一个 goroutine 获取写锁之后，其他的 goroutine 无论是获取读锁还是写锁都会等待。

下面我们使用代码构造一个读多写少的场景，然后分别使用互斥锁和读写锁查看它们的性能差异。

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	x       int64
	wg      sync.WaitGroup
	mutex   sync.Mutex
	rwMutex sync.RWMutex
)

// 使用互斥锁的写操作
func writeWithLock() {
	mutex.Lock() // 加互斥锁
	x += 1
	time.Sleep(10 * time.Millisecond) // 假设读操作耗时10ms
	mutex.Unlock()                    // 释放互斥锁
	wg.Done()
}

// 使用互斥锁的读操作
func readWithLock() {
	mutex.Lock()
	time.Sleep(time.Millisecond) // 假设读操作耗时1ms
	mutex.Unlock()
	wg.Done()
}

// 使用读写互斥锁的写操作
func writeWithRWLock() {
	rwMutex.Lock()
	x += 1
	time.Sleep(10 * time.Millisecond)
	rwMutex.Unlock()
	wg.Done()
}

// 使用读写互斥锁的读操作
func readWithRWLock() {
	rwMutex.RLock()
	time.Sleep(time.Millisecond)
	rwMutex.RUnlock()
	wg.Done()
}

func do(writeFunc, readFunc func(), writeCount, readCount int) {
	start := time.Now()
	// writeCount个并发写操作
	for i := 0; i < writeCount; i++ {
		wg.Add(1)
		go writeFunc()
	}

	// readCount个并发读操作
	for i := 0; i < readCount; i++ {
		wg.Add(1)
		go readFunc()
	}

	wg.Wait()
	costTime := time.Since(start)
	fmt.Printf("x: %v costTime: %v\n", x, costTime)
}

func main() {
	// 使用互斥锁，操作10并发写，1000并发读耗时
	do(writeWithLock, readWithLock, 10, 1000)

	// 使用读写互斥锁，操作10并发写，1000并发读耗时
	do(writeWithRWLock, readWithRWLock, 10, 1000)
}
```

输出：

```text
x: 10 costTime: 1.2687892s
x: 20 costTime: 105.8625ms
```

从最终的执行结果可以看出，使用读写互斥锁在读多写少的场景下能够极大地提高程序的性能。

但如果一个程序中的读操作和写操作数量级差别不大，那么读写互斥锁的优势就发挥不出来。



### sync.WaitGroup

在代码中生硬的使用`time.Sleep`肯定是不合适的，Go语言中可以使用`sync.WaitGroup`来实现并发任务的同步。 `sync.WaitGroup`有以下几个方法：

| 方法名                               | 功能                |
| ------------------------------------ | ------------------- |
| func (wg * WaitGroup) Add(delta int) | 计数器+delta        |
| (wg *WaitGroup) Done()               | 计数器-1            |
| (wg *WaitGroup) Wait()               | 阻塞直到计数器变为0 |

`sync.WaitGroup`内部维护着一个计数器，计数器的值可以增加和减少。例如当我们启动了 N 个并发任务时，就将计数器值增加N。每个任务完成时通过调用 Done 方法将计数器减1。通过调用 Wait 来等待并发任务执行完，当计数器值为 0 时，表示所有并发任务已经完成。

示例：

```go
var wg sync.WaitGroup

func hello() {
	defer wg.Done()
	fmt.Println("Hello Goroutine!")
}
func main() {
	wg.Add(1)
	go hello() // 启动另外一个goroutine去执行hello函数
	fmt.Println("main goroutine done!")
	wg.Wait()
}
```

需要注意`sync.WaitGroup`是一个结构体，进行参数传递的时候要传递指针。



### sync.Once

在某些场景下我们需要确保某些操作即使在高并发的场景下也只会被执行一次，例如只加载一次配置文件等。

Go语言中的`sync`包中提供了一个针对只执行一次场景的解决方案—`sync.Once`

`sync.Once`只有一个`Do`方法，其签名如下：

```go
func (o *Once) Do(f func())
```

`sync.Once` 内部包含一个互斥锁和一个布尔值，互斥锁保证布尔值和数据的安全，而布尔值用来记录初始化是否完成。这样设计就能保证初始化操作的时候是并发安全的并且初始化操作也不会被执行多次。

`f` 函数只执行一次，如果是第1次执行，则会先加锁执行，将标志位置为已执行过该函数；如果下一次再调用时，发现标志位为已执行过，则不执行该函数了。

**注意**：如果要执行的函数 `f` 需要**传递参数**就需要搭配**闭包**来使用。

#### 加载配置文件示例

延迟一个开销很大的初始化操作到真正用到它的时候再执行是一个很好的实践。因为预先初始化一个变量（比如在init函数中完成初始化）会增加程序的启动耗时，而且有可能实际执行过程中这个变量没有用上，那么这个初始化操作就不是必须要做的。我们来看一个例子：

```go
var icons map[string]image.Image

func loadIcons() {
	icons = map[string]image.Image{
		"left":  loadIcon("left.png"),
		"up":    loadIcon("up.png"),
		"right": loadIcon("right.png"),
		"down":  loadIcon("down.png"),
	}
}

// Icon 被多个goroutine调用时不是并发安全的
func Icon(name string) image.Image {
	if icons == nil {
		loadIcons()
	}
	return icons[name]
}
```

多个 goroutine 并发调用Icon函数时不是并发安全的，现代的编译器和CPU可能会在保证每个 goroutine 都满足串行一致的基础上自由地重排访问内存的顺序。loadIcons 函数可能会被重排为以下结果：

```go
func loadIcons() {
	icons = make(map[string]image.Image)
	icons["left"] = loadIcon("left.png")
	icons["up"] = loadIcon("up.png")
	icons["right"] = loadIcon("right.png")
	icons["down"] = loadIcon("down.png")
}
```

在这种情况下就会出现即使判断了`icons`不是nil也不意味着变量初始化完成了。也就是说，当一个goroutine执行到make初始化了icons，但是还没有将图片初始化；此时另一个goroutine调用了Icon函数，而此时icons不为nil，则直接`return icons[name]`；但图片还没有被初始化，所以返回的是空，这就是问题所在。

考虑到这种情况，我们能想到的办法就是添加互斥锁，保证初始化`icons`的时候不会被其他的 goroutine 操作，但是这样做又会引发性能问题，所以可以使用`sync.Once`。

使用`sync.Once`改造的示例代码如下：

```go
var icons map[string]image.Image

var loadIconsOnce sync.Once

func loadIcons() {
	icons = map[string]image.Image{
		"left":  loadIcon("left.png"),
		"up":    loadIcon("up.png"),
		"right": loadIcon("right.png"),
		"down":  loadIcon("down.png"),
	}
}

// Icon 是并发安全的
func Icon(name string) image.Image {
        // loadIcons函数只执行一次，如果是第1次执行，则会先加锁执行，将标志位置为已执行过该函数；如果下一次再调用时，发现标志位为已执行过，则不执行该函数了。
	loadIconsOnce.Do(loadIcons)
	return icons[name]
}
```

#### 并发安全的单例模式

下面是借助`sync.Once`实现的并发安全的单例模式：

```go
package singleton

import (
    "sync"
)

type singleton struct {}

var instance *singleton
var once sync.Once

func GetInstance() *singleton {
    once.Do(func() {
        instance = &singleton{}
    })
    return instance
}
```



### sync.Map

Go 语言中内置的 map 不是并发安全的，请看下面这段示例代码。

```go
package main

import (
	"fmt"
	"strconv"
	"sync"
)

var m = make(map[string]int)

func get(key string) int {
	return m[key]
}

func set(key string, value int) {
	m[key] = value
}

func main() {
	wg := sync.WaitGroup{}
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func(i int) {
             	     // 将int类型的i转为string
			key := strconv.Itoa(i)
			set(key, i)
			fmt.Printf("k=:%v,v:=%v\n", key, get(key))
			wg.Done()
		}(i)
	}
	wg.Wait()
}
```

将上面的代码编译后执行，会报出`fatal error: concurrent map writes`错误。我们不能在多个 goroutine 中并发对内置的 map 进行读写操作，否则会存在数据竞争问题。

像这种场景下就需要为 map 加锁来保证并发的安全性了，Go语言的`sync`包中提供了一个开箱即用的并发安全版 map——`sync.Map`。开箱即用表示其不用像内置的 map 一样使用 make 函数初始化就能直接使用。同时`sync.Map`内置了诸如`Store`、`Load`、`LoadOrStore`、`Delete`、`Range`等操作方法。

| 方法名                                                       | 功能                            |
| ------------------------------------------------------------ | ------------------------------- |
| func (m *Map) Store(key, value interface{})                  | 存储key-value数据               |
| func (m *Map) Load(key interface{}) (value interface{}, ok bool) | 查询key对应的value              |
| func (m *Map) LoadOrStore(key, value interface{}) (actual interface{}, loaded bool) | 查询或存储key对应的value        |
| func (m *Map) LoadAndDelete(key interface{}) (value interface{}, loaded bool) | 查询并删除key                   |
| func (m *Map) Delete(key interface{})                        | 删除key                         |
| func (m *Map) Range(f func(key, value interface{}) bool)     | 对map中的每个key-value依次调用f |

下面的代码示例演示了并发读写`sync.Map`。

```go
package main

import (
	"fmt"
	"strconv"
	"sync"
)

// 并发安全的map
var m = sync.Map{}

func main() {
	wg := sync.WaitGroup{}
	// 对m执行20个并发的读写操作
	for i := 0; i < 20; i++ {
		wg.Add(1)
		go func(i int) {
			key := strconv.Itoa(i)
			m.Store(key, i)         // 存储key-value
			value, _ := m.Load(key) // 根据key取值
			fmt.Printf("k=:%v,v:=%v\n", key, value)
			wg.Done()
		}(i)
	}
	wg.Wait()
}
```



## 原子操作

针对整数数据类型（int32、uint32、int64、uint64）我们还可以使用原子操作来保证并发安全，通常直接使用原子操作比使用锁操作效率更高。Go语言中原子操作由内置的标准库`sync/atomic`提供。

### atomic包

| 方法                                                         | 解释           |
| ------------------------------------------------------------ | -------------- |
| func LoadInt32(addr *int32) (val int32) func LoadInt64(addr *int64) (val int64) func LoadUint32(addr *uint32) (val uint32) func LoadUint64(addr *uint64) (val uint64) func LoadUintptr(addr *uintptr) (val uintptr) func LoadPointer(addr *unsafe.Pointer) (val unsafe.Pointer) | 读取操作       |
| func StoreInt32(addr *int32, val int32) func StoreInt64(addr *int64, val int64) func StoreUint32(addr *uint32, val uint32) func StoreUint64(addr *uint64, val uint64) func StoreUintptr(addr *uintptr, val uintptr) func StorePointer(addr *unsafe.Pointer, val unsafe.Pointer) | 写入操作       |
| func AddInt32(addr *int32, delta int32) (new int32) func AddInt64(addr *int64, delta int64) (new int64) func AddUint32(addr *uint32, delta uint32) (new uint32) func AddUint64(addr *uint64, delta uint64) (new uint64) func AddUintptr(addr *uintptr, delta uintptr) (new uintptr) | 修改操作       |
| func SwapInt32(addr *int32, new int32) (old int32) func SwapInt64(addr *int64, new int64) (old int64) func SwapUint32(addr *uint32, new uint32) (old uint32) func SwapUint64(addr *uint64, new uint64) (old uint64) func SwapUintptr(addr *uintptr, new uintptr) (old uintptr) func SwapPointer(addr *unsafe.Pointer, new unsafe.Pointer) (old unsafe.Pointer) | 交换操作       |
| func CompareAndSwapInt32(addr *int32, old, new int32) (swapped bool) func CompareAndSwapInt64(addr *int64, old, new int64) (swapped bool) func CompareAndSwapUint32(addr *uint32, old, new uint32) (swapped bool) func CompareAndSwapUint64(addr *uint64, old, new uint64) (swapped bool) func CompareAndSwapUintptr(addr *uintptr, old, new uintptr) (swapped bool) func CompareAndSwapPointer(addr *unsafe.Pointer, old, new unsafe.Pointer) (swapped bool) | 比较并交换操作 |

### 示例

我们填写一个示例来比较下互斥锁和原子操作的性能。

```go
package main

import (
	"fmt"
	"sync"
	"sync/atomic"
	"time"
)

type Counter interface {
	Inc()
	Load() int64
}

// 普通版
type CommonCounter struct {
	counter int64
}

func (c CommonCounter) Inc() {
	c.counter++
}

func (c CommonCounter) Load() int64 {
	return c.counter
}

// 互斥锁版
type MutexCounter struct {
	counter int64
	lock    sync.Mutex
}

func (m *MutexCounter) Inc() {
	m.lock.Lock()
	defer m.lock.Unlock()
	m.counter++
}

func (m *MutexCounter) Load() int64 {
	m.lock.Lock()
	defer m.lock.Unlock()
	return m.counter
}

// 原子操作版
type AtomicCounter struct {
	counter int64
}

func (a *AtomicCounter) Inc() {
	atomic.AddInt64(&a.counter, 1)
}

func (a *AtomicCounter) Load() int64 {
	return atomic.LoadInt64(&a.counter)
}

func test(c Counter) {
	var wg sync.WaitGroup
	start := time.Now()
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			c.Inc()
			wg.Done()
		}()
	}
	wg.Wait()
	end := time.Now()
	fmt.Println(c.Load(), end.Sub(start))
}

func main() {
	c1 := CommonCounter{} // 非并发安全
	test(c1)
	c2 := MutexCounter{} // 使用互斥锁实现并发安全
	test(&c2)
	c3 := AtomicCounter{} // 并发安全且比互斥锁效率更高
	test(&c3)
}
```

`atomic`包提供了底层的原子级内存操作，对于同步算法的实现很有用。这些函数必须谨慎地保证正确使用。除了某些特殊的底层应用，使用通道或者 sync 包的函数/类型实现同步更好。

# 网络编程

## 互联网分层模型

互联网的逻辑实现被分为好几层。每一层都有自己的功能，就像建筑物一样，每一层都靠下一层支持。用户接触到的只是最上面的那一层，根本不会感觉到下面的几层。要理解互联网就需要自下而上理解每一层的实现的功能。 

![image-20220508090931190](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/dbb4d3d000c099efff46249a29b4229c.png)

如上图所示，互联网按照不同的模型划分会有不用的分层，但是不论按照什么模型去划分，越往上的层越靠近用户，越往下的层越靠近硬件。在软件开发中我们使用最多的是上图中将互联网划分为五个分层的模型。

接下来我们一层一层的自底向上介绍一下每一层。

### 物理层

我们的电脑要与外界互联网通信，需要先把电脑连接网络，我们可以用双绞线、光纤、无线电波等方式。这就叫做”实物理层”，它就是把电脑连接起来的物理手段。它主要规定了网络的一些电气特性，作用是负责传送0和1的电信号。

### 数据链路层

单纯的0和1没有任何意义，所以我们使用者会为其赋予一些特定的含义，规定解读电信号的方式：例如：多少个电信号算一组？每个信号位有何意义？这就是”数据链接层”的功能，它在”物理层”的上方，确定了物理层传输的0和1的分组方式及代表的意义。早期的时候，每家公司都有自己的电信号分组方式。逐渐地，一种叫做”以太网”（Ethernet）的协议，占据了主导地位。

以太网规定，一组电信号构成一个数据包，叫做”帧”（Frame）。每一帧分成两个部分：标头（Head）和数据（Data）。其中”标头”包含数据包的一些说明项，比如发送者、接受者、数据类型等等；”数据”则是数据包的具体内容。”标头”的长度，固定为18字节。”数据”的长度，最短为46字节，最长为1500字节。因此，整个”帧”最短为64字节，最长为1518字节。如果数据很长，就必须分割成多个帧进行发送。

那么，发送者和接受者是如何标识呢？以太网规定，连入网络的所有设备都必须具有”网卡”接口。数据包必须是从一块网卡，传送到另一块网卡。网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址。每块网卡出厂的时候，都有一个全世界独一无二的MAC地址，长度是48个二进制位，通常用12个十六进制数表示。前6个十六进制数是厂商编号，后6个是该厂商的网卡流水号。有了MAC地址，就可以定位网卡和数据包的路径了。

我们会通过ARP协议来获取接受方的MAC地址，有了MAC地址之后，如何把数据准确的发送给接收方呢？其实这里以太网采用了一种很”原始”的方式，它不是把数据包准确送到接收方，而是向本网络内所有计算机都发送，让每台计算机读取这个包的”标头”，找到接收方的MAC地址，然后与自身的MAC地址相比较，如果两者相同，就接受这个包，做进一步处理，否则就丢弃这个包。这种发送方式就叫做”广播”（broadcasting）。

### 网络层

按照以太网协议的规则我们可以依靠MAC地址来向外发送数据。理论上依靠MAC地址，你电脑的网卡就可以找到身在世界另一个角落的某台电脑的网卡了，但是这种做法有一个重大缺陷就是以太网采用广播方式发送数据包，所有成员人手一”包”，不仅效率低，而且发送的数据只能局限在发送者所在的子网络。也就是说如果两台计算机不在同一个子网络，广播是传不过去的。这种设计是合理且必要的，因为如果互联网上每一台计算机都会收到互联网上收发的所有数据包，那是不现实的。

因此，必须找到一种方法区分哪些MAC地址属于同一个子网络，哪些不是。如果是同一个子网络，就采用广播方式发送，否则就采用”路由”方式发送。这就导致了”网络层”的诞生。它的作用是引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个子网络。这套地址就叫做”网络地址”，简称”网址”。

“网络层”出现以后，每台计算机有了两种地址，一种是MAC地址，另一种是网络地址。两种地址之间没有任何联系，MAC地址是绑定在网卡上的，网络地址则是网络管理员分配的。网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包送到该子网络中的目标网卡。因此，从逻辑上可以推断，必定是先处理网络地址，然后再处理MAC地址。

规定网络地址的协议，叫做IP协议。它所定义的地址，就被称为IP地址。目前，广泛采用的是IP协议第四版，简称IPv4。IPv4这个版本规定，网络地址由32个二进制位组成，我们通常习惯用分成四段的十进制数表示IP地址，从0.0.0.0一直到255.255.255.255。

根据IP协议发送的数据，就叫做IP数据包。IP数据包也分为”标头”和”数据”两个部分：”标头”部分主要包括版本、长度、IP地址等信息，”数据”部分则是IP数据包的具体内容。IP数据包的”标头”部分的长度为20到60字节，整个数据包的总长度最大为65535字节。

### 传输层

有了MAC地址和IP地址，我们已经可以在互联网上任意两台主机上建立通信。但问题是同一台主机上会有许多程序都需要用网络收发数据，比如QQ和浏览器这两个程序都需要连接互联网并收发数据，我们如何区分某个数据包到底是归哪个程序的呢？也就是说，我们还需要一个参数，表示这个数据包到底供哪个程序（进程）使用。这个参数就叫做”端口”（port），它其实是每一个使用网卡的程序的编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己所需要的数据。

“端口”是0到65535之间的一个整数，正好16个二进制位。0到1023的端口被系统占用，用户只能选用大于1023的端口。有了IP和端口我们就能实现唯一确定互联网上一个程序，进而实现网络间的程序通信。

我们必须在数据包中加入端口信息，这就需要新的协议。最简单的实现叫做UDP协议，它的格式几乎就是在数据前面，加上端口号。UDP数据包，也是由”标头”和”数据”两部分组成：”标头”部分主要定义了发出端口和接收端口，”数据”部分就是具体的内容。UDP数据包非常简单，”标头”部分一共只有8个字节，总长度不超过65,535字节，正好放进一个IP数据包。

UDP协议的优点是比较简单，容易实现，但是缺点是可靠性较差，一旦数据包发出，无法知道对方是否收到。为了解决这个问题，提高网络可靠性，TCP协议就诞生了。TCP协议能够确保数据不会遗失。它的缺点是过程复杂、实现困难、消耗较多的资源。TCP数据包没有长度限制，理论上可以无限长，但是为了保证网络的效率，通常TCP数据包的长度不会超过IP数据包的长度，以确保单个TCP数据包不必再分割。

### 应用层

应用程序收到”传输层”的数据，接下来就要对数据进行解包。由于互联网是开放架构，数据来源五花八门，必须事先规定好通信的数据格式，否则接收方根本无法获得真正发送的数据内容。”应用层”的作用就是规定应用程序使用的数据格式，例如我们TCP协议之上常见的Email、HTTP、FTP等协议，这些协议就组成了互联网协议的应用层。

如下图所示，发送方的HTTP数据经过互联网的传输过程中会依次添加各层协议的标头信息，接收方收到数据包之后再依次根据协议解包得到数据。

![image-20220508091003324](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9cefe6b842ed0f3252219cd4946fc372.png)

## socket 编程

Socket是BSD  UNIX的进程通信机制，通常也称作 “套接字”，用于描述IP地址和端口，是一个通信链的句柄。Socket可以理解为TCP/IP网络的API，它定义了许多函数或例程，程序员可以用它们来开发TCP/IP网络上的应用程序。电脑上运行的应用程序通常通过”套接字”向网络发出请求或者应答网络请求。

### socket图解

`Socket`是应用层与TCP/IP协议族通信的中间软件抽象层。在设计模式中，`Socket`其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在`Socket`后面，对用户来说只需要调用Socket规定的相关函数，让`Socket`去组织符合指定的协议数据然后进行通信。

![socket图解](https://www.liwenzhou.com/images/Go/socket/socket.png)

## TCP 通信

### TCP 协议

TCP/IP(Transmission Control Protocol/Internet Protocol)  即传输控制协议/网间协议，是一种面向连接（连接导向）的、可靠的、基于字节流的传输层（Transport  layer）通信协议，因为是面向连接的协议，数据像水流一样传输，会存在黏包问题。

### TCP 服务端

一个TCP服务端可以同时连接很多个客户端，例如世界各地的用户使用自己电脑上的浏览器访问淘宝网。因为Go语言中创建多个goroutine实现并发非常方便和高效，所以我们可以每建立一次链接就创建一个goroutine去处理。

TCP服务端程序的处理流程：

1. 监听端口
2. 接收客户端请求建立连接
3. 创建goroutine处理连接。

使用Go语言的net包实现的TCP服务端代码如下：

```go
// tcp/server/main.go

// TCP server端

package main

import (
	"bufio"
	"fmt"
	"net"
)

func process(conn net.Conn) {
	defer conn.Close()
	// 针对当前连接做数据的发送和接收
	for {
		// 创建一个从当前连接里面去读取数据的对象
		reader := bufio.NewReader(conn)
		var buf [128]byte
		// 将buf数组转为切片去读取，因为Read方法需要的是切片
		// 返回的n是读取的数量
		n, err := reader.Read(buf[:])
		if err != nil {
			fmt.Println("read from conn failed, err: ", err)
			break
		}
		recv := string(buf[:n])
		fmt.Println("来自client：", recv)
		// 把收到的数据发送回去
		conn.Write(buf[:n])
	}
}

func main() {
	// 1.开启服务，监听端口
	listen, err := net.Listen("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("listen failed, err: ", err)
		return
	}
	for {
		conn, err := listen.Accept() // 2.等待客户端来请求建立连接
		if err != nil {
			fmt.Println("accept failed, err: ", err)
			continue
		}
		go process(conn) // 3.启动一个goroutine处理连接
	}
}
```



### TCP 客户端

一个TCP客户端进行TCP通信的流程如下：

1. 建立与服务端的连接
2. 进行数据收发和接收
3. 关闭链接

使用Go语言的net包实现的TCP客户端代码如下：

```go
// tcp/client/main.go

// 客户端

package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
	"strings"
)

func main() {
	// 1.用Dial函数去与服务端建立连接
	conn, err := net.Dial("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("err: ", err)
		return
	}
	defer conn.Close()
	// 2.利用该连接进行数据的发送和接收
	input := bufio.NewReader(os.Stdin) // os.Stdin从终端去读取数据
	for {
		input, _ := input.ReadString('\n')       // 读取用户输入，遇到\n停止
		inputInfo := strings.Trim(input, "\r\n") // 需要把\n删除掉
		if inputInfo == "exit" {
			return
		}

		// 给服务端发信息
		_, err = conn.Write([]byte(inputInfo)) // 发送数据
		if err != nil {
			return
		}

		// 从服务端接收信息
		buf := [512]byte{}
		n, err := conn.Read(buf[:])
		if err != nil {
			fmt.Println("recv failed, err: ", err)
			return
		}
		fmt.Println("来自server：", string(buf[:n]))
	}
}
```

先启动 server 端再启动 client 端，在 client 端输入任意内容回车之后就能够在 server 端看到client 端发送的数据，实现了tcp通信。

client 端：

![image-20220508094701777](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3f2ea422c56b0a306cb895634be51ffb.png)

service 端：

![image-20220508094727614](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a18753a94202c0f7db797aa98d1dec09.png)



## TPC 黏包

### 黏包示例

服务端代码如下：

```go
// socket_stick/server/main.go

func process(conn net.Conn) {
	defer conn.Close()
	reader := bufio.NewReader(conn)
	var buf [1024]byte
	for {
		n, err := reader.Read(buf[:])
		if err == io.EOF {
			break
		}
		if err != nil {
			fmt.Println("read from client failed, err:", err)
			break
		}
		recvStr := string(buf[:n])
		fmt.Println("收到client发来的数据：", recvStr)
	}
}

func main() {

	listen, err := net.Listen("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	defer listen.Close()
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn)
	}
}
```

客户端代码如下：

```go
// socket_stick/client/main.go

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("dial failed, err", err)
		return
	}
	defer conn.Close()
	for i := 0; i < 20; i++ {
		msg := `Hello, Hello. How are you?`
		conn.Write([]byte(msg))
	}
}
```

将上面的代码保存后，分别编译。先启动服务端再启动客户端，可以看到服务端输出结果如下：

```bash
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?Hello, Hello. How are you?
收到client发来的数据： Hello, Hello. How are you?Hello, Hello. How are you?
```

客户端分10次发送的数据，在服务端并没有成功的输出10次，而是多条数据“粘”到了一起。

### 为什么会出现粘包

主要原因就是tcp数据传递模式是流模式，在保持长连接的时候可以进行多次的收和发。

“粘包”可发生在发送端也可发生在接收端：

1. 由Nagle算法造成的发送端的粘包：Nagle算法是一种改善网络传输效率的算法。简单来说就是当我们提交一段数据给TCP发送时，TCP并不立刻发送此段数据，而是等待一小段时间看看在等待期间是否还有要发送的数据，若有则会一次把这两段数据发送出去。
2. 接收端接收不及时造成的接收端粘包：TCP会把接收到的数据存在自己的缓冲区中，然后通知应用层取数据。当应用层由于某些原因不能及时的把TCP的数据取出来，就会造成TCP缓冲区中存放了几段数据。

### 解决办法

出现”粘包”的关键在于接收方不确定将要传输的数据包的大小，因此我们可以对数据包进行封包和拆包的操作。

封包：封包就是给一段数据加上包头，这样一来数据包就分为包头和包体两部分内容了(过滤非法包时封包会加入”包尾”内容)。包头部分的长度是固定的，并且它存储了包体的长度，根据包头长度固定以及包头中含有包体长度的变量就能正确的拆分出一个完整的数据包。

我们可以自己定义一个协议，比如数据包的前4个字节为包头，里面存储的是发送的数据的长度。

```go
// socket_stick/proto/proto.go
package proto

import (
	"bufio"
	"bytes"
	"encoding/binary"
)

// Encode 将消息编码
func Encode(message string) ([]byte, error) {
	// 读取消息的长度，转换成int32类型（占4个字节）
	var length = int32(len(message))
	var pkg = new(bytes.Buffer)
	// 写入消息头
	err := binary.Write(pkg, binary.LittleEndian, length)
	if err != nil {
		return nil, err
	}
	// 写入消息实体
	err = binary.Write(pkg, binary.LittleEndian, []byte(message))
	if err != nil {
		return nil, err
	}
	return pkg.Bytes(), nil
}

// Decode 解码消息
func Decode(reader *bufio.Reader) (string, error) {
	// 读取消息的长度
	lengthByte, _ := reader.Peek(4) // 读取前4个字节的数据
	lengthBuff := bytes.NewBuffer(lengthByte)
	var length int32
	err := binary.Read(lengthBuff, binary.LittleEndian, &length)
	if err != nil {
		return "", err
	}
	// Buffered返回缓冲中现有的可读取的字节数。
	if int32(reader.Buffered()) < length+4 {
		return "", err
	}

	// 读取真正的消息数据
	pack := make([]byte, int(4+length))
	_, err = reader.Read(pack)
	if err != nil {
		return "", err
	}
	return string(pack[4:]), nil
}
```

接下来在服务端和客户端分别使用上面定义的`proto`包的`Decode`和`Encode`函数处理数据。

服务端代码如下：

```go
// socket_stick/server2/main.go

func process(conn net.Conn) {
	defer conn.Close()
	reader := bufio.NewReader(conn)
	for {
		msg, err := proto.Decode(reader)
		if err == io.EOF {
			return
		}
		if err != nil {
			fmt.Println("decode msg failed, err:", err)
			return
		}
		fmt.Println("收到client发来的数据：", msg)
	}
}

func main() {

	listen, err := net.Listen("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	defer listen.Close()
	for {
		conn, err := listen.Accept()
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn)
	}
}
```

客户端代码如下：

```go
// socket_stick/client2/main.go

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:30000")
	if err != nil {
		fmt.Println("dial failed, err", err)
		return
	}
	defer conn.Close()
	for i := 0; i < 20; i++ {
		msg := `Hello, Hello. How are you?`
		data, err := proto.Encode(msg)
		if err != nil {
			fmt.Println("encode msg failed, err:", err)
			return
		}
		conn.Write(data)
	}
}
```



## UDP 通信

### UDP协议

UDP协议（User Datagram Protocol）中文名称是用户数据报协议，是OSI（Open System Interconnection，开放式系统互联）参考模型中一种**无连接**的传输层协议，不需要建立连接就能直接进行数据发送和接收，属于不可靠的、没有时序的通信，但是UDP协议的实时性比较好，通常用于视频直播相关领域。

### UDP服务端

使用Go语言的`net`包实现的UDP服务端代码如下：

```go
// UDP/server/main.go

// UDP server端

package server

import (
	"fmt"
	"net"
)

func main() {
	// 开启服务，监听端口
	listen, err := net.ListenUDP("udp", &net.UDPAddr{
		IP:   net.IPv4(0, 0, 0, 0),
		Port: 30000,
	})
	if err != nil {
		fmt.Println("listen failed, err: ", err)
		return
	}
	defer listen.Close()
	// UDP不需要建立连接，直接进行数据的收发
	for {
		var data [1024]byte
		n, addr, err := listen.ReadFromUDP(data[:]) // 接收数据
		if err != nil {
			fmt.Println("read udp failed, err: ", err)
			continue
		}
		fmt.Printf("data: %v addr:%v count: %v\n", string(data[:n]), addr, n)
		_, err = listen.WriteToUDP(data[:n], addr)	// 发送数据
		if err != nil {
			fmt.Println("write to udp failed, err: ", err)
			continue
		}
	}
}
```

### UDP客户端

使用Go语言的`net`包实现的UDP客户端代码如下：

```go
// UDP 客户端

package main

import (
	"bufio"
	"fmt"
	"net"
	"os"
)

func main() {
	socket, err := net.DialUDP("udp", nil, &net.UDPAddr{
		IP:   net.IPv4(0, 0, 0, 0),
		Port: 30000,
	})
	if err != nil {
		fmt.Println("dial failed, err: ", err)
		return
	}
	defer socket.Close()

	input := bufio.NewReader(os.Stdin)

	// 进行数据收发
	for {
		inputInfo, _ := input.ReadString('\n')
		_, err = socket.Write([]byte(inputInfo)) // 发送数据，需要转换成byte切片
		if err != nil {
			fmt.Println("send to server failed, err: ", err)
			return
		}

		var buf [1024]byte
		n, Addr, err := socket.ReadFromUDP(buf[:]) // 接收数据
		if err != nil {
			fmt.Println("recv from udp failed, err: ", err)
			return
		}
		fmt.Printf("read from: %v msg: %v\n", Addr, string(buf[:n]))
	}
}

```

先启动server端，再启动client端进行测试。

client端：

![image-20220508160424375](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/53c07ff7374cbd66933f0c25e3ac68f9.png)

server端：

![image-20220508160440142](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/64d3bcf4fe10a2511bde7200980a197c.png)

















# 单元测试

## go test 工具

Go语言中的测试依赖 `go test` 命令。编写测试代码和编写普通的Go代码过程是类似的，并不需要学习新的语法、规则或工具。

`go test` 命令是一个按照一定约定和组织的测试代码的驱动程序。在包目录内，所有以`_test.go` 为后缀名的源代码文件都是 `go test` 测试的一部分，不会被 `go build` 编译到最终的可执行文件中。

在`*_test.go`文件中有三种类型的函数，单元测试函数、基准测试函数和示例函数。

| 类型     | 格式                  | 作用                           |
| -------- | --------------------- | ------------------------------ |
| 测试函数 | 函数名前缀为Test      | 测试程序的一些逻辑行为是否正确 |
| 基准函数 | 函数名前缀为Benchmark | 测试函数的性能                 |
| 示例函数 | 函数名前缀为Example   | 为文档提供示例文档             |

`go test` 命令会遍历所有的`*_test.go`文件中符合上述命名规则的函数，然后生成一个临时的 main包 用于调用相应的测试函数，然后构建并运行、报告测试结果，最后清理测试中生成的临时文件。

Golang单元测试对文件名和方法名，参数都有很严格的要求：

- 文件名必须以 `xx_test.go` 命名
- 方法必须是 `Test[^a-z]` 开头
- 方法参数必须 `t *testing.T`
- 使用 `go test` 执行单元测试



## 测试函数

### 格式

每个测试函数必须导入 testing 包，测试函数的基本格式（签名）如下：

```go
func TestName(t *testing.T) {
    // ...
}
```

测试函数的名字必须以 Test 开头，可选的后缀名必须以大写字母开头，举几个例子：

```go
func TestAdd(t *testing.T) { ... }
func TestSum(t *testing.T) { ... }
func TestLog(t *testing.T) { ... }
```

其中 参数t 用于报告测试失败和附加的日志信息。 `testing.T` 拥有的方法如下：

```go
func (c *T) Error(args ...interface{})
func (c *T) Errorf(format string, args ...interface{})
func (c *T) Fail()
func (c *T) FailNow()
func (c *T) Failed() bool
func (c *T) Fatal(args ...interface{})
func (c *T) Fatalf(format string, args ...interface{})
func (c *T) Log(args ...interface{})
func (c *T) Logf(format string, args ...interface{})
func (c *T) Name() string
func (t *T) Parallel()
func (t *T) Run(name string, f func(t *T)) bool
func (c *T) Skip(args ...interface{})
func (c *T) SkipNow()
func (c *T) Skipf(format string, args ...interface{})
func (c *T) Skipped() bool
```

### 示例

定义一个 split 的包，包中定义了一个 Split 函数，具体实现如下：

```go
// split/split.go
package split

import "strings"

// Split 以sep切割s，将切割的结果放入result
func Split(s, sep string) (result []string) {
	// Index() 返回s中sep第一次出现的位置，没有则返回-1
	i := strings.Index(s, sep)

	for i > -1 {
		// 将i索引之前的先添加到res中
		result = append(result, s[:i])
		// 再将s赋值为i索引以后的值
		s = s[i+1:]
		// 继续寻找下一个sep的索引
		i = strings.Index(s, sep)
	}
	// 循环结束后，将最后一段值添加到res中
	result = append(result, s)
	return
}
```

在当前目录下，我们创建一个`split_test.go`的测试文件，并定义一个测试函数如下：

```go
// split/split_test.go
package split

import (
	"reflect"
	"testing"
)

func TestSplit(t *testing.T) { // 测试函数名必须以Test开头，必须接收一个*testing.T类型参数
	got := Split("a:b:c", ":")      // 程序输出的结果
	want := []string{"a", "b", "c"} // 期望的结果
	// 因为slice不能比较直接，借助反射包中的方法比较
	if !reflect.DeepEqual(want, got) {
		// 测试失败输出错误提示
		t.Errorf("excepted: %v, got: %v", want, got)
	}
}
```

在split包路径下，执行`go test`命令，可以看到测试通过：

![image-20220504191711547](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/38411928ba51f194566fc5ef20b4606c.png)

一个测试用例有点单薄，我们再编写一个测试使用多个字符切割字符串的例子，在`split_test.go`中添加如下测试函数：

```go
func TestSplit2(t *testing.T) {
	got := Split("abcde", "bc")
	want := []string{"a", "de"}
	if !reflect.DeepEqual(want, got) {
		t.Errorf("excepted: %v, got: %v", want, got)
	}
}
```

再次运行`go test`命令，可以为其添加`-v`参数，查看测试函数名称和运行时间：

![image-20220504192240674](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/448deddd78d0ad7a9f31411cc92ae057.png)

可以看到是`TestSplit2`这个测试没有成功。 还可以在`go test`命令后添加`-run`参数，它对应一个正则表达式，只有函数名匹配上的测试函数才会被`go test`命令执行：

![image-20220504192403869](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7db6b2987e3b2fdd7da2871c3c184654.png)

现在我们回过头来解决我们程序中的问题。很显然我们最初的split函数并没有考虑到sep为多个字符的情况，我们来修复下这个Bug：

```go
package split

import "strings"

// Split 以sep切割s，将切割的结果放入result
func Split(s, sep string) (result []string) {
	// Index() 返回s中sep第一次出现的位置，没有则返回-1
	i := strings.Index(s, sep)

	for i > -1 {
		// 将i索引之前的先添加到res中
		result = append(result, s[:i])
		// 再将s赋值为i索引以后的值
		s = s[i+len(sep):] //// 这里使用len(sep)获取sep的长度
		// 继续寻找下一个sep的索引
		i = strings.Index(s, sep)
	}
	// 循环结束后，将最后一段值添加到res中
	result = append(result, s)
	return
}
```

这一次我们再来测试一下，我们的程序。注意，当我们修改了我们的代码之后不要仅仅执行那些失败的测试函数，我们应该完整的运行所有的测试，保证不会因为修改代码而引入了新的问题。

![image-20220504192529980](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/eaf0fbb1a29500b1755238d0ef6cb35e.png)



### 测试组

我们现在还想要测试一下`split`函数对中文字符串的支持，这个时候我们可以再编写一个`TestChineseSplit`测试函数。

可以使用如下更友好的一种方式来添加更多的测试用例：

```go
func TestChineseSplit(t *testing.T) {
	// 定义一个测试用例类型
	type test struct {
		input string
		sep string
		want []string
	}
	
	// 定义一个存储测试用例的切片
	tests := []test {
		{input: "a:b:c", sep: ":", want: []string{"a", "b", "c"}},
		{input: "abcde", sep: "bc", want: []string{"a", "de"}},
		{input: "我爱中国", sep: "中", want: []string{"我爱", "国"}},
	}
	
	// 遍历切片，逐一执行测试用例
	for _, tc := range tests {
		got := Split(tc.input, tc.sep)
		if !reflect.DeepEqual(got, tc.want) {
			t.Errorf("expected: %v, got: %v", tc.want, got)
		}
	}
}
```

通过上面的测试组就把多个测试用例合到一起了，再次执行`go test`命令。

![image-20220504194107947](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6af368067096eed28932aec38ca6119e.png)

我们的测试出现了问题，仔细看打印的测试失败提示信息：`expected:[河有 又有河], got:[ 河有 又有河]`，你会发现`[ 河有 又有河]`中有个不明显的空串(首字符为空)，这种情况下十分推荐使用`%#v`的格式化方式。

我们修改下测试用例的格式化输出错误提示部分：

```go
func TestChineseSplit(t *testing.T) {
	...
    
	for _, tc := range tests {
		got := Split(tc.input, tc.sep)
		if !reflect.DeepEqual(got, tc.want) {
			t.Errorf("expected: %#v, got: %#v", tc.want, got)
		}
	}
}
```

此时运行`go test`命令后就能看到比较明显的提示信息了：

![image-20220504194506665](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1611f5886ec03e520fd1d421aeb1f3fb.png)



### 子测试

上面的测试看起来挺不错的，但如果测试用例比较多的时候，我们是没办法一眼看出来具体是哪个测试用例失败了。

我们可能会想到下面的解决办法，使用 map 在每个测试用例上加上一个名字：

```go
func TestSplit(t *testing.T) {
	type test struct { // 定义test结构体
		input string
		sep   string
		want  []string
	}
	tests := map[string]test{ // 测试用例使用map存储
		"simple":      {input: "a:b:c", sep: ":", want: []string{"a", "b", "c"}},
		"wrong sep":   {input: "a:b:c", sep: ",", want: []string{"a:b:c"}},
		"more sep":    {input: "abcd", sep: "bc", want: []string{"a", "d"}},
		"leading sep": {input: "沙河有沙又有河", sep: "沙", want: []string{"河有", "又有河"}},
	}
	for name, tc := range tests {
		got := Split(tc.input, tc.sep)
		if !reflect.DeepEqual(got, tc.want) {
			t.Errorf("name:%s expected:%#v, got:%#v", name, tc.want, got) // 将测试用例的name格式化输出
		}
	}
}
```

上面的做法是能够解决问题的。同时Go1.7+中新增了子测试，我们可以按照如下方式使用`t.Run`执行子测试：

```go
func TestSplit(t *testing.T) {
	type test struct { // 定义test结构体
		input string
		sep   string
		want  []string
	}
	tests := map[string]test{ // 测试用例使用map存储
		"simple":      {input: "a:b:c", sep: ":", want: []string{"a", "b", "c"}},
		"wrong sep":   {input: "a:b:c", sep: ",", want: []string{"a:b:c"}},
		"more sep":    {input: "abcd", sep: "bc", want: []string{"a", "d"}},
		"leading sep": {input: "沙河有沙又有河", sep: "沙", want: []string{"河有", "又有河"}},
	}
	for name, tc := range tests {
		t.Run(name, func(t *testing.T) { // 使用t.Run()执行子测试
			got := Split(tc.input, tc.sep)
			if !reflect.DeepEqual(got, tc.want) {
				t.Errorf("expected:%#v, got:%#v", tc.want, got)
			}
		})
	}
}
```

此时我们再执行`go test`命令就能够看到更清晰的输出内容了：

![image-20220504195728449](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2084befaf9bb1ecd8c6d7fed2b1c0d3d.png)

这个时候我们要把测试用例中的错误修改回来：

```go
"leading sep": {input: "沙河有沙又有河", sep: "沙", want: []string{"", "河有", "又有河"}},
```

我们都知道可以通过`-run=RegExp`来指定运行的测试用例，还可以通过`/`来指定要运行的子测试用例，例如：`go test -v -run=Split/simple`只会运行`simple`对应的子测试用例：

![image-20220504195943561](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7848fc2aac8cbf5e52f9a66f72501a71.png)

### 测试覆盖率

测试覆盖率是你的代码被测试套件覆盖的百分比。通常我们使用的都是语句的覆盖率，也就是在测试中至少被运行一次的代码占总代码的比例。

Go提供内置功能来检查你的代码覆盖率。我们可以使用`go test -cover`来查看测试覆盖率。例如：

![image-20220504200047991](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bdba802cb3e118eb6d5eceda649b92bc.png)

从上面的结果可以看到我们的测试用例覆盖了100%的代码。

Go还提供了一个额外的`-coverprofile`参数，用来将覆盖率相关的记录信息输出到一个文件。例如：



## 基准测试(压力测试)

### 基准测试函数格式

基准测试就是在一定的工作负载之下检测程序性能的一种方法。基准测试的基本格式如下：

```go
func BenchmarkName(b *testing.B){
    // ...
}
```

基准测试以`Benchmark`为前缀，需要一个`*testing.B`类型的参数b，基准测试必须要执行`b.N`次，这样的测试才有对照性，`b.N`的值是系统根据实际情况去调整的，从而保证测试的稳定性。 `testing.B`拥有的方法如下：

```go
func (c *B) Error(args ...interface{})
func (c *B) Errorf(format string, args ...interface{})
func (c *B) Fail()
func (c *B) FailNow()
func (c *B) Failed() bool
func (c *B) Fatal(args ...interface{})
func (c *B) Fatalf(format string, args ...interface{})
func (c *B) Log(args ...interface{})
func (c *B) Logf(format string, args ...interface{})
func (c *B) Name() string
func (b *B) ReportAllocs()
func (b *B) ResetTimer()
func (b *B) Run(name string, f func(b *B)) bool
func (b *B) RunParallel(body func(*PB))
func (b *B) SetBytes(n int64)
func (b *B) SetParallelism(p int)
func (c *B) Skip(args ...interface{})
func (c *B) SkipNow()
func (c *B) Skipf(format string, args ...interface{})
func (c *B) Skipped() bool
func (b *B) StartTimer()
func (b *B) StopTimer()
```



### 基准测试示例

我们在`split_test.go`中为split包中的`Split`函数编写基准测试如下：

```go
// 基准测试
func BenchmarkSplit(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Split("沙河有沙又有河", "沙")
	}
}
```

基准测试并不会默认执行，需要增加`-bench`参数，所以我们通过执行`go test -bench=Split`命令执行基准测试，输出结果如下：

![image-20220504202739502](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/294550d69b94888c21a5f9cc41287ef1.png)

信息说明：

- `BenchmarkSplit-12`表示对Split函数进行基准测试，数字`12`表示`GOMAXPROCS`的值，这个对于并发基准测试很重要。

- `5459712`和`220.5ns/op`表示每次调用`Split`函数耗时`220.5ns`，这个结果是`5459712`次调用的平均值。

我们还可以为基准测试添加`-benchmem`参数，来获得内存分配的统计数据：

![image-20220504202955863](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1ae7c6c308073bf1ca9aa4859dd55f87.png)

其中，`112 B/op`表示每次操作内存分配了112字节，`3 allocs/op`则表示每次操作进行了3次内存分配。 

为什么每次操作进行了3次内存分配？因为我们每次都是动态的对result进行append操作。

我们将我们的`Split`函数优化如下：

```go
// Split 以sep切割s，将切割的结果放入result
func Split(s, sep string) (result []string) {
	// 初始化res的容量，避免下面append多次进行内存分配
	result = make([]string, 0, strings.Count(s, sep) + 1)	// 容量为s中sep的个数+1，因为n个sep把s切割成n+1段
	// Index() 返回s中sep第一次出现的位置，没有则返回-1
	i := strings.Index(s, sep)

	for i > -1 {
		// 将i索引之前的先添加到res中
		result = append(result, s[:i])
		// 再将s赋值为i索引以后的值
		s = s[i+len(sep):] // 这里使用len(sep)获取sep的长度
		// 继续寻找下一个sep的索引
		i = strings.Index(s, sep)
	}
	// 循环结束后，将最后一段值添加到res中
	result = append(result, s)
	return
}
```

这一次我们提前使用make函数将result初始化为一个容量足够大的切片（容量为s中sep的个数+1，因为n个sep把s切割成n+1段），而不再像之前一样通过调用append函数来追加。我们来看一下这个改进会带来多大的性能提升：

![image-20220504203627238](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/834caffef01be0ff1dfbe0b9c83c0662.png)

这个使用make函数提前分配内存的改动，减少了2/3的内存分配次数，并且减少了一半多的内存分配。



### 性能比较函数

上面的基准测试只能得到给定操作的绝对耗时，但是在很多性能问题是发生在两个不同操作之间的相对耗时，比如同一个函数处理1000个元素的耗时与处理1万甚至100万个元素的耗时的差别是多少？再或者对于同一个任务究竟使用哪种算法性能最佳？我们通常需要对两个不同算法的实现使用相同的输入来进行基准比较测试。

性能比较函数通常是一个**带有参数**的函数，被**多个不同的Benchmark函数传入不同的值来调用**。举个例子如下：

```go
func benchmark(b *testing.B, size int) {/* ... */}

func Benchmark10(b *testing.B) { benchmark(b, 10) }
func Benchmark100(b *testing.B) { benchmark(b, 100) }
func Benchmark1000(b *testing.B) { benchmark(b, 1000) }
```

例如我们编写了一个计算斐波那契数列的函数如下：

```go
// fib/fib.go
package fib

// Fib 是一个计算第n个斐波那契数的函数
func Fib(n int) int {
	if n < 2 {
		return n
	}
	return Fib(n-1) + Fib(n-2)
}
```

我们编写的性能比较函数如下：

```go
// fib/fib_test.go
package fib

import "testing"

func benchmarkFib(b *testing.B, n int) {
	for i := 0; i < b.N; i++ {
		Fib(n)
	}
}

// 使用性能比较函数
func BenchmarkFib1(b *testing.B)  { benchmarkFib(b, 1) }
func BenchmarkFib2(b *testing.B)  { benchmarkFib(b, 2) }
func BenchmarkFib3(b *testing.B)  { benchmarkFib(b, 3) }
func BenchmarkFib10(b *testing.B) { benchmarkFib(b, 10) }
func BenchmarkFib20(b *testing.B) { benchmarkFib(b, 20) }
func BenchmarkFib40(b *testing.B) { benchmarkFib(b, 40) }
```

运行基准测试：

![image-20220504205820376](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/457a7bb141e4ba794c7de58cc3912d90.png)

这里需要注意的是，默认情况下，每个基准测试至少运行1秒。如果在Benchmark函数返回时没有到1秒，则b.N的值会按1,2,5,10,20,50，…增加，并且函数再次运行。

最终的BenchmarkFib40只运行了两次，每次运行的平均值只有不到一秒。像这种情况下我们应该可以使用`-benchtime`标志增加最小基准时间，以产生更准确的结果。例如：

![image-20220504210141501](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a8a9845a316128760436fd4f87bf9c64.png)

这一次`BenchmarkFib40`函数运行了44次，结果就会更准确一些了。

使用性能比较函数做测试的时候一个容易犯的错误就是把`b.N`作为输入的大小，例如以下两个例子都是**错误的示范**：

```go
// 错误示范1
func BenchmarkFibWrong(b *testing.B) {
	for n := 0; n < b.N; n++ {
		Fib(n)
	}
}

// 错误示范2
func BenchmarkFibWrong2(b *testing.B) {
	Fib(b.N)
}
```



## 重置时间

`b.ResetTimer`之前的处理不会放到执行时间里，也不会输出到报告中，所以可以在之前做一些不计划作为测试报告的操作。例如：

```go
func BenchmarkSplit(b *testing.B) {
	time.Sleep(5 * time.Second) // 假设需要做一些耗时的无关操作
	b.ResetTimer()              // 重置计时器
	for i := 0; i < b.N; i++ {
		Split("沙河有沙又有河", "沙")
	}
}
```



## 并行测试

`func (b *B) RunParallel(body func(*PB))`会以并行的方式执行给定的基准测试。

`RunParallel`会创建出多个`goroutine`，并将`b.N`分配给这些`goroutine`执行， 其中`goroutine`数量的默认值为`GOMAXPROCS`。

用户如果想要增加非CPU受限（non-CPU-bound）基准测试的并行性， 那么可以在`RunParallel`之前调用`SetParallelism` 。`RunParallel`通常会与`-cpu`标志一同使用。

```go
func BenchmarkSplitParallel(b *testing.B) {
	// b.SetParallelism(1) 	// 设置使用的CPU数
	b.RunParallel(func(pb *testing.PB) {
		for pb.Next() {
			Split("沙河有沙又有河", "沙")
		}
	})
}
```

执行一下基准测试：

![image-20220504213556736](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ddc8539b3e123723f5d35c2e7b90aafc.png)

还可以通过在测试命令后添加`-cpu`参数如`go test -bench=Parallel -cpu 1`来指定使用的CPU数量。



## Setup 与 TearDown

测试程序有时需要在测试之前进行额外的设置（setup）或在测试之后进行拆卸（teardown）。

### TestMain

通过在`*_test.go`文件中定义`TestMain`函数来可以在测试之前进行额外的设置（setup）或在测试之后进行拆卸（teardown）操作。

如果测试文件包含函数：`func TestMain(m *testing.M)`那么生成的测试会先调用 `TestMain(m)`，然后再运行具体测试。

`TestMain`运行在主`goroutine`中, 可以在调用 `m.Run`前后做任何设置（setup）和拆卸（teardown）。退出测试的时候应该使用`m.Run`的返回值作为参数调用`os.Exit`。

一个使用`TestMain`来设置Setup和TearDown的示例如下：

```go
func TestMain(m *testing.M) {
	fmt.Println("write setup code here...") // 测试之前的做一些设置
	// 如果 TestMain 使用了 flags，这里应该加上flag.Parse()
	retCode := m.Run()                         // 执行测试
	fmt.Println("write teardown code here...") // 测试之后做一些拆卸工作
	os.Exit(retCode)                           // 退出测试
}
```

`flag.parse()`：主要是把用户传递到命令行的参数解析为对应变量的值。

需要注意的是：在调用`TestMain`时, `flag.Parse()`并没有被调用。所以如果`TestMain` 依赖于 command-line 标志 (包括 testing 包的标记), 则应该显示的调用`flag.Parse()`。



### 子测试的Setup与Teardown

有时候我们可能需要为每个测试集设置Setup与Teardown，也有可能需要为每个子测试设置Setup与Teardown。

下面定义两个函数工具函数如下：

```GO
// 测试集的 Setup 与 Teardown
func setupTestCase(t *testing.T) func(t *testing.T) {
	t.Log("如有需要在此执行: 测试之前的setup")
	return func(t *testing.T) {
		t.Log("如有需要在此执行: 测试之后的teardown")
	}
}

// 子测试的 Setup 与 Teardown
func setupSubTest(t *testing.T) func(t *testing.T) {
	t.Log("如有需要在此执行: 测试之前的setup")
	return func(t *testing.T) {
		t.Log("如有需要在此执行: 测试之后的teardown")
	}
}
```

使用方式如下：

```go
func TestSetupAndTeardownSplit(t *testing.T) {
	type test struct { // 定义test结构体
		input string
		sep   string
		want  []string
	}

	tests := map[string]test{ // 测试用例使用map存储
		"simple":      {input: "a:b:c", sep: ":", want: []string{"a", "b", "c"}},
		"wrong sep":   {input: "a:b:c", sep: ",", want: []string{"a:b:c"}},
		"more sep":    {input: "abcd", sep: "bc", want: []string{"a", "d"}},
		"leading sep": {input: "沙河有沙又有河", sep: "沙", want: []string{"", "河有", "又有河"}},
	}

	teardownTestCase := setupTestCase(t) // 测试集之前执行setup操作
	defer teardownTestCase(t)            // 测试集之后执行teardown操作

	for name, tc := range tests {
		t.Run(name, func(t *testing.T) { // 使用t.Run()执行子测试
			teardownSubTest := setupSubTest(t) // 子测试之前执行setup操作
			defer teardownSubTest(t)           // 子测试之后执行teardown操作
			got := Split(tc.input, tc.sep)
			if !reflect.DeepEqual(got, tc.want) {
				t.Errorf("expected:%#v, got:%#v", tc.want, got)
			}
		})
	}
}
```

测试结果：

![image-20220504224218983](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/33cc6d24f8ca97feb940112d118a66a5.png)



## 示例函数

### 示例函数的格式

被`go test`特殊对待的第三种函数就是示例函数，它们的函数名以`Example`为前缀。它们既没有参数也没有返回值。标准格式如下：

```go
func ExampleName() {
    // ...
}
```



### 示例函数示例

下面的代码是为`Split`函数编写的一个示例函数：

```go
func ExampleSplit() {
	fmt.Println(split.Split("a:b:c", ":"))
	fmt.Println(split.Split("沙河有沙又有河", "沙"))
	// Output:
	// [a b c]
	// [ 河有 又有河]
}
```



为你的代码编写示例代码有如下三个用处：

1. 示例函数能够作为文档直接使用，例如基于web的godoc中能把示例函数与对应的函数或包相关联。

2. 示例函数只要包含了`Output`输出，也是可以通过`go test`运行的可执行测试：

   ![image-20220504225001454](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f1af5658b1eeadb5da99f336cbfaf5bb.png)

3. 示例函数提供了可以直接运行的示例代码，可以直接在`golang.org`的`godoc`文档服务器上使用`Go Playground`运行示例代码。下图为`strings.ToUpper`函数在Playground的示例函数效果。 ![Go Playground](https://www.liwenzhou.com/images/Go/unit_test/example.png)
