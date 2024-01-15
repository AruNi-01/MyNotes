# 接口

接口（interface）定义了一个对象的行为规范，只定义规范不实现，由具体的对象来实现规范的细节。

在Go语言中接口（interface）是一种类型，一种抽象的类型。相较于之前章节中讲到的那些具体类型（字符串、切片、结构体等）更注重“我是谁”，接口类型更注重“我能做什么”的问题。接口类型就像是一种约定——概括了一种类型应该具备哪些方法，在Go语言中提倡使用面向接口的编程方式实现解耦。

## 接口类型

接口是一种由程序员来定义的类型，**一个接口类型就是一组方法的集合**，它规定了需要实现的所有方法。

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

- **接口类型名**：Go语言的接口在命名时，一般会在单词后面添加`er`，如写操作的接口叫`Writer`。接口名最好要能突出该接口的类型含义。
- **方法名**：当方法名首字母是大写且这个接口类型名首字母也是大写时，这个方法可以被接口所在的包（package）之外的代码访问。
- **参数列表、返回值列表**：参数列表和返回值列表中的参数变量名可以省略。

### 实现接口的条件

接口就是规定了一个 **需要实现的方法列表**，在 Go 语言中**一个类型只要实现了接口中规定的所有方法，那么我们就称它实现了这个接口**。

我们定义的 `Singer` 接口类型，它包含一个 `Sing` 方法。

```go
// Singer 接口
type Singer interface {
	Sing()
}
```

我们有一个 `Bird` 结构体类型如下。

```go
type Bird struct {}
```

因为 `Singer` 接口只包含一个 `Sing` 方法，所以只需要给 `Bird` 结构体添加一个 `Sing` 方法就可以满足 `Singer` 接口的要求。

```go
// Sing Bird类型的Sing方法
func (b Bird) Sing() {
	fmt.Println("汪汪汪")
}
```

这样就称为 `Bird` 实现了 `Singer` 接口。

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

利用这一特性，就能很方便的实现多态了。

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

但是 **不能将`Cat`类型的变量赋值给`Mover`接口类型的变量`x`**。

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

## 接口组合（嵌套）

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

对于这种由多个接口类型组合形成的新接口类型，同样 **只需要实现新接口类型中规定的所有方法就算实现了该接口类型**。

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

通过在结构体中嵌入一个接口类型，**从而让该结构体类型实现了该接口类型，并且还可以改写该接口的方法**。

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

空接口是指没有定义任何方法的接口类型。因此 **任何类型都可以视为实现了空接口**。也正是因为空接口类型的这个特性，**空接口类型的变量可以存储任意类型的值**。

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
var x interface{}  // 声明一个空接口类型变量 x
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

**注意：**此时接口变量`m`与`nil`并不相等，因为它 **只是动态值的部分为`nil`**，而动态类型部分保存着对应值的类型。

```go
fmt.Println(m == nil) // false
```

接口值是支持相互比较的，**仅当接口值的动态类型和动态值都相等** 时才相等。

```go
var (
	x Mover = new(Dog)
	y Mover = new(Car)
)
fmt.Println(x == y) // false
```

但是有一种特殊情况需要特别注意，如果接口值保存的动态类型相同，但是这个 动态类型不支持互相比较（比如切片），那么对它们相互比较时就会引发 panic。

```go
var z interface{} = []int{1, 2, 3}
fmt.Println(z == z) // panic: runtime error: comparing uncomparable type []int
```

## 类型断言

接口值可能赋值为任意类型的值，那我们 **如何从接口值获取其存储的具体数据呢**？

我们可以借助标准库`fmt`包的格式化打印获取到接口值的动态类型。

```go
var m Mover

m = &Dog{Name: "旺财"}
fmt.Printf("%T\n", m) // *main.Dog

m = new(Car)
fmt.Printf("%T\n", m) // *main.Car
```

而`fmt`包内部其实是使用反射的机制在程序运行时获取到动态类型的名称。关于反射的内容会在后面详细学习。



而想要 **从接口值中获取到对应的实际值** 需要使用 **类型断言**，其语法格式如下。

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

如果 **对一个接口值有多个实际类型需要判断，推荐使用`switch`语句来实现**。

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

断言还有一个用法就是，通过类型断言来判断变量是否实现了某个接口，如果有则走接口的函数，否则走默认的函数。就像下面这样：

```go
type xmlWriter interface {
    WriteXML(w io.Writer) error
}

// Exported XML streaming function.
func StreamXML(v interface{}, w io.Writer) error {
    if xw, ok := v.(xmlWriter); ok {
        // It’s an  xmlWriter, use method of asserted type.
        return xw.WriteXML(w)
    }
    // No implementation, so we have to use our own function (with perhaps reflection):
    return encodeToXML(v, w)
}

// Internal XML encoding function.
func encodeToXML(v interface{}, w io.Writer) error {
    // ...
}
```

这样就实现了适用于该流类型的任何变量的 StreamXML 函数，并用类型断言检查传入的变量是否实现了该接口；如果没有，我们就调用内建的 encodeToXML 来完成相应工作





# 反射

Go语言中的变量是分为两部分的：

- 类型信息：预先定义好的元信息
- 值信息：程序运行过程中可动态变化的



## 反射介绍

反射是指在程序运行期对程序本身进行访问和修改的能力。程序在编译时，变量被转换为内存地址，变量名不会被编译器写入到可执行部分。在运行程序时，程序无法获取自身的信息。

支持反射的语言可以在程序编译期将变量的反射信息，如字段名称、类型信息、结构体信息等整合到可执行文件中，并给程序提供接口访问反射信息，这样就可以在程序运行期获取类型的反射信息，并且有能力修改它们。

Go程序在运行期使用reflect包访问程序的反射信息。

实际上，**反射是通过检查一个接口的值，变量首先被转换成空接口**。这从下面两个函数签名能够很明显的看出来：

```go
func TypeOf(i interface{}) Type
func ValueOf(i interface{}) Value
```

接口的值包含一个 type 和 value。反射可以从接口值反射到对象，也可以从对象反射回接口值。

空接口可以存储任意类型的变量，那我们如何知道这个空接口保存的数据是什么呢？ 反射就是在运行时动态的获取一个变量的类型信息和值信息。



## reflect 包

在Go语言的反射机制中，**任何接口值都由是`一个具体类型`和`具体类型的值`两部分组成的**。

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

在反射中关于类型还划分为两种：`类型（Type）`（type.Name 就是类型的名字）和`种类（Kind）`。

因为在Go语言中我们**可以使用type关键字构造很多自定义类型**，**而`种类（Kind）`就是指底层的类型**，但在反射中，当需要**区分指针、结构体等大品种的类型时，就会用到`种类（Kind）`**。 举个例子，我们定义了两个指针类型和两个结构体类型，通过反射查看它们的类型和种类。

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
	reflectType(a) // type: 空 kind:ptr
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

`reflect.ValueOf()`返回的是`reflect.Value`类型，其中包含了原始值的值信息。**`reflect.Value`与原始值之间可以互相转换**。

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

想要在函数中通过反射修改变量的值，需要注意**函数参数传递的是值拷贝**，必须**传递变量的地址才能修改变量值**。而 **反射中使用专有的`Elem()`方法来获取指针对应的值**。

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
     // 反射中使用 Elem()方法获取指针对应的值，如果直接 v.Kind() 则返回的是指针类型
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

所以在反射中，**如果需要使用 setter 方法，那么就需要传入地址，在反射中就需要使用 Elem() 方法获取该指针对应的值**，然后才能进行下一步的处理，切记不可直接操作指针，否则将会引发 panic。

如果不需要使用 setter 方法，那么还是推荐直接传值，这样在反射中就不用使用 Elem() 了，减少了出错的概率。



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

### StructField 类型

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



# 泛型

Go 1.18版本增加了对泛型的支持，泛型也是自 Go 语言开源以来所做的最大改变。

泛型允许程序员在编写某些代码或数据结构时先不提供值的类型，而是之后再提供。

泛型是一种独立于所使用的特定类型的编写代码的方法。使用泛型可以编写出适用于一组类型中的任何一种的函数和类型。

### 为什么需要泛型

假设我们需要实现一个反转切片的函数——`reverse`。

```go
func reverse(s []int) []int {
	l := len(s)
	r := make([]int, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}

fmt.Println(reverse([]int{1, 2, 3, 4}))  // [4 3 2 1]
```

可是这个函数只能接收`[]int`类型的参数，如果我们想支持`[]float64`类型的参数，我们就需要再定义一个`reverseFloat64Slice`函数。

```go
func reverseFloat64Slice(s []float64) []float64 {
	l := len(s)
	r := make([]float64, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}
```

如果要想支持`[]string`类型切片就要定义`reverseStringSlice`函数，如果想支持`[]xxx`就需要定义一个`reverseXxxSlice`…

一遍一遍地编写相同的功能是低效的，实际上这个反转切片的函数并不需要知道切片中元素的类型，但为了适用不同的类型我们把一段代码重复了很多遍。

Go1.18 之前我们可以尝试使用反射去解决上述问题，但是使用反射在运行期间获取变量类型会降低代码的执行效率并且失去编译期的类型检查，同时大量的反射代码也会让程序变得晦涩难懂。

类似这样的场景就非常适合使用泛型。从 Go1.18 开始，使用泛型就能够编写出适用所有元素类型的“普适版”`reverse`函数。

```go
package main

import "fmt"

func main() {
	sliInt := []int32{1, 2, 3, 4, 5}
	sliFloat := []float64{1.1, 2.2, 3.3, 4.4, 5.5}
	sliString := []string{"1", "2", "3", "4", "5"}
	reverseInt := reverseWithGenerics(sliInt)
	reverseFloat := reverseWithGenerics(sliFloat)
	reverseString := reverseWithGenerics(sliString)
	fmt.Printf("reverseInt: %v\nreverseFloat: %v\nreverseString: %v\n", reverseInt, reverseFloat, reverseString)
	/*
		输出：
		reverseInt: [5 4 3 2 1]
		reverseFloat: [5.5 4.4 3.3 2.2 1.1]
		reverseString: [5 4 3 2 1]
	*/
}

// 利用泛型，对任意类型的 slice 进行反转
func reverseWithGenerics[T any](slice []T) []T {
	l := len(slice)
	newSlice := make([]T, l)
	for i, e := range slice {
		newSlice[l-i-1] = e
	}
	return newSlice
}
```

### 泛型语法

泛型为 Go 语言添加了三个新的重要特性:

1. **函数和类型的类型参数**；
2. **将接口类型定义为类型集**，包括没有方法的类型；
3. **类型推断**，它允许在调用函数时在许多情况下省略类型参数。

#### 类型参数

> 类型形参和类型实参

我们之前已经知道函数定义时可以指定形参，函数调用时需要传入实参。

![形参与实参](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305060931906.png)

现在，Go语言中的函数和类型支持添加类型参数。类型参数列表看起来像普通的参数列表，只不过它使用方括号（`[]`）而不是圆括号（`()`）。

