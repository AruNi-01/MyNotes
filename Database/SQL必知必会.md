# 1. 了解SQL

## 1.1 数据库

### 1.1.1 概念

数据库（database）

- 保存有组织的数据的容器（通常是一个文件或一组文件）

注意：

- 我们使用的数据库软件称为数据库管理系统（DBMS）
- 数据库是通过DBMS创建和操纵的容器

### 1.1.2 表

表（table）

- 某种特定类型数据的结构化清单
- 表是一种结构化的文件，可用来存储某种特定类型的数据

表名：

- 数据库中的每个表都有一个名字来标识自己，这个名字是唯一的

模式：

- 关于数据库和表的布局及特性的信息

### 1.1.3 列和数据类型

列（column）

- 表中的一个字段，所有表都是由一个或多个列组成的
- 数据库中每个列都有相应的数据类型

### 1.1.4 行

行（row）

- 表中的一个记录
- 表中的数据是按行存储的，所保存的每个记录存储在自己的行内

### 1.1.5 主键

- 一列（或一组列），其值能够**唯一标识**表中每一行
- 应该总是定义主键，以便于以后的数据操作和管理

## 1.2 什么是SQL

SQL（发音为字母S-Q-L或 sequel）是 Structured Query Language（结构化查询语言）的缩写。SQL是一种专门用来与数据库沟通的语言。

优点：

- SQL不是某个特定数据库供应商专有的语言，几乎所有重要的DBMS都支持SQL
- SQL简单易学
- SQL实际上是一种强有力的语言，可以进行非常复杂和高级的数据库操作



# 2. 检索数据

这一课介绍如何使用SELECT语句从表中检索一个或多个数据列。

接下来的示例采用MySQL，表如下：

- Customers表：存储所有顾客信息
- Orders表：存储顾客订单
- OrderItems表：存储每个订单中的实际物品
- Products表：包含产品目录
- Vendors表：存储销售产品的供应商

![image-20220501165607249](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501165607249.png)

### 2.1 SELECT语句

最经常使用的 SQL 语句就是 SELECT 语句。它的用途是从一个或多个表中检索信息。

关键字（keyword）

- 作为SQL组成部分的保留字。关键字不能用作表或列的名字

### 2.2 检索单个列

简单的SQL SELECT语句。

输入：

```sql
SELECT prod_name
FROM products;
```

输出：

![image-20220501170631004](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501170631004.png)

说明：未排序数据

- 如果没有明确排序查询结果，则返回的数据没有特定的顺序

### 2.3 检索多个列

在SELECT关键字后给出多个列名，列名之间必须以逗号分隔。

输入：

```sql
SELECT prod_id, prod_name, prod_price
FROM products
```

输出：

![image-20220501170941136](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501170941136.png)

### 2.4 检索所有列

在实际列名的位置使用星号（＊）通配符可以检索所有的列。

输入：

```sql
SELECT *
FROM products
```

输出：

![image-20220501171127489](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501171127489.png)

### 2.5 检索不同的值

使用 **DISTINCT** 关键字，它指示数据库只返回不同的值，它必须直接放在**列名的前面**。

输入：

```sql
SELECT DISTINCT vend_id
FROM products
```

输出：

![image-20220501171353646](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501171353646.png)

注意：

- DISTINCT关键字作用于所有的列，不仅仅是跟在其后的那一列

### 2.6 限制结果

MySQL 使用 LIMIT 子句

输入：

```sql
SELECT prod_name
FROM products
LIMIT 5
```

输出：

![image-20220501171619522](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501171619522.png)

为了得到后面的5行数据，需要指定从哪儿开始以及检索的行数，像这样：

输入：

```sql
SELECT prod_name
FROM products
LIMIT 5 OFFSET 5
```

输出：Products 表中只有9种产品，所以 `LIMIT 5 OFFSET 5` 只返回了4行数据（因为没有第5行）

![image-20220501171719252](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501171719252.png)

分析：

`LIMIT 5 OFFSET 5` 指示 MySQL 等DBMS返回从第5行起的5行数据。第一个数字是检索的行数，第二个数字是指从哪儿开始。

> MySQL中支持简化版的 `LIMIT 4 OFFSET 3` 语句：`LIMIT 3, 4`

### 2.7 使用注释

- `-- `：-- 之后的文本就是注释
- `#`：# 之后的文本就是注释
- `/*...*/`：多行注释



# 3. 排序检索数据

使用 SELECT 语句的 ORDER BY 子句，根据需要排序检索出的数据。

### 3.1 排序数据

ORDER BY 子句取一个或多个列的名字，据此对输出进行排序。

输入：

```sql
SELECT prod_name
FROM products
ORDER BY prod_name
```

输出：

![image-20220501172408802](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501172408802.png)

注意：

- 在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中**最后一条子句**

### 3.2 按多个列排序

要按多个列排序，简单指定列名，列名之间用逗号分开即可（就像选择多个列时那样）。

输入：

```sql
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price, prod_name
```

输出：

![image-20220501172650892](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501172650892.png)

对于上述例子中的输出，仅在多个行具有相同的prod_price值时才对产品按prod_name进行排序。如果prod_price列中所有的值都是唯一的，则不会按prod_name排序。

### 3.3 按列位置排序

一般不使用！！

缺点：

不明确给出列名有可能造成错用列名排序。其次，在对SELECT清单进行更改时容易错误地对数据进行排序（忘记对ORDER BY子句做相应的改动）



### 3.4 指定排序方向

数据排序默认是升序。还可以使用 ORDER BY 子句进行降序。为了进行降序，必须指定 DESC 关键字。

下面的例子以价格降序来排序产品（最贵的排在最前面）：

输入：

```sql
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC
```

输出：

![image-20220501173404068](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501173404068.png)

注意：

- 如果想在多个列上进行降序排序，必须对每一列指定 DESC 关键字。

# 4. 过滤数据

使用 SELECT 语句的 WHERE 子句指定搜索条件。

### 4.1 使用WHERE子句

在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。WHERE子句在表名（FROM子句）之后给出，如下所示：

输入：

```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price = 3.49
```

输出：

![image-20220501173617468](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501173617468.png)

注意：WHERE子句的位置

- 在同时使用ORDER BY和WHERE子句时，应该让ORDER BY位于WHERE之后。

### 4.2 WHERE子句操作符

我们在做相等检验时看到了第一个WHERE子句，它确定一个列是否包含指定的值。SQL支持下表列出的所有条件操作符。

![image-20220501173750431](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501173750431.png)

注意：操作符兼容

- 某些操作符是冗余的（如＜ >与！=相同，! ＜相当于>=）。并非所有DBMS都支持这些操作符。想确定你的DBMS支持哪些操作符，请参阅相应的文档

#### 4.2.1 检查单个值

列出所有价格小于10美元的产品。

输入：

```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price < 10
```

输出：

![image-20220501174532167](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501174532167.png)

