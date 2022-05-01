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