![类型形参与类型实参](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305060931109.png)

借助泛型，我们可以声明一个适用于 **一组类型** 的 `min` 函数。

```go
// 类型参数列表
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

> 类型实例化

这次定义的`min`函数就同时支持`int`和`float64`两种类型，也就是说当调用`min`函数时，我们既可以传入`int`类型的参数。

```go
m1 := min[int](1, 2)  // 1
```

也可以传入`float64`类型的参数。

```go
m2 := min[float64](-0.1, -0.2)  // -0.2
```

**向 `min` 函数提供类型参数(在本例中为`int`和`float64`)称为实例化**（ *instantiation* ）。

**类型实例化分两步进行**：

1. 首先，编译器在整个泛型函数或类型中将所有类型形参（type parameters）替换为它们各自的类型实参（type arguments）。
2. 其次，编译器验证每个类型参数是否满足相应的约束。

**在成功实例化之后，我们将得到一个非泛型函数**，它可以像任何其他函数一样被调用。例如：

```go
fmin := min[float64] // 类型实例化，编译器生成T=float64的min函数
m2 = fmin(1.2, 2.3)  // 1.2
```

`min[float64]` 得到的是类似我们之前定义的 `minFloat64` 函数——`fmin`，我们可以在函数调用中使用它。

```go
func main() {
	// 类型参数列表的使用：
	// 类型实例化
	min[int](1, 3)
	min[float64](1.1, 3.3)
	// 类型实例化时，将返回一个实参的类型函数
	IntMinFunc := min[int]
	IntMinFunc(1, 2)
	FloatMinFunc := min[float64]
	FloatMinFunc(1.1, 3.3)
}