#### 4.2.2 不匹配检查

列出所有不是供应商DLL01制造的产品：

输入：

```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id != 'DLL01'
```

输出：

![image-20220501174848649](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501174848649.png)

#### 4.2.3 范围值检查

要检查某个范围的值，可以使用BETWEEN操作符。其语法与其他WHERE子句的操作符稍有不同，因为它需要两个值，即范围的开始值和结束值。

下面的例子说明如何使用BETWEEN操作符，它检索价格在5美元和10美元之间的所有产品。

输入：

```sql
SELECT prod_name, prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10
```

输出：

![image-20220501180019953](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501180019953.png)

#### 4.2.4 空值检查

空值NULL

- 无值（no value），它与字段包含0、空字符串或仅仅包含空格不同

确定值是否为NULL，不能简单地检查是否 = NULL。应该使用 IS NULL子句。其语法如下：

输入：

```sql
SELECT cust_name
FROM customers
WHERE cust_email IS NULL
```

输出：

![image-20220501180322198](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501180322198.png)

# 5. 高级数据过滤

通过组合WHERE子句可以建立功能更强、更高级的搜索条件。

### 5.1 组合WHERE子句

为了进行更强的过滤控制，SQL允许给出多个WHERE子句。这些子句有两种使用方式，即以AND子句或OR子句的方式使用。

操作符（operator）：

- 用来联结或改变WHERE子句中的子句的关键字，也称为逻辑操作符（logical operator）。

#### 5.1.1 AND操作符

要通过不止一个列进行过滤，可以使用AND操作符给WHERE子句附加条件。

输入：

```sql
SELECT prod_id, prod_price, prod_name
FROM products
WHERE vend_id = 'DLL01' AND prod_price <= 4
```

输出：

![image-20220501181035384](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501181035384.png)

#### 5.1.2 OR操作符

OR操作符与AND操作符正好相反，它指示DBMS检索匹配任一条件的行。

输入：

```sql
SELECT prod_price, prod_name
FROM products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
```

输出：

![image-20220501181415122](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501181415122.png)

#### 5.1.3 求值顺序

SQL（像多数语言一样）在处理OR操作符前，优先处理AND操作符。

任何时候使用具有AND和OR操作符的WHERE子句，都应该使用圆括号明确地分组操作符。



### 5.2 IN操作符

IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。IN取一组由逗号分隔、括在圆括号中的合法值。下面的例子说明了这个操作符。

输入：

```sql
SELECT prod_name, prod_price
FROM products
WHERE vend_id IN ('DLL01', 'BRS01')
ORDER BY prod_name
```

输出：

![image-20220501181714029](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501181714029.png)

你可能会猜测IN操作符完成了与OR相同的功能，恭喜你猜对了！

为什么要使用IN操作符？其优点如下：

- 在有很多合法选项时，IN操作符的语法更清楚，更直观

- 在与其他AND和OR操作符组合使用IN时，求值顺序更容易管理

- IN操作符一般比一组OR操作符**执行得更快**（在上面这个合法选项很少的例子中，你看不出性能差异）

- IN的最大优点是可以**包含其他SELECT语句**，能够更动态地建立WHERE子句



### 5.3 NOT操作符

NOT

- WHERE子句中用来否定其后条件的关键字

输入：

```sql
SELECT prod_name
FROM products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name
```

输出：

![image-20220501182033285](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501182033285.png)

为什么使用NOT ？

对于这里的这种简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作符联合使用时，NOT可以非常简单地找出与条件列表不匹配的行。



# 6. 用通配符进行过滤

这节课学习什么是通配符、如何使用通配符以及怎样使用LIKE操作符进行通配搜索，以便对数据进行复杂过滤。

### 6.1 LIKE操作符

通配符（wildcard）

- 用来匹配值的一部分的特殊字符。

为在搜索子句中使用通配符，必须使用LIKE操作符。

LIKE指示DBMS，后跟的搜索模式利用通配符匹配而不是简单的相等匹配进行比较。

#### 6.1.1 百分号（%）通配符

在搜索串中，% 表示任何字符出现任意次数。

输入：

```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE 'Fish%'
```

输出：

![image-20220501193959658](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501193959658.png)

注意：

- 根据DBMS的不同及其配置，搜索可以是区分大小写的。如果区分大小写，则`’fish%’`与 Fishbean bag toy 就不匹配。

#### 6.1.2 下划线（_）通配符

下划线的用途与%一样，但它只匹配单个字符，而不是多个字符。

输入，注意下面LIKE中有2个 '_'：

```sql
SELECT prod_id, prod_name
FROM products
WHERE prod_name LIKE '__ inch teddy bear'
```

输出：

![image-20220501194439591](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501194439591.png)

#### 6.1.3 方括号（[ ]）通配符

方括号（[]）通配符用来指定一个字符集，它必须匹配指定位置（通配符的位置）的**一个字符**。

此通配符可以用前缀字符 ^（脱字号）来否定 `[^xx] `。

输入：

```sql
SELECT cust_contact
FROM customers
WHERE cust_contact LIKE '[JM]%'
ORDER BY cust_contact
```

输出：

![image-20220501195915637](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501195915637.png)



# 7. 创建计算字段

### 7.1 计算字段

存储在表中的数据一般不是应用程序所需要的。我们需要直接从数据库中检索出转换、计算或格式化过的数据，而不是检索出数据，然后再在客户端应用程序中重新格式化。

### 7.2 拼接字段

在SQL中的SELECT语句中，可使用一个特殊的操作符来拼接两个列。在MySQL中，必须使用特殊的函数`Concat`

别名

- 别名（alias）是一个字段或值的替换名。别名用AS关键字赋予。

输入：

```sql
SELECT CONCAT(vend_name, '(', vend_country, ')')
AS vend_title
FROM vendors
ORDER BY vend_name
```

输出：

![image-20220501200604522](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501200604522.png)

### 7.3 执行算术计算

计算字段的另一常见用途是对检索出的数据进行算术计算。

SQL表中列出的基本算术操作符。此外，圆括号可用来区分优先顺序。

![image-20220501200726410](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501200726410.png)

输入：

```sql
SELECT prod_id, 
	    quantity, 
	     item_price, 
	    quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 20008
```

输出：

![image-20220501200926408](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501200926408.png)

# 8. 使用函数处理数据

### 8.1 函数

与大多数其他计算机语言一样，SQL也可以用函数来处理数据。函数一般是在数据上执行的，为数据的转换和处理提供了方便。

### 8.2 使用函数

大多数SQL实现支持以下类型的函数。

- 用于处理文本字符串（如删除或填充值，转换值为大写或小写）的文本函数。
- 用于在数值数据上进行算术操作（如返回绝对值，进行代数运算）的数值函数。
- 用于处理日期和时间值并从这些值中提取特定成分（如返回两个日期之差，检查日期有效性）的日期和时间函数。
- 返回DBMS正使用的特殊信息（如返回用户登录信息）的系统函数。

