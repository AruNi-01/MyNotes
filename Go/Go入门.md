# 初识Go

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

   ![image-20220430110143079](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430110143079.png)

2. Go的工程目录，新建系统变量 GOPATH，变量值为工程目录：

   ![image-20220430111204206](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430111204206.png)

3. 在用户变量里也修改相应的 GOPATH：

   ![image-20220430111449566](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430111449566.png)

4. 在 Path 变量中添加Go的 bin 目录

5. 在终端输入 `go env` 命令，查看 GOPATH 和 GOROOT 路径是否正确和 Go 的版本号：

   ![image-20220430112241689](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430112241689.png)

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

![image-20220430122822728](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430122822728.png)

现在我们来创建第一个Go项目—`hello`，创建一个`hello`目录。





> go mod init

使用 `go module` 模式新建项目时，我们需要通过 `go mod init 项目名 `命令对项目进行初始化，该命令会在项目根目录下生成`go.mod`文件。例如，我们使用`hello`作为我们第一个Go项目的名称，执行如下命令：

```bash
go mod init hello
```

![image-20220430141109052](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430141109052.png)

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

![image-20220430142211489](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430142211489.png)





> go install

`go install`表示安装的意思，它先编译源代码得到可执行文件，然后将可执行文件移动到`GOPATH`的bin目录下。因为我们的环境变量中配置了`GOPATH`下的bin目录，所以我们就可以在任意地方直接执行可执行文件了。

![image-20220430142355022](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430142355022.png)

可以看到 hello 的同级目录下，自动生成了 bin 和 pkg 目录，并且 bin 目录下有 hello.exe可执行文件：

![image-20220430142453035](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430142453035.png)

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

![image-20220501140644801](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501140644801.png)

## 4. VS Code配置代码片段快捷键

按`Ctrl+Shift+P`，输入`snippets`，选择命令并执行，然后再输入go：

![image-20220430143304398](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430143304398.png)

在空白位置就可以自定义代码片段了，其中`$0`表示最终光标提留的位置：

```js
“这里放个名字”:{
    "prefix": "这个是快捷键",
    "body": "这里是按快捷键插入的代码片段",
    "description": "这里放提示信息的描述"
}
```

示例：创建了两个快捷方式，一个是输入`pln`就会在编辑器中插入`fmt.Println()`代码；输入`plf`，就会插入`fmt.Printf("")`代码。

![image-20220430143944404](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430143944404.png)

测试：

![image-20220430144036544](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220430144036544.png)

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

![image-20220502192740753](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220502192740753.png)

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



# JSON and Go

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