// 类型参数列表
func min[T int | float64](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

> 类型参数的使用

除了函数中支持使用类型参数列表外，类型也可以使用类型参数列表。

```go
type Slice[T int | string] []T

type Map[K int | string, V float32 | float64] map[K]V

type Tree[T interface{}] struct {
	left, right *Tree[T]
	value       T
}
```

在上述泛型类型中，`T`、`K`、`V`都属于类型形参，类型形参后面是类型约束，类型实参需要满足对应的类型约束。

泛型类型可以有方法，例如为上面的`Tree`实现一个查找元素的`Lookup`方法。

```go
func (t *Tree[T]) Lookup(x T) *Tree[T] { ... }
```

要使用泛型类型，必须进行实例化。`Tree[string]`是使用类型实参`string`实例化 `Tree` 的示例。

```go
var stringTree Tree[string]
```

```go
type Slice[T int | string] []T

type Map[K int | string, V float32 | float64] map[K]V

type Tree[T interface{}] struct {
	left, right *Tree[T]
	value       T
}

// Lookup 泛型类型的方法
func (t *Tree[T]) Lookup(x T) *Tree[T] {
	t1 := new(Tree[T])
	return t1
}

func main() {
	// 泛型类型必须实例化后才能使用
	var stringTree Tree[string]
	stringTree.left = nil
	stringTree.right = nil
	stringTree.value = "val"
}
```

> 类型约束

普通函数中的每个参数都有一个类型; 该类型定义一系列值的集合。例如，我们上面定义的非泛型函数`minFloat64`那样，声明了参数的类型为`float64`，那么在函数调用时允许传入的实际参数就必须是可以用`float64`类型表示的浮点数值。

类似于参数列表中每个参数都有对应的参数类型，类型参数列表中每个类型参数都有一个 **类型约束**。**类型约束定义了一个类型集——只有在这个类型集中的类型才能用作类型实参**。

Go语言中的类型约束是接口类型。

就以上面提到的`min`函数为例，我们来看一下类型约束常见的两种方式。

类型约束接口可以直接在类型参数列表中使用。

```go
// 类型约束字面量，通常外层interface{}可省略
func min[T interface{ int | float64 }](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

作为类型约束使用的接口类型可以事先定义并支持复用。

```go
// 事先定义好的类型约束类型
type Value interface {
	int | float64
}
func min[T Value](a, b T) T {
	if a <= b {
		return a
	}
	return b
}
```

在使用类型约束时，如果省略了外层的`interface{}`会引起歧义，那么就不能省略。例如：

```go
type IntPtrSlice [T *int] []T  // 编译报错：T*int ? 数组的长度必须是一个非负整数常量

type IntPtrSlice[T *int,] []T  // 只有一个类型约束时可以添加`,`
type IntPtrSlice[T interface{ *int }] []T // 使用interface{}包裹
```

#### 类型集



#### 类型推断











# 错误异常处理

Go 没有像 Java 和 .NET 那样的 `try/catch` 异常机制：不能执行抛异常操作。但是有一套 `defer-panic-and-recover` 机制。Go 语言中把错误当成一种值来处理，更强调判断错误、处理错误，而不是一股脑的 catch 捕获异常。

Go 是怎么处理普通错误的呢？通过在函数和方法中返回错误对象作为它们的唯一或最后一个返回值 — 如果返回 nil，则没有错误发生 — 并且主调（calling）函数总是应该检查收到的错误。

处理错误并且在函数发生错误的地方给用户返回错误信息：照这样处理就算真的出了问题，你的程序也能继续运行并且通知给用户。`panic and recover` 是用来处理真正的异常（无法预测的错误）而不是普通的错误。

在前面的章节中我们了解了 Go 检查和报告错误条件的惯有方式：

- 产生错误的函数会返回两个变量，一个值和一个错误码；如果后者是 nil 就是成功，非 nil 就是发生了错误。
- 为了防止发生错误时正在执行的函数（如果有必要的话甚至会是整个程序）被中止，在调用函数后必须检查错误。

下面这段来自 pack1 包的代码 Func1 测试了它的返回值：

```go
if value, err := pack1.Func1(param1); err != nil {
    fmt.Printf(“Error %s in pack1.Func1 with parameter %v”, err.Error(), param1)
    return    // or: return err
} else {
    // Process(value)
}
```

为了更清晰的代码，应该总是使用包含错误值变量的 if 复合语句。上例除了 `fmt.Printf` 还可以使用 `log` 中对应的方法（后面会讲解），如果程序中止也没关系的话甚至可以使用 `panic`。

## 错误处理

### Error 接口

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

**注意**：当我们使用`fmt`包打印错误时会 **自动调用 error 类型的 Error 方法**，也就是会打印出错误的描述信息。

### 创建错误

我们可以根据需求自定义 error，最简单的方式是使用`errors` 包提供的`New`函数创建一个错误。函数签名如下：

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



### fmt.Errorf

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



### 错误结构体类型

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

在大部分情况下自定义错误结构类型很有意义的，可以包含除了（低层级的）错误信息以外的其它有用信息，其实就是自定义错误的结构体，然后实现 String() 方法。

例如，正在进行的操作（打开文件等），全路径或名字。看下面例子中 os.Open 操作触发的 PathError 错误：

```go
// PathError records an error and the operation and file path that caused it.
type PathError struct {
    Op string    // "open", "unlink", etc.
    Path string  // The associated file.
    Err error  // Returned by the system call.
}

func (e *PathError) String() string {
    return e.Op + " " + e.Path + ": "+ e.Err.Error()
}
```

如果有不同错误条件可能发生，那么对实际的错误使用类型断言或类型判断（type-switch）是很有用的，并且可以根据错误场景做一些补救和恢复操作。

```go
//  err != nil 时，接下来使用断言处理 PathError，进行补救
if e, ok := err.(*os.PathError); ok {
    // remedy situation（补救措施）
}
```

或者：

```go
switch err := err.(type) {
    case ParseError:
        PrintParseError(err)
    case PathError:
        PrintPathError(err)
    ...
    default:
        fmt.Printf("Not a special error, just %s\n", err)
}
```

## 运行时异常和 panic

当发生像数组下标越界或类型断言失败这样的运行错误时，Go 运行时会触发运行时 panic，伴随着程序的崩溃抛出一个 runtime.Error 接口类型的值。这个错误值有个 RuntimeError() 方法用于区别普通错误。

panic 可以直接从代码初始化：当错误条件（我们所测试的代码）很严苛且不可恢复，程序不能继续运行时，可以使用 panic 函数产生一个中止程序的运行时错误。panic 接收一个做任意类型的参数，通常是字符串，在程序死亡时被打印出来。Go 运行时负责中止程序并给出调试信息。

```go
package main

import "fmt"

func main() {
    fmt.Println("Starting the program")
    panic("A severe error occurred: stopping the program!")
    fmt.Println("Ending the program")
}
```

输出如下：

```go
Starting the program
panic: A severe error occurred: stopping the program!

goroutine 1 [running]:
main.main()
        /Users/aarynlu/MyKB/GolangStudy/basics/error_exception/exception_panic.go:7 +0x65

Process finished with the exit code 2

```

当发生错误必须中止程序时，panic 可以用于错误处理模式：

```go
if err != nil {
    panic("ERROR occurred:" + err.Error())
}
```

**Go panicking**：**在多层嵌套的函数调用中调用 panic**，可以马上中止当前函数的执行，**所有的 defer 语句都会保证执行** 并把控制权交还给接收到 panic 的函数调用者。这样向上冒泡直到最顶层，并执行（每层的） defer，在栈顶处程序崩溃，并在命令行中用传给 panic 的值报告错误情况。这个终止过程就是 panicking。

不能随意地用 panic 中止程序，必须尽力补救错误让程序能继续执行。

## Recover：从 panic 恢复

正如名字一样，这个（recover）内建函数被用于从 panic 或 错误场景中恢复：**让程序可以从 panicking 重新获得控制权，停止终止过程进而恢复正常执行**。

**recover 只能在 defer 修饰的函数中使用**：用于取得 panic 调用中传递过来的错误值，如果是正常执行，调用 recover 会返回 nil，且没有其它效果。

总结：**panic 会导致栈被展开直到 defer 修饰的 recover() 被调用或者程序中止**。

下面例子中的 protect 函数调用函数参数 g 来保护调用者防止从 g 中抛出的运行时 panic，并展示 panic 中的信息：

```go
func protect(g func()) {
	// 用 defer 做最后的 panic 捕获
	defer func() {
		log.Println("done")
		if err := recover(); err != nil {
			log.Printf("run time panic: %v", err)
		}
	}()
	log.Println("start")
	g() // 可能会发生 runtime error
}
```

下面是一个展示 panic，defer 和 recover 怎么结合使用的完整例子：

```go
package main

import "fmt"

func badCall() {
	// 引发一个 panic，如果不进行捕获则会使程序崩溃
	panic("this is a badCall, will be panic")
}

func test() {
	// 使用 recover 捕获 panic
	defer func() {
		if err := recover(); err != nil {
			fmt.Printf("panicing: %s\n", err)
		}
	}()
	badCall()
	fmt.Println("badCall after")
}

func main() {
	fmt.Println("test() call")
	test()
	fmt.Println("test() completed")
	
	/* 输出：
	test() call
	panicing: this is a badCall, will be panic
	test() completed
	 */
}
```

从输出可以看出，badCall 函数中引发的 panic 在 test 中被捕获处理了，整个程序是正常运行了的。否则将会引发 panic 导致整个程序崩溃。

Go 标准库中许多地方都用了这个机制，例如，json 包中的解码和 regexp 包中的 Complie 函数。Go 库的原则是即使在包的内部使用了 panic，在它的对外接口（API）中也必须用 recover 处理成返回显式的错误。

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

线程（thread）：操作系统基于进程开启的轻量级进程，是进程的一个执行实体，**是操作系统调度执行的最小单位**。





> 进程和线程的联系

- **一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程**；

- 资源分配给进程，**同一进程的所有线程共享该进程的所有资源**；
- 处理机分给线程，即 **真正在处理机上运行的是线程**；
- 线程在执行过程中，需要协作同步。不同进程的线程间要利用消息通信的办法实现同步。





**协程**（coroutine）：**比线程更加轻量级，协程不是被操作系统内核所管理，而完全是由程序所控制**（也就是在用户态执行）

![在这里插入图片描述](http://5b0988e595225.cdn.sohucs.com/images/20180622/6765e36cc4604fba897976638af03524.jpeg)

**当系统调用（比如等待 I/O）阻塞协程时，其他协程会继续在其他线程上工作**。协程的设计隐藏了许多线程创建和管理方面的复杂工作。

**协程是轻量的，比线程更轻**。它们痕迹非常不明显（使用少量的内存和资源）：使用 4K 的栈内存就可以在堆中创建它们。因为创建非常廉价，必要的时候可以轻松创建并运行大量的协程（在同一个地址空间中 100,000 个连续的协程）。并且它们对栈进行了分割，从而动态的增加（或缩减）内存的使用；栈的管理是自动的，但不是由垃圾回收器管理的，而是在协程退出后自动释放。 

**协程可以运行在多个操作系统线程之间，也可以运行在线程之内**，让你可以很小的内存占用就可以处理大量的任务。由于操作系统线程上的协程时间片，你可以 **使用少量的操作系统线程就能拥有任意多个提供服务的协程**，而且 Go runtime 可以聪明的意识到哪些协程被阻塞了，暂时搁置它们并处理其他协程。

### 并发模型

业界将如何实现并发编程总结归纳为各式各样的并发模型，常见的并发模型有以下几种：

- 线程&锁模型
- Actor模型
- CSP模型
- Fork&Join模型

Go语言中的并发程序主要是通过基于 CSP（communicating sequential processes）的 goroutine 和 channel 来实现，当然也支持使用传统的多线程共享内存的并发方式。



## Goroutine

**Goroutine** 是 Go  语言支持并发的核心，在一个 Go 程序中同时创建成百上千个 goroutine 是非常普遍的，一个 goroutine 会以一个很小的栈开始其生命周期，一般只需要 2KB。

协程的栈会根据需要进行伸缩，不会出现栈溢出；开发者无需关心栈的大小。当协程结束的时候，它会静默退出：用来启动这个协程的函数也不会得到任何的返回值。

区别于操作系统线程由系统内核进行调度，**goroutine 是由 Go 运行时（runtime）负责调度**。**例如 Go 运行时会智能地将 m 个 goroutine  合理地分配给 n 个操作系统线程，实现类似 m:n 的调度机制，不再需要 Go 开发者自行在代码层面维护一个线程池**。

Goroutine 是 Go 程序中最基本的并发执行单元。每一个 Go 程序都至少包含一个 goroutine——main goroutine，当 Go 程序启动时它会自动创建。

在 Go 语言编程中你不需要去自己写进程、线程、协程，你的技能包里只有一个技能——goroutine，**当你需要让某个任务并发执行的时候，你只需要把这个任务包装成一个函数，开启一个 goroutine 去执行这个函数就可以了**，就是这么简单粗暴。



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

**当 main 函数结束时整个程序也就结束了，同时 main  goroutine 也结束了，所有由 main goroutine 创建的 goroutine 也会一同退出**。也就是说我们的 main  函数退出太快，另外一个 goroutine 中的函数还未执行完程序就退出了，导致未打印出 “hello”。

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

为什么会先打印 `你好` 呢？

这是因为在程序中创建 goroutine 执行函数需要一定的开销，而与此同时 main 函数所在的 goroutine 是继续执行的。

![main goroutine和hello goorutine](https://www.liwenzhou.com/images/Go/concurrence/goroutine02.png)

在上面的程序中使用`time.Sleep`让 main goroutine 等待 hello goroutine执行结束是不优雅的，当然也是不准确的。

Go 语言中通过 `sync`包 为我们提供了一些常用的并发原语，我们会在后面学习`sync`包中的内容。

先介绍一下 sync 包中的`WaitGroup`。当你并不关心并发操作的结果或者有其它方式收集并发操作的结果时，**`WaitGroup`是实现等待一组并发操作完成的好方法**。

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

### 父协程与子协程的关系

**注意：只有 main 协程会影响到它的子协程，其他父协程不会影响其子协程**。

为了说明上面的结论，请看下面这个示例：

```go
func main() {
	fmt.Println("main 协程开始...")
	go func() {
		fmt.Println("父协程开始...")
		go func() {
			fmt.Println("子协程开始...")
			for {
				fmt.Println("子协程 doing...")
			}
		}()
		time.Sleep(time.Microsecond * 20)
		fmt.Println("父协程退出...")
	}()
	time.Sleep(time.Microsecond * 80)
	fmt.Println("main 协程退出...")
}
```

输出：

```text
main 协程开始...
父协程开始...
子协程开始...
子协程 doing...
子协程 doing...
........................
子协程 doing...
子协程 doing...
父协程退出...
main 协程退出...
子协程 doing...
子协程 doing...
子协程 doing...
子协程 doing...

Process finished with the exit code 0
```

可以发现，**哪怕 main 协程都退出了，由所谓的父协程创建的子协程也还在运行**。

**因为 main 协程在结束时，会调用 `exit(0)` 退出整个进程，`main` 协程创建的协程是由进程管理的，当进程结束时，所有协程都会被强制终止。而如果一个协程在创建时被设置为独立的协程（使用 `go` 关键字，且不由 main 协程创建），则该协程将不依赖于 `main` 协程，可以继续运行直到它自己完成或被终止**。

所以至于为什么 Golang 要这样设计，可能是由于在 Golang 中，main 函数是程序的入口，同时 main 函数终止就代表整个程序的终止，所以 main 协程结束意味着整个程序都结束了，其他协程也应该结束。

而至于 main 协程结束后，其子子协程或者子子子协程还能继续执行，这也许是无法控制的吧。因为我们的程序一般都不会在 main 协程中执行，所以在其他地方需要异步执行，而不会说等一个协程返回后在执行后续的操作（如果需要，那么可以使用信号量等方式来实现即可），所以需要让父协程和子协程脱掉干系。

如果我们就是想要让父协程和子协程挂上钩呢？比如说我在父协程中开启了 100 个协程去异步处理任务，在父协程中如果遇到了某种情况（比如内存到达阈值了），我就要让所有子协程都终止。这时候就要使用到 context 了。

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

这是因为 **这个匿名函数也是一个闭包**，匿名函数中包含了一个外部作用域的变量 i，**当匿名函数的 goroutine 在执行的时候，还要去外面找 i 的值；可能外面的for循环已经遍历好多次了，匿名函数的 goroutine 才开始执行，所以慢的那些 goroutine 就会打印出相同的值**。此例中就是for循环的 i 已经遍历到100了，还有好几个 goroutine 才开始执行，所以都输出了相同的。

**解决方法**：让匿名函数中的 i 在自己内部去查找，只需要将外部作用域的 i 当成参数传进匿名函数中即可。

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

操作系统的线程一般都有固定的栈内存（通常为2MB），而 goroutine 的栈不是固定的，可以根据需要动态地增大或缩小，**Go 的 runtime 会自动为 goroutine 分配合适的栈空间**。

而且 Go 的 goroutine 非常轻量级，一个 goroutine  的初始栈空间很小（一般为2KB），所以在 Go 语言中一次创建数万个 goroutine 也是可能的。



### goroutine 调度

操作系统的线程被操作系统内核调度时会挂起当前执行的线程，并将它的寄存器内容保存到内存中，选出下一次要执行的线程并从内存中恢复该线程的寄存器信息，然后恢复执行该线程的现场并开始执行线程。**从一个线程切换到另一个线程需要完整的上下文切换，因为可能需要多次内存访问，所以这个切换上下文的操作开销较大，会增加运行的 cpu 周期**。

区别于操作系统内核调度操作系统线程，**goroutine 的调度是 Go 语言运行时（runtime）层面的实现，是完全由 Go  语言本身实现的一套调度系统 — go scheduler**。它的作用是 **按照一定的规则将所有的 goroutine 调度到操作系统线程上执行**。

在经历数个版本的迭代之后，目前 Go 语言的调度器采用的是 `GMP` 调度模型。

![gpm](https://www.liwenzhou.com/images/Go/concurrence/gpm.png)

其中：

- G：表示 goroutine，每执行一次 `go f()` 就创建一个 G，包含要执行的函数和上下文信息。
- 全局队列（Global Queue）：存放等待运行的 G。
- P：表示 goroutine 执行所需的资源，最多有 GOMAXPROCS 个。
- P 的本地队列：同全局队列类似，存放的也是等待运行的G，存的数量有限，不超过256个。新建 G 时，G 优先加入到 P 的本地队列，如果本地队列满了会批量移动部分 G 到全局队列。
- M：线程想运行任务就得获取 P，从 P 的本地队列获取 G，当 P 的本地队列为空时，M 也会尝试从全局队列或其他 P 的本地队列获取 G。M 运行 G，G 执行之后，M 会从 P 获取下一个 G，不断重复下去。
- Goroutine 调度器和操作系统调度器是通过 M 结合起来的，每个 M 都代表了1个内核线程，操作系统调度器负责把内核线程分配到 CPU 的核上执行。

P 与 M 一般也是一一对应的。他们关系是： **P 管理着一组 G  挂载在 M 上运行**。**当一个 G 长久阻塞在一个 M 上时，runtime 会新建一个 M，阻塞 G 所在的 P 会把其他的 G 挂载在新建的 M 上**。当旧的 G 阻塞完成或者认为其已经死掉时 回收旧的 M。

单从线程调度讲，Go 语言相比起其他语言的优势在于 OS 线程是由 OS 内核来调度的， **goroutine  则是由 Go 运行时（runtime）自己的调度器调度的，完全是在用户态下完成的，  不涉及内核态与用户态之间的频繁切换，包括内存的分配与释放，都是在用户态维护着一块大的内存池，不直接调用系统的 malloc 函数（除非内存池需要改变），成本比调度 OS 线程低很多**。  另一方面充分利用了多核的硬件资源，**近似的把若干 goroutine 均分在物理线程上**， 再加上本身 goroutine  的超轻量级，以上种种特性保证了 goroutine 调度方面的性能。

### GOMAXPROCS

**Go 运行时的调度器使用 `GOMAXPROCS` 参数来确定需要使用多少个 OS 线程来同时执行 Go 代码**，默认值是机器上的 CPU 核心数。

Go语言中可以通过 `runtime.GOMAXPROCS` 函数设置当前程序并发时占用的 CPU逻辑核心数。（Go1.15版本之前，默认使用的是单核心执行。Go1.15 版本之后，默认使用全部的CPU 逻辑核心数。）



## Goroutine 池

> worker pool（goroutine池）

在工作中我们通常会使用`worker pool`模式，控制`goroutine`的数量，防止`goroutine`泄露和暴涨。

一个简易的`worker pool`示例如下：

```go
package main

import (
	"fmt"
	"time"
)

/* goroutine 池：控制 goroutine 的数量，去执行多个任务 */

// worker：从 jobs 中获取任务，执行完毕后将结果写入 results
func worker(id int, jobs <-chan int, results chan<- int) {
	// 循环 jobs 获取任务
	for job := range jobs {
		fmt.Printf("worker(goroutine)-%d start job: %d\n", id, job)
		results <- job * 2
		time.Sleep(time.Millisecond * 500) // 模拟执行了 5ms
		fmt.Printf("worker(goroutine)-%d stop job: %d\n", id, job)
	}
	// results 通道接收完后关闭，避免下面从中取结果输出时一直等待，造成死锁
	close(results)
}

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 开启 3 个 goroutine 去执行下面的 5 个任务，一个 goroutine 对应一个 worker
	for i := 0; i < 3; i++ {
		go worker(i, jobs, results)
	}

	// 向 jobs 通道中发送 5 个任务
	for i := 0; i < 5; i++ {
		jobs <- i
	}
	// 发送完任务后记得关闭，避免上面从 jobs 中获取任务时一直等待，发生死锁
	close(jobs)

	// 从 results 通道中输出结果
	for result := range results {
		fmt.Println(result)
	}
}
```

输出如下，可以发现：worker0 和 worker2 都执行了 2 个任务（执行完一个任务后，继续循环获取任务，直到收到 channel 关闭的信号），worker1 只执行了 1 个，这就实现了 3 个 goroutine 完成 5 个工作任务。

```text
worker(goroutine)-0 start job: 1
worker(goroutine)-2 start job: 0
worker(goroutine)-1 start job: 2
2
0
4
worker(goroutine)-0 stop job: 1
worker(goroutine)-0 start job: 3
worker(goroutine)-2 stop job: 0
worker(goroutine)-2 start job: 4
worker(goroutine)-1 stop job: 2
6
8
```



## GMP 调度器

### 结论

\1. 为了解决 Go 早期多线程 M 对应多协程 G 调度器的**全局锁、中心化状态带来的锁竞争导致的性能下降**等问题**，**Go 开发者引入了处理器 P 结构，形成了当前经典的 GMP 调度模型；

\2. Go 调度器是指：**运行时在用户态提供的多个函数组成的一种机制，目的是高效地调度 G 到 M上去执行；**

\3. Go 调度器的核心思想是：**尽可能复用线程 M**，避免频繁的线程创建和销毁；**利用多核并行能力**，限制同时运行（不包含阻塞）的 M 线程数 等于 CPU 的核心数目； **Work Stealing 任务窃取机制**，M 可以从其他 M 绑定的 P 的运行队列偷取 G 执行；**Hand Off 交接机制**，为了提高效率，M 阻塞时，会将 M 上 P 的运行队列交给其他 M 执行；**基于协作的抢占机制**，为了保证公平性和防止 Goroutine 饥饿问题，Go 程序会保证 **每个 G 运行 10ms 就让出 M**，交给其他 G 去执行，这个 G 运行 10ms 就让出 M 的机制，是由单独的系统监控线程通过 retake() 函数给当前的 G 发送抢占信号实现的，如果所在的 P 没有陷入系统调用且没有满，让出的 G 优先进入本地 P 队列，否则进入全局队列；**基于信号的真抢占机制**，Go1.14 引入了基于信号的抢占式调度机制，解决了 GC 垃圾回收和栈扫描时无法被抢占的问题；

\4. 由于**数据局部性**，新创建的 G 优先放入本地队列，**在本地队列满了时，会将本地队列的一半 G 和新创建的 G 打乱顺序，一起放入全局队列**；**本地队列如果一直没有满**，也不用担心，**全局队列的 G 永远会有 1/61 的机会被获取到**，调度循环中，优先从本地队列获取 G 执行，不过每隔61次，就会直接从全局队列获取，至于为啥是 61 次，Dmitry 的视频讲解了，就是要一个既不大又不小的数，而且不能跟其他的常见的2的幂次方的数如 64 或 48 重合；

\5. M 优先执行其所绑定的 P 的本地运行队列中的 G，**如果本地队列没有 G，则会从全局队列获取**，为了**提高效率和负载均衡**，会从全局队列获取多个 G，而不是只取一个，个数是自己应该从全局队列中承担的，globrunqsize / nprocs + 1；同样，当全局队列没有时，会从其他 M 的 P 上偷取 G 来运行，偷取的个数通常是其他 P 运行队列的一半；

\6. G 在运行时中的状态可以简化成三种：等待中_Gwaiting、可运行_Grunnable、运行中_Grunning，运行期间大部分情况是在这三种状态间来回切换；

\7. M 的状态可以简化为只有两种：自旋和非自旋；**自旋状态，表示 M 绑定了 P 又没有获取 G**；非自旋状态，表示正在执行 Go 代码中，或正在进入系统调用，或空闲；

\8. P 结构体中最重要的，是持有一个可运行 G 的 **长度为 256 的本地环形队列**，可以通过 **CAS 的方式无锁访问**，跟需要加锁访问的全局队列 schedt.runq 相对应；

\9. 调度器的启动逻辑是：初始化 g0 和 m0，并将二者互相绑定， m0 是程序启动后的初始线程，g0 是 m0 线程的系统栈代表的 G 结构体，负责普通 G 在 M 上的调度切换 **-->** runtime.schedinit()：负责M、P 的初始化过程，分别调用runtime.mcommoninit() 初始化 M 的全局队列allm 、调用 runtime.procresize() 初始化全局 P 队列 allp **-->** runtime.newproc()：负责获取空闲的 G 或创建新的 G **-->** runtime.mstart() 启动调度循环；；

\10. 调度器的循环逻辑是：运行函数 schedule() **-->** 通过 runtime.globrunqget() 从全局队列、通过 runtime.runqget() 从 P 本地队列、 runtime.findrunnable 从各个地方，获取一个可执行的 G **-->** 调用 runtime.execute() 执行 G **-->** 调用 runtime.gogo() 在汇编代码层面上真正执行G --> 调用 runtime.goexit0() 执行 G 的清理工作，重新将 G 加入 P 的空闲队列 **-->** 调用 runtime.schedule() 进入下一次调度循环。



### 早期 GM 模型

为了解决传统内核级的线程的创建、切换、销毁开销较大的问题，Go 语言将线程分为了两种类型：内核级线程 M （Machine），轻量级的用户态的协程 Goroutine，至此，Go 语言调度器的三个核心概念出现了两个：

- **M**： Machine的缩写，代表了内核线程 OS Thread，CPU调度的基本单元；

- **G**： Goroutine的缩写，用户态、轻量级的协程，一个 G 代表了对一段需要被执行的 Go 语言程序的封装；每个 Goroutine 都有自己独立的栈存放自己程序的运行状态；分配的栈大小 2KB，可以按需扩缩容；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622864.jpeg)

在早期，Go 将传统线程拆分为了 M 和 G 之后，为了充分利用轻量级的 G 的低内存占用、低切换开销的优点，会在当前一个M上绑定多个 G，某个正在运行中的 G 执行完成后，Go 调度器会将该 G 切换走，将其他可以运行的 G 放入 M 上执行，这时一个 Go 程序中只有一个 M 线程：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622197.jpeg)

这个方案的优点是用户态的 G 可以快速切换，不会陷入内核态，缺点是每个 Go 程序都用不了硬件的多核加速能力，并且 G 阻塞会导致跟 G 绑定的 M 阻塞，其他 G 也用不了 M 去执行自己的程序了。

为了解决这些不足，Go 后来快速上线了多线程调度器：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622676.webp)

每个Go程序，都有多个 M 线程对应多个 G 协程，该方案有以下缺点：

- **全局锁、中心化状态带来的锁竞争导致的性能下降**； 
- M 会频繁交接 G，导致额外开销、性能下降；每个 M 都得能执行任意的 runnable 状态的 G； 
- 每个 M 都需要处理内存缓存，导致大量的内存占用并影响数据局部性； 
- 系统调用频繁阻塞和解除阻塞正在运行的线程，增加了额外开销；

### 高效 GMP 模型

为了解决多线程调度器的问题，Go 开发者 Dmitry Vyokov 在已有 G、M 的基础上，引入了 P 处理器，由此产生了当前 Go 中经典的 GMP 调度模型。

**P**：Processor的缩写，代表一个虚拟的处理器，它维护一个局部的可运行的 G 队列，可以通过 CAS 的方式无锁访问，工作线程 M 优先使用自己的局部运行队列中的 G，只有必要时才会去访问全局运行队列，这大大减少了锁冲突，提高了大量 G 的并发性。每个 G 要想真正运行起来，首先需要被分配一个 P。

如图 1.5 所示，是当前 Go 采用的 GMP 调度模型。可运行的 G 是通过处理器 P 和线程 M 绑定起来的，M 的执行是由操作系统调度器将 M 分配到 CPU 上实现的，Go 运行时调度器负责调度 G 到 M 上执行，主要在用户态运行，跟操作系统调度器在内核态运行相对应。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622393.webp)

需要说明的是，Go 调度器也叫 Go 运行时调度器，或 **Goroutine 调度器，指的是由运行时在用户态提供的多个函数组成的一种机制，目的是为了高效地调度 G 到 M上去执行**。可以跟操作系统的调度器 OS Scheduler 对比来看，后者负责将 M 调度到 CPU 上运行。从操作系统层面来看，运行在用户态的 Go 程序只是一个请求和运行多个线程 M 的普通进程，操作系统不会直接跟上层的 G 打交道。

至于 **为什么不直接将本地队列放在 M 上、而是要放在 P 上呢**？ 这是因为 **当一个线程 M 阻塞（可能执行系统调用或 IO请求）的时候，可以将和它绑定的 P 上的 G 转移到其他线程 M 去执行**，如果直接把可运行 G 组成的本地队列绑定到 M，则万一当前 M 阻塞，它拥有的 G 就不能给到其他 M 去执行了。

基于 GMP 模型的 Go 调度器的核心思想是：

1、**尽可能复用线程 M**：避免频繁的线程创建和销毁；

2、**利用多核并行能力**：限制同时运行（不包含阻塞）的 M 线程数为 N，N 等于 CPU 的核心数目，这里通过设置 P 处理器的个数为 GOMAXPROCS 来保证，GOMAXPROCS 一般为 CPU 核数，因为 M 和 P 是一一绑定的，没有找到 P 的 M 会放入空闲 M 列表，没有找到 M 的 P 也会放入空闲 P 列表；

3、**Work Stealing 任务窃取机制**：M 优先执行其所绑定的 P 的本地队列的 G，如果本地队列为空，可以从全局队列获取 G 运行，也可以从其他 M 偷取 G 来运行；为了提高并发执行的效率，M 可以从其他 M 绑定的 P 的运行队列偷取 G 执行，这种 GMP 调度模型也叫**任务窃取调度模型，**这里，任务就是指 G；

4、 **Hand Off 交接机制**：M 阻塞，会将 M 上 P 的运行队列交给其他 M 执行，交接效率要高，才能提高 Go 程序整体的并发度；

5、 **基于协作的抢占机制**：每个真正运行的G，如果不被打断，将会一直运行下去，为了保证公平，防止新创建的 G 一直获取不到 M 执行造成饥饿问题，Go 程序会保证每个 G 运行10ms 就要让出 M，交给其他 G 去执行；

6、 **基于信号的真抢占机制**：尽管基于协作的抢占机制能够缓解长时间 GC 导致整个程序无法工作和大多数 Goroutine 饥饿问题，但是还是有部分情况下，Go调度器有无法被抢占的情况，例如，for 循环或者垃圾回收长时间占用线程，为了解决这些问题， Go1.14 引入了基于信号的抢占式调度机制，能够解决 GC 垃圾回收和栈扫描时存在的问题。

### GMP 调度流程及场景

一个 goroutine 的调度流程大致如下：

![18-go-func调度周期.jpeg](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/https%2Fcdn.nlark.com%2Fyuque%2F0%2F2022%2Fjpeg%2F26269664-2024_01_15-1705329710.jpeg)

从上图我们可以分析出几个结论：

​	1、我们通过 go func()来创建一个goroutine；

​	2、有两个存储G的队列，一个是局部调度器P的本地队列、一个是全局G队列。新创建的G会先保存在P的本地队列中，如果P的本地队列已经满了就会保存在全局的队列中；

​	3、G只能运行在M中，一个M必须持有一个P，M与P是1：1的关系。M会从P的本地队列弹出一个可执行状态的G来执行，如果P的本地队列为空，就会想其他的MP组合偷取一个可执行的G来执行；

​	4、一个M调度G执行的过程是一个循环机制；

​	5、当M执行某一个G时候如果发生了syscall或则其余阻塞操作，M会阻塞，如果当前有一些G在执行，runtime会把这个线程M从P中摘除(detach)，然后再创建一个新的操作系统的线程(如果有空闲的线程可用就复用空闲线程)来服务于这个P；

​	6、当M系统调用结束时候，这个G会尝试获取一个空闲的P执行，并放入到这个P的本地队列。如果获取不到P，那么这个线程M变成休眠状态， 加入到空闲线程中，然后这个G会被放入全局队列中。





下面用几张图形象的描述下 GMP 调度机制的一些场景，帮助理解 GMP 调度器为了保证公平性、可扩展性、及提高并发效率，所设计的一些机制和策略。

1）**创建 G**： 正在 M1 上运行的P，有一个G1，通过go func() 创建 G2 后，由于局部性，G2优先放入P的本地队列；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622593.jpeg)



2）**G 运行完成后**：M1 上的 G1 运行完成后（调用goexit()函数），M1 上运行的 Goroutine 会切换为 G0，G0 负责调度协程的切换（运行schedule() 函数），从 M1 上 P 的本地运行队列获取 G2 去执行（函数execute()）；注意：这里 G0 是程序启动时的线程 M（也叫M0）的系统栈表示的 G 结构体，负责 M 上 G 的调度；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622012.webp)



3）**M 上创建的 G 个数大于本地队列长度时**：如果 P 本地队列最多能存 4 个G（实际上是256个），正在 M1 上运行的 G2 要通过go func()创建 6 个G，那么，前 4 个G 放在 P 本地队列中，G2 创建了第 5 个 G（G7）时，P 本地队列中前一半和 G7 一起打乱顺序放入全局队列，P 本地队列剩下的 G 往前移动，G2 创建的第 6 个 G（G8）时，放入 P 本地队列中，因为还有空间；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622144.webp)



4）**M 的自旋状态**：创建新的 G 时，运行的 G 会尝试唤醒其他空闲的 M 绑定 P 去执行，如果 G2 唤醒了M2，M2 绑定了一个 P2，会先运行 M2 的 G0，这时 M2 没有从 P2 的本地队列中找到 G，会进入自旋状态（spinning），自旋状态的 M2 会尝试从全局空闲线程队列里面获取 G，放到 P2 本地队列去执行，获取的数量满足公式：n = min(len(globrunqsize)/GOMAXPROCS + 1, len(localrunsize/2))，含义是每个P应该从全局队列承担的 G 数量，为了提高效率，不能太多，要给其他 P 留点；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622339.webp)



5）**任务窃取机制**：自旋状态的 M 会寻找可运行的 G，如果全局队列为空，则会从其他 P 偷取 G 来执行，个数是其他 P 运行队列的一半；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622879.webp)



6）**G 发生系统调用时**：如果 G 发生系统调度进入阻塞，其所在的 M 也会阻塞，因为会进入内核状态等待系统资源，和 M 绑定的 P 会寻找空闲的 M 执行，这是为了提高效率，不能让 P 本地队列的 G 因所在 M 进入阻塞状态而无法执行；需要说明的是，如果是 M 上的 G 进入 Channel 阻塞，则该 M 不会一起进入阻塞，因为 Channel 数据传输涉及内存拷贝，不涉及系统资源等待；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622791.webp)



7）**G 退出系统调用时**：如果刚才进入系统调用的 G2 解除了阻塞，其所在的 M1 会寻找 P 去执行，优先找原来的 P，发现没有找到，则其上的 G2 会进入全局队列，等其他 M 获取执行，M1 进入空闲队列；

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307251622031.webp)





## Channel

单纯地将函数并发执行是没有意义的。**函数与函数间需要交换数据才能体现并发执行函数的意义**。

虽然可以使用共享内存进行数据交换，但是 **共享内存在不同的 goroutine 中容易发生竞态问题**（对共享内存中的变量进行同步操作）。为了保证数据交换的正确性，很多并发模型中必须使用 **互斥量对内存进行加锁**，这种做法势必造成 **性能问题**。

Go 语言采用的并发模型是 CSP（Communicating Sequential Processes），提倡 **通过通信共享内存** 而不是 **通过共享内存来实现通信**。

如果说 **goroutine 是 Go 程序并发的执行体，`channel` 就是它们之间的连接**。**`channel`是可以让一个 goroutine 发送特定值到另一个 goroutine 的通信机制**。

Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循 **先进先出**（First In First Out）的规则，**保证收发数据的顺序**。每一个通道都是一个具体类型的导管，也就是声明 channel 的时候需要为其指定元素类型。



### channel 类型

`channel` 是 Go 语言中一种特有的类型。声明通道类型变量的格式如下：

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

初始化 channel：声明的通道类型变量需要使用内置的 `make` 函数初始化之后才能使用。具体格式如下：

```go
make(chan 元素类型, [缓冲大小])
```

其中，channel的缓冲大小是可选的。

举几个例子：

```go
ch4 := make(chan int)
ch5 := make(chan bool, 1)  // 声明一个缓冲区大小为1的通道
```

### channel 操作符 <-

**通道共有发送（send）、接收(receive）和关闭（close）三种操作**。而发送和接收操作都使用 `<-` 符号。

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

注意：`<-ch` 可以单独调用获取通道的（下一个）值，当前值会被丢弃，但是可以用来验证，所以以下代码是合法的：

```go
if <- ch != 1000 {
    ...
}