#### 8.2.1 文本处理函数

常用的文本处理函数：

![image-20220501201332303](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501201332303.png)

输入：

```sql
SELECT vend_name, UPPER(vend_name) AS vend_name_upcase
FROM vendors
ORDER BY vend_name
```

输出：

![image-20220501201503619](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501201503619.png)

下面给出一个使用SOUNDEX()函数的例子。如果两个值的发音相似，则他们的SOUNDEX值匹配，可以查找出对应的数据：

```sql
SELECT cust_name, cust_contact
FROM customers
WHERE SOUNDEX(cust_contact) = SOUNDEX('Michael Green')
```

Michelle Green 与 Michael Green 发音相似，输出：

![image-20220501202008612](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501202008612.png)

#### 8.2.2 日期和时间处理函数

MySQL可使用名为YEAR()的函数从日期中提取年份：

输入：

```sql
SELECT order_num
FROM orders
WHERE YEAR(order_date) = 2012
```

输出：

![image-20220501202218035](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501202218035.png)

这里给出的例子提取和使用日期的成分（年）。按月份过滤，可以进行相同的处理，使用AND操作符可以进行年和月份的比较。

#### 8.2.3 数值处理函数

数值处理函数仅处理数值数据。这些函数一般主要用于代数、三角或几何运算。

一些常用的数值处理函数。

![image-20220501202306918](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501202306918.png)

# 9. 汇总数据

### 9.1 聚集函数

我们经常需要汇总数据而不用把它们实际检索出来，为此SQL提供了专门的函数。

使用这些函数，SQL查询可用于检索数据，以便分析和报表生成。

这种类型的检索例子有：

- 确定表中行数（或者满足某个条件或包含某个特定值的行数）；
- 获得表中某些行的和；
- 找出表列（或所有行或某些特定的行）的最大值、最小值、平均值。

为方便这种类型的检索，SQL给出了5个聚集函数：

![image-20220501202508796](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501202508796.png)

#### 9.1.1 AVG()函数

AVG()通过对表中行数计数并计算其列值之和，求得该列的平均值。AVG()可用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

下面的例子使用AVG()返回Products表中所有产品的平均价格：

输入：

```sql
SELECT AVG(prod_price) AS avg_price
FROM products
```

输出：

![image-20220501202631526](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501202631526.png)

注意：

- AVG()只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。为了获得多个列的平均值，必须使用多个AVG()函数。
- AVG()函数忽略列值为NULL的行。

#### 9.1.2 COUNT()函数

COUNT()函数进行计数。可利用COUNT()确定表中行的数目或符合特定条件的行的数目。

COUNT()函数有两种使用方式：

- `COUNT(＊)`对表中行的数目进行计数，**不管表列中包含的是空值（NULL）还是非空值**。
- `COUNT(column)`对特定列中具有值的行进行计数，**忽略NULL值**。

输入：

```sql
SELECT COUNT(*) AS num_cust
FROM customers
```

输出：

![image-20220501203126542](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501203126542.png)

在此例子中，利用COUNT(＊)对所有行计数，不管行中各列有什么值。计数值在num_cust中返回。

下面的例子只对具有电子邮件地址的客户计数：

```sql
SELECT COUNT(cust_email) AS num_cust
FROM customers
```

输出：

![image-20220501203233079](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501203233079.png)

这条SELECT语句使用COUNT(cust_email)对cust_email列中有值的行进行计数。在此例子中，cust_email的计数为3（表示5个顾客中只有3个顾客有电子邮件地址）。



#### 9.1.3 MAX()函数

MAX()返回指定列中的最大值。MAX()要求指定列名，如下所示：

```sql
SELECT MAX(prod_price) AS max_price
FROM products
```

输出：

![image-20220501203439319](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501203439319.png)

注意：

- MAX()函数忽略列值为NULL的行。



#### 9.1.4 MIN()函数

MIN()的功能正好与MAX()功能相反，它返回指定列的最小值。与MAX()一样，MIN()要求指定列名，如下所示：

```sql
SELECT MIN(prod_price) AS min_price
FROM products
```

输出：

![image-20220501203554001](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501203554001.png)

注意：

- MIN()函数忽略列值为NULL的行。



#### 9.1.5 SUM()函数

SUM()用来返回指定列值的和（总计）。

下面举一个例子，OrderItems包含订单中实际的物品，每个物品有相应的数量。可如下检索所订购物品的总数（所有quantity值之和）：

```sql
SELECT SUM(quantity) AS items_ordered
FROM orderitems
WHERE order_num = 20005
```

输出：

![image-20220501203727314](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501203727314.png)

### 9.2 聚集不同值

以上5个聚集函数都可以如下使用。

- 对所有行执行计算，指定ALL参数或不指定参数（因为ALL是默认行为）。
- 只包含不同的值，指定DISTINCT参数。

下面的例子使用AVG()函数返回特定供应商提供的产品的平均价格。它与上面的SELECT语句相同，但使用了DISTINCT参数，因此平均值只考虑各个不同的价格：

```sql
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM products
WHERE vend_id = 'DLL01'
```

输出：

![image-20220501204010531](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501204010531.png)

以看到，在使用了DISTINCT后，此例子中的avg_price比较高，因为有多个物品具有相同的较低价格。排除它们提升了平均价格。

注意：

- 如果指定列名，则DISTINCT只能用于COUNT()。DISTINCT不能用于COUNT(＊)。

### 9.3 组合聚集函数

目前为止的所有聚集函数例子都只涉及单个函数。但实际上，SELECT语句可根据需要包含多个聚集函数。

输入：

```sql
SELECT COUNT(*) AS num_items,
			 MIN(prod_price) AS price_min,
			 MAX(prod_price) AS price_max,
			 AVG(prod_price) AS price_avg
FROM products
```

输出：

![image-20220501204411497](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501204411497.png)

# 10. 分组数据

这一课介绍如何分组数据，以便汇总表内容的子集。这涉及两个新SELECT语句子句：GROUP BY子句和HAVING子句。

### 10.1 数据分组

使用分组可以将数据分为多个逻辑组，对每个组进行聚集计算。

### 10.2 创建分组

分组是使用SELECT语句的GROUP BY子句建立的。

输入：

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
GROUP BY vend_id
```

输出：

![image-20220501204827227](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501204827227.png)

规定：

- GROUP BY子句中列出的每一列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。**不能使用别名**。
- 如果分组列中包含具有NULL值的行，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。
- GROUP BY子句**必须出现在WHERE子句之后，ORDER BY子句之前**。



### 10.3 过滤分组

除了能用GROUP BY分组数据外，SQL还允许过滤分组，规定包括哪些分组，排除哪些分组。

那么，不使用WHERE使用什么呢？SQL为此提供了另一个子句，就是HAVING子句。HAVING非常类似于WHERE。

输入：

```sql
SELECT cust_id, COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 1
```

输出：

![image-20220501205404952](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501205404952.png)

事实上，目前为止所学过的所有类型的WHERE子句都可以用HAVING来替代。

唯一的差别是，**WHERE过滤行，而HAVING过滤分组**。

那么，有没有在一条语句中同时使用WHERE和HAVING子句的需要呢？

来看下面的例子，它列出具有两个以上产品且其价格大于等于4的供应商：

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >= 1
```

