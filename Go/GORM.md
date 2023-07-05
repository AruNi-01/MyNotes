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

在使用 ORM 工具时，通常我们需要在代码中 **定义模型（Models）与数据库中的数据表进行映射**，在 GORM 中模型（Models）通常是正常定义的结构体、基本的 go 类型或它们的指针。 同时也支持 `sql.Scanner` 及 `driver.Valuer` 接口（interfaces）。

### 约定

GORM 倾向于约定优于配置，默认情况下，GORM 使用 `ID` 作为主键，使用结构体名的 `蛇形复数` 作为表名，字段名的 `蛇形` 作为列名，并使用 `CreatedAt`、`UpdatedAt` 字段追踪创建、更新时间。

如果您遵循 GORM 的约定，您就可以少写的配置、代码。 如果约定不符合您的实际要求，[GORM 允许你配置它们](https://gorm.io/zh_CN/docs/conventions.html)。

### gorm.Model

为了方便，GORM 已经定义好了一个 `gorm.Model` 结构体，其包括字段 `ID`、`CreatedAt`、`UpdatedAt`、`DeletedAt`

```go
// gorm.Model 的定义
type Model struct {
  ID        uint           `gorm:"primaryKey"`
  CreatedAt time.Time
  UpdatedAt time.Time
  DeletedAt gorm.DeletedAt `gorm:"index"`
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



对于匿名字段，GORM 会将其字段包含在父结构体中，例如：

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

对于正常的结构体字段，你也可以通过标签 `embedded` 将其嵌入，例如：

```go
type Author struct {
    Name  string
    Email string
}

type Blog struct {
  ID      int
  Author  Author `gorm:"embedded"`
  Upvotes int32
}
// 等效于
type Blog struct {
  ID    int64
  Name  string
  Email string
  Upvotes  int32
}
```

并且，您可以使用标签 `embeddedPrefix` 来为 db 中的字段名添加前缀，例如：

```go
type Blog struct {
  ID      int
  Author  Author `gorm:"embedded;embeddedPrefix:author_"`
  Upvotes int32
}
// 等效于
type Blog struct {
  ID          int64
  AuthorName string
  AuthorEmail string
  Upvotes     int32
}
```



### 结构体标记（tags）

使用结构体声明模型时，标记（tags）是可选项。gorm 支持以下标记：

#### 支持的结构体标记（Struct tags）

| 结构体标记（Tag） | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| Column            | 指定列名                                                     |
| Type              | 指定列数据类型                                               |
| Size              | 指定列大小, 默认值255                                        |
| PRIMARY_KEY       | 将列指定为主键                                               |
| UNIQUE            | 将列指定为唯一                                               |
| DEFAULT           | 指定列默认值                                                 |
| PRECISION         | 指定列精度                                                   |
| NOT NULL          | 将列指定为非 NULL                                            |
| AUTO_INCREMENT    | 指定列是否为自增类型                                         |
| INDEX             | 创建具有或不带名称的索引, 如果多个索引同名则创建复合索引     |
| UNIQUE_INDEX      | 和 `INDEX` 类似，只不过创建的是唯一索引                      |
| EMBEDDED          | 将结构设置为嵌入                                             |
| EMBEDDED_PREFIX   | 设置嵌入结构的前缀                                           |
| -                 | 忽略此字段                                                   |
| check             | 创建检查约束，例如 `check:age > 13`，查看 [约束](https://gorm.io/zh_CN/docs/constraints.html) 获取详情 |
| <-                | 设置字段写入的权限， `<-:create` 只创建、`<-:update` 只更新、`<-:false` 无写入权限、`<-` 创建和更新权限 |
| ->                | 设置字段读的权限，`->:false` 无读权限                        |
| comment           | 迁移时为字段添加注释                                         |

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

也可以通过 `Table()` 指定表名：

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

**注意：所有字段的零值, 比如`0`, `""`,`false`或者其它`零值`，都不会保存到数据库内，但会使用它们的默认值**。 

如果想避免这种情况，可以使用 **指针 **或 **实现`Scanner/Valuer`接口** 的方式 **将零值存入数据库表中**，比如：

**结构体属性使用指针**：

```go
// User 1.定义模型
type User struct {
	ID   int64
	Name string
        // 使用指针
	Age  *int64 `gorm:"default:'18'"`
}
u := User{Name: "AruNi", Age: new(int64)}	// Age 需要使用 new
db.Create(&u)  // 此时数据库中该条记录age字段的值就是 0
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

所以在项目中，如果使用 `First()` 时想让 record 为 0 时不报错，则需要提前捕获一下 `ErrRecordNotFound` 错误，将 err 置为 nil。就像下面这样：

```go
result := db.First(&user)
if errors.Is(result.Error, gorm.ErrRecordNotFound) {
    result.Error = nil
}
```



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



##### Struct & Map 条件查询

```go
// Struct
var user User
db.Where(&User{Name: "AruNi", Age: 18}).First(&user)
// SELECT * FROM users WHERE name = "AruNi" AND age = 18 LIMIT 1;

// Map
var users []User
db.Where(map[string]interface{}{"name": "AruNi", "age": 18}).Find(&users)
// SELECT * FROM users WHERE name = "AruNi" AND age = 18;

// 主键的切片
db.Where([]int64{2, 4, 5}).Find(&users)
// SELECT * FROM users WHERE id IN (2, 4, 5);
```



**提示**：

当通过结构体进行查询时，GORM 将会只通过 **非零值字段查询**，这意味着如果你的字段值为 `0`，`''`，`false` 或者其他 **`零值`** 时，**将不会被用于构建查询条件**，例如：

```go
var users []User
db.Where(&User{Name: "AruNi", Age: 0}).Find(&users)
// SELECT * FROM users WHERE name = "AruNi";
```

如果 **要在查询时包含零值**:

- 可以使用 map，它将包含所有 k-v 作为查询条件：

  ```go
  db.Where( map[string]interface{}{ "Name" : "AruNi" , "Age" : 0 }).Find(&users) 
  // SELECT * FROM users WHERE name = "AruNi" AND age = 0;
  ```

- 你也可以使用指针或实现 `Scanner/Valuer` 接口来避免这个问题：

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



#### Or 条件

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

使用 `Set()` 方法设置额外选项：

```go
// 为查询 SQL 添加额外的 SQL 操作
db.Set("gorm:query_option", "FOR UPDATE").First(&user, 1)
// SELECT * FROM users WHERE id = 1 FOR UPDATE;
```

`FOR UPDATE`：锁定查出来的这些行，别的会话不能修改，只有当前会话提交（[回滚](https://so.csdn.net/so/search?q=回滚&spm=1001.2101.3001.7020)）后，或当前会话结束事，别的会话才可以修改，一般会造成线程等待，死锁的发生；



#### FirstOrInit

获取匹配的第一条记录，否则根据给定的条件 **初始化一个新的对象** (仅支持 struct 和 map 条件)，**此对象不会插入到表中**！

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

获取匹配的第一条记录, 否则根据给定的条件创建一个新的记录 (仅支持 struct 和 map 条件)，**此对象会插入到表中**！

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

##### Select 选择字段

GORM 中查询会默认会选择全部字段（*），使用 Select 指定你想从数据库中检索出的字段。

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

// 多个 order 写法
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

// Distinct() 去重
db.Distinct( "name" , "age" ).Order( "name, age desc" ).Find(&users)

// 去重查询 count
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

GORM 允许您在 `Table()` 方法中通过 FROM 子句使用子查询，例如：

```go
db.Table("(?) as u", db.Model(&User{}).Select("name", "age")).Where("age = ?", 18}).Find(&User{})
// SELECT * FROM (SELECT `name`,`age` FROM `users`) as u WHERE `age` = 18

subQuery1 := db.Model(&User{}).Select("name")
subQuery2 := db.Model(&Pet{}).Select("name")
db.Table("(?) as u, (?) as p", subQuery1, subQuery2).Find(&User{})
// SELECT * FROM (SELECT `name` FROM `users`) as u, (SELECT `name` FROM `pets`) as p
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

##### Jsons 预加载

您可以对单个 SQL 使用 `Joins` 来预加载关联：

```go
db.Joins("Company").Find(&users)
// SELECT users.id, users.name, users.age, 
//                Company.id AS Company_id, Company.name AS Company_name 
// FROM users 
// LEFT JOIN companies AS Company ON users.company_id = Company.id;


// inner join
db.InnerJoins("Company").Find(&users)
// SELECT users.id, users.name, users.age, 
//                Company.id AS Company_id, Company.name AS Company_name 
// FROM users 
// INNER JOIN companies AS Company ON users.company_id = Company.id;
```

有条件的连接：

```go
db.Joins("Company", db.Where(&Company{Alive: true})).Find(&users)
// SELECT users.id, users.name, users.age, 
//                Company.id AS Company_id, Company.name AS Company_name 
// FROM users 
// LEFT JOIN companies AS Company ON users.company_id = Company.id;
// AND Company.alive = true;
```



#### Pluck

Pluck，查询 model 中的 **一个列** 作为切片，如果您想要查询多个列，应该使用 `Select` 和 `Scan`

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



#### 查询钩子

对于查询操作，GORM 支持 `AfterFind` 钩子，查询记录后会调用它

```go
func (u *User) AfterFind(tx *gorm.DB) (err error) {
  if u.Role == "" {
    u.Role = "user"
  }
  return
}
```



### 链式操作相关

#### 链式操作

Method Chaining，Gorm 实现了链式操作接口，所以你可以把代码写成这样，相当于动态 SQL：

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

在调用立即执行方法前不会生成 `Query` 语句，借助这个特性你可以创建一个函数来处理一些通用逻辑。



#### 立即执行方法

Immediate methods ，立即执行方法是指那些会立即生成 `SQL` 语句并发送到数据库的方法, 他们一般是 `CRUD` 方法，比如：

`Create`, `First`, `Find`, `Take`, `Save`, `UpdateXXX`, `Delete`, `Scan`, `Row`, `Rows`…

这有一个基于上面链式方法代码的立即执行方法的例子：

```go
tx.Find(&user)
```

生成的 SQL 语句如下：

```sql
SELECT * FROM users where name = 'AruNi' AND age = 30 AND active = 1;
```



#### 范围 Scopes

`Scopes`，Scope 是建立在链式操作的基础之上的。

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
    return db.Where("status IN (?)", status)
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

// 不复用前面的 Find 立即执行方法，因为它是内联条件
SELECT count(*) FROM users WHERE name LIKE 'Aru%'
```



### Update

#### 更新所有字段

`Save()` 是一个组合功能。如果保存值不包含主键，它将执行 Create，否则它将执行 Update(包含所有字段)。

`Save()` 默认会更新该对象的所有字段，即使你没有赋值，字段是零值。

```go
var user User
db.Where("name = ?", "AruNi").Find(&user)
user.Age = sql.NullInt64{Int64: 99, Valid: true}
db.Debug().Save(&user)
// UPDATE users SET `name` = 'AruNi', `age` = '99'  WHERE users.id = 1

db.Save(&User{Name: "jinzhu", Age: 100})
// INSERT INTO `users` (`name`,`age`,`birthday`,`update_at`) VALUES ("jinzhu",100,"0000-00-00 00:00:00","0000-00-00 00:00:00")

db.Save(&User{ID: 1, Name: "jinzhu", Age: 100})
// UPDATE `users` SET `name`="jinzhu",`age`=100,`birthday`="0000-00-00 00:00:00",`update_at`="0000-00-00 00:00:00" WHERE `id` = 1
```

注意：不要在 `Save()` 中使用 `Model()`，这是未定义的。

#### 更新修改字段

如果你只希望更新指定字段，可以使用 `Update` 或者 `Updates`

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

如果你想只更新或忽略某些字段，你可以使用 `Select`，`Omit`

```go
// 更新name字段
db.Model(&user).Select("name").Updates(map[string]interface{}{"name": "hello", "age": 18, "active": false})
// UPDATE users SET name='hello', updated_at='2013-11-17 21:34:10' WHERE id=111;

// 更新时忽略name字段
db.Model(&user).Omit("name").Updates(map[string]interface{}{"name": "hello", "age": 18, "active": false})
// UPDATE users SET age=18, active=false, updated_at='2013-11-17 21:34:10' WHERE id=111;
```



#### 无 Hooks 更新

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



#### 使用 SQL 表达式更新

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



#### 修改 Hooks 中更新的值

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

#### 删除一条记录

**警告**：删除记录时，请 **确保主键字段有值**，GORM 会通过主键去删除记录，如果主键为空，GORM 会删除该 model 的所有记录（批量删除）。

```go
// 删除现有记录
db.Delete(&email)
// DELETE from emails where id=10;

// 为删除 SQL 添加额外的 SQL 操作
db.Set("gorm:delete_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Delete(&email)
// DELETE from emails where id=10 OPTION (OPTIMIZE FOR UNKNOWN);
```

#### 根据主键删除

GORM 允许通过主键(可以是复合主键)和内联条件来删除对象，它可以使用数字（如以下例子。也可以使用字符串）。

```go
db.Delete(&User{}, 10)
// DELETE FROM users WHERE id = 10;

db.Delete(&User{}, "10")
// DELETE FROM users WHERE id = 10;

db.Delete(&users, []int{1,2,3})
// DELETE FROM users WHERE id IN (1,2,3);
```

#### 批量删除

删除全部匹配的记录

```go
db.Where("email LIKE ?", "%jinzhu%").Delete(Email{})
// DELETE from emails where email LIKE "%jinzhu%";

db.Delete(Email{}, "email LIKE ?", "%jinzhu%")
// DELETE from emails where email LIKE "%jinzhu%";
```

可以将一个主键切片传递给`Delete` 方法，以便更高效的删除数据量大的记录

```go
var users = []User{{ID: 1}, {ID: 2}, {ID: 3}}
db.Delete(&users)
// DELETE FROM users WHERE id IN (1,2,3);

db.Delete(&users, "name LIKE ?", "%jinzhu%")
// DELETE FROM users WHERE name LIKE "%jinzhu%" AND id IN (1,2,3); 
```

#### 阻止全局删除

当你试图执行不带任何条件的批量删除时，GORM将不会运行并返回 `ErrMissingWhereClause` 错误

如果一定要这么做，你必须添加一些条件，或者使用原生SQL，或者开启 `AllowGlobalUpdate` 模式，如下例：

```go
// 引发 ErrMissingWhereClause 错误
db.Delete(&User{}).Error // gorm.ErrMissingWhereClause
db.Delete(&[]User{{Name: "jinzhu1"}, {Name: "jinzhu2"}}).Error // gorm.ErrMissingWhereClause

db.Where("1 = 1").Delete(&User{})
// DELETE FROM `users` WHERE 1=1

db.Exec("DELETE FROM users")
// DELETE FROM users

db.Session(&gorm.Session{AllowGlobalUpdate: true}).Delete(&User{})
// DELETE FROM users
```

#### 软删除

如果一个 model 有 `DeletedAt` 字段，他将自动获得软删除的功能！ 

当调用 `Delete` 方法时， 记录不会真正的从数据库中被删除， 只会将`DeletedAt` 字段的值会被设置为当前时间。而后的一般查询方法将无法查找到此条记录。

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

## 关联

