// 示例：
func main() {
	ch := make(chan int, 5)
	// channel 只能使用 <- 进行操作
	ch <- 10
	ch <- 20
	var x1 int
	// 从 ch 中读取一个数据，与 10 进行对比
	if <-ch != 10 {
		x1 = <-ch
	}
	x2 := <-ch
	close(ch)
	fmt.Printf("x1: %d, x2: %d\n", x1, x2)
	// x1: 0, x2: 20
}
```



### channel 关闭

通道可以被显式的关闭；尽管它们和文件不同：不必每次都关闭。**只有在当需要告诉接收者不会再提供新的值的时候，才需要关闭通道**，并且只有发送者需要关闭通道。

我们通过调用内置的 `close` 函数来关闭通道。

```go
close(ch)
```

关闭后的通道有以下特点：

1. 对一个关闭的通道再发送值就会导致 panic。
2. **对一个关闭的通道进行接收操作会一直获取值直到通道为空**。
3. **对一个关闭且没有值的通道执行接收操作会得到对应类型的零值**。
4. 关闭一个已经关闭的通道会导致 panic。

调用 channel 的 `close` 方法表示不再有新的值被发送到该 channel 中。在调用 `close` 方法时，会发生以下几件事情：

1. 该 channel 上的所有已经发送但 **尚未被接收的值都可以被接收**，接收操作会返回这些值以及一个表示该 channel 已关闭的标志。
2. 该 channel 上的所有后续的发送操作都会导致 panic。
3. 任何试图接收该 channel 的操作都将立即返回一个表示该 channel 已关闭的标志，以及一个默认值（对于 channel 元素类型的零值）。

总的来说，**调用 `close` 方法可以用来通知接收者该 channel 已经没有更多的值需要接收了，从而避免接收者在无需等待的情况下一直阻塞**。





### 无缓冲的通道（阻塞）

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

因为我们使用 `ch := make(chan int)` 创建的是无缓冲的通道，**无缓冲的通道只有在有接收方能够接收值的时候才能发送成功，否则会一直处于等待发送的阶段**。同理，如果对一个无缓冲通道执行接收操作时，没有任何向通道中发送值的操作那么也会导致接收操作阻塞。简单来说就是 **无缓冲的通道必须有至少一个接收方才能发送成功**。

上面的代码会阻塞在 `ch <- 10 `这一行代码形成死锁，那如何解决这个问题呢？

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

**首先无缓冲通道 `ch ` 上的发送操作会阻塞，直到另一个 goroutine  在该通道上执行接收操作，这时数字 10 才能发送成功**，两个 goroutine 将继续执行。相反，如果接收操作先执行，接收方所在的  goroutine 将阻塞，直到 main goroutine 中向该通道发送数字 10。

使用无缓冲通道进行通信将导致发送和接收的 goroutine 同步化。因此，**无缓冲通道也被称为 `同步通道`**。

注意，当使用阻塞通道时，很容易写出死锁代码，比如下面这个：

```go
package main