输出：

![image-20220501205633934](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501205633934.png)



说明：使用HAVING和WHERE

- 使用HAVING时应该结合GROUP BY子句，而WHERE子句用于标准的行级过滤。



### 10.4 分组和排序

GROUP BY和ORDER BY经常完成相同的工作，但它们非常不同，理解这一点很重要。

![image-20220501205731181](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501205731181.png)

提示：不要忘记ORDER BY

- 一般在使用GROUP BY子句时，应该也给出ORDER BY子句。这是保证数据正确排序的唯一方法。千万不要仅依赖GROUP BY排序数据。

输入：

```sql
SELECT order_num, COUNT(*) AS items
FROM orderitems
GROUP BY order_num
HAVING COUNT(*) >= 3
ORDER BY items, order_num
```

输出：

![image-20220501210014064](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501210014064.png)

在这个例子中，使用GROUP BY子句按订单号（order_num列）分组数据，以便COUNT(＊)函数能够返回每个订单中的物品数目。HAVING子句过滤数据，使得只返回包含三个或更多物品的订单。最后，用ORDER BY子句排序输出。

### 10.5 SELECT子句顺序

下面回顾一下SELECT语句中子句的顺序。在SELECT语句中使用时必须遵循的次序，列出迄今为止所学过的子句：

![image-20220501210047530](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220501210047530.png)

# 11. 使用子查询

### 11.1 子查询

SQL 还允许创建子查询（subquery），即嵌套在其他查询中的查询。

MySQL 对子查询的支持是从4.1版本引入的。MySQL 的早期版本不支持子查询。

### 11.2 利用子查询进行过滤

示例：列出订购物品RGAN01的所有顾客信息

```sql
SELECT cust_name, cust_contact
FROM customers
WHERE cust_id IN (SELECT cust_id
                                  FROM orders
                                  WHERE order_num IN (SELECT order_num
                                                                          FROM orderitems
                                                                          WHERE prod_id = 'RGAN01'))
```

输出：

![image-20220503191508379](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503191508379.png)

最里边的子查询返回订单号列表，此列表用于其外面的子查询的WHERE子句。外面的子查询返回顾客ID列表，此顾客ID列表用于最外层查询的WHERE子句。最外层查询返回所需的数据。



注意：

- 作为子查询的SELECT语句只能查询单个列。企图检索多个列将返回错误。
- 使用子查询并不总是执行这类数据检索的最有效方法，性能不是最好的。



### 11.3 作为计算字段使用子查询

示例：查询每个顾客的订单数

```sql
SELECT cust_name,
                cust_state,
                (SELECT COUNT(*)
                 FROM orders
                 WHERE orders.cust_id = customers.cust_id) AS orders
FROM customers
ORDER BY cust_name
```

注意：

- 子句中的 WHERE 语句必须使用 完全限定列名`orders.cust_id = customers.cust_id`，因为orders和customers表中的 `cust_id` 字段相同

输出：

![image-20220503192303188](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503192303188.png)

这条SELECT语句对Customers表中每个顾客返回三列：cust_name、cust_state和orders。orders是一个计算字段，它是由圆括号中的子查询建立的。该子查询对检索出的每个顾客执行一次。在此例中，该子查询执行了5次，因为检索出了5个顾客。



# 12. 联结表

### 12.1 联结

创建联结非常简单，指定要联结的所有表以及关联它们的方式即可。

输入：

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors, products
WHERE vendors.vend_id = products.vend_id
```

输出：

![image-20220503192931809](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503192931809.png)

注意：

- 若无 WHERE 子句限定，第一个表中的每一行将与第二个表中的每一行配对，而不管它们逻辑上是否能配在一起。



#### 12.2.1 内联结

内联结使用 `INNER JOIN...ON`。

下面的 SELECT 语句返回与前面例子完全相同的数据：

```sql
SELECT vend_name, prod_name, prod_price
FROM vendors INNER JOIN products
ON vendors.vend_id = products.vend_id
```

注意：

- 内联结的条件使用特定的 `ON` 子句而不是 `WHERE`
- 相似地，还有左联结`LEFT JOIN`和右联结`RIGHT JOIN`



> 性能问题：应该注意不要联结不必要的表，联结的表越多，性能下降越厉害



# 13. 创建高级联结

### 13.1 使用表别名

SQL除了可以对列名和计算字段使用别名，还允许给表名起别名。

这样做有两个主要理由：

- 缩短SQL语句；
- 允许在一条SELECT语句中多次使用相同的表。

示例，与前一课例子中所用的语句基本相同，但改成了使用别名：

```sql
SELECT cust_name, cust_contact
FROM customers AS C, orders AS O, orderitems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RGAN01'
```

注意：

- 表别名只在查询执行中使用。与列别名不一样，表别名不返回到客户端。



### 13.2 使用不同类型的联结

迄今为止，我们使用的只是内联结或等值联结的简单联结。现在来看三种其他联结：自联结（self-join）、自然联结（natural join）和外联结（outer join）。



#### 13.2.1 自联结

查询与Jim Jones 同一公司的所有顾客：

```sql
SELECT c1.cust_id, c1.cust_name, c1.cust_contact
FROM customers AS c1, customers AS c2
WHERE c1.cust_name = c2.cust_name
AND c2.cust_contact = 'Jim Jones'
```

输出：

![image-20220503194631725](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503194631725.png)

分析：

此查询中需要的两个表实际上是相同的表，因此Customers表在FROM子句中出现了两次。虽然这是完全合法的，但对Customers的引用具有歧义性，因为DBMS不知道你引用的是哪个Customers表。

解决此问题，需要使用表别名。Customers第一次出现用了别名c1，第二次出现用了别名c2。



#### 13.2.2 自然联结

自然联结要求你只能选择那些唯一的列，一般通过对一个表使用通配符（SELECT ＊），而对其他表的列使用明确的子集来完成。

输出：

```sql
SELECT C.*, O.order_num, O.order_date,
		OI.prod_id, OI.quantity, OI.item_price
FROM customers AS C, orders AS O, orderitems AS OI
WHERE C.cust_id = O.cust_id
AND OI.order_num = O.order_num
AND prod_id = 'RANG01'
```

分析：

在这个例子中，通配符只对第一个表使用。所有其他列明确列出，所以没有重复的列被检索出来。



#### 13.2.3 外联结

要检索包括没有订单顾客在内的所有顾客，可如下进行：

输入：

```sql
SELECT customers.cust_id, orders.order_num
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id
```

输出：

![image-20220503200137467](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503200137467.png)



### 13.3 使用带聚集函数的联结

聚集函数也可以方便地与其他联结一起使用。

示例：检索所有顾客及每个顾客所下的订单数

```sql
SELECT customers.cust_id,
				COUNT(orders.order_num) AS num_ord
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id
```

输出：

![image-20220503200401720](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503200401720.png)



# 14. 组合查询

利用UNION操作符将多条SELECT语句组合成一个结果集。

### 14.1 组合查询

SQL允许执行多个查询（多条SELECT语句），并将结果作为一个查询结果集返回。这些组合查询通常称为并（union）或复合查询（compound query）。

主要有两种情况需要使用组合查询：

- 在一个查询中从不同的表返回结构数据；
- 对一个表执行多个查询，按一个查询返回数据。

### 14.2 创建组合查询

可用UNION操作符来组合数条SQL查询。利用UNION，可给出多条SELECT语句，将它们的结果组合成一个结果集。



#### 14.2.1 使用UNION

使用UNION很简单，所要做的只是给出每条SELECT语句，在各条语句之间放上关键字UNION。

输入：

```sql
SELECT cust_name, cust_contact, cust_email
FROM customers
WHERE cust_state IN ('IL', 'IN', 'MI')
UNION
SELECT cust_name, cust_contact, cust_email
FROM customers
WHERE cust_name = 'Fun4All'
```

输出：

![image-20220503201324422](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503201324422.png)



#### 14.2.2 UNION规则

UNION非常容易使用，但在进行组合时需要注意几条规则：

- UNION中的**每个查询必须包含相同的列、表达式或聚集函数**（不过，各个列不需要以相同的次序列出）。
- 列数据类型必须兼容：**类型不必完全相同**，但必须是DBMS可以**隐含转换**的类型（例如，不同的数值类型或不同的日期类型）。
- UNION从查询结果集中**自动去除了重复的行**，如果确实需要每个条件的匹配行全部出现（包括重复行），就必须使用UNION ALL
- 在用UNION组合查询时，**只能使用一条ORDER BY子句**，它必须位于**最后一条SELECT语句之后**



# 15. 插入数据

利用SQL的INSERT语句可以将数据插入表中。

### 15.1 数据插入

通过使用`insert into`语句来向数据库中插入一条数据（一条记录）：

```sql
INSERT INTO 表名 VALUES(值1, 值2, 值3)
```



如果插入的数据与列一一对应，那么可以省略列名，但是如果希望向指定列上插入数据，就需要给出列名：

```sql
INSERT INTO 表名(列名1, 列名2) VALUES(值1, 值2)
```



我们也可以一次性向数据库中插入多条数据：

```sql
INSERT INTO 表名(列名1, 列名2) VALUES(值1, 值2), (值1, 值2), (值1, 值2)
```



插入检索出的数据：

```sql
INSERT INTO 表名(列名1, 列名2...) SELECT 列名1, 列名2... FROM 表名
```

INSERT SELECT 中 SELECT 语句可以包含 WHERE 子句，以过滤插入的数据。



示例：

```sql
INSERT INTO customers(cust_id,
                      cust_name,
                      cust_address,
                      cust_city,
                      cust_state,
                      cust_zip,
                      cust_country)
VALUES('1000000006',
               'Toy Land',
               '123 Any Street',
               'New York',
               'NY',
               '1111',
               'USA')
```



# 16. 更新和删除数据

利用UPDATE和DELETE语句可以进行更新和删除操作。

### 16.1 更新数据

更新（修改）表中的数据，可以使用UPDATE语句。

有两种使用UPDATE的方式：

- 更新表中的特定行；

- 更新表中的所有行。

注意：

- 不要省略WHERE子句，否则会更新表中的所有行



输入：

```sql
UPDATE customers
SET cust_contact = 'Sam Roberts',
		cust_email = 'sam@toyland.com'