import (
    "fmt"
)

func f1(in chan int) {
    fmt.Println(<-in)
}

func main() {
    out := make(chan int)
    out <- 2
    go f1(out)
}
```

会在 `out <- 2` 处引发 deadlock，因为此处执行写入的时候，goroutine 还未启动，那么就没有 channel 的接受者，而此 channel 又无缓冲区，所以会一直阻塞在发送数据。正确的做法只需要将 goroutine 放到发数据前面即可。

再看看下面这个没有使用 goroutine 的例子，无论如何都会造成 deadlock：

```go
ch := make(chan int)
// 无论操作 a、b 的顺序如何，都会发生 deadlock，
// 因为这里的代码顺序是同步的，任何一个操作（发送/接收）都会阻塞，等另一个操作（接收/发送）
ch <- 10 // 操作 a
<-ch     // 操作 b
```



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

只要通道的容量大于零，那么该通道就属于有缓冲的通道，通道的容量表示通道中最大能存放的元素数量。**当通道内已有元素数达到最大容量后，再向通道执行发送操作就会阻塞，除非有从通道执行接收操作**。就像你小区的快递柜只有那么个多格子，格子满了就装不下了，就阻塞了，等到别人取走一个快递员就能往里面放一个。

所以缓冲满载（发送）或变空（接收）之前通信不会阻塞，元素会按照发送的顺序被接收。如果容量是 0 或者未设置，通信仅在收发双方准备好的情况下才可以成功。

我们可以使用内置的 `len `函数获取通道内元素的数量，使用 `cap` 函数获取通道的容量，虽然我们很少会这么做。



### 从通道中循环取值

#### 原始方法

我们定义一个通道的发送者和接受者，简单的进行收发操作，可能会写出如下代码：

```go
package main

import "fmt"

func send(ch chan int) {
	for i := 0; i < 10; i++ {
		ch <- i
	}
}

func receive(ch chan int) {
	var v int
	for {
		v = <-ch
		fmt.Println(v)
	}
}

func main() {
	ch := make(chan int, 10)
	send(ch)
	receive(ch)
}
```

输出：

```go
0
1
2
3
4
5
6
7
8
9
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.receive(0x0?)
        /Users/aarynlu/MyKB/GolangStudy/basics/concurrency/channel/channel_use.go:30 +0x2c
main.main()
        /Users/aarynlu/MyKB/GolangStudy/basics/concurrency/channel/channel_use.go:38 +0x5c

Process finished with the exit code 2
```

可以发现，在 receive 函数中发生了错误：`fatal error: all goroutines are asleep - deadlock!`。我们在上面也提到了这个错误，但是这里我们有缓冲区啊，而且也有发送者和接受者。为什么还会出现 deadlock 错误呢？

我们可以看到，channel 中的数据是被全部取出后才发生的错误，说明 **receive 函数中的 channel 在接收完所有数据后，还在向 channel 中取数据，而又没有发送者去发送数据了，所以会一直阻塞，Go 在运行时会检测到这种情况，所以就引发了 deadlock 错误**。

解决办法也很简单，我们在 **发送完数据后，将 channel 进行关闭**，在接收数据时，使用多返回值的方式，**可以接收到是否能正常读取数据的信号**，如果接收到 channel 关闭的信号，说明读取不到数据了，那么就会返回 false。实现代码如下：

```go
func send(ch chan int) {
	for i := 0; i < 10; i++ {
		ch <- i
	}
	close(ch)
}

func receive(ch chan int) {
	var v int
	var ok bool
	for {
		if v, ok = <-ch; !ok {
			break
		}
		fmt.Println(v)
	}
}

func main() {
	ch := make(chan int, 10)
	send(ch)
	receive(ch)
}
```

此时便可以正常打印输出了。这是因为 **当一个 channel 被关闭时，还有一些数据留在 channel 的缓冲区中，那么这些数据可以被读取到；如果缓冲区已经被读空了，那么读取操作会返回该 channel 的零值，同时返回一个标志位表明该 channel 已经被关闭**。

#### 推荐方法

当通过通道发送有限的数据时，我们可以 **通过 close 函数关闭通道来告知从该通道接收值的 goroutine 停止等待**。当通道被关闭时，往该通道发送值会引发 panic，从该通道里接收的值一直都是类型零值。

那如何判断一个通道是否被关闭了呢？下面示例给出两种方法：

```go
package main

import "fmt"

// 判断 chan 是否关闭方式一：使用多值返回模式
func channelIsCloseByMultiRet(ch chan int) {
	for {
		v, ok := <-ch
		if !ok {
			fmt.Println("channel closed")
			break
		}
		fmt.Printf("v: %#v, ok: %#v\n", v, ok)
	}
	/*
		v: 1, ok: true
		v: 2, ok: true
		channel closed
	*/
}

// 判断 chan 是否关闭方式二：使用 for-range 来遍历 chan
func channelIsCloseByForRange(ch chan int) {
     // 循环将一直阻塞，直到 channel 被关闭
	for v := range ch {
		fmt.Println(v)
	}
	fmt.Println("channel closed")
	/*
		1
		2
		channel closed
	*/
}

func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	close(ch)
	//channelIsCloseByMultiRet(ch)
	channelIsCloseByForRange(ch)
}
```

在使用 **for-range** 时：**如果 channel 中没有数据可用，则循环将阻塞，直到有数据可用或者 channel 被关闭。channel 被关闭后则会退出循环**。

**注意**：目前 Go 语言中并 **没有提供一个不对通道进行读取操作就能判断通道是否被关闭的方法。不能简单的通过 `len(ch)` 操作来判断通道是否被关闭**。因为通道的收发都是阻塞的，所以可能数据还在传输过程中。



### 单向通道

在某些场景下我们可能会将通道作为参数在多个任务函数间进行传递，通常我们会选择在不同的任务函数中对通道的使用进行限制，比如 **限制通道在某个函数中只能执行发送或只能执行接收操作**。

Go 语言中提供了**单向通道** 来处理这种需要限制通道只能进行某种操作的情况：

```go
<- chan int 	// 只接收通道，只能接收不能发送
chan <- int 	// 只发送通道，只能发送不能接收
```

其中，箭头`<-`和关键字`chan`的相对位置表明了当前通道允许的操作，这种限制将在编译阶段进行检测。另外 **对一个只接收通道执行 close 也是不允许的**，因为默认通道的关闭操作应该由发送方来完成。

示例：

```go
package main

import "fmt"

// 只进行发送的 chan
func counter(ch chan<- int) {
	for i := 0; i < 100; i++ {
		ch <- i
	}
	close(ch) // 发送完毕 -> close
}

// 从 ch1 中取数据，发送到 ch2 中。即只进行接收的 ch1，只进行发送的 ch2
func squarer(ch1 <-chan int, ch2 chan<- int) {
	for v := range ch1 {
		ch2 <- v * v
	}
	close(ch2)
}

// 打印 chan 中的数据，即只进行接收的 chan
func printer(ch <-chan int) {
	for v := range ch {
		fmt.Print(v, " ")
	}
	fmt.Println()
}

func main() {
	ch1 := make(chan int, 100)
	ch2 := make(chan int, 100)
	go counter(ch1)
	go squarer(ch1, ch2)
	printer(ch2)
}
```

运行测试，成功输出 0-100 平方后的数值。



### 总结

![image-20220507194919278](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ab1eb1083f8cada8dfa0d90fcac49c57.png)

**注意：**对已经关闭的通道再执行 close 也会引发 panic。

### 信号量模式

当我们开启一个协程去做某任务时，有时需要监控，等到该任务被处理完后，main 协程才继续下面的操作。

这个模式就类似于信号量，让协程完成任务后，改变信号量，让 main 协程知晓。

使用 channel 的阻塞模式可以很方便的实现：

```go
package main

import (
	"fmt"
	"time"
)

func doTask(ch chan int) {
	ch <- task() // task() 任务完成后，向 chan 中发送信号
}

func task() int {
	fmt.Println("doing task")
	time.Sleep(time.Second * 3)
	fmt.Println("task done")
	return 1
}

func main() {
	ch := make(chan int)
	go doTask(ch)
	// ......
	fmt.Println("main wait ~~")
	<-ch                           // 阻塞读取 chan
	fmt.Println("after operation") // 后续操作...

	/*
		main wait ~~
		doing task
		task done
		after operation
	*/
}
```

这个信号也可以是其他的，不返回结果，比如下面这个协程中的匿名函数（lambda）协程：

```go
    ch := make(chan int)
    go func() {
      task()
      ch <- 1
    }()

    <- ch
```

或者等待两个协程完成，每一个都会对切片 s 的一部分进行排序，片段如下：

```go
// 等待两个协程完成任务
func waitTowGoroutine() {
	done := make(chan bool)
	// 定义一个做排序的函数，调用完 sort() 后将一个数据塞入 chan
	doSort := func(s []int) {
		sort.Ints(s)
		done <- true
	}
	sli := []int{2, 1, 8, 7, 9}
	i := 2
	go doSort(sli[:i])
	go doSort(sli[i:])
	fmt.Println("maybe not sorted absolutely: ", sli)
	<-done
	<-done
	fmt.Println("sorted absolutely: ", sli)

	/*
		在信号量到达之前输出 sli，可能会出现还没全部排好序的情况
		maybe not sorted absolutely:  [1 2 8 7 9]
		sorted absolutely:  [1 2 7 8 9]
	*/
}
```

一般常用的是等待 N 个任务，例如，用完整的信号量模式对长度为 N 的 float64 切片进行了 N 个 `doSomething()` 计算并同时完成，通道 sem 分配了相同的长度（且包含空接口类型的元素），待所有的计算都完成后，发送信号（通过放入值）。在循环中从通道 sem 不停的接收数据来等待所有的协程完成。

```go
// 等待 N 个协程完成任务
const N = 20
type Empty interface{}
var empty Empty
func doSomething(i int, xi float64) float64 { return 0 }

func waitNGoroutine() {
	data := make([]float64, N)
	res := make([]float64, N)
	sem := make(chan Empty, N)

	// range data, let goroutine run the task
	for i, xi := range data {
		/*
			注意：这里需要拷贝一份 i，xi 传入匿名函数，再让 goroutine 执行，否则就会形成闭包，
			这样外部 i/xi 改变时，会影响到 goroutine 的值，所以需要和外部变量隔离。
			而 res 变量不用拷贝一份给 goroutine，因为它是共享的，保证 i 不受影响即可。
		*/
		go func(i int, xi float64) {
			res[i] = doSomething(i, xi)
			sem <- empty
		}(i, xi)
	}

	// wait for goroutine to finish
	for i := 0; i < N; i++ {
		<-sem
	}
}
```

注意闭合：i、xi 都是作为参数传入闭合函数的，从外层循环中隐藏了变量 i 和 xi。让每个协程有一份 i 和 xi 的拷贝。另外，切片 res 没有传入闭合函数，因为协程不需要单独拷贝一份切片，res 也在闭合函数中但并不是参数。

上面的 for 循环中，每次循环都开启一个 goroutine，即实现了 **并行的 for 循环**，带来了很好的性能提升，不过要所有的迭代必须是独立完成的。

还可以利用带缓冲的 channel 实现 PV 操作，然后实现互斥，用于限制资源的访问：

- 带缓冲 channel 的容量和同步的资源容量相同；
- channel 的长度（当前存放的元素个数）与当前资源被使用的数量相同；
- 容量减去长度就是未处理的资源个数。

```go
type Empty interface {}
type semaphore chan Empty
```

将可用资源的数量 N 来初始化信号量 semaphore：`sem := make(semaphore, N)`

然后直接对信号量进行操作：

```go
// acquire n resources
func (s semaphore) P(n int) {
    e := new(Empty)
    for i := 0; i < n; i++ {
        s <- e
    }
}