WHERE cust_id = '1000000006'
```



### 16.2 删除数据

从一个表中删除（去掉）数据，使用DELETE语句。

有两种使用DELETE的方式：

- 从表中删除特定的行；
- 从表中删除所有行。

注意：

- 不要省略WHERE子句，否则会错误地删除表中所有行



输入：

```sql
DELETE FROM customers
WHERE cust_id = '1000000006'
```



# 17. 创建和操纵表

### 17.1 创建表

用程序创建表，可以使用SQL的CREATE TABLE语句。

#### 17.1.1 表创建基础

利用CREATE TABLE创建表，必须给出下列信息：

- 新表的名字，在关键字CREATE TABLE之后给出；
- 表列的名字和定义，用逗号分隔；
- 可以用`DEFAULT`指定默认值

示例：

![image-20220503203432685](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503203432685.png)



### 17.2 更新表

更新表定义，可以使用ALTER TABLE语句。

下面分别给出在表中新增列和删除列的操作：

![image-20220503203709980](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503203709980.png)



### 17.3 删除表

删除表（删除整个表而不是其内容）非常简单，使用DROP TABLE语句即可：

![image-20220503203738787](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503203738787.png)



# 18. 使用视图

### 18.1 视图

视图是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

MySQL从版本5起开始支持视图。

#### 18.1.1 为什么使用视图

- 重用SQL语句。
- 简化复杂的SQL操作。在编写查询后，可以方便地重用它而不必知道其基本查询细节。
- 使用表的一部分而不是整个表。
- 保护数据。可以授予用户访问表的特定部分的权限，而不是整个表的访问权限。
- 更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

创建视图之后，可以用与表基本相同的方式使用它们。可以对视图执行SELECT操作，过滤和排序数据，将视图联结到其他视图或表，甚至添加和更新数据。

重要的是，要知道视图仅仅是用来查看存储在别处数据的一种设施。视图本身不包含数据，因此返回的数据是从其他表中检索出来的。在添加或更改这些表中的数据时，视图将返回改变过的数据。

#### 18.1.2 视图的规则和限制

关于视图创建和使用的一些最常见的规则和限制：

- 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的名字）。
- 对于可以创建的视图数目没有限制。
- 创建视图，必须具有足够的访问权限。这些权限通常由数据库管理人员授予。
- 视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造视图。所允许的嵌套层数在不同的DBMS中有所不同（嵌套视图可能会严重降低查询的性能，因此在产品环境中使用之前，应该对其进行全面测试）。
- 许多DBMS禁止在视图查询中使用ORDER BY子句。
- 有些DBMS要求对返回的所有列进行命名，如果列是计算字段，则需要使用别名（关于列别名的更多信息，请参阅第7课）。
- 视图不能索引，也不能有关联的触发器或默认值。
- 有些DBMS把视图作为只读的查询，这表示可以从视图检索数据，但不能将数据写回底层表。详情请参阅具体的DBMS文档。
- 有些DBMS允许创建这样的视图，它不能进行导致行不再属于视图的插入或更新。



### 18.2 创建视图

视图用CREATE VIEW语句来创建。与CREATE TABLE一样，CREATE VIEW只能用于创建不存在的视图。

删除视图，可以使用DROP语句，其语法为DROP VIEW viewname;。覆盖（或更新）视图，必须先删除它，然后再重新创建。



#### 18.2.1 利用视图简化复杂的联结

输入：

```sql
CREATE VIEW ProductCustomers AS
SELECT cust_name, cust_contact, prod_id
FROM customers, orders, orderitems
WHERE customers.cust_id = orders.cust_id
AND orderitems.order_num = orders.order_num
```

分析：

这条语句创建一个名为ProductCustomers的视图，它联结三个表，返回已订购了任意产品的所有顾客的列表。如果执行SELECT ＊ FROM ProductCustomers，将列出订购了任意产品的顾客。

检索订购了产品RGAN01的顾客，可如下进行：

```sql
SELECT cust_name, cust_contact
FROM ProductCustomers
WHERE prod_id = 'RGAN01'
```

输出：

![image-20220503204542041](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503204542041.png)

可以看出，视图极大地简化了复杂SQL语句的使用。利用视图，可一次性编写基础的SQL，然后根据需要多次使用。



#### 18.2.2 用视图过滤不想要的数据

视图对于应用普通的WHERE子句也很有用。

例如，可以定义CustomerEMailList视图，过滤没有电子邮件地址的顾客：

![image-20220503204652794](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503204652794.png)

#### 18.2.3 使用视图与计算字段

在简化计算字段的使用上，视图也特别有用。

例如，创建检索某个订单中的物品，计算每种物品的总价格的视图：

![image-20220503204826701](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503204826701.png)

检索订单200008的详细内容：

![image-20220503204920830](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220503204920830.png)



# 19. 使用存储过程

### 19.1 存储过程

MySQL 5.0 版本开始支持存储过程。

存储过程（Stored Procedure）是一种在数据库中存储复杂程序，以便外部程序调用的一种数据库对象。

存储过程是为了完成特定功能的SQL语句集，经编译创建并保存在数据库中，用户可通过指定存储过程的名字并给定参数(需要时)来调用执行。

存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。

#### 优点

- 存储过程可封装，并隐藏复杂的商业逻辑。
- 存储过程可以回传值，并可以接受参数。
- 存储过程无法使用 SELECT 指令来运行，因为它是子程序，与查看表，数据表或用户定义函数不同。
- 存储过程可以用在数据检验，强制实行商业逻辑等。

#### 缺点

- 存储过程，往往定制化于特定的数据库上，因为支持的编程语言不同。当切换到其他厂商的数据库系统时，需要重写原有的存储过程。
- 存储过程的性能调校与撰写，受限于各种数据库系统。

### 19.2 存储过程的创建和调用

- 存储过程就是具有名字的一段代码，用来完成一个特定的功能。
- 创建的存储过程保存在数据库的数据字典中。

#### 创建存储过程

可以使用 **CREATE PROCEDURE** 语句创建存储过程，语法格式如下：

```sql
CREATE PROCEDURE <过程名> ( [过程参数[,…] ] ) <过程体>
[过程参数[,…] ] 格式
[ IN | OUT | INOUT ] <参数名> <类型>
```

语法说明如下：

1）过程名
存储过程的名称，默认在当前数据库中创建。若需要在特定数据库中创建存储过程，则要在名称前面加上数据库的名称，即 db_name.sp_name。

需要注意的是，名称应当尽量避免选取与 MySQL 内置函数相同的名称，否则会发生错误。

2）过程参数
存储过程的参数列表。其中，<参数名>为参数名，<类型>为参数的类型（可以是任何有效的 MySQL 数据类型）。当有多个参数时，参数列表中彼此间用逗号分隔。存储过程可以没有参数（此时存储过程的名称后仍需加上一对括号），也可以有 1 个或多个参数。

MySQL 存储过程支持三种类型的参数，即输入参数、输出参数和输入/输出参数，分别用 IN、OUT 和 INOUT 三个关键字标识。其中，输入参数可以传递给一个存储过程，输出参数用于存储过程需要返回一个操作结果的情形，而输入/输出参数既可以充当输入参数也可以充当输出参数。

需要注意的是，参数的取名不要与数据表的列名相同，否则尽管不会返回出错信息，但是存储过程的 SQL 语句会将参数名看作列名，从而引发不可预知的结果。

3）过程体
存储过程的主体部分，也称为存储过程体，包含在过程调用的时候必须执行的 SQL 语句。这个部分以关键字 BEGIN 开始，以关键字 END 结束。若存储过程体中只有一条 SQL 语句，则可以省略 BEGIN-END 标志。

在存储过程的创建中，经常会用到一个十分重要的 MySQL 命令，即 DELIMITER 命令，特别是对于通过命令行的方式来操作 MySQL 数据库的使用者，更是要学会使用该命令。

在 MySQL 中，服务器处理 SQL 语句默认是以分号作为语句结束标志的。然而，在创建存储过程时，存储过程体可能包含有多条 SQL 语句，这些 SQL 语句如果仍以分号作为语句结束符，那么 MySQL 服务器在处理时会以遇到的第一条 SQL 语句结尾处的分号作为整个程序的结束符，而不再去处理存储过程体中后面的 SQL 语句，这样显然不行。

为解决以上问题，通常使用 `DELIMITER` 命令将结束命令修改为其他字符。语法格式如下：

```sql
DELIMITER $$
```

语法说明如下：

- `$$` 是用户定义的结束符，通常这个符号可以是一些特殊的符号，如两个 “?” 或两个 “￥” 等。
- 当使用 `DELIMITER` 命令时，应该避免使用反斜杠 “\” 字符，因为它是 MySQL 的转义字符。


在 MySQL 命令行客户端输入如下 SQL 语句。

```sql
mysql > DELIMITER ??
```

成功执行这条 SQL 语句后，任何命令、语句或程序的结束标志就换为两个问号 “??” 了。

若希望换回默认的分号 “;” 作为结束标志，则在 MySQL 命令行客户端输入下列语句即可：

```sql
mysql > DELIMITER ;
```

注意：`DELIMITER` 和 分号“;” 之间一定要有一个**空格**。在创建存储过程时，必须具有 CREATE ROUTINE 权限。 



示例(Oracle版本)：

![image-20220504231833336](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504231833336.png)

这个存储过程有一个名为ListCount的参数。此**参数**从存储过程**返回一个值**而不是传递一个值给存储过程。关键字OUT用来指示这种行为。Oracle支持IN（传递值给存储过程）、OUT（从存储过程返回值，如这里）、INOUT（既传递值给存储过程也从存储过程传回值）类型的参数。存储过程的代码括在BEGIN和END语句中，这里执行一条简单的SELECT语句，它检索具有邮件地址的顾客。然后用检索出的行数设置ListCount（要传递的输出参数）。

调用Oracle例子可以像下面这样：

![image-20220504231958080](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504231958080.png)

声明了一个变量来保存存储过程返回的任何值，然后执行存储过程，再使用SELECT语句显示返回的值。



# 20. 管理事务处理

### 20.1 事务处理

使用事务处理（transaction processing），通过确保成批的SQL操作要么**完全执行**，要么**完全不执行**，来维护数据库的**完整性**。

关于事务处理的几个术语：

- 事务（transaction）指一组SQL语句；
- 回退（rollback）指撤销指定SQL语句的过程；
- 提交（commit）指将未存储的SQL语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），可以对它发布回退（与回退整个事务处理不同）。



### 20.2 控制事务处理

MySQL 中事务处理的开始和结束：

```sql
START TRANSACTION
...
COMMIT TRANSACTION
```

BEGIN TRANSACTION和COMMIT TRANSACTION语句之间的SQL必须完全执行或者完全不执行。



#### 20.2.1 使用ROLLBACK

SQL的ROLLBACK命令用来回退（撤销）SQL语句：

```sql
DELETE FROM Orders
ROLLBACK
```

执行DELETE操作，然后用ROLLBACK语句撤销前面的删除操作。



#### 20.2.2 使用COMMIT

一般的SQL语句都是针对数据库表直接执行和编写的。这就是所谓的隐式提交（implicitcommit），即提交（写或保存）操作是自动进行的。

在事务处理块中，提交不会隐式进行。不过，不同DBMS的做法有所不同。有的DBMS按隐式提交处理事务端，有的则不这样。

进行明确的提交，使用COMMIT语句。下面是一个 MySQL 的例子：

```sql
START TRANSACTION
DELETE OrderItems WHERE order_num = 12345
DELETE Orders WHERE order_num = 12345
COMMIT TRANSACTION
```

最后的COMMIT语句仅在不出错时写出更改。如果第一条DELETE起作用，但第二条失败，则DELETE不会提交。



#### 20.2.3 使用保留点

使用简单的ROLLBACK和COMMIT语句，就可以写入或撤销整个事务。但是，只对简单的事务才能这样做，复杂的事务可能需要部分提交或回退。

每个保留点都要取能够标识它的唯一名字，以便在回退时，DBMS知道回退到何处。

在MariaDB、MySQL和Oracle中创建占位符，可使用SAVEPOINT语句：

```sql
SAVEPOINT delete1
```

要回退到本例给出的保留点，在MariaDB、MySQL和Oracle中，如下进行：

```sql
ROLLBACK TO delete1
```



# 21. 使用游标

### 21.1 游标

MySQL 5已经支持游标。

有时，需要在检索出来的行中前进或后退一行或多行，这就是游标的用途所在。

游标（cursor）是一个存储在DBMS服务器上的数据库查询，它不是一条SELECT语句，而是被该语句检索出来的结果集。

在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

常见的一些选项和特性如下：

- 能够标记游标为只读，使数据能读取，但不能更新和删除。
- 能控制可以执行的定向操作（向前、向后、第一、最后、绝对位置、相对位置等）。
- 能标记某些列为可编辑的，某些列为不可编辑的。
- 规定范围，使游标对创建它的特定请求（如存储过程）或对所有请求可访问。
- 指示DBMS对检索出的数据（而不是指出表中活动数据）进行复制，使数据在游标打开和访问期间不变化。



### 21.2 使用游标

使用游标涉及几个明确的步骤：

- 在使用游标前，必须声明（定义）它。这个过程实际上没有检索数据，它只是定义要使用的SELECT语句和游标选项。
- 一旦声明，就必须打开游标以供使用。这个过程用前面定义的SELECT语句把数据实际检索出来。
- 对于填有数据的游标，根据需要取出（检索）各行。
- 在结束游标使用时，必须关闭游标，可能的话，释放游标（有赖于具体的DBMS）。

声明游标后，可根据需要频繁地打开和关闭游标。在游标打开时，可根据需要频繁地执行取操作。



#### 21.2.1 创建游标

使用DECLARE语句创建游标，DECLARE命名游标，并定义相应的SELECT语句，根据需要带WHERE和其他子句。

示例：创建一个游标来检索没有电子邮件地址的所有顾客

MySQL版本：

```sql
DECLARE CustCursor CURSOR
FOR
SELECT * FROM Customers
WHERE cust_email IS NULL
```

DECLARE语句用来定义和命名游标，这里为CustCursor。SELECT语句定义一个包含没有电子邮件地址（NULL值）的所有顾客的游标。

定义游标之后，就可以打开它了。



#### 21.2.2 使用游标

使用OPEN CURSOR语句打开游标：

```sql
OPEN CURSOR CustCursor
```

在处理OPEN CURSOR语句时，执行查询，存储检索出的数据以供浏览和滚动。

现在可以用FETCH语句访问游标数据了。FETCH指出要检索哪些行，从何处检索它们以及将它们放于何处（如变量名）。

第一个例子使用Oracle语法从游标中检索一行（第一行）：

![image-20220504233455418](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504233455418.png)

FETCH用来检索当前行（自动从第一行开始），放到声明的变量CustRecord中。对于检索出来的数据不做任何处理。

下一个例子（也使用Oracle语法）中，从第一行到最后一行，对检索出来的数据进行循环：

![image-20220504233520235](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504233520235.png)

与前一个例子一样，这个例子使用FETCH检索当前行，放到一个名为CustRecord的变量中。但不一样的是，这里的FETCH位于LOOP内，因此它反复执行。代码EXIT WHENCustCursor%NOTFOUND使在取不出更多的行时终止处理（退出循环）。这个例子也没有做实际的处理，实际例子中可用具体的处理代码替换占位符．..。



#### 21.2.3 关闭游标

游标在使用完毕时需要关闭：

```sql
CLOSE CustCursor
```

CLOSE语句用来关闭游标。一旦游标关闭，如果不再次打开，将不能使用。第二次使用它时不需要再声明，只需用OPEN打开它即可。



# 22. 高级SQL特性

SQL所涉及的几个高级数据处理特性：约束、索引和触发器。

### 22.1 约束

约束（constraint）：管理如何插入或处理数据库数据的规则。



#### 22.1.1 主键

主键是一种特殊的约束，用来保证一列（或一组列）中的值是唯一的，而且永不改动。换句话说，表中的一列（或多个列）的值唯一标识表中的每一行。这方便了直接或交互地处理表中的行。

没有主键，要安全地UPDATE或DELETE特定行而不影响其他行会非常困难。

表中任意列只要满足以下条件，都可以用于主键：

- 任意两行的主键值都不相同。

- 每行都具有一个主键值（即列中不允许NULL值）。

- 包含主键值的列从不修改或更新。（大多数DBMS不允许这么做，但如果你使用的DBMS允许这样做，好吧，千万别！）

- 主键值不能重用。如果从表中删除某一行，其主键值不分配给新行。

  

一种定义主键的方法是创建它，如下所示：

![image-20220504234037348](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234037348.png)

在此例子中，给表的vend_id列定义添加关键字PRIMARY KEY，使其成为主键。



在ALTER TABLE语句中用CONSTRAINT语法的方式：

![image-20220504234121632](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234121632.png)

这里定义相同的列为主键，但使用的是CONSTRAINT语法。此语法也可以用于CREATE TABLE和ALTER TABLE语句。



#### 22.1.2 外键

外键是表中的一列，其值必须列在另一表的主键中。外键是保证引用完整性的极其重要部分。

示例：在Orders的顾客ID列上定义一个外键，使该列只能接受Customers表的主键值

![image-20220504234324699](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234324699.png)

其中的表定义使用了REFERENCES关键字，它表示cust_id中的任何值都必须是Customers表的cust_id中的值。



在ALTER TABLE语句中用CONSTRAINT语法的方式：

![image-20220504234430593](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234430593.png)

注意：

- 外键有助防止意外删除，例如：不能删除关联订单的顾客。删除该顾客的唯一方法是首先删除相关的订单（这表示还要删除相关的订单项）。由于需要一系列的删除，因而利用外键可以防止意外删除数据。
- 有的DBMS支持称为级联删除（cascading delete）的特性。如果启用，该特性在从一个表中删除行时删除所有相关的数据。例如，如果启用级联删除并且从Customers表中删除某个顾客，则任何关联的订单行也会被自动删除。



#### 22.1.3 唯一约束

唯一约束用来保证一列（或一组列）中的数据是唯一的。它们类似于主键，但存在以下重要区别：

- 表可包含多个唯一约束，但每个表只允许一个主键。
- 唯一约束列可包含NULL值。
- 唯一约束列可修改或更新。
- 唯一约束列的值可重复使用。
- 与主键不一样，唯一约束不能用来定义外键。



employees表是一个使用约束的例子。每个雇员都有唯一的社会安全号，但我们并不想用它作主键，因为它太长（而且我们也不想使该信息容易利用）。因此，每个雇员除了其社会安全号外还有唯一的雇员ID（主键）。

雇员ID是主键，可以确定它是唯一的。你可能还想使DBMS保证每个社会安全号也是唯一的（保证输入错误不会导致使用他人号码）。可以通过在社会安全号列上定义UNIQUE约束做到。

唯一约束的语法类似于其他约束的语法。唯一约束既可以用UNIQUE关键字在表定义中定义，也可以用单独的CONSTRAINT定义。



#### 22.1.4 检查约束

检查约束用来保证一列（或一组列）中的数据满足一组指定的条件。

检查约束的常见用途有以下几点：

- 检查最小或最大值。例如，防止0个物品的订单（即使0是合法的数）。
- 指定范围。例如，保证发货日期大于等于今天的日期，但不超过今天起一年后的日期。
- 只允许特定的值。例如，在性别字段中只允许M或F。



示例：下面的例子对OrderItems表施加了检查约束，它保证所有物品的数量大于0

![image-20220504234726951](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234726951.png)

利用这个约束，任何插入（或更新）的行都会被检查，保证quantity大于0。

检查名为gender的列只包含M或F，可编写如下的ALTER TABLE语句：

![image-20220504234744267](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504234744267.png)



### 22.2 索引

索引用来排序数据以加快搜索和排序操作的速度。

主键数据总是排序的，这是DBMS的工作。因此，按主键检索特定行总是一种快速有效的操作。



但是，搜索其他列中的值通常效率不高。例如，如果想搜索住在某个州的客户，怎么办？因为表数据并未按州排序，DBMS必须读出表中所有行（从第一行开始），看其是否匹配。这就像要从没有索引的书中找出词汇一样。

解决方法是使用索引。可以在一个或多个列上定义索引，使DBMS保存其内容的一个排过序的列表。在定义了索引后，DBMS以使用书的索引类似的方法使用它。DBMS搜索排过序的索引，找出匹配的位置，然后检索这些行。



在开始创建索引前，应该记住以下内容：

- 索引改善检索操作的性能，但降低了数据插入、修改和删除的性能。在执行这些操作时，DBMS必须动态地更新索引。
- 索引数据可能要占用大量的存储空间。
- 并非所有数据都适合做索引。取值不多的数据（如州）不如具有更多可能值的数据（如姓或名），能通过索引得到那么多的好处。
- 索引用于数据过滤和数据排序。如果你经常以某种特定的顺序排序数据，则该数据可能适合做索引。
- 可以在索引中定义多个列（例如，州加上城市）。这样的索引仅在以州加城市的顺序排序时有用。如果想按城市排序，则这种索引没有用处。



索引用CREATE INDEX语句创建（不同DBMS创建索引的语句变化很大）。下面的语句在Products表的产品名列上创建一个简单的索引：

![image-20220504235001564](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504235001564.png)

索引必须唯一命名。这里的索引名prod_name_ind在关键字CREATE INDEX之后定义。ON用来指定被索引的表，而索引中包含的列（此例中仅有一列）在表名后的圆括号中给出。

注意：

- 索引的效率随表数据的增加或改变而变化。



### 22.3 触发器

触发器是特殊的存储过程，它在特定的数据库活动发生时自动执行。触发器可以与特定表上的INSERT、UPDATE和DELETE操作（或组合）相关联。

与存储过程不一样（存储过程只是简单的存储SQL语句），触发器与单个的表相关联。

触发器的一些常见用途：

- 保证数据一致。例如，在INSERT或UPDATE操作中将所有州名转换为大写。
- 基于某个表的变动在其他表上执行活动。例如，每当更新或删除一行时将审计跟踪记录写入某个日志表。
- 进行额外的验证并根据需要回退数据。例如，保证某个顾客的可用资金不超限定，如果已经超出，则阻塞插入。
- 计算计算列的值或更新时间戳。



示例：对所有INSERT和UPDATE操作，将Customers表中的cust_state列转换为大写的触发器（SQL Server版本）

![image-20220504235329462](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220504235329462.png)

tips：

- 一般来说，约束的处理比触发器快，因此在可能的时候，应该尽量使用约束。



### 22.4 数据库安全

任何安全系统的基础都是用户授权和身份确认。这是一种处理，通过这种处理对用户进行确认，保证他是有权用户，允许执行他要执行的操作。

一般说来，需要保护的操作有：

- 对数据库管理功能（创建表、更改或删除已存在的表等）的访问；
- 对特定数据库或表的访问；
- 访问的类型（只读、对特定列的访问等）；
- 仅通过视图或存储过程对表进行访问；
- 创建多层次的安全措施，从而允许多种基于登录的访问和控制；
- 限制管理用户账号的能力。

安全性使用SQL的 GRANT 和 REVOKE 语句来管理，不过，大多数DBMS提供了交互式的管理实用程序，这些实用程序在内部使用 GRANT 和 REVOKE 语句。