// release n resouces
func (s semaphore) V(n int) {
    for i:= 0; i < n; i++{
        <- s
    }
}
```

可以用来实现一个互斥的例子：

```go
/* mutexes */
func (s semaphore) Lock() {
    s.P(1)
}

func (s semaphore) Unlock(){
    s.V(1)
}

/* signal-wait */
func (s semaphore) Wait(n int) {
    s.P(n)
}

func (s semaphore) Signal() {
    s.V(1)
}
```



## select 多路复用

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

这种方式虽然可以实现从多个通道接收值的需求，但是程序的运行性能会差很多。**Go 语言内置了 `select` 关键字，使用它可以同时响应多个通道的操作**。

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
- 如果多个 case 同时满足，select 会 **随机** 选择一个执行。
- 对于没有 case 的 select 会一直阻塞，可用于阻塞 main 函数，防止退出。

下面的示例代码能够在终端打印出 10 以内的奇数，我们借助这个代码片段来看一下 select 的具体使用：

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

初始 i=1 时通道中没有值，执行第 2 个 case；i=2 时通道中有一个 1，执行第 1 个 case，如此反复，就只会把奇数打印出：

```bash
1
3
5
7
9
```



下面再举一个典型的生产者-消费者模式：有 2 个通道 ch1 和 ch2，三个协程 `pump1()`、`pump2()` 和 `suck()`。在无限循环中，ch1 和 ch2 通过 `pump1()` 和 `pump2()` 填充整数；`suck()` 也是在无限循环中轮询输入的，通过 select 语句获取 ch1 和 ch2 的整数并输出。选择哪一个 case 取决于哪一个通道收到了信息。

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)

	go pump1(ch1)
	go pump2(ch2)
	go suck(ch1, ch2)

	time.Sleep(time.Millisecond)
}

func pump1(ch chan int) {
	for i := 0; ; i++ {
		ch <- i + 1
	}
}

func pump2(ch chan int) {
	for i := 0; ; i++ {
		ch <- i + 100
	}
}

func suck(ch1, ch2 chan int) {
	for {
		select {
		case v := <-ch1:
			fmt.Printf("received on channel 1: %d\n", v)
		case v := <-ch2:
			fmt.Printf("received on channel 2: %d\n", v)
		}
	}
}
```

输出：

```text
received on channel 2: 100
received on channel 1: 1
received on channel 2: 101
received on channel 2: 102
received on channel 1: 2
received on channel 2: 103
received on channel 2: 104
received on channel 1: 3
received on channel 1: 4
received on channel 1: 5
received on channel 2: 105
received on channel 1: 6
......
```



## Context 上下文

Context 的主要目的是为了解决在并发编程中的一些问题，比如在一个请求处理过程中，可能会启动多个 goroutine 来处理不同的任务，这些任务可能需要共享一些上下文信息，比如请求的截止时间、请求的 ID 等等。如果没有一个统一的机制来管理这些上下文信息，那么代码就会变得非常混乱，难以维护。 通过引入 context，开发者可以将这些上下文信息封装在一个 context 对象中，并将其传递给各个 goroutine，从而实现了上下文信息的统一管理。这样一来，代码就变得更加清晰、易于理解和维护了。

**上下文 `context.Context`** 是 Go 语言中用来设置 **截止日期、同步信号，传递请求相关值** 的结构体。上下文与 Goroutine 有比较密切的关系，是 Go 语言中独特的设计。

`context.Context` 是 Go 语言在 1.7 版本中引入标准库的接口，该接口定义了四个需要实现的方法，其中包括：

- `Deadline()` — 返回 `context.Context` 被取消的时间，也就是完成工作的截止日期；
- `Done()` — 返回一个 Channel，这个 Channel 会在当前工作完成或者上下文被取消后关闭，多次调用 `Done` 方法会返回同一个 Channel；
- `Err()` — 返回 `context.Context` 结束的原因，它只会在 `Done` 方法对应的 Channel 关闭时返回非空的值（Channel 未关闭则返回 nil）：
  - 如果 `context.Context` 被取消，会返回 `Canceled` 错误；
  - 如果 `context.Context` 超时，会返回 `DeadlineExceeded` 错误；
- `Value()` — 从 `context.Context` 中获取键对应的值，对于同一个上下文来说，多次调用 Value 并传入相同的 Key 会返回相同的结果，该方法可以用来传递请求特定的数据；

```go
type Context interface {
	Deadline() (deadline time.Time, ok bool)
	Done() <-chan struct{}
	Err() error
	Value(key interface{}) interface{}
}
```

`context` 包中提供的 `context.Background`、`context.TODO`、`context.WithDeadline` 和 `context.WithValue` 函数会返回实现该接口的私有结构体，可以直接拿来使用。

### 使用 Context 同步信号

我们可以通过一个代码片段了解 `context.Context` 是如何对信号进行同步的。在这段代码中，我们创建了一个过期时间为 1s 的上下文，并向上下文传入 `handle` 函数，该方法会使用 500ms 的时间处理传入的请求：

```go
func main() {
	// 创建一个过期时间为 1s 的 context
	ctx, cancel := context.WithTimeout(context.Background(), 1*time.Second)
	defer cancel()

	// 向 ctx 中传入 handle 函数
	go handle(ctx, 500*time.Millisecond)
	// select 轮询 ctx.Done() channel
	select {
	case <-ctx.Done():
		fmt.Println("func-main ctx.Err:", ctx.Err())
	}
}

// handle 函数使用 500ms 处理传入的请求
func handle(ctx context.Context, duration time.Duration) {
	// select 轮询 ctx.Done() channel 和 time.After() channel
	select {
	case <-ctx.Done():
		fmt.Println("func-handle ctx.Err:", ctx.Err())
	// 模拟在 duration 时间已完成请求
	case <-time.After(duration):
		fmt.Println("finished: process request with", duration)
	}
}
```

在上面的代码中，handle 函数使用了select 语句来同时监听 `ctx.Done()` channel 和 `time.After()` channel。这里如果 `time.After(duration)` 先被选中了，则 handle 函数会在规定的时间内处理请求，而不会接收到 `ctx.Done()` channel 的消息。

因为过期时间大于处理时间，所以我们有足够的时间处理该请求。运行上述代码会打印出下面的内容：

```text
finished: process request with 500ms
func-main ctx.Err: context deadline exceeded
```

`handle` 函数没有进入超时的 selec 分支，但是 `main` 函数的 select 却会等待 `context.Context` 超时并打印出 `main context deadline exceeded`。

而如果在规定时间内请求没有被处理完成，那么 `ctx.Done()` channel 就会被关闭，此时 `ctx.Err()` 将返回一个 `context.DeadlineExceeded` 错误。比如我们将处理请求时间增加至 1500ms，整个程序都会因为上下文的过期而被中止：

```text
func-main ctx.Err: context deadline exceeded
func-handle ctx.Err: context deadline exceeded
```

通过上面的两个例子，可以发现 `context.Context` 其实是 **多个 Goroutine 同时订阅 `ctx.Done()` channel 中的消息，一旦接收到取消信号就立刻停止当前正在执行的工作**。

在使用 `select` 语句监听 context 的取消信号时，可以使用 `default` 分支来执行一些默认操作，而不必担心会错过取消信号。即使在执行 `default` 分支的过程中，`select` 语句仍然会重新监听 `ctx.Done()` 方法返回的通道（select 是阻塞监听多个管道），并在下一次取消信号到来时触发相应的处理。

### 默认 Context

`context` 包中最常用的方法还是 `context.Background`、`context.TODO`，这两个方法都会返回预先初始化好的私有变量 `background` 和 `todo`，它们会在同一个 Go 程序中被复用：

```go
func Background() Context {
	return background
}

func TODO() Context {
	return todo
}
```

我们代码中最开始都是以这两个内置的上下文对象作为最顶层的 `partent context`，衍生出更多的子上下文对象。

这两个私有变量都是通过 `new(emptyCtx)` 语句初始化的，它们是指向私有结构体 `context.emptyCtx` 的指针，这是最简单、最常用的上下文类型：

```go
type emptyCtx int

func (*emptyCtx) Deadline() (deadline time.Time, ok bool) {
	return
}

func (*emptyCtx) Done() <-chan struct{} {
	return nil
}

func (*emptyCtx) Err() error {
	return nil
}

func (*emptyCtx) Value(key interface{}) interface{} {
	return nil
}
```

从上述代码中，我们不难发现 `context.emptyCtx` 通过空方法实现了 `context.Context` 接口中的所有方法，它没有任何功能，是一个不可取消，没有设置截止时间，没有携带任何值的 Context。

从源代码来看，`context.Background` 和 `context.TODO` 也只是互为别名，没有太大的差别，只是在使用和语义上稍有不同：

- `context.Background` 是上下文的默认值，所有其他的上下文都应该从它衍生出来。主要用于 main 函数、初始化以及测试代码中，作为 Context 这个树结构的最顶层的 Context，也就是根 Contex；
- `context.TODO` 目前还不知道具体的使用场景，如果我们不知道该使用什么 Context 的时候，可以使用这个；

![image-20230503141231410](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305031412928.png)

在多数情况下，如果当前函数没有上下文作为入参，我们都会使用 `context.Background` 作为起始的上下文向下传递。



### With 系列函数

`context` 包中定义了四个 With 系列函数，来方便我们使用。

#### WithCancel

`context.WithCancel()` 用于创建一个可取消的 context。它的签名如下：

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
```

该函数接收一个 `parent context` 作为参数，并返回一个新的 context 对象 `ctx` 和一个 `cancel` 函数。`ctx` 继承了 `parent` 的值，但是可以 **通过调用 `cancel` 函数来取消该 context**。

当调用返回的 `cancel` 函数或者关闭 `parent context` 的 `Done` 通道时，所有阻塞在该 channel 上的 goroutine 都会收到一个 `context.Canceled` 错误，从而可以做一些清理工作并退出。

```go
func main() {
	// 创建一个可取消的 context 和 cancel 函数
	ctx, cancel := context.WithCancel(context.Background())

	// 启动一个 goroutine 并传入 context
	go task(ctx)

	// 等待一段时间后取消 context
	time.Sleep(3 * time.Second)
	cancel()

	// 等待一段时间后退出程序
	time.Sleep(1 * time.Second)
	fmt.Println("main is exited.")
}

func task(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("goroutine is cancelled, it will return soon.")
			return
		default:
			fmt.Println("goroutine is running ~~~")
			time.Sleep(time.Second)
		}
	}
}
```

在上面的代码中，我们使用 `context.WithCancel()` 创建了一个可取消的 context，并将其传递给了一个 goroutine。在主函数中，我们等待 3s 后调用 `cancel` 函数来取消 context，然后等待 1s 后退出程序。运行上面的代码，可以看到在调用 `cancel` 函数后，goroutine 中的循环会被中止：

```text
goroutine is running ~~~
goroutine is running ~~~
goroutine is running ~~~
goroutine is cancelled, it will return soon.
main is exited.
```

#### WithDeadline

`context.WithDeadline()` 用于创建一个带有截止时间的 context。它的签名如下：

```go
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
```

该函数接收一个 `parent context` 作为参数和一个截止时间 `deadline`，并返回一个新的 context 对象和一个 `cancel` 函数。

**该函数创建的 context 会在 `deadline` 时间到达或 context 被取消时自动取消**：

- 当 context 被取消时（调用返回的 `cancel` 函数），`ctx.Done()` channel 会被关闭，所有阻塞在该 channel 上的 goroutine 都会收到一个 `context.Canceled` 错误；
- 当 `deadline` 时间到达时，`ctx.Done()` channel 也会被关闭，所有阻塞在该 channel 上的 goroutine 都会收到一个 `context.DeadlineExceeded` 错误。

这两种情况下，可以通过调用 `ctx.Err()` 来获取 context 取消的原因。

例如，下面的代码演示了如何使用 `context.WithDeadline()` 创建一个带有截止时间的 context 并在其中启动一个 goroutine：

```go
func main() {
	// 创建一个带有截止时间(当前时间 + 3s)的 context 和 cancel 函数
	ctx, cancel := context.WithDeadline(context.Background(), time.Now().Add(3*time.Second))

	// 启动一个 goroutine 并传入 context
	go task2(ctx)

	// 等待一段时间(5s)后取消 context
	time.Sleep(5 * time.Second)
	cancel()

	// 等待一段时间后退出程序
	time.Sleep(time.Second)
	fmt.Println("main is exited.")
}

func task2(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("goroutine is canceled due to", ctx.Err())
			return
		default:
			fmt.Println("goroutine is running ~~~")
			time.Sleep(time.Second)
		}
	}
}
```

由于我们创建的是 3s 后截止的 context，而我们最后是等待 5s 才调用 `cancel` 函数取消 context，所以上面代码会由于到达截止时间而导致 context 退出：

```text
goroutine is running ~~~
goroutine is running ~~~
goroutine is running ~~~
goroutine is canceled due to context deadline exceeded
main is exited.
```

如果我们把最后的等待时间 5s 调整到 2s，那么就会由于执行了 `cancel` 函数而导致 context 退出：

```text
goroutine is running ~~~
goroutine is running ~~~
goroutine is canceled due to context canceled
main is exited.
```

> 注意：调用 `cancel` 函数取消 context 会释放与其相关的资源，因此代码中应该在此 context 中的操作完成后立即调用 `cancel`，配合 `defer` 使用。

#### WithTimeout

`context.WithTimeout()` 与 `context.WithDeadline()` 类似，只不过是用于创建一个带有超时时间的 context。它的签名如下：

```go
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
```

该函数接收一个 `parent context` 作为参数和一个超时时间 `timeout`，并返回一个新的 context 对象和一个 `cancel` 函数。

**该函数创建的 context 会在超时时间到达或 context 被取消时自动取消**：

- 当 context 被取消时，`ctx.Done()` channel 会被关闭，所有阻塞在该 channel 上的 goroutine 都会收到一个 `context.Canceled` 错误；
- 当超时时间到达时，`ctx.Done()` channel 也会被关闭，所有阻塞在该 channel 上的 goroutine 都会收到一个 `context.DeadlineExceeded` 错误。

这两种情况下，可以通过调用 `ctx.Err()` 来获取 context 取消的原因。

通常用于数据库或者网络连接的超时控制。具体示例如下，如果超时时间到达或者 context 被取消，就及时退出数据库连接并返回错误信息，从而避免长时间的等待和阻塞：

```go
var wg sync.WaitGroup

func main() {
	// 创建一个 50ms 超时的 context
	ctx, cancel := context.WithTimeout(context.Background(), time.Millisecond*50)
	defer cancel() // 通知子 goroutine 结束
	wg.Add(1)

	var ok bool
	var connection any
	go func(ctx context.Context) {
		connection, ok = connectToDB(ctx)
		wg.Done()
	}(ctx)

	// 启动另一个 goroutine 去等待数据库连接完成，否则 main goroutine 会阻塞在 wg.Wait()
	go func() {
		wg.Wait()
		if ok {
			fmt.Println("connect success, connection is:", connection)
		} else {
			fmt.Println("connect fail, connection is:", connection)
		}
	}()

	time.Sleep(time.Second) // 防止 main goroutine 结束太快
}

func connectToDB(ctx context.Context) (connection any, ok bool) {
	fmt.Println("db connecting...")
	time.Sleep(time.Millisecond * 20) // 模拟数据库连接
	select {
	case <-ctx.Done():
		fmt.Println("goroutine is cancel due to", ctx.Err())
		connection = nil
		ok = false
	default:
		fmt.Println("db connected!")
		connection = "my-db-connection"
		ok = true
	}
	return
}
```

在上面的代码中，我们创建了一个超时时间为 50ms 的 context（也就是允许连接数据库的最长耗时为 50ms），然后传入 connectToDB 函数中，执行数据库连接如果在 50ms 时间内完成，则会返回连接成功：

```text
db connecting...
db connected!
connect success, connection is: my-db-connection
```

当把数据库连接设置为 60ms，则会返回连接失败：

```text
db connecting...
goroutine is cancel due to context deadline exceeded
connect fail, connection is: <nil>
```

#### WithValue

`context.WithValue()` 用于创建一个新的 `Context` 对象，该对象包含了一个 key-value 键值对。这个函数的签名如下：

```go
func WithValue(parent Context, key, val interface{}) Context
```

其中，`parent` 参数是一个已有的 `Context` 对象，`key` 参数和 `val` 参数是任意类型的值，用于表示一个键值对。`WithValue()` 函数会返回一个新的 `Context` 对象，该对象会继承 `parent` 对象的 deadline、cancelation 等属性，同时保存一个额外的键值对信息。

这个函数的作用是在一个 `Context` 对象中添加一些额外的信息，这些信息可以在整个 `Context` 对象的生命周期内传递和访问。这个函数主要用于在一个 `Context` 对象中传递上下文信息，比如请求 ID、用户身份等。

下面是一个简单的示例，展示了如何使用 `context.WithValue()` 来传递请求 ID：

```go
func main() {
	ctx := context.WithValue(context.Background(), "requestID", "123456")
	// 在另一个 goroutine 函数中使用 Context 对象
	go doSomething(ctx)

	time.Sleep(time.Millisecond)
}

func doSomething(ctx context.Context) {
	// 从 Context 对象中获取请求 ID
	requestID := ctx.Value("requestID").(string)
	fmt.Println("Request ID:", requestID)
}

// 输出：Request ID: 123456
```

需要注意的是，`ctx.Value()` 函数返回的是一个空接口类型，因此我们需要使用类型断言将其转换为实际的类型。如果在 `Context` 对象中没有找到指定键的值，则 `ctx.Value()` 函数会返回 `nil`。

## 并发安全和锁

有时候我们的代码中可能会存在多个 goroutine 同时操作一个资源（临界区）的情况，这种情况下就会发生 `竞态问题`（数据竞态）。这就好比现实生活中十字路口被各个方向的汽车竞争，还有火车上的卫生间被车厢里的人竞争。

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

**`go test` 命令会遍历所有的`*_test.go`文件中符合上述命名规则的函数**，然后生成一个临时的 main包 用于调用相应的测试函数，然后构建并运行、报告测试结果，最后清理测试中生成的临时文件。

Golang 单元测试对 **文件名和方法名，参数** 都有很严格的要求：

- 文件名必须以 `xx_test.go` 命名
- 方法必须是 `Test[^a-z]` 开头
- **方法参数必须 `t *testing.T`**
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

### Goland 自动生成测试

其实，如果使用的是 Goland 则并不用编写上面的测试代码，可以一键生成，只需要自己填写测试用例和想要得到的结果即可，在 Mac 中的快捷键为 `control + enter`：

![image-20230426113026023](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304261130801.png)

选择为整个包生成测试代码，就会自动生成一个测试文件，只需要在 TODO 中添加测试用例即可：

```go
package test

import (
	"reflect"
	"testing"
)

func TestSplit(t *testing.T) {
	type args struct {
		s   string
		sep string
	}
	tests := []struct {
		name       string
		args       args
		wantResult []string
	}{
		// TODO: Add test cases.
		{
			name: "字符串切割测试 1",
			args: struct {
				s   string
				sep string
			}{s: "this is bad good bad.", sep: "bad"},
			wantResult: []string{"this is ", " good ", "."},
		},
		{
			name: "字符串切割测试 2",
			args: args{
				s:   "abbcdibbdis",
				sep: "bb",
			},
			wantResult: []string{"a", "cdi", "dis"},
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			// 因为 slice 不能比较直接，借助反射包中的 DeepEqual 方法比较
			if gotResult := Split(tt.args.s, tt.args.sep); !reflect.DeepEqual(gotResult, tt.wantResult) {
				t.Errorf("Split() = %#v, want %#v", gotResult, tt.wantResult)
			}
		})
	}
}
```

输出：

![image-20230426113206195](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202304261132248.png)



### 测试覆盖率

测试覆盖率是你的代码被测试套件覆盖的百分比。通常我们使用的都是语句的覆盖率，也就是在测试中至少被运行一次的代码占总代码的比例。

Go提供内置功能来检查你的代码覆盖率。我们可以使用`go test -cover`来查看测试覆盖率。例如：

![image-20220504200047991](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bdba802cb3e118eb6d5eceda649b92bc.png)

从上面的结果可以看到我们的测试用例覆盖了100%的代码。

Go还提供了一个额外的`-coverprofile`参数，用来将覆盖率相关的记录信息输出到一个文件。



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

3. 示例函数提供了可以直接运行的示例代码，可以直接在`golang.org`的`godoc`文档服务器上使用`Go Playground`运行示例代码。下图为`strings.ToUpper`函数在Playground的示例函数效果。

   ![Go Playground](https://www.liwenzhou.com/images/Go/unit_test/example.png) 
