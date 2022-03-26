笔记参考视频：[Java入门基础视频教程，java零基础自学首选黑马程序员Java入门教程（含Java项目和Java真题）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Cv411372m?)

# Java基础语法

## 1. 数据类型

### 1.1 计算机存储单元

计算机存储设备的最小信息单元叫“位（bit）”，又称为“比特位”，用小写的字母“b”表示。而计算机中最小的存储单元叫“字节（byte）”，用大写字母“B”表示，字节是由连续的8个位组成。

### 1.2 数据类型

- 基本数据类型
  - 数值型
    - 整数（byte,short,int,long）
    - 浮点数（float,double）
    - 字符（char）
  - 非数值型
    - 布尔（boolean）
- 引用数据类型
  - 类（class）
  - 接口（interface）
  - 数组（ [ ] ）

### 1.3 数据类型内存占用和取值范围

| 数据类型 | 占用内存 |                           取值范围                           |
| :------: | :------: | :----------------------------------------------------------: |
|   byte   |    1     |                           -128~127                           |
|  short   |    2     |                         -32768~32767                         |
|   int    |    4     |                    -2147483648~2147483647                    |
|   long   |    8     |           -9223372036854775808~9223372036854775808           |
|  float   |    4     | 负数：-3.402823E+38~到-1.401298E-45     正数：1.401298E-45到3.402823E+38 |
|  double  |    8     | 负数：-1.797693E+308~到-4.9000000E-324     正数：4.9000000E-324到1.797693E+308 |
|   char   |    2     |                           0到65535                           |
| boolean  |    1     |                         true / false                         |



## 2. 标识符

### 2.1 标识符定义规则

- 由数字，字母，下划线，和美元符组成
- 不能以数字开头
- 不能是关键字
- 区分大小写

### 2.2 常见命名约定

小驼峰命名法：==方法，变量==

- 标识符是一个单词的时候，首字母小写

  `name`

- 标识符由多个单词组成的时候，第一个单词首字母小写，其他单词首字母大写

  `firstName`

大驼峰命名法：==类==

- 标识符是一个单词的时候，首字母大写

  `Student`

- 标识符是由多个单词组成的时候，每个单词的首字母大写

  `SuperStudent`



## 3. 逻辑运算符

### 3.1 逻辑运算符

| 符号 |   作用   |          说明           |
| :--: | :------: | :---------------------: |
|  &   |  逻辑与  |     a&b，都真才为真     |
|  \|  |  逻辑或  |    a\|b，都假才为假     |
|  ^   | 逻辑异或 | a^b，不同为真，相同为假 |
|  !   |  逻辑非  |     !a，结果与a相反     |

### 3.2 短路逻辑运算符

| 符号 |  作用  |               说明               |
| :--: | :----: | :------------------------------: |
|  &&  | 短路与 | 作用与 & 相同，但是会有短路效果  |
| \|\| | 短路或 | 作用与 \| 相同，但是会有短路效果 |

**短路效果：**

- 逻辑与&，无论左边真假，右边都要执行

  短路与&&，如果左边为真，右边执行；如果左边为假，右边不执行

- 逻辑与|，无论左边真假，右边都要执行

  短路与||，如果左边为假，右边执行；如果左边为真，右边不执行



注：当&或者|两边的表达式不是boolean类型的时候，运算符表示按位操作

例如：12&5    12的二进制数：0000 1100，5的二进制数：0000 0101，则12&5按位运算后结果为：0000 0100(4)。



## 4. 数据输入

### 4.1 Scanner使用的基本步骤

1. 导包(如果编译器自带，则不需要)

   `import java.util.Scanner;`

1. 创建对象

   `Scanner sc=new Scanner(System.in);`

2. 接收数据

   `int i=sc.nextInt();`



## 5. 随机数Random

### 5.1 Random的作用和使用步骤

作用：产生一个随机数

使用步骤：

1. 导包

   `import java.util.Random;`

1. 创建对象

   `Random r=new Randow();`

2. 获取随机数

   `int number=r.nextInt(10);	//获取数据的范围：[0, 10)`



## 6. 数组

数组就是用来存储一批同种类型数据的内存区域。（可以理解为一个容器）

### 6.1 数组的定义

#### 6.1.1 静态初始化数组

在定义数组的时候直接给数组赋值

格式：

```java
//完整格式
数据类型[] 数组名 = new 数据类型[]{元素1，元素2，······};
double[] scores = new double[]{80.5, 85.0, 90.5};
int [] ages = new int[]{12, 24, 35};
```

```java
//简化格式
数据类型[] 数组名 = {元素1，元素2，······};
int [] ages = {12, 24, 35};
```

用C++的写法也可以，``int ages[] = {12, 24, 35};`

基本原理：

`int [] ages = {12, 24, 35};`

先new一段内存存放12，24，35     再将这段内存的地址赋给ages，数组名通过引用来操作。

数组变量名中存储的是数组在内存中的地址，数组是引用类型。



数组的长度获取：`数组名.length`

==当两个数组变量指向同一个数组对象时，两个数组变量都存放这个数组对象的地址，无论哪个变量改变这个数组对象的元素值，都会改变其值。

#### 6.1.2 动态初始化数组

在定义数组的时候只确定元素的类型和数组的长度，之后再存入具体数据。

格式：

```java
数据类型[] 数组名 = new 数据类型[长度];
int [] arr = new int[3];
arr[0]=10;
```

**注意：**静态和动态初始化数组不可以混用。



## 7. 方法

方法时一种语法结构，它可以把一段代码封装成一个功能，以方便重复调用。

使用方法的好处：

- 提高代码的复用性
- 让程序的逻辑更加清晰

方法的内存运行机制：

未调用时，在方法区，调用运行时，在栈区。

### 7.1 方法的定义

格式：

```java
修饰符 返回值类型 方法名(形参列表){
       方法体代码
       return 返回值;
}
```

示例：

```
//使用方法对2个整数求和并返回
public static int sum(int a, int b){
	int c=a+b;
	return c;
}
```





## 8. 案例

### 8.1 验证码

定义方法实现随机产生n位的验证码，每位可能是数字，大小写字母

```java
import java.util.Random;
import java.util.Scanner;

public class Test {
    public static void main(String[] args){
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入想要获取验证码的位数：");
        int tm=sc.nextInt();
        System.out.println("您的验证码为: "+creatCode(tm));
    }
    public static String creatCode(int n){
        Random r=new Random();
        String code="";
        for (int i = 0; i < n; i++) {
            int temp=r.nextInt(3);
            switch(temp){
                case 0:
                    code+=(char)(r.nextInt(26)+65);     //A-Z
                    break;
                case 1:
                    code+=(char)(r.nextInt(26)+97);     //a-z
                    break;
                case 2:
                    code+=r.nextInt(10);
                    break;
            }
        }
        return code;
    }
}
```



### 8.2 数字加密

加密规则：先得到每位数，然后每位数都加上5，再对10取余，最后将所有数字反转，得到一串新数。

```java
public class Test {
    public static void main(String[] args){
        System.out.println("请输入需要加密的数字位数：");
        Scanner sc=new Scanner(System.in);
        int length=sc.nextInt();
        int[] arr=new int[length];
        for (int i = 0; i < length; i++) {
            System.out.println("请输入第"+(i+1)+"个数字：");
            arr[i]=sc.nextInt();
        }
        System.out.println("加密前的数字为：");
        printArray(arr);
        System.out.println("加密后的数字为：");
        addMi(arr);
        printArray(arr);
    }
    public static void addMi(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            arr[i]=(arr[i]+5)%10;
        }
        for (int j = 0, k = arr.length-1; j<k; j++,k--){       //j为数组首，k为数组尾，当j<k时，交换j，k位置的数据
            int temp=arr[j];
            arr[j]=arr[k];
            arr[k]=temp;
        }
    }
    public static void printArray(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]+" ");
        }
        System.out.println();
    }
}
```



### 8.3 双色球系统

1. 业务分析、随机产生一组中奖号码

   投注号码由6个红色球号码和一个蓝色球号码组成。红色球号码从1-33中选择；蓝色球号码从1-16中选择。

   ​												         双色球中奖条件和奖金表

   ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f22ce608a1e418179e1ec2127c367312.png)

2. 用户输入一组双色球号码

3. 判断中奖情况

```java
import java.util.Random;
import java.util.Scanner;

public class Test {
    public static void main(String[] args) {
        int[] luckyNumbers=createLuckNumber();
        int[] userNumbers=userInputNumbers();
        judge(luckyNumbers,userNumbers);
    }

    //生成一组中奖号码
    public static int[] createLuckNumber() {
        int[] numbers = new int[7];
        Random r = new Random();
        //生成前6个不重复的红球号码
        for (int i = 0; i < numbers.length - 1; i++) {
            while (true) {
                int data = r.nextInt(33) + 1;
                boolean flag=true;      //标记当前没有重复号码为true
                for (int j = 0; j < i; j++) {       //将当前生成的随机数与之前的随机数作比较
                    if(numbers[j]==data){
                        flag=false;     //当有重复号码时，标记为false，跳出此次循环，继续生成随机数
                        break;
                    }
                }
                if(flag){
                    numbers[i]=data;
                    break;
                }
            }
        }
        //随机生成最后一个蓝球号码
        numbers[numbers.length-1]=r.nextInt(16)+1;
        return numbers;
    }

    //用户输入一组双色球号码
    public static int[] userInputNumbers(){
        int[] numbers=new int[7];
        Scanner sc=new Scanner(System.in);
        for (int i = 0; i < numbers.length-1; i++) {
            System.out.println("请输入第"+(i+1)+"个红球号码（1-33，不能重复）：");
            numbers[i]=sc.nextInt();
        }
        System.out.println("请输入一个篮球号码（1-16）：");
        numbers[numbers.length-1]=sc.nextInt();
        return numbers;
    }

    //判断中奖情况
    public static void judge(int[] luckNumbers,int[] userNumbers){
        //定义2个变量分别存储红球和蓝球命中的个数
        int redHitNumbers=0;
        int blueHitNumbers=0;
        //判断红球命中个数
        for (int i = 0; i < userNumbers.length-1; i++) {
            for (int j = 0; j < luckNumbers.length-1; j++) {
                if(userNumbers[i]==luckNumbers[j]){
                    redHitNumbers++;
                    break;
                }
            }
        }
        //判断红球命中个数
        blueHitNumbers=userNumbers[userNumbers.length-1]==luckNumbers[luckNumbers.length-1]?1:0;
        //给用户提示中奖号码
        System.out.println("您投注的号码为：");
        print(userNumbers);
        System.out.println("中奖号码为：");
        print(luckNumbers);
        System.out.println("您命中的红球个数："+redHitNumbers);
        System.out.println("您命中的篮球个数："+blueHitNumbers);
        //判断中奖情况
        if(redHitNumbers<3&&blueHitNumbers==1){
            System.out.println("恭喜您，中了六等奖，5元！");
        }else if((redHitNumbers==3&&blueHitNumbers==1)
                ||(redHitNumbers==4&&blueHitNumbers==0)){
            System.out.println("恭喜您，中了五等奖，10元！");
        }else if((redHitNumbers==4&&blueHitNumbers==1)
                ||(redHitNumbers==5&&blueHitNumbers==0)){
            System.out.println("恭喜您，中了四等奖，200元！");
        }else if(redHitNumbers==5&&blueHitNumbers==1){
            System.out.println("恭喜您，中了三等奖，3000元！");
        }else if(redHitNumbers==6&&blueHitNumbers==0){
            System.out.println("恭喜您，中了二等奖，500万元！");
        }else if(redHitNumbers==6&&blueHitNumbers==1){
            System.out.println("恭喜您，中了一等奖，1000万元！");
        }else{
            System.out.println("很遗憾，您未中奖，祝您生活愉快！");
        }
    }

    //打印数组
    public static void print(int[] arr){
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]+"\t");
        }
        System.out.println();
    }
}
```



# 面向对象

## 1. 设计对象并使用

### 1.1 设计类，创建对象并使用

类：对象共同特征的描述；对象：真实存在的具体实例。

定义类的格式：

```
public class 类名{
	1、成员变量（代表属性，一般是名词）
	2、成员方法（代表行为，一般是动词）
	3、构造器
	4、代码块
	5、内部类
}
```

**注意：**

一个Java文件中可以定义多个class类，但只能有一个类是public修饰，而且public修饰的类名必须是代码文件名。

但在实际开发中，建议==一个文件定义一个class类==。



## 2. 对象的内存图

### 2.1 多个对象的内存图

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e2d830ddcbcb1ac12759d8ece361d506.png)

### 2.2 两个对象指向同一个类

当两个对象指向同一个类时，两个对象存放的地址相同，无论哪个对象改变的成员变量改变，另一个对象也会随之改变。



### 2.3 垃圾回收

- 当堆内存中的类对象或数组对象，没有被任何变量引用（指向）时，就会被判定为内存中的“垃圾”。
- Java的虚拟机中存在自动垃圾回收器，会定期进行清理。



## 3. 面向对象案例 — 购物车

需求：模拟购物车模块的功能，需要实现添加商品到购物车中去，同时需要提供修改商品的购买数量，结算商品价格的功能。

分析：

- 购物车中的每个商品都是一个对象，需要定义一个商品类
- 购物车本身也是一个对象：可以使用数组对象代表它
- 完成界面架构，让用户选择操作的功能

Goods类：

```java
public class Goods {
    int id;
    String name;
    double price;
    int buyNumber;
}
```

ShopCar类：

```java
import java.util.Scanner;

public class ShopCar {
    public static void main(String[] args) {
        Goods[] shopCar=new Goods[100];     //定义购物车对象
        //菜单
        while (true) {
            System.out.println("选择如下：");
            System.out.println("添加商品到购物车：add");
            System.out.println("查询购物车商品：query");
            System.out.println("修改商品购买数量：update");
            System.out.println("结算：pay");
            Scanner sc=new Scanner(System.in);
            System.out.println("请输入您的选择：");
            String command=sc.next();
            switch(command){
                case "add":
                    addGoods(shopCar);
                    break;
                case "query":
                    queryGoods(shopCar);
                    break;
                case "update":
                    updateGoods(shopCar);
                    break;
                case "pay":
                    pay(shopCar);
                    break;
                default:
                    System.out.println("没有该功能，请重新输入！");
            }
        }
    }

    public static void addGoods(Goods[] shopCar){
        Goods g=new Goods();
        Scanner sc=new Scanner(System.in);
        System.out.println("请输入购买商品的编号：");
        g.id=sc.nextInt();
        System.out.println("请输入购买商品的名称：");
        g.name=sc.next();
        System.out.println("请输入购买商品的数量：");
        g.buyNumber=sc.nextInt();
        System.out.println("请输入购买商品的价格：");
        g.price=sc.nextDouble();
        for (int i = 0; i < shopCar.length ; i++) {
            if(shopCar[i]==null){
                shopCar[i]=g;
                break;
            }
        }
        System.out.println("添加成功！");
    }

    public static void queryGoods(Goods[] shopCar){
        System.out.println("=================查询购物车信息如下=================");
        System.out.println("编号\t\t名称\t\t价格\t\t购买数量");
        for (int i = 0; i < shopCar.length; i++) {
            if(shopCar[i]!=null){
                System.out.print(shopCar[i].id+"\t\t\t"+shopCar[i].name+"\t\t"+shopCar[i].price+"\t\t"+shopCar[i].buyNumber);
                System.out.println();
            }else{
                break;
            }
        }
    }

    public static void updateGoods(Goods[] shopCar){
        System.out.println("请输入需要修改数量的商品id：");
        Scanner sc=new Scanner(System.in);
        int tempId=sc.nextInt();
        for (int i = 0; i < shopCar.length; i++) {      //遍历购物车
            if (shopCar[i]!=null) {        //购物车中不一定有100个商品，所以加个限制条件
                if(tempId==shopCar[i].id){
                    System.out.println("请输入修改后的商品数量：");
                    shopCar[i].buyNumber=sc.nextInt();
                    System.out.println("修改成功！");
                    break;
                }
            }else{
                System.out.println("该购物车没有此商品，请先添加商品！");
                break;
            }
        }
    }

    public static void pay(Goods[] shopCar){
        double sum=0;
        for (int i = 0; i < shopCar.length; i++) {
            if(shopCar[i]!=null){
                sum+=shopCar[i].buyNumber*shopCar[i].price;
            }else{
                if(i==0){           //若购物车一个商品也没有，则结算失败
                    System.out.println("您的购物车为空，请先添加商品到购物车！");
                    return;
                }
                break;
            }
        }
        System.out.println("您购买的商品信息如下：");
        System.out.println("编号\t\t名称\t\t价格\t\t购买数量");
        for (int i = 0; i < shopCar.length; i++) {
            if(shopCar[i]!=null){
                System.out.print(shopCar[i].id+"\t\t\t"+shopCar[i].name+"\t\t"+shopCar[i].price+"\t\t"+shopCar[i].buyNumber);
                System.out.println();
            }
        }
        System.out.println("您需要支付的总金额为: "+sum);
        System.out.println("结算成功，欢迎下次光临！");
    }
}
```

测试案例：

```
选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
add

请输入购买商品的编号：
001
请输入购买商品的名称：
卡西欧
请输入购买商品的数量：
2
请输入购买商品的价格：
200.0
添加成功！

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
add

请输入购买商品的编号：
002
请输入购买商品的名称：
天梭
请输入购买商品的数量：
3
请输入购买商品的价格：
500.0
添加成功！

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
query

=================查询购物车信息如下=================
编号		名称		价格		购买数量
1		   卡西欧	    200.0	      2
2		   天梭	     500.0		3

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
update

请输入需要修改数量的商品id：
001
请输入修改后的商品数量：
1
修改成功！

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
update

请输入需要修改数量的商品id：
002
请输入修改后的商品数量：
1
修改成功！

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：
pay

您购买的商品信息如下：
编号		名称		价格		购买数量
1		  卡西欧	   200.0	  1
2		  天梭	    500.0	     1
您需要支付的总金额为: 700.0
结算成功，欢迎下次光临！

选择如下：
添加商品到购物车：add
查询购物车商品：query
修改商品购买数量：update
结算：pay
请输入您的选择：

```



## 4. 构造器

构造器的作用：用于初始化一个类的对象，并返回对象的地址。

例：`Car c = new Car();`      创建对象实际上就是new一个构造器

构造器的定义格式：`修饰符 类名(形参列表){ ······ }`		（类似于C++的构造函数）

```
public class Car{
	//无参构造器
	public Car(){
		······
	}
	//有参构造器
	public Car(String n, int a){
		······
	}
}
```

**注意：*

- 任何类定义出来，都带有无参构造器
- 一旦定义了有参构造器，无参构造器就没有了，需要自己提供



## 5. this关键字

- 作用：出现在成员方法、构造器中代表当前对象的地址，用于访问当前对象的成员变量、成员方法



## 6. 封装

面向对象的三大特征：封装、继承、多态

### 6.1 封装的实现步骤

- 一般对成员变量使用private关键字修饰进行隐藏，private修饰后该成员变量就只能在当前类中访问。
- 提供public修饰公开的getter、setter方法来取值和赋值。

示例：

```java
public class Test {
    public static void main(String[] args) {
        Student stu=new Student();
        stu.setAge(18);
        System.out.println("年龄为："+stu.getAge());
    }
}

public class Student {
    private int age;

    public int getAge(){
        return age;
    }
    public void setAge(int age){
        if(age>=0&&age<=200){
            this.age=age;
        }else{
            System.out.println("请输入符合实际的年龄！");
        }
    }
}
```

封装的好处：

1. 加强了程序代码的安全性
2. 适当的封装可以提高开发效率，同时可以让程序更容易理解和维护



## 7. 标准JavaBean

可以理解成实体类，其对象可以用于在程序中封装数据。

**标准Javabean须满足如下要求：**

- 成员变量使用==private==修饰
- 提供每一个成员变量对应的==setXxx() / getXxx()==
- 必须提供一个==无参构造器==(默认自带)

标准Javabean示例：

```java
public class User {
    private String name;
    private double height;
    private double salary;

    public User() {
    }

    public User(String name, double height, double salary) {
        this.name = name;
        this.height = height;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        this.height = height;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }
}
```



# 常用API（String、ArrayList类）

**API(Application Programming Interface,应用程序编程接口)**

- Java写好的技术（功能代码），可以直接使用。
- Oracle也为Java提供的这些功能代码提供了相应的API文档（技术使用说明书），可到官网下载查阅。

## 1. String类

### 1.1 String类概述

- java.lang.String类代表字符串，String类定义的变量可以用于指向字符串对象，然后操作该字符串。

特点：

- String常被称为不可变字符串类型，他的对象在创建后不能被更改。（只是字符串变量指向了新的字符串对象的地址，原来的依旧存在，没有被改变）

### 1.2 创建字符串对象

- 方式一：直接使用 " " 定义。（常用）

- 方式二：通过String类的构造器创建对象。

  |             构造器             |                  说明                   |
  | :----------------------------: | :-------------------------------------: |
  |        public String()         |   创建一个空白字符串，不含有任何内容    |
  | public String(String original) | 根据传入的字符串内容，来 创建字符串对象 |
  |   public String(char[] chs)    |  根据字符数组的内容，来创建字符串对象   |
  |   public String(byte[] chs)    |  根据字节数组的内容，来创建字符串对象   |



```java
public class Test {
    public static void main(String[] args) {
        //1、public String()
        String s1=new String();
        //2、public String(String original)
        String s2=new String("中国");
        //3、public String(char[] chs)
        char[] c={'我','爱','你'};
        String s3=new String(c);
        //4、Public String(byte[] chs)
        byte[] b={97,98,99,65,66,67};
        String s4=new String(b);

        System.out.println(s1);     //  
        System.out.println(s2);     //中国
        System.out.println(s3);     //我爱你
        System.out.println(s4);     //abcABC
    }
}
```



**两种方式的区别：**

- 以 " " 方式给出的字符串对象，在字符串常量池中储存，而且相同内容只会在其中存储一份。
- 通过构造器new对象，每new一次都会产生一个新对象，放在堆内存中。

```
String s1="abc";
String s2="abc";
System.out.println(s1==s2);		//true

char[] chs={'a' , 'b' , 'c'};
String s3=new String(chs);
String s4=new String(chs);
System.out.println(s3==s4);		//false
```

内存图：

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a0cce7b373c7e07d2af8219807f2d34d.png)

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/92cb78c4a9f797d9c37ec70273007c95.png)

### 1.3 String常见问题

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7fe35b360a5b9cbb3437b73b06add884.png)

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/eb93d96d5cdc684cd528fe117ab09a4b.png)



### 1.4 String类常用API

#### 1.4.1 字符串内容比较

在Java中，当字符串使用==比较时，比较的是两边变量的地址，所以字符串内容的比较，不能使用 = =

- **可以用String类提供的 "equals" 比较，它只比较内容是否相同**

  ```
  String s1="abc123";
  String s2=new String("abc123");
  System.out.println(s1 == s2);           //false
  System.out.println(s1.equals(s2));      //true
  ```

- **当要忽略大小写（验证码）时，使用 "equalsIgnoreCase" **

  ```
  String s3="A3d8f";
  String s4="a3D8F";
  System.out.println(s3.equalsIgnoreCase(s4));		//true
  ```



#### 1.4.2 String其他常用API

| 方法名                                                       | 说明                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- |
| public int length()                                          | 返回此字符串的长度                                       |
| public char charAt(int index)                                | 获取某个索引位置处的字符                                 |
| public char[] toCharArray()：                                | 将当前字符串转换成字符数组返回                           |
| public String substring(int beginIndex, int endIndex)        | 根据开始和结束索引进行截取，得到新的字符串（包前不包后） |
| public String substring(int beginIndex)                      | 从传入的索引处截取到末尾，得到新的字符串                 |
| public String replace(CharSequence target, CharSequence replacement) | 使用新值，将字符串的旧值替换，得到新的字符串             |
| public boolean contains(CharSequence s)                      | 判断字符串中有没有指定的子串(须相连)                     |
| public boolean startsWiths(String prefix)                    | 判断字符串是否以prefix开头                               |
| ublic String[] split(String regex)                           | 根据传入的规则切割字符串，得到字符串数组返回             |

**示例：**

```java
public class Test {
    public static void main(String[] args) {
        //1、public int length()
        String s1="我爱你中国";
        System.out.println(s1.length());        //5

        //2、public char charAt(int index)
        char c1=s1.charAt(1);
        System.out.println(c1);                 //爱
        //也可以用charAt来遍历字符串
        for (int i = 0; i < s1.length(); i++) {
            System.out.print(s1.charAt(i));     //我爱你中国
        }

        //3、public char[] toCharArray()：
        char[] c2=s1.toCharArray();
        for (int i = 0; i < c2.length; i++) {       //遍历字符串
            System.out.print(c2[i]);                //我爱你中国
        }

        //4、public String substring(int beginIndex, int endIndex)
        String s2="我骄傲，我是中国人";
        String rs1=s2.substring(4,8);
        System.out.println(rs1);             //我是中国
        //5、public String substring(int beginIndex)
        String rs2=s2.substring(4);
        System.out.println(rs2);             //我是中国人

        //6、public String replace(CharSequence target, CharSequence replacement)
        String s3="你个菜逼玩的真菜";
        String rs3=s3.replace("菜逼","***");
        System.out.println(rs3);

        //7、public boolean contains(CharSequence s)
        System.out.println(s3.contains("菜逼"));             //true
        System.out.println(s3.contains("你玩的真菜"));       //false

        //8、public boolean startsWiths(String prefix)
        System.out.println(s3.startsWith("你个菜逼"));      //true
        System.out.println(s3.startsWith("真菜"));          //false

        //9、public String[] split(String regex)
        String s4="杀猪饲料，井川里予，易梦玲，曹芬";
        String[] goddess=s4.split("，");
        for (int i = 0; i < goddess.length; i++) {
            System.out.println("第"+(i+1)+"位女神："+goddess[i]);
        }
        /*输出： 第1位女神：杀猪饲料
                      第2位女神：井川里予
                      第3位女神：易梦玲
                      第4位女神：曹芬 
        */
    }
}
```



### 1.4.3 String类案例

**案例1：验证码功能**

需求：

- 随机产生一个5位的验证码，每位可能是数字，大小写字母。

分析：

1. 定义一个String类型的变量存储a-z A-Z 0-9 之间的全部字符。
2. 循环5次，随机一个范围内的索引，获取对应字符连接起来即可。

```java
public class Test {
    public static void main(String[] args) {
        String systemCode="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        Random r=new Random();
        String userCode="";
        for (int i = 0; i < 5; i++) {
            int temp=r.nextInt(systemCode.length());
            userCode+=systemCode.charAt(temp);
        }
        System.out.println("您的验证码为："+userCode);
    }
}
```



**案例2：模拟用户登录功能**

需求：

- 模拟用户登录功能，最多只给三次机会

分析：

1. 系统后台定义好正确的登录名称，密码
2. 使用循环控制三次，让用户输入正确的登录名和密码，判断是否登录成功，登录成功则退出循环；登录失败给处提示，并让用户继续登录

```java
public class Test {
    public static void main(String[] args) {
        String sysLoginName="admin123";
        String sysPassword="abc123";
        Scanner sc=new Scanner(System.in);
        for (int i = 0; i < 3; i++) {
            System.out.println("请您输入登录名：");
            String loginName = sc.next();
            System.out.println("请您输入登录密码：");
            String password = sc.next();

            if (sysLoginName.equals(loginName)) {
                if (sysPassword.equals(password)) {
                    System.out.println("登录成功，欢迎您！");
                    break;
                } else {
                    System.out.println("登录失败，密码错误！您还剩余" + (2 - i) + "次登录机会。");
                }
            } else {
                System.out.println("登录失败，账户名错误！您还剩余" + (2 - i) + "次登录机会");
            }
            
            //当i=2都还没有跳出循环(输入成功)
            if(i==2){
                System.out.println("您输入账户密码次数错误过多，请2分钟后再重试！");
            }
        }
    }
}
```



**案例3：手机号码屏蔽**

需求：

- 以字符串的形式从键盘接收一个手机号，将中间的四位号码屏蔽。

分析：

1. 截取字符串前三位和后四位
2. 将截取后的两个字符串，中间加上 * * * * 进行拼接即可

```java
public class Test {
    public static void main(String[] args) {
       Scanner sc=new Scanner(System.in);
       System.out.println("请输入您的手机号码：");
       String phoneNum=sc.next();
       String before=phoneNum.substring(0,3);
       String end=phoneNum.substring(7,11);
       String newNum=before+"****"+end;
       System.out.println("屏蔽后的手机号为: "+ newNum);
    }
}
```



## 2. ArrayList集合

- ArrayList是集合中的一种，它支持索引。

- ArrayList集合的对象获取

  |       构造器       |         说明         |
  | :----------------: | :------------------: |
  | public ArrayList() | 创建一个空的集合对象 |

- ArrayList集合添加元素的方法

  |                方法名                 |                  说明                  |
  | :-----------------------------------: | :------------------------------------: |
  |        public boolean add(E d)        | 将指定的元素(任何类型)追加到此集合末尾 |
  | public void add(int index, E element) |   在此集合中的指定位置插入指定的元素   |

```java
public class Test {
    public static void main(String[] args) {
        ArrayList list=new ArrayList();
        list.add("Java");
        list.add("SE");
        list.add(666);
        System.out.println(list);		//[Java, SE, 666]

        list.add(2,"真的是");
        System.out.println(list);		//[Java, SE, 真的是, 666]
    }
}
```



### 2.1 泛型概述

- ArrayList<E>：其实就是一个泛型类，可以在编译阶段约束集合对象只能操作某种数据类型。

**举例：**

- ArrayList<String>：此集合只能操作字符串类型的元素。
- ArrayList<Integer>：此集合只能操作整数类型的元素。

==注意：集合中只能存储引用类型，不支持基本数据类型。==



### 2.2 ArrayList常用API

- ArrayList集合常用方法

|              方法名称              |                             说明                             |
| :--------------------------------: | :----------------------------------------------------------: |
|      public E get(int index)       |                     返回指定索引处的元素                     |
|         public int size()          |                     返回集合中的元素个数                     |
|     public E remove(int index)     |            删除指定索引处的元素，返回被删除的元素            |
|  public boolean remove(object o)   | 删除指定的元素，返回删除是否成功(当集合中有多个相同的元素时，只会删除前面第一个) |
| public E set(int index, E element) |            修改指定索引处的元素，返回被修改的元素            |

```java
public class Test {
    public static void main(String[] args) {
        ArrayList<String> list=new ArrayList<>();
        list.add("Java");
        list.add("MySQL");
        list.add("MyBatis");
        list.add("HTML");

        //1、public E get(int index)               返回指定索引处的元素
        String e=list.get(1);
        System.out.println(e);      //MySQL

        //2、public int size()                     返回集合中的元素个数
        System.out.println(list.size());    //4
        //遍历集合：
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }

        //3、public E remove(int index)            删除指定索引处的元素，返回被删除的元素
        String e2=list.remove(3);
        System.out.println(e2);        //HTML
        System.out.println(list);      //[Java, MySQL, MyBatis]

        //4、public boolean remove(object o)       删除指定的元素，返回删除是否成功
        System.out.println(list.remove("MyBatis"));     //true
        System.out.println(list);       //[Java, MySQL]

        //5、public E set(int index, E element)    修改指定索引处的元素，返回被修改的元素
        String e3=list.set(1,"JS");
        System.out.println(e3);         //MySQL
        System.out.println(list);       //[Java, JS]
    }
}
```



==注意：当需要按条件删除集合中的元素时，要采用倒删法。因为正删删除某个位置元素后，后面的元素会自动往前，就会遗漏某些元素。==



**案例1：影片信息表示**

需求:

- 某影院系统需要在后台存储三部电影，然后依次展示出来。

分析：

1. 三部电影是3个对象，定义一个电影类，定义一个集合存储电影对象
2. 创建3个电影对象，封装相关数据，把3个对象存到集合中
3. 遍历集合中的3个对象，输出影片信息

Movie类：

```java
public class Movie {
    private String name;
    private double score;
    private String actor;

    public Movie() {
    }

    public Movie(String name, double score, String actor) {
        this.name = name;
        this.score = score;
        this.actor = actor;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    public String getActor() {
        return actor;
    }

    public void setActor(String actor) {
        this.actor = actor;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        ArrayList<Movie> movies=new ArrayList<>();
        movies.add(new Movie("《肖生客的救赎》", 9.7, "罗宾斯"));
        movies.add(new Movie("《霸王别姬》", 9.6, "张国荣、张丰毅"));
        movies.add(new Movie("《阿甘正传》", 9.5, "汤姆·汉克斯"));
        
        //System.out.println(movies);       直接打印集合，是打印地址
        //采用遍历的方式打印：
        for (int i = 0; i < movies.size(); i++) {
            Movie movie=movies.get(i);
            System.out.println("片名："+movie.getName());
            System.out.println("评分："+movie.getScore());
            System.out.println("主演："+movie.getActor());
            System.out.println();
        }
    }
}
```

内存图分析：

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/52c71d6adba8ccabb9c59b09d4e8e010.png)

**结论：集合中存储的元素并不是对象本身，而是对象的地址。**



**案例2：学生信息系统的数据搜索**

需求：

- 后台程序需要存储学生信息并展示，然后要提供按学号搜索学生信息的功能

分析：

1. 定义Student类，定义ArrayList集合存储学生对象信息，并遍历展示出来
2. 提供一个方法，可以接收ArrayList集合和要搜索的学号，返回搜索到的对对象信息并展示
3. 使用死循环，让用户可以不断的搜索

Student类：

```java
public class Student {
    private String id;
    private String name;
    private int age;
    private String className;

    public Student() {
    }

    public Student(String id, String name, int age, String className) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.className = className;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getClassName() {
        return className;
    }

    public void setClassName(String className) {
        this.className = className;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        ArrayList<Student> students=new ArrayList<>();
        students.add(new Student("2005210211","叶孤城",23,"计科2班"));
        students.add(new Student("2005210212","东方不败",21,"网安1班"));
        students.add(new Student("2005210213","西门吹雪",27,"数媒6班"));
        students.add(new Student("2005210214","梅超风",25,"软工4班"));
        System.out.println("学号\t\t\t姓名\t\t年龄\t\t班级");
        for (int i = 0; i < students.size(); i++) {
            Student s=students.get(i);
            System.out.println(s.getId()+"\t\t"+s.getName()+"\t\t"+s.getAge()+"\t\t\t"+s.getClassName());
        }
        Scanner sc=new Scanner(System.in);
        while (true) {
            System.out.println("请输入您要查询学生的学号：");
            String tempId=sc.next();
            Student stu=getStudentById(students,tempId);
            if (stu == null) {
                System.out.println("查无此人！");
            }else{
                System.out.println("您查询的学生信息如下：");
                System.out.println(stu.getId()+"\t\t"+stu.getName()+"\t\t"+stu.getAge()+"\t\t\t"+stu.getClassName());
            }
        }
    }

    public static Student getStudentById(ArrayList<Student> students,String id){
        for (int i = 0; i < students.size(); i++) {
            Student s=students.get(i);
            if(s.getId().equals(id)){       //id是String类型
                return s;
            }
        }
        return null;
    }
}
```



# 综合案例 - ATM取款机

Account类：

```java
public class Account {
    private String cardId; // 卡号
    private String userName; // 用户名称
    private String passWord; // 密码
    private double money; // 账户余额
    private double quotaMoney; // 每次取现额度限度。

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassWord() {
        return passWord;
    }

    public void setPassWord(String passWord) {
        this.passWord = passWord;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    public double getQuotaMoney() {
        return quotaMoney;
    }

    public void setQuotaMoney(double quotaMoney) {
        this.quotaMoney = quotaMoney;
    }
}
```

```java
import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;

public class ATMSystem {
    public static void main(String[] args) {
        // 2、创建一个集合对象，用于后期存入账户对象。
        ArrayList<Account> accounts = new ArrayList<>();

        // 3、展示系统欢迎页面
        System.out.println("======欢迎您进入到黑马ATM系统===============");
        while (true) {
            System.out.println("1、登录账户");
            System.out.println("2、注册账户");
            System.out.println("请您选择操作：");
            Scanner sc = new Scanner(System.in);
            int command = sc.nextInt();
            switch (command){
                case 1:
                    // 登录操作
                    login(accounts, sc);
                    break;
                case 2:
                    // 注册账户
                    register(accounts, sc);
                    break;
                default:
                    System.out.println("当前输入的操作不存在！");
            }
        }
    }

    /**
       用户登录功能
     */
    private static void login(ArrayList<Account> accounts, Scanner sc) {
        System.out.println("==================欢迎您进入到登录操作======================");
        if(accounts.size() > 0){
            while (true) {
                System.out.println("请您输入登录的卡号：");
                String cardId = sc.next();
                // 2、根据卡号去集合中查询是否存在账户对象
                Account acc = getAccountByCardId(cardId , accounts);
                // 判断账户对象是否存在，存在说明卡号正确
                if(acc != null){
                    while (true) {
                        // 3、判断密码是否正确
                        System.out.println("请您输入登录的密码：");
                        String passWord = sc.next();
                        if(acc.getPassWord().equals(passWord)){
                            // 登录成功了！
                            System.out.println("欢迎你：" + acc.getUserName() + "先生/女士进入系统，您可开始办理你的业务了!");
                            // 展示登录成功后的操作界面。
                            showCommand(sc, acc, accounts);
                            return;
                        }else {
                            System.out.println("您的密码不正确！");
                        }
                    }
                }else {
                    System.out.println("卡号不存在，请确认！");
                }
            }
        }else {
            System.out.println("当前系统无任何账户，请先注册再登录！");
        }
    }

    /**
      登录后展示的界面
     */
    private static void showCommand(Scanner sc, Account acc, ArrayList<Account> accounts) {
        while (true) {
            System.out.println("==================欢迎您进入到操作界面======================");
            System.out.println("1、查询");
            System.out.println("2、存款");
            System.out.println("3、取款");
            System.out.println("4、转账");
            System.out.println("5、修改密码");
            System.out.println("6、退出");
            System.out.println("7、注销账户");
            System.out.println("请您输入操作命令：");
            int command = sc.nextInt();
            switch (command) {
                case 1:
                    // 查询账户信息展示
                    showAccount(acc);
                    break;
                case 2:
                    // 存款
                    depositMoney(acc,sc);
                    break;
                case 3:
                    // 取款
                    drawMoney(acc,sc);
                    break;
                case 4:
                    // 转账
                    transferMoney(acc, accounts, sc);
                    break;
                case 5:
                    // 修改密码
                    updatePassWord(acc, sc);
                    return; // 跳出当前操作的方法，最终回到首页
                case 6:
                    // 退出 回到首页
                    System.out.println("欢迎下次继续光临！！");
                    return; // 结束登录后的全部操作
                case 7:
                    // 注销账户
                    accounts.remove(acc); // 从集合对象中删除当前账户对象。
                    System.out.println("您的账户已经完成了销毁，您将不可以进行登录了！");
                    return;
                default:
                    System.out.println("您的操作命令有误！");
            }
        }

    }


    /**
      修改当前账户对象的密码
     * @param acc
     */
    private static void updatePassWord(Account acc, Scanner sc) {
        // 1、判断旧密码是否正确
        while (true) {
            System.out.println("请您输入当前密码认证：");
            String passWord = sc.next();
            if(acc.getPassWord().equals(passWord)){
                while (true) {
                    // 2、输入新密码
                    System.out.println("请您输入新密码：");
                    String newPassWord = sc.next();
                    System.out.println("请您确认新密码：");
                    String okPassWord = sc.next();
                    // 3、比对两次密码是否一致
                    if(newPassWord.equals(okPassWord)){
                        acc.setPassWord(okPassWord);
                        System.out.println("密码已经修改成功，请重新登录！");
                        return;
                    }else {
                        System.out.println("两次密码不一致！");
                    }
                }
            }else {
                System.out.println("您输入的密码有误。请重新确认密码！");
            }
        }
    }

    /**
     * 从当前账户对象中把金额转给其他账户对象。
     * @param acc
     * @param accounts
     * @param sc
     */
    private static void transferMoney(Account acc, ArrayList<Account> accounts, Scanner sc) {
        // 1、判断自己的账户中是否有钱
        if(acc.getMoney() <= 0){
            System.out.println("您自己都没钱，就别转了吧！");
            return;
        }

        // 2、判断总账户数量是否大于等于2个。
        if(accounts.size() >= 2){
            while (true) {
                // 3、让当前用户输入对方的账号进行转账
                System.out.println("请您输入对方卡号：");
                String cardId = sc.next();

                // 4、根据卡号查询出集合中的账户对象
                Account otherAcc = getAccountByCardId(cardId , accounts);
                // 5、判断账户对象是否存在，而且这个账户对象不能是自己。
                if(otherAcc != null){
                    // 6、判断当前账户是否是自己。
                    if(acc.getCardId().equals(otherAcc.getCardId())){
                        System.out.println("不能给自己账户转账！");
                    }else {
                        // 7、正式进入到转账逻辑了
                        // 黑马刘德华
                        String rs = "*" + otherAcc.getUserName().substring(1);
                        System.out.println("请您确认["+rs +"]的姓氏来确认！");
                        System.out.println("请您输入对方的姓氏：");
                        String preName = sc.next();
                        if(otherAcc.getUserName().startsWith(preName)){
                            // 认证通过
                            while (true) {
                                System.out.println("请您输入转账的金额（您最多可以转账：" + acc.getMoney() +"元）：");
                                double money = sc.nextDouble();
                                if(money > acc.getMoney()){
                                    System.out.println("你不听话，没有这么多钱可以转！");
                                }else {
                                    // 开始转
                                    acc.setMoney(acc.getMoney() - money); // 更新自己账户
                                    otherAcc.setMoney(otherAcc.getMoney() + money);
                                    System.out.println("您已经完成转账！您当前还剩余：" + acc.getMoney());
                                    return;
                                }
                            }

                        }else {
                            System.out.println("您输入对方的信息有误！");
                        }
                    }
                }else {
                    System.out.println("您输入的转账卡号不存在！");
                }
            }
        }else {
            System.out.println("当前系统中没有其他账户可以转账，去注册一个账户吧！");
        }

    }

    private static void drawMoney(Account acc, Scanner sc) {
        System.out.println("==================欢迎进入账户取款操作======================");
        // 1、判断账户的余额是否高于等于100
        double money = acc.getMoney();
        if(money >= 100) {
            while (true) {
                // 2、输入取钱的金额
                System.out.println("请您输入取钱的金额：");
                double drawMoney = sc.nextDouble();
                // 3、判断取钱金额是否超过了当次限额
                if(drawMoney > acc.getQuotaMoney()){
                    System.out.println("您当前取款金额超过了每次限额！");
                }else {
                    // 4、判断当前取钱金额是超过了账户的余额
                    if(drawMoney > money){
                        System.out.println("当前余额不足！当前余额是：" + money);
                    }else {
                        // 更新账户余额
                        acc.setMoney(money - drawMoney);
                        System.out.println("您当前取钱完成，请拿走你的钱，当前剩余余额是：" + acc.getMoney());
                        break;
                    }
                }
            }
        }else {
            System.out.println("您当前账户余额不足100元，存钱去吧！");
        }
    }

    private static void depositMoney(Account acc, Scanner sc) {
        System.out.println("==================欢迎进入账户存款操作======================");
        System.out.println("请您输入存款金额：");
        double money = sc.nextDouble();
        acc.setMoney(acc.getMoney() + money);
        showAccount(acc);
    }

    private static void showAccount(Account acc) {
        System.out.println("==================您当前账户详情信息如下======================");
        System.out.println("卡号：" + acc.getCardId());
        System.out.println("户主：" + acc.getUserName());
        System.out.println("余额：" + acc.getMoney());
        System.out.println("当次取现额度：" + acc.getQuotaMoney());
    }

    /**
        开户功能
     */
    private static void register(ArrayList<Account> accounts, Scanner sc) {
        System.out.println("==================欢迎您进入到开户操作======================");
        // 2、创建一个账户对象封装账户信息
        Account acc = new Account();
        // 1、录入用户账户信息
        System.out.println("请您输入账户名称：");
        String userName =sc.next();
        acc.setUserName(userName);

        while (true) {
            System.out.println("请您输入账户密码：");
            String passWord =sc.next();
            System.out.println("请您输入确认密码：");
            String okPassWord =sc.next();
            if(okPassWord.equals(passWord)){
                // 密码无问题
                acc.setPassWord(okPassWord);
                break;
            }else {
                System.out.println("两次输入的密码不一致！");
            }
        }
        System.out.println("请您设置当次取现额度：");
        double quataMoney = sc.nextDouble();
        acc.setQuotaMoney(quataMoney);
        // 关键点：为当前账户生成一个随机的8位数字作为卡号，卡号不能与其他用户的卡号重复。
        String cardId = createCardId(accounts);
        acc.setCardId(cardId);

        // 3、把账户对象存入到集合容器对象中去
        accounts.add(acc);
        System.out.println("恭喜您,"+acc.getUserName()+"先生/女士，您开户完成，您的卡号是：" + acc.getCardId());
    }

    public static String createCardId(ArrayList<Account> accounts){
        while (true) {
            String cardId = "";
            // 随机8个数字
            Random r = new Random();
            for (int i = 1; i <= 8 ; i++) {
                cardId += r.nextInt(10);
            }
            // 判断这个卡号是否重复：根据卡号去查询账户对象
            Account account = getAccountByCardId(cardId , accounts);
            if(account == null){
                return cardId;
            }
        }
    }

    public static Account getAccountByCardId(String cardId , ArrayList<Account> accounts){
        for (int i = 0; i < accounts.size(); i++) {
            Account acc = accounts.get(i);
            if(acc.getCardId().equals(cardId)){
                return acc;
            }
        }
        return null;
    }
}

```





# 面向对象进阶

## 1. static关键字

**static关键字的作用：**

- static是静态的意思，可以修饰成员变量，表示该成员变量只在内存(堆区)中存储一份，可以被共享访问、修改

### 1.1 成员变量

成员变量分为两种：

- 静态成员变量（有static修饰，属于类，内存中加载一次）：常表示在线人数信息，需要被共享的信息，可以被共享访问。

  静态成员变量可以使用`类名.静态成员变量`(推荐)访问，也可以使用`对象.静态成员变量`访问。

- 实例成员变量（无static修饰，存在于每个对象中）：常表示姓名name、年龄age、等属于每个对象的信息。

  实例成员变量使用`对象.实例成员变量`访问。

### 1.2 成员方法

成员方法分为两种：

- 静态成员方法（有static修饰，属于类和对象共享），可以使用类名/对象访问。（当方法是以执行一个通用功能为目的，或者需要方便访问时使用）
- 实例成员方法（无static修饰，属于对象），使用对象访问。

### 1.3 工具类

- 对于一些应用程序中多次需要用到的功能，可以将这些功能==封装成静态方法==，==放在一个类中==，这个类就是工具类。
- 作用：方便调用，提高了代码的复用性

示例：封装一个创建n位随机验证码的工具类，当想获取验证码时就直接调用。

```java
public class VerifyTool {
    public static String createCode(int n){
        String chars="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        String code="";
        Random r=new Random();
        for (int i = 0; i < n; i++) {
            int index=r.nextInt(chars.length());
            code+=chars.charAt(index);
        }
        return code;
    }
}
```

**注意：**

- 建议将工具类的构造器设为私有，不让工具类对外产生对象，节省内存。

- 工具类中的方法不用示例方法的原因：实例方法需要创建对象使用，此时用对象只是为了调用方法，浪费内存。而使用静态方法可以直接用类名调用。

**案例：定义数组工具类**

需求：

1. 数组对象直接输出的时候是输出对象的地址，而项目中很多地方需要返回数组的内容，请在ArrayUtils中提供一个工具类方法toString，用于返回整数数组的内容，返回的字符串格式如：[10，20，50，34，100]（只考虑整数一维数组）
2. 经常需要统计平均值，平均值位去掉最低分和最高分后的平均分值，请提供工具方法getAverage，用于返回平均分。
3. 定义一个测试类Test，调用该工具类的工具方法，并返回结果。

ArrayUtils工具类：

```java
public class ArrayUtils {
    private ArrayUtils(){
    }

    public static String toString(int[] arr){
        if(arr!=null) {
            String res = "[";
            for (int i = 0; i < arr.length; i++) {
                res += (i == arr.length - 1 ? arr[i] : arr[i] + "，");
            }
            res += "]";
            return res;
        }else{
            return null;
        }
    }

    public static double getAverage(int[] arr){
        int min=arr[0];
        int max=arr[0];
        int sum=0;
        for (int i = 0; i < arr.length; i++) {
            if(min>arr[i]){
                min=i;
            }
            if(max<arr[i]){
                max=i;
            }
            sum+=arr[i];
        }
        return(sum-max-min)*1.0/(arr.length-2);     //sum为int类型，*1.0保留小数
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        int[] arr={10,20,30};
        System.out.println(arr);        //[I@776ec8df
        System.out.println(ArrayUtils.toString(arr));       //[10，20，30]
        System.out.println(ArrayUtils.getAverage(arr));     //20.0
    }
}
```



**static访问注意事项：**

- 静态方法只能访问静态的成员(变量，方法都可)，不可以直接访问实例成员。
- 实例方法可以访问静态成员，也可以访问实例成员。
- 静态方法中是不可以出现this关键字的（静态方法用类调用，而this是指向当前对象的）



### 1.4 代码块

- 代码块是类的五大成分之一，定义在类中，方法外。

- 在Java类下，用{}括起来的代码被成为代码块。

代码块分为两种：

- 静态代码块
  - 格式：`static{}`
  - 特点：需要通过static关键字修饰，随着类的加载而加载(在main方法前)，并且自动触发、==只执行一次==
  - 使用场景：在类加载的时候做一些==静态数据初始化==的操作，以便后续使用

- 构造代码块(属于对象)，了解即可，用的少
  - 格式：`{}`
  - 特点：每次创建对象，调用构造器时，都会执行该代码块中的代码，并且在构造器执行前执行
  - 使用场景：初始化实例资源(一般不会用，会将所有对象都赋成相同的值)

**案例：**斗地主游戏

需求：

- 在启动游戏房间时，应该提前准备好54张牌，后续才可以直接使用这些牌数据

分析：

1. 定义一个静态的ArrayList集合存储54张牌对象，静态的集合只会加载一份
2. 在启动游戏房间前，应该将54张牌初始化好
3. 当系统启动的同时需要准备好54张牌数据，此时可以用静态代码块完成

```java
public class Test {
    static ArrayList<String> cards=new ArrayList<>();
    static{
        String[] colors={"♠","♥","♣","♦"};
        String[] sizes = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
        //先遍历点数，再组合花数，每个点组合4个花
        for (int i = 0; i < sizes.length; i++) {
            for (int j = 0; j < colors.length; j++) {
                cards.add(sizes[i]+colors[j]);
            }
        }
        //添加大小王
        cards.add("大🃏");
        cards.add("小🃏");
    }

    public static void main(String[] args) {
        System.out.println(cards);		//[输出54张牌]
    }
}
```



### 1.5 单例设计模式

什么是设计模式(Design pattern)

- 设计模式是一套被前人反复使用，多数人知晓，经过分类编目的代码设计经验的总结，后来者可以直接拿来解决问题
- 设计模式是软件设计中常见的解决方案，好的设计模式可以进一步的提高代码的重用性

**单例模式：**

- 可以保证系统中，应用该模式的这个类永远只有一个实例，即一个类永远只能创建一个对象

**单例的实现方式有饿汉单例模式、懒汉单例模式等等**

#### 1.5.1饿汉单例模式

-  再用类获取对象的时候，对象已经提前创建好了

设计步骤：

1. 定义一个类，把构造器私有(保证只在此类中创建一个对象，防止在其他地方创建对象)
2. 定义一个静态变量存储一个对象，此时要new对象(可以让其他地方通过类名访问到)

```java
public class SingleInstance {
    public static SingleInstance instance=new SingleInstance();
    private SingleInstance(){
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        SingleInstance s1=SingleInstance.instance;
        SingleInstance s2=SingleInstance.instance;
        SingleInstance s3=SingleInstance.instance;

        System.out.println(s1);     //com.demo.SingleInstance@4eec7777    
        System.out.println(s2);     //com.demo.SingleInstance@4eec7777
        System.out.println(s3);     //com.demo.SingleInstance@4eec7777
    }
}
```

**无论创建几次对象，都是同一个对象的地址**



#### 1.5.2 懒汉单例模式

- 在需要该对象的时候，才去创建一个对象（延迟加载效果）

设计步骤：

1. 定义一个类，把构造器私有
2. 定义一个静态变量存储一个对象，此时不要new对象
3. 提供一个返回单例对象的方法

```java
public class SingleInstance {
    //public static SingleInstance instance;
    private static SingleInstance instance;
    private SingleInstance(){
    }
    public static SingleInstance getInstance(){
        if(instance==null){
            instance=new SingleInstance();      //当第一次创建该对象时，先new一个，下次就直接返回instance即可
        }
        return instance;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        SingleInstance s1=SingleInstance.getInstance();
        SingleInstance s2=SingleInstance.getInstance();
        SingleInstance s3=SingleInstance.getInstance();

        System.out.println(s1);     //com.demo.SingleInstance@4eec7777
        System.out.println(s2);     //com.demo.SingleInstance@4eec7777
        System.out.println(s3);     //com.demo.SingleInstance@4eec7777
    }
}
```



**优化：当使用懒汉单例的时候，常常将变量instance设置成私有 `private static SingleInstance instance;`，因为类内提供了一个public的getInstance()，为了防止在其他地方直接调用该变量(此时为null)而引发bug，而饿汉单例因为就是要直接调用该变量，所以设置成public。**





## 2.继承

- 多个类继承单独的某个类，多个类就可以使用单独的这个类的属性和行为了，同时还可以有自己的特征和行为。
- 多个类称为子类（派生类），单独的这个类成为父类（基类/超类）。

继承的好处：

- 提高代码的复用性

继承的格式：

- 在Java中，继承的关键字是 "extends"。

  `public class 子类名 extends 父类名 {};`

**继承内存图：**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f3babb2ae2161a3ad7d4161a8bb586aa.png)



### 2.1 继承的特点

1. 子类可以继承父类的属性和行为，但是子类不能继承父类的构造器。
2. 子类可以继承父类private的属性和行为，只是不能直接访问。
3. 子类可以直接使用父类的静态成员，但是不能继承（个人观点）。
4. Java是单继承模式：一个类只能继承一个直接父类（防止出现二义性）。
5. Java不支持多继承，但是支持多层继承（B继承A，C再继承B）。
6. Java中所有的类都是Object类的子类（默认继承Object类）。



### 2.2 继承后：成员变量/方法的访问特点

**在子类方法中访问成员（成员变量/方法）满足：就近原则**

- 先 子类局部范围查找
- 然后子类成员范围查找
- 最后父类成员范围查找，如果父类范围还没有找到则报错

**如果子父类中，出现了重名的成员，会优先使用子类的，此时如果要在子类中使用使用父类的成员，需要加super关键字，指定访问父类的成员**

格式：`super.父类成员变量/方法;`



### 2.3 继承后：方法重写

- 在继承体系中，子类出现了和父类中一模一样的方法声明，就称子类这个方法是重写的方法。

- 当子类需要父类的功能，但父类的该功能不完全满足自己的需求时，子类可以重写父类中的方法。

**@Override注解：**

- @Override是放在重写后的方法上，作为重写是否正确的校验注解
- 加上该注解后如果重写错误，编译阶段就会出现错误提示

- 当重写方法时，推荐加上@Override，可以避免一些错误，更加安全。

**注意事项：**

- 方法重写时，子类的方法名和参数列表必须和父类一样
- 私有方法不能被重写

- 子类重写父类方法时，访问权限必须大于等于父类（缺省 < protected < public）

- 子类不能重写父类的静态成员方法（子类没有继承父类的静态成员），否则会报错



### 2.4 继承后：子类构造器的特点

- 子类中所有的构造器默认都会先访问父类中的无参构造器，再执行自己的。

原因：

- 子类初始化时，有可能会使用到父类中的数据，如果父类没有完成初始化，子类将无法使用父类的数据
- 子类初始化之前，一定要调用父类构造器先完成父类数据空间的初始化

子类构造器的第一行默认是`super();`，不写也存在



### 2.5 this和super关键字

- this：代表本类对象的引用；super：代表父类存储空间的标识。

- `this();`调用本类中的兄弟构造器；`super();`调用父类构造器

**注意：**

- ``this(···); super(···);``都只能放在构造器的第一行，所以二者不能共存再同一个构造器中。
- 子类通过`this();`去调用本类中的其他构造器，本类其他构造器会通过super去手动调用父类的构造器，最终还是会调用父类构造器的。

```java
public class Student{
	private String schoolName;
	private String name;
	
        //子类通过this()调用本类中其他构造器，此类不会调用父类构造器，但是调用其他构造器时，其他构造器会调用父类构造器
	public Student(String name){		
		this(name, "张三");
	}
	
	public Student(String name, String schoolName){
        	//super();			默认会调用父类构造器
		this.name=name;
		this.schoolName=schoolName;
	}
}
```





## 3. 包

- 包时用来分门别类的管理各种不同类，类似于文件夹、建包利于程序的管理和维护。
- 建包的语法格式：`package 公司域名倒写.技术名称`，建议包名全部英文小写，且具备意义。
- 建包语句必须在第一行，一般IDEA工具会帮助创建。

**导包：**

- 导包格式：`import 包名.类名;`

- 相同包下的类可以直接访问，不同包下的类必须导包，才可以使用！
- 假如一个类中需要用到不同类，而这两个类的名称时相同的，那么默认只能导入一个类，另一个类要带包名访问。（把路径写出来）



## 4. 权限修饰符

- 权限修饰符：用来控制一个成员能够被访问的范围
- 可以修饰成员变量，方法，构造器，内部类，不同权限修饰符修饰的成员能够被访问的范围将受到限制

**权限修饰符的分类和作用范围：**

|  修饰符   | 同一个类中 | 同一个包中其他类 | 不同包下的子类 | 不同包下的无关类 |
| :-------: | :--------: | :--------------: | :------------: | :--------------: |
|  private  |     √      |                  |                |                  |
|   缺省    |     √      |        √         |                |                  |
| protected |     √      |        √         |       √        |                  |
|  public   |     √      |        √         |       √        |        √         |



## 5. final关键字

final关键字可以修饰方法，变量，类。

- 修饰方法：表明该方法时最终方法，不能被重写
- 修饰变量：表示该变量第一次赋值后，不能再次被赋值
- 修饰类：表明该类时最终类，不能被继承

和C++的const类似

**final修饰变量的注意：**

- 修饰基本类型：变量存储的数据值不能发生改变
- 修饰引用类型：变量存储的地址值(数组)不能发生改名，但是地址指向的对象的内容可以发生改变



## 6. 常量

- 常量是使用了public static final修饰的成员变量，必须有初始化值，而且执行的过程中其值不能被改变
- 常量的作用和好处：可以用于系统的配置信息，方便程序的维护，同时也能提高可读性

常量命名规范：英文单词全部大写，多个单词用下划线连接起来。

**常量的执行原理：**

- 在编译阶段会进行“宏替换”，把使用常量的地方全部替换成真实的字面量(常量的值)
- 这样做的好处是让使用常量的程序的执行性能与直接使用字面量是一样的

**常量也常常用来做信息标志和分类，使代码的可读性更好，实现了软编码的形式。**



## 7. 枚举

- 枚举是一种特殊类型，为了做信息的标志和信息的分类。

定义格式：`修饰符 enum 枚举名称{ 第一行都是罗列枚举实例的名称};`

**反编译后观察枚举的特征：**



![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5ca4b17ab0c40bfd1446872b56ea4273.png)



案例说明：

- 接收用户输入的四个方向信号（上下左右），来控制人物的移动

选择常量做信息标志和分类：虽然可以实现可读性，但是入参值不受约束，代码相对不够严谨。

选择枚举做信息标志和分类：代码可读性好，入参约束严谨，是最好的信息分类技术，建议使用。

Orientation枚举类：

```java
public enum Orientation{
    UP,DOWN,LEFT,RIGHT;
}
```

当需要使用时，直接用类名访问即可。



## 8. 抽象类/方法

抽象类：

- 某个父类知道其所有子类要完成某功能，但是每个子类完成的内容都不一样，父类就只定义该功能的基本要求，具体实现由子类完成，这个类就可以是一个抽象类。
- 抽象类必须使用abstract修饰：`修饰符 abstract class 类名{}`

抽象方法：

- 抽象类中定义的子类必须完成的功能的基本要求
- 没有方法体，只有方法签名，必须abstract修饰   `修饰符 abstract 返回值类型 方法名称(形参列表);`



**抽象的使用总结与注意事项**：

- 抽象类可以理解成类的不完整设计图，是用来被子类继承的。

- 一个类如果继承了抽象类，那么这个类必须重写完抽象类的全部抽象方法，否则这个类也必须定义成抽象类。



**实例：**

**系统需求**

- 某加油站推出了2种支付卡，一种是预存10000的**金卡**，后续加油享受**8**折优惠，另一种是预存5000的**银卡** ,后续加油享受**8.5**折优惠。

- 请分别实现2种卡片进入收银系统后的逻辑，卡片需要包含主人名称，余额，支付功能。



**分析实现**

1. 创建一张卡片父类：定义属性包括主人名称、余额、支付功能（具体实现交给子类）
2. 创建一张白金卡类：重写支付功能，按照原价的8折计算输出。
3. 创建一张银卡类：重写支付功能，按照原价的8.5折计算输出。

```java
//抽象父类：

public abstract class Card {
    private String name;
    private double money;

    //子类支付的情况不一样，所以父类把支付定义成抽象方法
    public abstract void pay(double money);

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }
}
```

```java
//金卡子类

public class GoldCard extends Card{
    @Override
    public void pay(double money) {
        double rs=money*0.8;
        double lastMoney=getMoney()-rs;
        System.out.println(getName()+"消费前账户总金额："+getMoney()+"，当前消费："+rs+"，消费后余额剩余："+lastMoney);
        setMoney(lastMoney);    //更新账户金额
    }
}
```

```java
//银卡子类

public class SilverCard extends Card{
    @Override
    public void pay(double money) {
        double rs=money*0.85;
        double lastMoney=getMoney()-rs;
        System.out.println(getName()+"消费前账户总金额："+getMoney()+"，当前消费："+rs+"，消费后余额剩余："+lastMoney);
        setMoney(lastMoney);
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        GoldCard c=new GoldCard();
        c.setMoney(10000);
        c.setName("张三");
        c.pay(300); //冲300油费
        //张三消费前账户总金额：10000.0，当前消费：240.0，消费后余额剩余：9760.0

        SilverCard s=new SilverCard();
        s.setMoney(5000);
        s.setName("李四");
        s.pay(200);		
        //李四消费前账户总金额：5000.0，当前消费：170.0，消费后余额剩余：4830.0
    }
}
```



**抽象类的特征、注意事项**

- 得到了抽象方法，**失去了创建对象**的能力。（抽象方法没有实例化，创建对象调用抽象方法运行不了）

- 类有的成员（成员变量、方法、构造器）抽象类都具备
- 抽象类中不一定有抽象方法，有抽象方法的类一定要是抽象类
- 一个类继承了抽象类必须重写完抽象类的全部抽象方法，否则这个类也必须定义成抽象类。
- 不能用abstract修饰变量、代码块、构造器，只能修饰类和方法。

**final**和**abstract**的关系：**互斥关系**

- abstract定义的抽象类作为模板让子类继承，final定义的类不能被继承。

- 抽象方法定义通用功能让子类重写，final定义的方法子类不能重写。



## 9. 模板方法模式

- 使用场景：当系统中出现同一个功能多处在开发，而该功能中大部分代码是一样的，只有其中部分可能不同的时候。

**模板方法模式实现步骤**：

1. 把功能定义成一个所谓的模板方法，放在抽象类中，模板方法中只定义通用且能确定的代码。
2. 模板方法中不能决定的功能定义成抽象方法让具体子类去实现。



**案例：银行利息结算系统**

需求：

- 某软件公司要为某银行的业务支撑系统开发一个利息结算系统，账户有活期和定期账户两种
- 活期是0.35%，定期是 1.75%，定期如果满10万额外给予3%的收益
- 结算利息要先进行用户名、密码验证，验证失败直接提示，登录成功进行结算

分析：

无论存定期还是活期，都需要登录验证，计算利息，输出利息。其中只有计算利息的不同的，所以可以将其单独实现，而其他的代码可以用抽象方法实现。

1. 创建一个抽象的账户类Account作为父类模板，提供属性（卡号，余额）
2. 在父类Account中提供一个模板方法实现登录验证，利息结算、利息输出
3. 具体的利息结算定义成抽象方法，交给子类实现
4. 定义活期账户类，让子类重写实现具体的结算方法
5. 定义定期账户类，让子类重写实现具体的结算方法
6. 创建账户对象，完成相关功能。



```java
//抽象类

public abstract class Account {
    private String cardId;
    private double money;

    public Account() {
    }

    public Account(String cardId, double money) {
        this.cardId = cardId;
        this.money = money;
    }

     //模板方法
    public final void handle(String loginName , String passWord ){
        // a.判断登录是否成功
        if("itheima".equals(loginName) && "123456".equals(passWord)){
            System.out.println("登录成功。。");
            // b.正式结算利息
            // 当前模板方法知道所有子类账户都要结算利息，但是具体怎么结算，模板不清楚，交给具体的子类来计算
            double result = calc();
            // c.输出利息详情
            System.out.println("本账户利息是："+ result);
        }else{
            System.out.println("用户名或者密码错误了");
        }
    }

    public abstract double calc();

    public String getCardId() {
        return cardId;
    }

    public void setCardId(String cardId) {
        this.cardId = cardId;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }
}
```

```java
//活期类

public class CurrentAccount extends Account{
    public CurrentAccount(String cardId,double money){
        super(cardId,money);
    }

    @Override
    public double calc() {
        double result=getMoney()*0.0035;
        return result;
    }
}

```

```java
//定期类

public class FixedAccount extends Account{
    public FixedAccount(String cardId,double money){
        super(cardId,money);
    }

    @Override
    public double calc() {
        double result=getMoney()*0.0175;
        if(getMoney()>=100000){
            result+=result*0.03;
        }
        return result;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        CurrentAccount currentAccount=new CurrentAccount("ICBC-101",100000);
        currentAccount.handle("itheima","123456");
        FixedAccount fixedAccount=new FixedAccount("IIAC-123",100000);
        fixedAccount.handle("itheima","123456");

    }
}
```



**模板方法**我们是建议使用**final**修饰的，这样会更专业，因为模板方法是给子类直接使用的，不是让子类重写的，一旦子类重写了模板方法就失效了。



## 10. 接口

- 接口就是体现规范的，其中用抽象方法定义的一组**行为规范**，接口是更加彻底的抽象。
- 体现了现实世界 “如果你是这类事物...则必须完成某些行为...” 的思想。

### 10.1 接口的定义与特点

- 接口的格式如下：

  ```java
  //接口用关键字interface来定义
  public interface 接口名 {
         // 常量
         // 抽象方法
  }
  ```

- **JDK1.8之前**接口中只能是抽象方法和常量，没有其他成分了。（1.8版本开始后，允许接口中直接定义带有方法体(具体实现)的方法）

- 接口不能实例化(不能创建对象)。

- 接口中的成员都是public修饰的，写不写都是，因为规范的目的是为了公开化。

```java
public interface SportManInterface {
    // 接口中的成员：JDK 1.8之前只有常量 和 抽象方法
    // public static final 可以省略不写，接口默认会为你加上！
    // public static final String SCHOOL_NAME = "黑马";
    String SCHOOL_NAME = "黑马";

    // 2、抽象方法
    //  public abstract 可以省略不写，接口默认会为你加上！
    // public abstract void run();
    void run();

    // public abstract void eat();
    void eat();
}
```



### 10.2 接口的基本使用：被实现

**接口的用法：**

- 接口是用来被类实现（implements）的，实现接口的类称为实现类。实现类可以理解成所谓的子类
- 接口可以被类单实现，也可以被类多实现

实现的关键字：implements

`修饰符 class 实现类 implements 接口1, 接口2, 接口3 , ... { }`

**接口实现的注意事项：**

- 一个类实现接口，必须重写完全部接口的全部抽象方法，否则这个类需要定义成抽象类。



**示例：**

定义两个接口类Law和SportMan

```java
public interface Law {
    public abstract void rule();
}
```

```java
public interface SportMan {
    public abstract void run();
    void competition();
}
```

定义一个实现类PingPongMan

```java
public class PingPongMan implements Law,SportMan{
    private String name;

    public PingPongMan(String name) {
        this.name = name;
    }

    @Override
    public void rule() {
        System.out.println(name+"要遵守运动员规则！");
    }

    @Override
    public void run() {
        System.out.println(name+"开始跑步训练！");
    }

    @Override
    public void competition() {
        System.out.println(name+"开始参加比赛！");
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        PingPongMan p=new PingPongMan("张继科");
        p.rule();
        p.run();
        p.competition();
        //张继科要遵守运动员规则！
	//张继科开始跑步训练！
	//张继科开始参加比赛！
    }
}
```



### 10.3 接口与接口的关系：多继承

- 类和类的关系：单继承。
- 类和接口的关系：多实现。
- 接口和接口的关系：多继承，一个接口可以同时继承多个接口。

**接口多继承的作用：**

- 规范合并，整合多个接口为同一个接口，便于子类实现。

例如，可以让上面案例的SportMan类继承Law类，这样在实现类实现的时候就只用写`public class PingPongMan implements SportMan{······}`即可，整合了接口。



### 10.4 JDK8开始接口新增方法

为了在丰富接口功能的同时又不对子类代码进行更改，JDK1.8开始后，允许接口中直接定义带有方法体(具体实现)的方法。

**第一种：默认方法**

- 类似之前写的普通实例方法：必须用default修饰

- 默认会public修饰，需要用接口的实现类的对象来调用

  ```java
  default void run(){
  	System.out.println("--开始跑--");
  }
  ```

**第二种：静态方法**

- 默认会public修饰，必须static修饰。

- **注意：接口的静态方法必须用本身的接口名(接口类的名称)来调用。**

  ```java
  static void inAddr(){
  	System.out.println("我们都在黑马培训中心快乐的学习Java!");
  }
  ```

**第三种：私有方法**

- 就是私有的实例方法:，必须使用private修饰，从**JDK 1.9才开始有的**。

- 只能在本类中被其他的默认方法或者私有方法访问。

  ```java
  private void go(){
  	System.out.println("--准备--");
  }
  ```

  



**接口的注意事项：**

1. 接口不能创建对象。
2. 一个类实现多个接口，多个接口中有同样的静态方法，不冲突（接口的静态方法只能自己调，实现类无法他调用，所以不存在二义性）。
3. 一个类继承了父类，同时又实现了接口，父类中和接口中有**同名方法**，默认用**父类**的。(即一个类既有继承又有接口实现类，继承在前，接口实现在后)
4. 一个类实现了多个接口，多个接口中存在同名的默认方法，不冲突，这个类重写该方法即可（用自己的方法，永远用不了接口的方法）。
5. 一个接口继承多个接口，是没有问题的，如果多个接口中存在规范冲突则(当方法名相同而方法返回值不同)不能多继承。



## 11. 多态

- 同类型的对象，执行同一个行为，会表现出不同的行为特征。

**多态的常见形式：**

```
父类类型 对象名称 = new 子类构造器;
接口     对象名称 = new 实现类构造器;
```

**多态中成员访问特点：**

- 方法调用：编译看左边，运行看右边。
- 变量调用：编译看左边，运行也看左边。（多态侧重行为多态）

==多态的前提条件：==

- 有继承/实现关系；有父类引用指向子类对象；有方法重写



**多态的优势：**

- 在多态形式下，右边对象可以实现解耦合，便于扩展和维护。
- 定义方法的时候，使用父类作为参数，该方法就可以接收这个父类的一切子类对象，体现出多态的扩展性与便利。

**多态下会产生的问题：**

- 多态下不能使用子类的独有功能

**多态下引用数据类型的类型转换：**

自动类型转换（从子到父）：子类对象赋值给父类类型的变量指向。

**强制类型转换**（从父到子）：

- 此时必须进行强制类型转换：子类 对象变量 = (子类)父类类型的变量

- 作用：可以解决多态下的劣势，可以实现调用子类独有的功能

- 注意： 如果转型后的类型和对象真实类型不是同一种类型，那么在转换的时候就会出现ClassCastException(类型转换异常)

  ```
  Animal t = new Tortoise();
  Dog d = (Dog)t; 
  //t是Tortoise类型，强转为Dog类型，出现异常 ClassCastException
  ```

**Java建议强转转换前使用 instanceof 判断当前对象的真实类型，再进行强制转换。**

`变量名 instanceof 真实类型`

判断关键字左边的变量指向的对象的真实类型，是否是右边的类型或者是其子类类型，是则返回true，反之返回false。

```java
// 注意：多态下直接强制类型转换，可能出现类型转换异常
// 规定：有继承或者实现关系的2个类型就可以强制类型转换，运行时可能出现问题。
//Animal a = new Dog();
// Tortoise t1 = (Tortoise) a;		//编译正常，运行出错，a是Dog类型，强转为了Tortoise类型

// 建议强制转换前，先判断变量指向对象的真实类型，再强制类型转换。
if(a instanceof Tortoise){
    Tortoise t = (Tortoise) a;
    t.layEggs();				//子类独有功能
}else if(a instanceof Dog){
    Dog d1 = (Dog) a;
    d1.lookDoor();
}

//强转转换判断常常用在父类/接口中，根据传进来的类类型来选择不同的独有功能，公有功能直接调用。
{
	Animal a1 = new Dog();
	go(a1);
}
    
public static void go(Animal a){
        a.run();	//共有功能
        // 独有功能，判断传进来的a到底是Tortoise还是Dog
        if(a instanceof Tortoise){
            Tortoise t = (Tortoise) a;
            t.layEggs();
        }else if(a instanceof Dog){
            Dog d1 = (Dog) a;
            d1.lookDoor();
        }
}
```



**多态案例：**

需求：

- 使用面向对象编程模拟：设计一个电脑对象，可以安装2个USB设备。
- 鼠标：被安装时可以完成接入、调用点击功能、拔出功能。
- 键盘：被安装时可以完成接入、调用打字功能、拔出功能。

分析：

1. 定义一个USB的接口（申明USB设备的规范必须是：可以接入和拔出）。
2. 提供2个USB实现类代表鼠标和键盘，让其实现USB接口，并分别定义独有功能。
3. 创建电脑对象，创建2个USB实现类对象，分别安装到电脑中并触发功能的执行。

USB接口类：

```java
public interface USB {
    void connect();
    void unconnect();
}
```

KeyBoard实现类：

```java
public class KeyBoard implements USB{
    private String name;
    public KeyBoard(String name){
        this.name=name;
    }

    public void type(){
        System.out.println(name+"键盘开始打字！");
    }

    @Override
    public void connect() {
        System.out.println(name+"成功接入电脑！");
    }

    @Override
    public void unconnect() {
        System.out.println(name+"成功拔出电脑！");
    }
}
```

Mouse实现类：

```java
public class Mouse implements USB{
    private String name;
    public Mouse(String name){
        this.name=name;
    }

    public void click(){
        System.out.println(name+"鼠标开始点击！");
    }

    @Override
    public void connect() {
        System.out.println(name+"成功接入电脑！");
    }

    @Override
    public void unconnect() {
        System.out.println(name+"成功拔出电脑！");
    }
}
```

Computer类：

```java
public class Computer {
    private String name;

    public Computer() {
    }

    public Computer(String name) {
        this.name = name;
    }

    public void start(){
        System.out.println(name+"开机了！");
    }

    public void installUSB(USB usb){
        usb.connect();
        if(usb instanceof KeyBoard){
            KeyBoard k=(KeyBoard) usb;
            k.type();
        }else if(usb instanceof Mouse){
            Mouse m=(Mouse) usb;
            m.click();
        }
        usb.unconnect();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        Computer c=new Computer("外星人RW800");
        c.start();

        USB u=new KeyBoard("KeyChrom Q1");
        c.installUSB(u);

        c.installUSB(new Mouse("Logic G903"));
    }
}
```





## 12. 内部类

### 12.1 内部类概述

- 内部类就是定义在一个类里面的类，里面的类可以理解成（寄生），外部类可以理解成（宿主）。

  ```java
  public class People{
      // 内部类
          public class Heart{
  	}
  }
  ```

**内部类的使用场景、作用**

- 当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为外部事物提供服务，那么整个内部的完整结构可以选择使用内部类来设计。
- 内部类通常可以方便访问外部类的成员，包括私有的成员。
- 内部类提供了更好的封装性，内部类本身就可以用private protectecd等修饰，封装性可以做更多控制。

### 12.2 内部类的分类

#### 12.2.1 静态内部类（了解）

- 有static修饰，属于外部类本身。

- 它的特点和使用与普通类是完全一样的，类有的成分它都有，只是位置在别人里面而已。

  ```java
  public class Outer{
  	//静态成员内部类
  	public static class Inner{
  	}
  }
  ```

**静态内部类创建对象的格式：**

`外部类名.内部类名 对象名 = new 外部类名.内部类构造器;`

`Outer.Inner in = new Outer.Inner();`

- 静态内部类中**可以**直接访问**外部类的静态成员**，外部类的静态成员只有一份可以被共享访问。

- 静态内部类中**不可以**直接访问**外部类的实例成员**，外部类的实例成员必须用外部类对象访问。



#### 12.2.2 成员内部类（了解）

- 无static修饰，属于外部类的对象

- JDK16之前，成员内部类中不能定义静态成员，JDAK16开始也可以定义静态成员了

  ```java
  public class Outer{
  	//成员内部类
  	public class Inner{
  	}
  }
  ```

**成员内部类创建对象的格式：**

`外部类名.内部类名 对象名 = new  外部类构造器.new 内部类构造器();`
`Outer.Inner in =  new Outer().new Inner();`

- 成员内部类中**可以**直接访问**外部类的静态成员**，外部类的静态成员只有一份可以被共享访问。

- 成员内部类中**可以**直接访问**外部类的实例成员**，因为必须先有外部类对象，才能有成员内部类对象，所以可以直接访问外部类对象的实例成员。

**在成员内部类中，访问外部类对象，格式：`外部类名.this`**

```java
 class People {
	private int hertbeat=150;
	public class Heart{
             	private int heartbeat=110;
             	public void show(){
                    	 int heartbeat=78;
                    	 System.out.println(heartbeat);       //78
                    	 System.out.println(this.heartbeat);      //110
                    	 System.out.println(People.this.hertbeat);     //150
                   }
         }
}
```



#### 12.2.3 局部内部类（鸡肋语法，了解）

- 局部内部类放在方法、代码块、构造器等执行体中
- 局部内部类的类文件名为： 外部类$N内部类.class



#### 12.2.4 匿名内部类（重点）

- 本质上是一个没有名字的局部内部类，定义在方法中、代码块中等。

- 作用：方便创建子类对象，最终目的是为了简化代码编写。

**格式：**

```java
new 类 | 抽象类名 | 接口名（）{
	重写方法;
};
```

**示例：**

```java
public class Test{
    public static void main(String[] args){
        //匿名内部类：
        Animal a = new Animal(){		//并没有创建抽象类对象，产生的是匿名内部类的对象
            @override
            public void run(){
                System.out.println("老虎跑得快");
            }
        };
        a.run()		
    }
}

/*	简化此代码
    class Tiger extends Animal{
    @Override
    public void run(){
        System.out.println("老虎跑得快");
    }
}
*/

abstrct class Animal{
    public abstract void run();
}
```

接口类似。



**特点总结：**

- 匿名内部类是一个没有名字的内部类。
- 匿名内部类写出来就会产生一个**匿名内部类的对象**（不是产生抽象类的对象，抽象类无法实例化）。
- 匿名内部类的对象类型相当于是当前new的那个的类型的子类类型。



**匿名内部类常见使用形式：**

- **匿名内部类可以作为方法的实际参数进行传输。**



# 常用API

- API(Application Programming interface) 应用程序编程接口。
- **简单来说：就是Java帮我们已经写好的一些方法，我们直接拿过来用就可以了。**

## 1. Object

Object类的作用：

- Object类的方法是一切子类对象都可以直接使用的，所以我们要学习Object类的方法。
- 一个类要么默认继承了Object类，要么间接继承了Object类，Object类是Java中的祖宗类。

Object类的常用方法：

|             方法名              |                             说明                             |
| :-----------------------------: | :----------------------------------------------------------: |
|    public String toString()     | 默认是返回当前对象在堆内存中的地址信息：类的全限名@内存地址  |
| public boolean equals(Object o) | 默认是比较当前对象与另一个对象的地址是否相同，相同返回true，不同返回false |

### 1.1 toString方法

**问题引出：**

- 开发中直接输出对象，默认输出对象的地址其实是毫无意义的。
- 开发中输出对象变量，更多的时候是希望看到对象的内容数据而不是对象的地址信息。

**toString存在的意义：**

- 父类toString()方法存在的意义就是为了**被子类重写**，以便返回对象的内容信息，而不是地址信息。

### 1.2 equals方法

**问题引出：**

- 直接比较两个对象的地址是否相同完全可以用 “ == ” 替代equals。

**equals存在的意义：**

- 父类equals方法存在的意义就是为了被子类重写，以便子类自己来定制比较规则。



示例：

Student类

```java
public class Student {
    private String name;
    private char sex;
    private int age;

    public Student() {
    }

    public Student(String name, char sex, int age) {
        this.name = name;
        this.sex = sex;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    /**
     官方重写equals，定制相等规则。
     两个对象的内容一样就认为是相等的
     s1.equals(s2)
     比较者：s1 ==> this
     被比较者： s2 ==> o
     */
    @Override
    public boolean equals(Object o) {
        // 1、判断是否是同一个对象比较，如果是返回true。
        if (this == o) return true;
        // 2、如果o是null返回false  如果o不是学生类型返回false（getClass()是去类类型）
        if (o == null || this.getClass() != o.getClass()) return false;
        // 3、说明o一定是学生类型而且不为null
        Student student = (Student) o;
        return sex == student.sex && age == student.age && Objects.equals(name, student.name);
        //String类型的比较调用Objects类的equals更安全(因为传入进来的对象可能为null)
    }


    /**
     自己重写equals，自己定制相等规则。
     两个对象的内容一样就认为是相等的
     s1.equals(s2)
     比较者：s1 ==> this
     被比较者： s2 ==> o
     */

    /*   @Override
    public boolean equals(Object o){
        // 1、判断o是不是学生类型
        if(o instanceof Student){
            Student s2 = (Student) o;   //先强制类型转换，因为Object不能创建对象，无法获得变量
            // 2、判断2个对象的内容是否一样。
            return this.name.equals(s2.name) && this.age == s2.age
                    && this.sex == s2.sex ;

        }else {
            // 学生只能和学生比较，否则结果一定是false
            return false;
        }
    }
    */

    //重写toString
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", sex=" + sex +
                ", age=" + age +
                '}';
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        Student s1 = new Student("周雄", '男', 19);
        Student s2 = new Student("周雄", '男', 19);
        // equals默认是比较2个对象的地址是否相同，子类重写后会调用子类重写的来比较内容是否相同。
        System.out.println(s1.equals(s2));		//true
        System.out.println(s1 == s2);		//false
        
        //System.out.println(s1.toString());
        System.out.println(s1);     //直接输出对象变量，默认可以省略toString调用不写
        //Student{name='周雄', sex=男, age=19}
    }
}
```





## 2. Objects

- Objects是一个工具类，提供了一些方法去完成一些功能。

**官方在进行字符串比较时，没有用字符串对象的equals方法(若传入进来的对象为null，则会发生空指针异常)，而是选择了Objects的equals方法来比较。使用Objects的equals方法在进行对象的比较会更安全。**



**Objects常见方法：**

|                      方法名                      |                             说明                             |
| :----------------------------------------------: | :----------------------------------------------------------: |
| public static boolean equals(Object a, Object b) | 比较两个对象的，底层会先进行非空判断，从而可以避免空指针异常。再进行equals比较 |
|     public static boolean isNull(Object obj)     |        判断变量是否为null ,为null返回true ,反之false         |

Objects的equals源码分析：

```java
public static boolean equals(Object a, Object b) {
	return (a == b) || (a != null && a.equals(b));		//进行了判断
}
```



**建议使用Objects提供的equals方法。比较的结果是一样的，但是更安全。**



## 3. StringBuilder

- StringBuilder是一个可变的字符串类，我们可以把它看成是一个对象容器。
- **作用**：提高字符串的操作效率，如拼接、修改等。

**StringBuilder构造器：**

|               名称               |                      说明                      |
| :------------------------------: | :--------------------------------------------: |
|      public StringBuilder()      | 创建一个空白的可变的字符串对象，不包含任何内容 |
| public StringBuilder(String str) |     创建一个指定字符串内容的可变字符串对象     |

**StringBuilder常用方法：**

|               方法名称                |                          说明                           |
| :-----------------------------------: | :-----------------------------------------------------: |
| public StringBuilder append(任意类型) | 添加数据并返回StringBuilder对象本身（**支持链式编程**） |
|    public StringBuilder reverse()     |          将对象的内容反转（**支持链式编程**）           |
|          public int length()          |                    返回对象内容长度                     |
|       public String toString()        |   通过toString()就可以实现把StringBuilder转换为String   |

**注意：**

**StringBuilder只是拼接字符串的手段，效率高，最终还是要转换成String类型(用toString())，方便使用。**



**String类拼接字符串的原理图：**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0db0b8f16a9cc178dbfd092a1e7d6e11.png)

String类拼接字符串的原理：先创建StringBuilder对象，进行拼接后，再通过toString转换成String类型，每次用+号拼接时都会产生两个对象。



**StringBuilder拼接字符串的原理图：**

![image-20220108161916846](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0cc01831ff21eb082d5781944f1a9540.png)

**拼接、反转字符串建议使用StringBuilder：**

- **String** **：内容是不可变的、拼接字符串性能差。**

- **StringBuilder：内容是可变的、拼接字符串性能好、代码优雅。**

- **定义字符串使用String**

- **拼接、修改等操作字符串使用StringBuilder**



**示例：打印整型数组内容**

```java
public class StringBuilderDemo2 {
    public static void main(String[] args) {
        int [] arr1={1,2,3,4,5};
        System.out.println(toString(arr1));     //[1，2，3，4，5]
    }
    
    public static String toString(int[] arr){
        if(arr!=null){
            StringBuilder sb=new StringBuilder("[");
            for (int i = 0; i < arr.length; i++) {
                sb.append(arr[i]).append(i== arr.length-1?"":"，");		//拼接数组的内容
            }
            sb.append("]");
            return sb.toString();
        }else{
            return null;
        }
    }
}
```



## 4. Math

- 包含执行基本数字运算的方法，Math类没有提供公开的构造器。
- Math类中的方法都是静态的，通过类名就可以直接调用。

**Math类的常用方法：**

|                                             |                                       |
| :-----------------------------------------: | :-----------------------------------: |
|        public static int abs(int a)         |            获取参数绝对值             |
|     public static double ceil(double a)     |               向上取整                |
|    public static double floor(double a)     |               向下取整                |
|      public static int round(float a)       |               四舍五入                |
|     public static int max(int a,int b)      |        获取两个int值中的较大值        |
| public static double pow(double a,double b) |           返回a的b次幂的值            |
|        public static double random()        | 返回值为double的随机值，范围[0.0,1.0) |



## 5. System

- System也是一个工具类，代表了当前系统，提供了一些与系统相关的方法。System类中的方法也都是静态的，无法实例化对象，调用时直接用类名。

**System类常用方法：**

|                            方法名                            |                             说明                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|             public static void exit(int status)              | 终止当前运行的 Java 虚拟机，非零表示异常终止，填零表示手动终止 |
|            public static long currentTimeMillis()            |                 返回当前系统的时间毫秒值形式                 |
| public static void arraycopy(数据源数组, 起始索引, 目的地数组, 起始索引, 拷贝个数) |                           数组拷贝                           |

**时间毫秒值：**

- 计算机认为时间是有起点的，起始时间： 1970年1月1日 00:00:00
- 时间毫秒值：指的是从1970年1月1日   00:00:00走到此刻的总的毫秒数，应该是很大的，一般用long数据类型接收。 1s = 1000ms

时间毫秒值方法可以用来测试分析：

```
public class Test(){
    public static void main(String[] args) {
            long startTime = System.currentTimeMillis();
            for (int i = 0; i < 100000; i++) {
                System.out.println("输出："+i);
            }
            long endTime = System.currentTimeMillis();
            System.out.println("该循环运行了"+(endTime-startTime)/1000.0+"s");
            //该循环运行了0.374s
        }
}
```



## 6. BigDecimal

**BigDecimal作用：**

- 用于解决浮点型运算精度失真的问题

  

**使用步骤：**

- 创建对象BigDecimal封装浮点型数据（最好的方式是调用valueOf方法）

  ```
  public static BigDecimal valueOf(double val);      包装浮点数成为BigDecimal对象。
  例如：
  BigDecimal b1 = BigDecimal.valueOf(0.1);
  ```

**BigDecima常用API：**

|                            方法名                            | 说明 |
| :----------------------------------------------------------: | :--: |
|             public BigDecimal add(BigDecimal b)              | 加法 |
|           public BigDecimal subtract(BigDecimal b)           | 减法 |
|           public BigDecimal multiply(BigDecimal b)           | 乘法 |
|            public BigDecimal divide(BigDecimal b)            | 除法 |
| public BigDecimal divide (另一个BigDecimal对象，精确几位，舍入模式) | 除法 |

**示例：**

```java
public class Test {
    public static void main(String[] args) {
        // 浮点型运算的时候直接+  * / 可能会出现数据失真（精度问题）。
        System.out.println(0.09 + 0.01);        //0.09999999999999999
        System.out.println(1.0 - 0.32);         //0.6799999999999999
        System.out.println(1.015 * 100);        //101.49999999999999
        System.out.println(1.301 / 100);        //0.013009999999999999

        System.out.println("-------------------------");
        double a = 0.1;
        double b = 0.2;
        double c = a + b;
        System.out.println(c);      //0.30000000000000004
        System.out.println("--------------------------");
        // 包装浮点型数据成为大数据对象 BigDeciaml
        BigDecimal a1 = BigDecimal.valueOf(a);
        BigDecimal b1 = BigDecimal.valueOf(b);
        BigDecimal c1 = a1.add(b1);
        // BigDecimal c1 = a1.subtract(b1);
        // BigDecimal c1 = a1.multiply(b1);
        // BigDecimal c1 = a1.divide(b1);
        System.out.println(c1);     //0.3

        // 目的：需要double
        double rs = c1.doubleValue();
        System.out.println(rs);     //0.3

        // 注意事项：BigDecimal是一定要精度运算的
        BigDecimal a11 = BigDecimal.valueOf(10.0);
        BigDecimal b11 = BigDecimal.valueOf(3.0);
        //BigDecimal c11 = a11.divide(b11);       //10.0/3.0除不尽，会报错

        //此时需要填参数：
        //参数一：除数 参数二：保留小数位数  参数三：舍入模式
        BigDecimal c11 = a11.divide(b11, 2, RoundingMode.HALF_UP);
        System.out.println(c11);        //3.33
    }
}
```

**用完BigDecimal后，常常需要将此类型的数据转换成我们需要的类型(通常为double)。**



**BigDecimal divide = bd1.divide(参与运算的对象, 小数点后精确到多少位, 舍入模式);**

- 参数1 ，表示参与运算的BigDecimal 对象
- 参数2 ，表示小数点后面精确到多少位
- 参数3 ，舍入模式：用`RoundingMode.`选择模式 



## 7. 日期与时间

### 7.1 Date

- Date类代表当前所在系统的日期时间信息。

**Date的构造器：**

|         构造器         |                             说明                             |
| :--------------------: | :----------------------------------------------------------: |
|     public Date()      |        创建一个Date对象，代表的是系统当前此刻日期时间        |
| public Date(long time) | 把时间毫秒值转换成Date日期对象。（**可以把时间毫秒值转换成日期对象**） |
|                        |                                                              |

**Date的常用方法：**

|            Date方法            |                             说明                             |
| :----------------------------: | :----------------------------------------------------------: |
|     public long getTime()      |        返回从1970年1月1日 00:00:00走到此刻的总毫秒数         |
| public void setTime(long time) | 设置日期对象的时间为当前时间毫秒值对应的时间（**可以把时间毫秒值转换成日期对象**） |
|                                |                                                              |



**案例：计算出当前时间往后走1小时121秒之后的时间是多少**

```java
public class Test {
    public static void main(String[] args) {
        // 1、得到当前时间
        Date d1 = new Date();
        System.out.println(d1);

        // 2、当前时间往后走 1小时  121s
        long time2 = System.currentTimeMillis();
        time2 += (60 * 60 + 121) * 1000;

        // 3、把时间毫秒值转换成对应的日期对象。
        //方案1：
        // Date d2 = new Date(time2);
        // System.out.println(d2);
	//方案2：
        Date d3 = new Date();
        d3.setTime(time2);
        System.out.println(d3);
    }
}

//Sat Jan 08 18:17:08 CST 2022
//Sat Jan 08 19:19:09 CST 2022
```



**总结：**

- 日期对象创建：`Date date=new Date();`
- 获取时间毫秒值：`long time=date.getTime();` or `long time=System.currentTimeMillis();`



- 时间毫秒值恢复成日期对象：
  - `public Date(long time);`
  - `public void setTime(long time);`



### 7.2 SimpleDateFormat

**作用：**

- 可以对Date对象或时间毫秒值格式化成我们方便观看的时间形式
- 可以把字符串的时间形式解析成日期对象



构造器：

|                 构造器                  |                   说明                   |
| :-------------------------------------: | :--------------------------------------: |
| public SimpleDateFormat(String pattern) | 构造一个SimpleDateFormat，使用指定的格式 |

格式化方法：

|                 格式化方法                  |               说明                |
| :-----------------------------------------: | :-------------------------------: |
|  public final String **format(Date date)**  |   将日期格式化成日期/时间字符串   |
| public final String **format(Object time)** | 将时间毫秒值式化成日期/时间字符串 |

解析方法：

|               解析方法               |                 说明                 |
| :----------------------------------: | :----------------------------------: |
| public Date **parse(String source)** | 从给定字符串的开始解析文本以生成日期 |

**示例：**

```java
public class SimpleDateFormatTest01 {
    public static void main(String[] args) throws ParseException {
        Date d=new Date();
        SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");
        //格式化日期对象
        String rs=sdf.format(d);
        System.out.println(rs);		//2022年01月08日 18:56:52 周六 下午	

        //格式化时间毫秒值
        //当前时间120秒后是多少？
        long time=System.currentTimeMillis()+120*1000;
        System.out.println(sdf.format(time));		//2022年01月08日 18:58:52 周六 下午

        //计算出 2021年08月06日11点11分11秒，往后走2天14小时49分06秒后的时间是多少
        String dateStr="2021年08月06日 11:11:11:";
        //格式与上面格式一样，解析才不会出现bug
        SimpleDateFormat sdf2=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        Date date=sdf2.parse(dateStr);
        long time2=date.getTime()+(2L*24*60*60+14*60*60+49*60+6)*1000;  //+L 全部数据用long类型计算，防止出现失真
        System.out.println(sdf2.format(time2));		//2021年08月09日 02:00:17
    }
}
```



**格式化的时间形式的常用的模式对应关系如下：**

y 年、M 月、d 日、H 时、m 分、s 秒、EEE 星期、a 上下午

```
2020-11-11 13:27:06	———— yyyy-MM-dd HH:mm:ss  
2020年11月11日 13:27:06  ———— yyyy年MM月dd日 HH:mm:ss  
2022年01月08日 18:58:52 周六 下午 ———— yyyy年MM月dd日 HH:mm:ss EEE a
```



**案例：秒杀活动**

需求：

- 某购物网站举办秒杀活动，时间为：2020年11月11日 0:0:0 到 2020年11月11日 0:10:0，当前活动结束后，系统记录到2位用户的付款时间分别如下：
  - 小贾下单并付款的时间为：2020年11月11日 0:03:47
  - 小皮下单并付款的时间为：2020年11月11日 0:10:11
- 规则：顾客的付款时间必须在秒杀时间之内，请判断出两位顾客是否秒杀成功



分析：

- 把4个字符串形式的时间解析成日期对象。
- 判断小贾和小皮的时间是否在秒杀时间范围之内，并给出相应的提示。

```java
public class MiaoSha {
    public static void main(String[] args) throws ParseException {
        String startTime="2020年11月11日 0:0:0";
        String endTime="2020年11月11日 0:10:0";
        String jiaTime="2020年11月11日 0:03:47";
        String piTime="2020年11月11日 0:10:11";
        SimpleDateFormat sdf=new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
        Date startTime2=sdf.parse(startTime);
        Date endTime2=sdf.parse(endTime);
        Date jiaTime2=sdf.parse(jiaTime);
        Date piTime2=sdf.parse(piTime);

        if(jiaTime2.before(endTime2)&&jiaTime2.after(startTime2)){
            System.out.println("小贾的付款时间在活动范围内，秒杀成功！");
        }else{
            System.out.println("小贾的付款时间不在活动范围内，秒杀失败！");
        }

        if(piTime2.before(endTime2)&&piTime2.after(startTime2)){
            System.out.println("小皮的付款时间在活动范围内，秒杀成功！");
        }else{
            System.out.println("小皮的付款时间不在活动范围内，秒杀失败！");
        }
    }
    
    //小贾的付款时间在活动范围内，秒杀成功！
    //小皮的付款时间不在活动范围内，秒杀失败！
```



### 7.3 Calendar

- Calendar代表了系统此刻日期对应的日历对象。
- Calendar是一个抽象类，不能直接创建对象。

**Calendar日历类创建日历对象的方法：**不用new，直接用类名，内部会自动new

|                方法名                |       说明       |
| :----------------------------------: | :--------------: |
| public static Calendar getInstance() | 获取当前日历对象 |

**Calendar常用方法：**

|                方法名                 |            说明             |
| :-----------------------------------: | :-------------------------: |
|       public int get(int field)       |  取日期中的某个字段信息。   |
| public void set(int field,int value)  |  修改日历的某个字段信息。   |
| public void add(int field,int amount) | 为某个字段增加/减少指定的值 |
|      public final Date getTime()      |     拿到此刻日期对象。      |
|     public long getTimeInMillis()     |     拿到此刻时间毫秒值      |

**注意：calendar是可变日期对象，一旦修改后其对象本身表示的时间将产生变化。**（一般不会修改）

**示例：**

```java
public class Calender {
    public static void main(String[] args) {
        // 1、拿到系统此刻日历对象
        Calendar cal = Calendar.getInstance();
        System.out.println(cal);		//打印整个日历（可以用来查看关键字段）

        // 2、获取日历的信息:public int get(int field)：取日期中的某个字段信息。
        int year = cal.get(Calendar.YEAR);
        System.out.println(year);       //2022

        int mm = cal.get(Calendar.MONTH) + 1;
        System.out.println(mm);     //1

        int days = cal.get(Calendar.DAY_OF_YEAR) ;
        System.out.println(days);       //8

        // 3、public void set(int field,int value)：修改日历的某个字段信息（一般不修改）
        // cal.set(Calendar.HOUR , 12);
        // System.out.println(cal);

        // 4.public void add(int field,int amount)：为某个字段增加/减少指定的值
        // 5.public final Date getTime(): 拿到此刻日期对象。
        // 请问64天59分后是什么时间
        System.out.println("当前时间: "+cal.getTime());     //当前时间: Sat Jan 08 20:01:55 CST 2022
        cal.add(Calendar.DAY_OF_YEAR , 64);
        cal.add(Calendar.MINUTE , 59);
        System.out.println("64天59分后的时间："+cal.getTime());    //64天59分后的时间：Sun Mar 13 21:00:55 CST 2022

        //  6.public long getTimeInMillis(): 拿到此刻时间毫秒值
        long time = cal.getTimeInMillis();
        System.out.println(time);       //1647176455646
    }
}
```



## 8. JDK8新增日期类

**概述**

- 从Java 8开始，java.time包提供了新的日期和时间API，主要涉及的类型有：
  - **LocalDate**：不包含具体时间的日期
  - **LocalTime**：不含日期的时间
  - **LocalDateTime**：包含了日期及时间
  - **Instant**：代表的是时间戳
  - **DateTimeFormatter** 用于做时间的格式化和解析的
  - **Duration**:用于计算两个“时间”间隔
  - **Period**:用于计算两个“日期”间隔
- 新增的API严格区分了时刻、本地日期、本地时间，并且，对日期和时间进行运算更加方便。
- 其次，新API的类型几乎全部是不变类型（和String的使用类似），可以放心使用不必担心被修改。

### 8.1 LocalDate、LocalTime、LoclaDateTime

- 他们 分别表示日期，时间，日期时间对象，他们的类的实例是不可变的对象。
- 他们三者构建对象和API都是通用的

**构建对象的方式如下：**

|          方法名           |              说明               |                                                              |
| :-----------------------: | :-----------------------------: | ------------------------------------------------------------ |
| public static Xxxx now(); | 静态方法，根据当前时间创建对象  | LocaDate localDate = LocalDate.now();<br/>LocalTime localTime = LocalTime.*now*(); <br/>LocalDateTime localDateTime = LocalDateTime.*now*(); |
| public static Xxxx of(…); | 静态方法，指定日期/时间创建对象 | LocalDate localDate1 = LocalDate.*of*(2099 , 11,11)<br/>LocalTime localTime1 = LocalTime.*of*(11, 11, 11);<br/>LocalDateTime localDateTime1 = LocalDateTime.*of*(2020, 10, 6, 13, 23, 43); |

**获取信息的API：**

|             方法名              |        说明        |
| :-----------------------------: | :----------------: |
|       public int geYear()       |       获取年       |
|   public int getMonthValue()    |  获取月份（1-12）  |
|   Public int getDayOfMonth()    | 获取月中第几天乘法 |
|    Public int getDayOfYear()    |   获取年中第几天   |
| Public DayOfWeek getDayOfWeek() |      获取星期      |

**LocalDateTime的转换API：**

|             方法名             |          说明           |
| :----------------------------: | :---------------------: |
| public LocalDate toLocalDate() | 转换成一个LocalDate对象 |
| public LocalTime toLocalTime() | 转换成一个LocalTime对象 |

```java
public class Demo01LocalDate {
    public static void main(String[] args) {
        // 1、获取本地日期对象。
        LocalDate nowDate = LocalDate.now();
        System.out.println("今天的日期：" + nowDate);

        int year = nowDate.getYear();
        System.out.println("year：" + year);


        int month = nowDate.getMonthValue();
        System.out.println("month：" + month);

        int day = nowDate.getDayOfMonth();
        System.out.println("day：" + day);

        //当年的第几天
        int dayOfYear = nowDate.getDayOfYear();
        System.out.println("dayOfYear：" + dayOfYear);

        //星期
        System.out.println(nowDate.getDayOfWeek());
        System.out.println(nowDate.getDayOfWeek().getValue());

        //月份
        System.out.println(nowDate.getMonth());//AUGUST
        System.out.println(nowDate.getMonth().getValue());//8

        System.out.println("------------------------");
        LocalDate bt = LocalDate.of(1991, 11, 11);
        System.out.println(bt);//直接传入对应的年月日
        System.out.println(LocalDate.of(1991, Month.NOVEMBER, 11));//相对上面只是把月换成了枚举
    }
}
```

```java
public class Demo02LocalTime {
    public static void main(String[] args) {
        // 1、获取本地时间对象。
        LocalTime nowTime = LocalTime.now();
        System.out.println("今天的时间：" + nowTime);//今天的时间：

        int hour = nowTime.getHour();//时
        System.out.println("hour：" + hour);//hour：

        int minute = nowTime.getMinute();//分
        System.out.println("minute：" + minute);//minute：

        int second = nowTime.getSecond();//秒
        System.out.println("second：" + second);//second：

        int nano = nowTime.getNano();//纳秒
        System.out.println("nano：" + nano);//nano：

        System.out.println("-----");
        System.out.println(LocalTime.of(8, 20));//时分
        System.out.println(LocalTime.of(8, 20, 30));//时分秒
        System.out.println(LocalTime.of(8, 20, 30, 150));//时分秒纳秒
        LocalTime mTime = LocalTime.of(8, 20, 30, 150);

        System.out.println("---------------");
        System.out.println(LocalDateTime.of(1991, 11, 11, 8, 20));
        System.out.println(LocalDateTime.of(1991, Month.NOVEMBER, 11, 8, 20));
        System.out.println(LocalDateTime.of(1991, 11, 11, 8, 20, 30));
        System.out.println(LocalDateTime.of(1991, Month.NOVEMBER, 11, 8, 20, 30));
        System.out.println(LocalDateTime.of(1991, 11, 11, 8, 20, 30, 150));
        System.out.println(LocalDateTime.of(1991, Month.NOVEMBER, 11, 8, 20, 30, 150));
    }
}
```

```java
public class Demo03LocalDateTime {
    public static void main(String[] args) {
        // 日期 时间
        LocalDateTime nowDateTime = LocalDateTime.now();
        System.out.println("今天是：" + nowDateTime);//今天是：
        System.out.println(nowDateTime.getYear());//年
        System.out.println(nowDateTime.getMonthValue());//月
        System.out.println(nowDateTime.getDayOfMonth());//日
        System.out.println(nowDateTime.getHour());//时
        System.out.println(nowDateTime.getMinute());//分
        System.out.println(nowDateTime.getSecond());//秒
        System.out.println(nowDateTime.getNano());//纳秒
        //日：当年的第几天
        System.out.println("dayOfYear：" + nowDateTime.getDayOfYear());//dayOfYear：249
        //星期
        System.out.println(nowDateTime.getDayOfWeek());//THURSDAY
        System.out.println(nowDateTime.getDayOfWeek().getValue());//4
        //月份
        System.out.println(nowDateTime.getMonth());//SEPTEMBER
        System.out.println(nowDateTime.getMonth().getValue());//9


        LocalDate ld = nowDateTime.toLocalDate();
        System.out.println(ld);

        LocalTime lt = nowDateTime.toLocalTime();
        System.out.println(lt.getHour());
        System.out.println(lt.getMinute());
        System.out.println(lt.getSecond());
    }
}
```



**修改相关API：**

- LocalDateTime 综合了 LocalDate 和 LocalTime 里面的方法，所以下面只用 LocalDate 和 LocalTime 来举例。
- 这些方法返回的是一个新的实例引用，因为LocalDateTime 、LocalDate 、LocalTime 都是不可变的。

|                       方法名                       |                             说明                             |
| :------------------------------------------------: | :----------------------------------------------------------: |
|     plusDays, plusWeeks, plusMonths, plusYears     |      向当前 LocalDate 对象添加几天、 几周、几个月、几年      |
|   minusDays, minusWeeks, minusMonths, minusYears   |      从当前 LocalDate 对象减去几天、 几周、几个月、几年      |
| withDayOfMonth, withDayOfYear, withMonth, withYear | 将月份天数、年份天数、月份、年 份 修 改 为 指 定 的 值 并 返 回 新 的 LocalDate 对象 |
|                 isBefore, isAfter                  |                      比较两个 LocalDate                      |

```java
public class Demo04UpdateTime {
    public static void main(String[] args) {
        LocalTime nowTime = LocalTime.now();
        System.out.println(nowTime);//当前时间
        System.out.println(nowTime.minusHours(1));//一小时前
        System.out.println(nowTime.minusMinutes(1));//一分钟前
        System.out.println(nowTime.minusSeconds(1));//一秒前
        System.out.println(nowTime.minusNanos(1));//一纳秒前

        System.out.println("----------------");

        System.out.println(nowTime.plusHours(1));//一小时后
        System.out.println(nowTime.plusMinutes(1));//一分钟后
        System.out.println(nowTime.plusSeconds(1));//一秒后
        System.out.println(nowTime.plusNanos(1));//一纳秒后

        System.out.println("------------------");
        // 不可变对象，每次修改产生新对象！
        System.out.println(nowTime);

        System.out.println("---------------");
        LocalDate myDate = LocalDate.of(2018, 9, 5);
        LocalDate nowDate = LocalDate.now();

        System.out.println("今天是2018-09-06吗？ " + nowDate.equals(myDate));//今天是2018-09-06吗？ false
        System.out.println(myDate + "是否在" + nowDate + "之前？ " + myDate.isBefore(nowDate));//2018-09-05是否在2018-09-06之前？ true
        System.out.println(myDate + "是否在" + nowDate + "之后？ " + myDate.isAfter(nowDate));//2018-09-05是否在2018-09-06之后？ false

        System.out.println("---------------------------");
        // 判断今天是否是你的生日
        LocalDate birDate = LocalDate.of(1996, 8, 5);
        LocalDate nowDate1 = LocalDate.now();

        MonthDay birMd = MonthDay.of(birDate.getMonthValue(), birDate.getDayOfMonth());
        MonthDay nowMd = MonthDay.from(nowDate1);

        System.out.println("今天是你的生日吗？ " + birMd.equals(nowMd));//今天是你的生日吗？ false
    }
}
```



### 8.2 Instant

**Instant时间戳：**

- JDK8获取时间戳特别简单，且功能更丰富。Instant类由一个静态的工厂方法now()可以返回当前时间戳。

```java
public class Demo05Instant {
    public static void main(String[] args) {
        // 1、得到一个Instant时间戳对象（默认时区的时间戳）
        Instant instant = Instant.now();
        System.out.println(instant);

        // 2、获取系统此刻的时间戳怎么办
        Instant instant1 = Instant.now();
        System.out.println(instant1.atZone(ZoneId.systemDefault()));

        // 3、Instant对象和Date对象相互转换
        Date date = Date.from(instant);
        System.out.println(date);

        Instant i2 = date.toInstant();
        System.out.println(i2);
    }
}
```

- 时间戳是包含日期和时间的，与java.util.Date很类似，事实上Instant就是类似JDK8以前的Date
- Instant和Date这两个类可以进行转换。



### 8.3 DateTimeFormatter

- 在JDK8中，引入了一个全新的日期与时间格式器DateTimeFormatter。
- 正反都能调用format方法。

```java
public class Demo06DateTimeFormat {
    public static void main(String[] args) {
        // 本地此刻  日期时间 对象
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        // 解析/格式化器
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss EEE a");
        // 正向格式化
        System.out.println(dtf.format(ldt));
        // 逆向格式化
        System.out.println(ldt.format(dtf));

        // 解析字符串时间
        DateTimeFormatter dtf1 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        // 解析当前字符串时间成为本地日期时间对象
        LocalDateTime ldt1 = LocalDateTime.parse("2019-11-11 11:11:11" ,  dtf1);
        System.out.println(ldt1);
        System.out.println(ldt1.getDayOfYear());
    }
}
```



### 8.4 Duration/Period

**Period：**

- 在Java8中，我们可以使用以下类来计算**日期间隔**差异：java.time.Period
- 主要的 Period 类方法 ：getYears()，getMonths() 和 getDays() ，只能精确到年月日。
- 用于 LocalDate 之间的比较。

```java
public class Demo07Period {
    public static void main(String[] args) {
        // 当前本地 年月日
        LocalDate today = LocalDate.now();
        System.out.println(today);//

        // 生日的 年月日
        LocalDate birthDate = LocalDate.of(1998, 10, 13);
        System.out.println(birthDate);

        Period period = Period.between(birthDate, today);//第二个参数减第一个参数

        System.out.println(period.getYears());
        System.out.println(period.getMonths());
        System.out.println(period.getDays());
    }
}
```



**Duration：**

- 在Java8中，我们可以使用以下类来计算**时间间隔**差异：java.time.Duration
- 提供了使用基于时间的值测量时间量的方法。
- 用于 LocalDateTime 之间的比较。也可用于 Instant 之间的比较。

```java
public class Demo08Duration {
    public static void main(String[] args) {
        // 本地日期时间对象。
        LocalDateTime today = LocalDateTime.now();
        System.out.println(today);

        // 出生的日期时间对象
        LocalDateTime birthDate = LocalDateTime.of(2021,8
                ,06,01,00,00);

        System.out.println(birthDate);

        Duration duration = Duration.between(  today , birthDate);//第二个参数减第一个参数

        System.out.println(duration.toDays());//两个时间差的天数
        System.out.println(duration.toHours());//两个时间差的小时数
        System.out.println(duration.toMinutes());//两个时间差的分钟数
        System.out.println(duration.toMillis());//两个时间差的毫秒数
        System.out.println(duration.toNanos());//两个时间差的纳秒数
    }
}
```



总结：

1. **Duration:** **用于计算两个“时间”间隔。** 
2. **Period:**   **用于计算两个“日期”间隔。**



### 8.5 ChronoUnit

**java.time.temporal.ChronoUnit**

- ChronoUnit类可用于在单个时间单位内测量一段时间，这个工具类是最全的了，可以用于比较所有的时间单位

```java
public class Demo09ChronoUnit {
    public static void main(String[] args) {
        // 本地日期时间对象：此刻的
        LocalDateTime today = LocalDateTime.now();
        System.out.println(today);

        // 生日时间
        LocalDateTime birthDate = LocalDateTime.of(1990,10,1,
                10,50,59);
        System.out.println(birthDate);

        System.out.println("相差的年数：" + ChronoUnit.YEARS.between(birthDate, today));
        System.out.println("相差的月数：" + ChronoUnit.MONTHS.between(birthDate, today));
        System.out.println("相差的周数：" + ChronoUnit.WEEKS.between(birthDate, today));
        System.out.println("相差的天数：" + ChronoUnit.DAYS.between(birthDate, today));
        System.out.println("相差的时数：" + ChronoUnit.HOURS.between(birthDate, today));
        System.out.println("相差的分数：" + ChronoUnit.MINUTES.between(birthDate, today));
        System.out.println("相差的秒数：" + ChronoUnit.SECONDS.between(birthDate, today));
        System.out.println("相差的毫秒数：" + ChronoUnit.MILLIS.between(birthDate, today));
        System.out.println("相差的微秒数：" + ChronoUnit.MICROS.between(birthDate, today));
        System.out.println("相差的纳秒数：" + ChronoUnit.NANOS.between(birthDate, today));
        System.out.println("相差的半天数：" + ChronoUnit.HALF_DAYS.between(birthDate, today));
        System.out.println("相差的十年数：" + ChronoUnit.DECADES.between(birthDate, today));
        System.out.println("相差的世纪（百年）数：" + ChronoUnit.CENTURIES.between(birthDate, today));
        System.out.println("相差的千年数：" + ChronoUnit.MILLENNIA.between(birthDate, today));
        System.out.println("相差的纪元数：" + ChronoUnit.ERAS.between(birthDate, today));
    }
}
```



## 9. 包装类

- 其实就是8种基本数据类型对应的引用类型。
  - Byte, Short, Integer, Long, Character, Float, Double, Boolean

Java提供包装类的原因：

- Java为了实现一切皆对象，为8种基本类型提供了对应的引用类型。
- 后面的集合和泛型其实也只能支持包装类型，不支持基本数据类型。

**自动装箱**：基本类型的数据和变量可以直接赋值给包装类型的变量。

**自动拆箱：**包装类型的变量可以直接赋值给基本数据类型的变量。

**包装类的特有功能：**

- 包装类的变量的默认值可以是null，容错率更高
- 可以把基本类型的数据转换成字符串类型(用处不大)
- 可以把字符串类型的数值转换成真实的数据类型（真的很有用）

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;
        Integer a1 = 11;
        Integer a2 = a; // 自动装箱
        System.out.println(a);
        System.out.println(a1);

        Integer it = 100;
        int it1 = it; // 自动拆箱
        System.out.println(it1);

        double db = 99.5;
        Double db2 = db; // 自动装箱了
        double db3 = db2; // 自动拆箱
        System.out.println(db3);

        // int age = null; // 报错了！
        Integer age1 = null;
        Integer age2 = 0;

        System.out.println("-----------------");
        // 1、包装类可以把基本类型的数据转换成字符串形式。（没啥用）
        Integer i3 = 23;
        String rs = i3.toString();
        System.out.println(rs + 1);		//231

        String rs1 = Integer.toString(i3);
        System.out.println(rs1 + 1);	//231

        // 可以直接+字符串得到字符串类型
        String rs2 = i3 + "字符串";
        System.out.println(rs2 + 1);

        System.out.println("-----------------");

        String number = "23";
        //2、字符串数据转换成整数
        // int age = Integer.parseInt(number);
        int age = Integer.valueOf(number);		//也可以使用valueOf（常用）
        System.out.println(age + 1);			//24

        String number1 = "99.9";
        //转换成小数
	//double score = Double.parseDouble(number1);
        double score = Double.valueOf(number1);
        System.out.println(score + 0.1);		//100.0
    }
}
```



## 10. 正则表达式

- 正则表达式可以用一些规定的字符来制定规则，并用来校验数据格式的合法性。

### 10.1 正则表达式的使用

**字符串对象提供了匹配正则表达式的方法**

`public boolean matches(String regex): 判断是否匹配正则表达式，匹配返回true，不匹配返回false`

**字符类(默认匹配一个字符)**

```
[abc]	       只能是a, b, 或c
[^abc]	       除了a, b, c之外的任何字符
[a-zA-Z]               a到z A到Z，包括（范围）
[a-d[m-p]]	       a到d，或m通过p：（[a-dm-p]联合）
[a-z&&[def]]	       d, e, 或f(交集)
[a-z&&[^bc]]	      a到z，除了b和c：（[ad-z]减法）
[a-z&&[^m-p]]    	 a到z，除了m到p：（[a-lq-z]减法）
```

**预定义的字符类(默认匹配一个字符)**

```
.	任何字符
\d	一个数字： [0-9]
\D	非数字： [^0-9]
\s	一个空白字符： [ \t\n\x0B\f\r]
\S	非空白字符： [^\s]
\w	[a-zA-Z_0-9] 英文、数字、下划线
\W	 [^\w] 一个非单词字符
```

注意：\需要转义，写成\\\d

**贪婪的量词（配合匹配多个字符）**

```
X?	X , 一次或根本不
X*	X，零次或多次
X+	X , 一次或多次
X {n}	X，正好n次
X {n, }	X，至少n次
X {n,m}	X，至少n但不超过m次
```

**示例：**

```java
public class RegexDemo02 {
    public static void main(String[] args) {
        //public boolean matches(String regex):判断是否与正则表达式匹配，匹配返回true
        // 只能是 a  b  c
        System.out.println("a".matches("[abc]")); // true
        System.out.println("z".matches("[abc]")); // false	

        // 不能出现a  b  c
        System.out.println("a".matches("[^abc]")); // false
        System.out.println("z".matches("[^abc]")); // true

        System.out.println("a".matches("\\d")); // false
        System.out.println("3".matches("\\d")); // true
        System.out.println("333".matches("\\d")); // false	(默认匹配一个字符，"333"里有3个数字)
        System.out.println("z".matches("\\w")); // true
        System.out.println("2".matches("\\w")); // true
        System.out.println("21".matches("\\w")); // false
        System.out.println("你".matches("\\w")); //false
        System.out.println("你".matches("\\W")); // true
        System.out.println("---------------------------------");
        //  以上正则匹配只能校验单个字符。

        // 校验密码
        // 必须是数字 字母 下划线 至少 6位
        System.out.println("2442fsfsf".matches("\\w{6,}"));	//true
        System.out.println("244f".matches("\\w{6,}"));	//false

        // 验证码 必须是数字和字符  必须是4位
        System.out.println("23dF".matches("[a-zA-Z0-9]{4}"));	//true
        System.out.println("23_F".matches("[a-zA-Z0-9]{4}"));	//false
        System.out.println("23dF".matches("[\\w&&[^_]]{4}"));	//true
        System.out.println("23_F".matches("[\\w&&[^_]]{4}"));	//false
    }
}
```



### 10.2 正则表达式的案例

**需求：**

1. 请编写程序模拟用户输入手机号码、验证格式正确，并给出提示，直到格式输入正确为止。
2. 请编写程序模拟用户输入邮箱号码、验证格式正确，并给出提示，直到格式输入正确为止。
3. 请编写程序模拟用户输入电话号码、验证格式正确，并给出提示，直到格式输入正确为止。

**手机号码的正则表达式：**

```java
public class Regex {
    public static void main(String[] args) {
        checkPhoneNum();
    }

    public static void checkPhoneNum () {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请输入您注册的手机号码：");
            String phoneNum = sc.next();
            if (phoneNum.matches("1[3-9]\\d{9}")) {       //手机号码第一位是1，第二位是3-9，后9位随意数字即可
                System.out.println("手机号码格式正确，注册完成！");
                break;
            } else {
                System.out.println("格式有误，请从新注册！");
            }
        }
    }
}
```

**邮箱格式的正则表达式：**`"\\w{1,15}@[a-z0-9]{2-20}\\.[a-z0-9]{2-20}"`			123abcd456@qq/163.com

`\\.表示一个真实的点(第二个\表示后面的.是一个点，第一个\表示后面的\是一个\)` 

解析：前面1-15位任意英文，数字，下划线；然后接一个@；后面是2-20位a-z, 0-9中的字符，再接一个. ；最后再是2-20位a-z，0-9中的字符。



### 10.3 正则表达式在方法中的应用

**正则表达式在字符串方法中的使用**

|                        方法名                        |                             说明                             |
| :--------------------------------------------------: | :----------------------------------------------------------: |
| public String replaceAll(String regex,String newStr) |               按照正则表达式匹配的内容进行替换               |
|        public String[] split(String regex)：         | 按照正则表达式匹配的内容进行分割字符串，反回一个字符串数组。 |

```java
public class RegexDemo04 {
    public static void main(String[] args) {
        String names = "小路dhdfhdf342蓉儿43fdffdfbjdfaf小何";

        String[] arrs = names.split("\\w+");		//按出现过一次或多次的字母数字下划线进行分割
        for (int i = 0; i < arrs.length; i++) {
            System.out.println(arrs[i]);
        }

        String names2 = names.replaceAll("\\w+", "、");		//将出现一次或多次的字母数字下划线替换成 、
        System.out.println(names2);
    }
}

/*输出：
	小路
	蓉儿
	过儿
	小路、蓉儿、过儿
*/
```



### 10.4 正则表达式爬取信息（了解）

```java
public class RegexDemo05 {
    public static void main(String[] args) {
        String rs = "来黑马程序学习Java,电话020-43422424，或者联系邮箱" +
                "itcast@itcast.cn,电话18762832633，0203232323" +
                "邮箱bozai@itcast.cn，400-100-3233 ，4001003232";

        // 需求：从上面的内容中爬取出 电话号码和邮箱。
        // 1、定义爬取规则，字符串形式
        String regex = "(\\w{1,30}@[a-zA-Z0-9]{2,20}(\\.[a-zA-Z0-9]{2,20}){1,2})|(1[3-9]\\d{9})" +
                "|(0\\d{2,6}-?\\d{5,20})|(400-?\\d{3,9}-?\\d{3,9})";

        // 2、把这个爬取规则编译成匹配对象。
        Pattern pattern = Pattern.compile(regex);

        // 3、得到一个内容匹配器对象
        Matcher matcher = pattern.matcher(rs);

        // 4、开始找了
        while (matcher.find()) {
            String rs1 = matcher.group();		//每找到一组，就取一组
            System.out.println(rs1);
        }
    }
}
```



## 11. Arrays类、Comparator比较器

**Arrays类概述**

- 数组操作工具类，专门用于操作数组元素的。

**Arrays类的常用API：**

|                            方法名                            |                       说明                       |
| :----------------------------------------------------------: | :----------------------------------------------: |
|           public static String toString(类型[] a)            |           返回数组的内容（字符串形式）           |
|              public static void sort(类型[] a)               |              对数组进行默认升序排序              |
| public static <T> void sort(类型[] a, Comparator<? super T> c) |             使用比较器对象自定义排序             |
|       public static int binarySearch(int[] a, int key)       | 二分搜索数组中的数据，存在返回索引，不存在返回-1 |

```java
public class ArraysDemo1 {
    public static void main(String[] args) {
        // 目标：学会使用Arrays类的常用API ,并理解其原理
        int[] arr = {10, 2, 55, 23, 24, 100};
        System.out.println(arr);	//[I@776ec8df

        // 1、返回数组内容的 toString(数组)
//        String rs = Arrays.toString(arr);
//        System.out.println(rs);

        System.out.println(Arrays.toString(arr));	//[10, 2, 55, 23, 24, 100]

        // 2、排序的API(默认自动对数组元素进行升序排序)
        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));		//[2, 10, 23, 24, 55, 100]

        // 3、二分搜索技术，返回索引（前提数组必须排好序才支持，否则出bug）
        int index = Arrays.binarySearch(arr, 55);
        System.out.println(index);		//4

        // 返回不存在元素的规律： - 应该插入的位置索引 - 1
        int index2 = Arrays.binarySearch(arr, 22);
        System.out.println(index2);		//-3（22不存在，会提示该值应该插在第2个位置）


        // 注意：数组如果没有排好序，可能会找不到存在的元素，从而出现bug!!
        int[] arr2 = {12, 36, 34, 25 , 13,  24,  234, 100};
        System.out.println(Arrays.binarySearch(arr2 , 36));		//-7(从中间开始，找36会往右找，则会找不到)
    }
}
```



### 11.1 Arrays类对于Comparator比较器的支持

**Arrays类的排序方法：**

|                            方法名                            |           说明           |
| :----------------------------------------------------------: | :----------------------: |
|              public static void sort(类型[] a)               |  对数组进行默认升序排序  |
| public static <T> void sort(类型[] a, Comparator<? super T> c) | 使用比较器对象自定义排序 |

**自定义排序规则**

- **设置Comparator接口对应的比较器对象，来定制比较规则。**
  - 如果认为左边数据 大于 右边数据 返回正整数
  - 如果认为左边数据 小于 右边数据 返回负整数
  - 如果认为左边数据 等于 右边数据 返回0

```java
public class ArraysDemo2 {
    public static void main(String[] args) {
        // 目标：自定义数组的排序规则：Comparator比较器对象。
        // 1、Arrays的sort方法对于有值特性的数组是默认升序排序
        int[] ages = {34, 12, 42, 23};
        Arrays.sort(ages);
        System.out.println(Arrays.toString(ages));

        // 2、需求：降序排序！(自定义比较器对象，只能支持引用类型的排序！！)
        Integer[] ages1 = {34, 12, 42, 23};
        /**
           参数一：被排序的数组 必须是引用类型的元素
           参数二：匿名内部类对象，代表了一个比较器对象。
         */
        Arrays.sort(ages1, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                // 指定比较规则(升序)
//                if(o1 > o2){
//                    return 1;
//                }else if(o1 < o2){
//                    return -1;
//                }
//                return 0;
                
                // return o1 - o2; // 默认升序
                return o2 - o1; //  降序
            }
        });
        System.out.println(Arrays.toString(ages1));

        System.out.println("-------------------------");
        Student[] students = new Student[3];
        students[0] = new Student("吴磊",23 , 175.5);
        students[1] = new Student("谢鑫",18 , 185.5);
        students[2] = new Student("王亮",20 , 195.5);
        System.out.println(Arrays.toString(students));

        // Arrays.sort(students);  // 直接运行奔溃
        
        Arrays.sort(students, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                // 自己指定比较规则
                // return o1.getAge() - o2.getAge(); // 按照年龄升序排序！
                // return o2.getAge() - o1.getAge(); // 按照年龄降序排序！！
                // return Double.compare(o1.getHeight(), o2.getHeight());   //升序 比较浮点型用Double.compare()
                return Double.compare(o2.getHeight(), o1.getHeight());      //降序
            }
        });
        System.out.println(Arrays.toString(students));
    }
```



## 12. 常见算法

### 12.1 选择排序

```java
public class Test1 {
    public static void main(String[] args) {
        int[] arr = {5, 1, 3, 2};
        //定义一个循环控制选择几轮： arr.length - 1
        for (int i = 0; i < arr.length - 1; i++) {
            //定义内部循环，控制选择几次
            for (int j = i + 1; j < arr.length; j++) {		
                if(arr[i] > arr[j]) {
                    int temp = arr[i];
                    arr[i] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}
```



### 12.2 冒泡排序

```java
public class Test2 {
    public static void main(String[] args) {
        int[] arr = {5, 1, 3, 2};
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j < arr.length-i-1; j++) {		
                if(arr[j] > arr[j+1]) {
                    int temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}
```



### 12.3 二分查找（效率高）

**步骤：**

1. 定义变量记录左边和右边位置
2. 使用while循环控制查询（条件是左边位置<=右边位置）
3. 循环内部获取中间元素索引
4. 判断当前要找的元素如果大于中间元素，左边位置=中间索引+1
5. 判断当前要找的元素如果小于中间元素，右边位置=中间索引-1
6. 判断当前要找的元素如果等于中间元素，返回当前中间元素索引

```java
public class Test3 {
    public static void main(String[] args) {
        // 1、定义数组
        int[] arr = {10, 14, 16, 25, 28, 30, 35, 88, 100};
        //                                            r
        //                                                l
        //
        System.out.println(binarySearch(arr , 35));
        System.out.println(binarySearch(arr , 350));
    }
    /**
     * 二分查找算法的实现
     * @param arr  排序的数组
     * @param data 要找的数据
     * @return  索引，如果元素不存在，直接返回-1
     */
    public static int binarySearch(int[] arr, int data){
        // 1、定义左边位置  和 右边位置
        int left = 0;
        int right = arr.length - 1;

        // 2、开始循环，折半查询。
        while (left <= right){
            // 取中间索引
            int middleIndex = (left + right) / 2;
            // 3、判断当前中间位置的元素和要找的元素的大小情况
            if(data > arr[middleIndex]) {
                // 往右边找，左位置更新为 = 中间索引+1
                left = middleIndex + 1;
            }else if(data < arr[middleIndex]) {
                // 往左边找，右边位置 = 中间索引 - 1
                right = middleIndex - 1;
            }else {
                return middleIndex;		//当data==arr[middleIndex]时，找到了，返回索引
            }
        }
        return -1; 		// 查无此元素(当left＝right时，循环中还没找到)
    }
}
```



## 13. Lambda表达式

**Lambda概述**

- Lambda表达式是JDK 8开始后的一种新语法形式。
-  **作用：简化匿名内部类的代码写法。**

**Lambda表达式的简化格式：**

```
(匿名内部类被重写方法的形参列表) -> {
	被重写方法的方法体代码。
}
注：-> 是语法形式，无实际含义
```

**注意：**Lambda表达式只能简化**函数式接口**的**匿名内部类**的写法形式

**函数式接口：**

- 首先必须是接口、其次接口中有且仅有一个抽象方法的形式

```java
public class LambdaDemo2 {
    public static void main(String[] args) {
        // 目标：学会使用Lambda的标准格式简化匿名内部类的代码形式
        // 注意：Lambda只能简化接口中只有一个抽象方法的匿名内部类形式（函数式接口）
//        Swimming s1 = new Swimming() {
//            @Override
//            public void swim() {
//                System.out.println("老师游泳贼溜~~~~~");
//            }
//        };
          //简化为：
//        Swimming s1 = () -> {
//            System.out.println("老师游泳贼溜~~~~~");
//        };
        //进一步简化(大括号):
        Swimming s1 = () -> System.out.println("老师游泳贼溜~~~~~");
        go(s1);

        System.out.println("---------------------");
//        go(new Swimming() {
//            @Override
//            public void swim() {
//                System.out.println("学生游泳很开心~~~");
//            }
//        });
          //简化为:
//        go(() ->{
//                System.out.println("学生游泳很开心~~~");
//        });
        //进一步简化(大括号)：
        go(() -> System.out.println("学生游泳很开心~~~"));
    }

    public static void go(Swimming s){
        System.out.println("开始。。。");
        s.swim();
        System.out.println("结束。。。");
    }
}

@FunctionalInterface // 一旦加上这个注解必须是函数式接口，里面只能有一个抽象方法
interface Swimming{
    void swim();
}
```



==注意：==**在Lambda表达式中，不允许修改对象的引用和基本类型的值。**

在JDK 8之前，如果在匿名类内部引用函数局部变量，必须将其声明为final，即不可变对象。

JDK 8开始后，在Lambda表达式以及匿名类内部，如果引用某局部变量，不用你自己声明，也会被直接视为final。所以不能修改引用值。

**Lambda修改外部基本类型变量值的方法：**

- 使用线程安全Integer的Atomiclnteger。

  `java.util.concurrent.atomic`包下全部是线程安全的包装类。

- ```java
  AtomicInteger i = new AtomicInteger(0);
  Optional.of("aaa").ifPresent(str -> {
  	i.set(2);
  });
  System.out.println(i)		    //2
  System.out.println( i.get() )	  //get()方法可以获取值
  ```

  

### 13.1 Lambda表达式的省略规则

**Lambda表达式的省略写法（进一步在Lambda表达式的基础上继续简化）**

- 参数类型可以省略不写。
- 如果只有一个参数，参数类型可以省略，同时()也可以省略。
- 如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写,同时要省略分号！
- 如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写。此时，如果这行代码是return语句，必须省略return不写，同时也必须省略";"不写

### 13.2 Lambda实战-简化常见函数式接口

**Lambda表达式简化Comparator接口和简化按钮监听器ActionListener的匿名形式：**

```java
public class LambdaDemo3 {
    public static void main(String[] args) {
        Integer[] ages1 = {34, 12, 42, 23};
        /**
         参数一：被排序的数组 必须是引用类型的元素
         参数二：匿名内部类对象，代表了一个比较器对象。
         */
        
          //简化Comparator接口：
//        Arrays.sort(ages1, new Comparator<Integer>() {
//            @Override
//            public int compare(Integer o1, Integer o2) {
//                return o2 - o1; //  降序
//            }
//        });

          //简化为:
//        Arrays.sort(ages1, (Integer o1, Integer o2) -> {
//                return o2 - o1; //  降序
//        });

          //进一步简化：
//        Arrays.sort(ages1, ( o1,  o2) -> {
//            return o2 - o1; //  降序
//        });

        //更近一步简化：
        Arrays.sort(ages1, ( o1,  o2 ) ->  o2 - o1 );

        System.out.println(Arrays.toString(ages1));
        
        System.out.println("---------------------------");
        //简化按钮监听器ActionListener：
        JFrame win = new JFrame("登录界面");
        JButton btn = new JButton("我是一个很大的按钮");
//        btn.addActionListener(new ActionListener() {
//            @Override
//            public void actionPerformed(ActionEvent e) {
//                System.out.println("有人点我，点我，点我！！");
//            }
//        });

          //简化为:
//        btn.addActionListener((ActionEvent e) -> {
//                System.out.println("有人点我，点我，点我！！");
//        });

          //只有一个参数，可以进一步简化：
//        btn.addActionListener(( e) -> {
//            System.out.println("有人点我，点我，点我！！");
//        });
          //小括号也可以不写
//        btn.addActionListener( e -> {
//            System.out.println("有人点我，点我，点我！！");
//        });
        //只有一个参数，代码只有一行，可以更简化:
        btn.addActionListener( e -> System.out.println("有人点我，点我，点我！！") );

        win.add(btn);
        win.setSize(400, 300);
        win.setVisible(true);
    }
}
```





# 集合

## 1. 集合的概述

集合和数组都是容器。

**数组的特点：**

- 数组定义完成并启动后，**类型确定、长度固定**。
- 适合元素的个数和类型确定的业务场景，不适合做需要增删数据操作。

**集合的特点：**

- 集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。集合更像气球。
- 集合非常适合做元素的增删操作。

**总结：**

1. **数组和集合的元素存储的个数问题**
   - **数组定义后类型确定，长度固定**。
   - **集合类型可以不固定，大小是可变的。**
2. **数组和集合存储元素的类型问题**
   - **数组可以存储基本类型和引用类型的数据。**
   - **集合只能存储引用数据类型的数据。**
3. **数组和集合适合的场景**
   - **数组适合做数据个数和类型确定的场景。**
   - **集合适合做数据个数不确定，且要做增删元素的场景。**



## 2. **集合的体系特点**

**集合类体系结构**

- 集合
  - Collection：单列集合，每个元素只包含一个值
  - Map：双列集合，每个元素包含两个值（键值对）

## 3. Collection集合体系：

- Collection（接口）
  - List（接口）
    - ArrayList（实现类）
    - LinkedList（实现类）
  - Set（接口）
    - HashSet（实现类）
      - LinkedHashSet（实现类）
    - TreeSet（实现类）

**Collection集合特点：**

- **List系列集合：**添加的元素是有序、可重复、有索引。
  - ArrayList、LinekdList ：有序、可重复、有索引。
- **Set系列集合：**添加的元素是无序、不重复、无索引。
  - HashSet: 无序、不重复、无索引
    - LinkedHashSet: **有序**、不重复、无索引。
  - TreeSet：**按照大小默认升序排序、**不重复、无索引。



**集合对于泛型的支持：**

- 集合都是支持泛型的，可以在编译阶段约束集合只能操作某种数据类型

```
Collection<String> lists = new ArrayList<String>();
Collection<String> lists = new ArrayList<>(); 	//JDK 1.7开始后面的泛型类型申明可以省略不写
```

==注意：集合和泛型都只能支持引用数据类型，不支持基本数据类型，所以集合中存储的元素都认为是对象。==



**集合中要存储基本类型的数据，可以使用基本数据类型的包装类：**

```
Collection<Integer> lists = new ArrayList<>();
Collection<Double> lists = new ArrayList<>();
```



### 3.1 Collection集合常用API

- Collection是单列集合的祖宗接口，它的功能是全部单列集合都可以继承使用的。

**Collection API如下:**

|              方法名称               |                        说明                        |
| :---------------------------------: | :------------------------------------------------: |
|       public boolean add(E e)       |            把给定的对象添加到当前集合中            |
|         public void clear()         |                清空集合中所有的元素                |
|     public boolean remove(E e)      |            把给定的对象在当前集合中删除            |
| public boolean contains(Object obj) |          判断当前集合中是否包含给定的对象          |
|      public boolean isEmpty()       |                判断当前集合是否为空                |
|          public int size()          |                返回集合中元素的个数                |
|      public Object[] toArray()      | 把集合中的元素，存储到数组中  (用Object[]数组接收) |

```java
public class CollectionDemo {
    public static void main(String[] args) {
        // HashSet:添加的元素是无序，不重复，无索引。
        Collection<String> c = new ArrayList<>();
        // 1.添加元素, 添加成功返回true。
        c.add("Java");
        c.add("HTML");
        System.out.println(c.add("HTML"));
        c.add("MySQL");
        c.add("Java");
        System.out.println(c.add("黑马"));
        System.out.println(c); // [Java, HTML, HTML, MySQL, Java, 黑马]

        // 2.清空集合的元素。
        // c.clear();
        // System.out.println(c);

        // 3.判断集合是否为空 是空返回true,反之。
        // System.out.println(c.isEmpty());

        // 4.获取集合的大小。
        System.out.println(c.size());

        // 5.判断集合中是否包含某个元素。
        System.out.println(c.contains("Java"));  // true
        System.out.println(c.contains("java")); // false
        System.out.println(c.contains("黑马")); // true

        // 6.删除某个元素:如果有多个重复元素默认删除前面的第一个！
        System.out.println(c.remove("java")); // false
        System.out.println(c);
        System.out.println(c.remove("Java")); // true
        System.out.println(c);

        // 7.把集合转换成数组  [HTML, HTML, MySQL, Java, 黑马]
        Object[] arrs = c.toArray();
        System.out.println("数组：" + Arrays.toString(arrs));

        System.out.println("----------------------拓展----------------------");
        Collection<String> c1 = new ArrayList<>();
        c1.add("java1");
        c1.add("java2");
        Collection<String> c2 = new ArrayList<>();
        c2.add("赵敏");
        c2.add("殷素素");
        // addAll把c2集合的元素全部倒入到c1中去。
        c1.addAll(c2);
        System.out.println(c1);
        System.out.println(c2);
    }
}
```



### 3.2 Collection集合的遍历方式

#### 3.2.1 方式一：迭代器

**迭代器遍历概述**

- 遍历就是一个一个的把容器中的元素访问一遍。 
- 迭代器在Java中的代表是**Iterator**，迭代器是集合的专用遍历方式。

**Collection集合获取迭代器：**

|            方法名称            |                             说明                             |
| :----------------------------: | :----------------------------------------------------------: |
| **Iterator<E>** **iterator()** | 返回集合中的迭代器对象，该迭代器对象**默认指向当前集合的0索引** |

**Iterator中的常用方法：**

|     方法名称      |                             说明                             |
| :---------------: | :----------------------------------------------------------: |
| boolean hasNext() |  询问当前位置是否有元素存在，存在返回true ,不存在返回false   |
|     E next()      | 获取当前位置的元素，并**同时将迭代器对象移向下一个位置**，注意**防止取出越界**。 |

**迭代器遍历集合常用方式：**

```
Iterator<String> it = lists.iterator();
while(it.hasNext()){
	String ele = it.next();
	System.out.println(ele);
}
```



#### 3.2.2 方式二：增强for循环

**增强for循环：**

- 既可以遍历集合也可以遍历数组。

```
for(元素数据类型 变量名 : 数组或者Collection集合) {
         //在此处使用变量即可，该变量就是元素
}
```

示例：

```java
//遍历集合：
Collection<String> list = new ArrayList<>();
...
for(String ele : list) {
    System.out.println(ele);
}
//遍历数组：
double[] scores={100,99.5,79};
for(double sc : scores){
    System.out.println(sc);
}
```



#### 3.2.3 方式三：Lambda表达式

**Lambda表达式遍历集合**

- 得益于JDK 8开始的新技术Lambda表达式，提供了一种更简单、更直接的遍历集合的方式。

**Collection集合Lambda遍历的API：**

|                     方法名称                      |        说明        |
| :-----------------------------------------------: | :----------------: |
| default void forEach(Consumer<? super T> action): | 结合lambda遍历集合 |

**forEach源码内部写了增强for循环**

```java
public class CollectionDemo03 {
    public static void main(String[] args) {
        Collection<String> lists = new ArrayList<>();
        lists.add("赵敏");
        lists.add("小昭");
        lists.add("殷素素");
        lists.add("周芷若");
        System.out.println(lists);        // [赵敏, 小昭, 殷素素, 周芷若]

        lists.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });
        
	  //简化为：
//        lists.forEach(s -> {
//                System.out.println(s);
//        });
        
	//再简化为：
        // lists.forEach(s ->  System.out.println(s) );
        
	//在进一步简化为：
        lists.forEach(System.out::println );
    }
}
```



### 3.2 Collection集合存储自定义类型的对象

重写电影案例：

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cadadd4b25eb3df57149db5e7891e1d2.png)



## 4. 常见数据结构

**数据结构概述**

- 数据结构是计算机底层存储、组织数据的方式。是指数据相互之间是以什么方式排列在一起的
- 通常情况下，精心选择的数据结构可以带来更高的运行或者存储效率

**常见的数据结构：**栈、队列、数组、链表、二叉树、二叉查找树、平衡二叉树、红黑树 .....

### 4.1 栈、队列

**栈**数据结构的执行特点：

- 先进后出，后进先出

**数据进入栈模型的过程称为：**压/进栈

**数据离开栈模型的过程称为：**弹/出栈



**队列**数据结构的执行特点：

- 先进先出，后进后出

数据从**后端**进入队列模型的过程称为：入队列

数据从**前端**离开队列模型的过程称为：出队列



### 4.2 数组

数组是一种**查询快，增删慢**的模型。

- **查询速度快：**查询数据通过地址值和索引定位，查询任意数据耗时相同**。**（元素在内存中是连续存储的）
- **删除效率低：**要将原始数据删除，同时后面每个数据前移。
- **添加效率极低：**添加位置后的每个数据后移，再添加元素。



### 4.3 链表

链表有单向链表和双向链表

**链表的特点**

- 链表中的元素是在内存中不连续存储的，每个元素节点包含数据值和下一个元素的地址。
- 链表中的元素是游离存储的，每个元素节点包含数据值和下一个元素的地址。
- 链表**查询慢**。无论查询哪个数据都要从头开始找
- **链表增删相对快**(对比数组)



### 4.4 二叉树、 二叉查找树

**二叉树的特点**

- **只能有一个根节点**，每个节点最多支持2个直接子节点。
- **节点的度：** 节点拥有的子树的个数，二叉树的度不大于2 叶子节点 度为0的节点，也称之为终端结点。
- **高度：**叶子结点的高度为1，叶子结点的父节点高度为2，以此类推，根节点的高度最高。
- **层：**根节点在第一层，以此类推
- **兄弟节点 ：**拥有共同父节点的节点互称为兄弟节点



![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/369469110649322bafa02dcf8d144d6b.png)

**二叉树查找树(二叉排序树or二叉搜索数)：**

- 每一个节点上最多有两个子节点
- 左子树上所有节点的值都小于根节点的值
- 右子树上所有节点的值都大于根节点的值

**目的：提高检索数据的性能**。

**二叉树查找树添节点的规则：**

- 小的存左边，大的存右边，一样的不存



### 4.5 平衡二叉树

**二叉树查找存在的问题：**

- **问题：当节点偏向一边时，出现瘸子现象，导致查询的性能与单链表一样，查询速度变慢！**

**平衡二叉树：**

- **平衡二叉树是在满足查找二叉树的大小规则下，让树尽可能矮小，以此提高查数据的性能。**

  ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/86a4e1e57ff06312786a877f698df955.png)

**平衡二叉树的要求：**

- 任意节点的左右两个子树的高度差不超过1，任意节点的左右两个子树都是一颗平衡二叉树

**平衡二叉树在添加元素后可能导致不平衡**

- 基本策略是进行**左旋**，或者**右旋**保证平衡。

- 左边高左旋，右边高右旋。（哪个节点不平衡就旋哪个节点）

### 4.6 红黑树

**红黑树概述**

- 红黑树是一种自平衡的二叉查找树，是计算机科学中用到的一种数据结构。
- 1972年出现，当时被称之为平衡二叉B树。1978年被修改为如今的"红黑树"。
- 每一个节点可以是红或者黑；红黑树不是通过高度平衡的，它的平衡是通过“红黑规则”进行实现的。

**红黑规则**

- 每一个节点或是红色的，或者是黑色的，**根节点必须是黑色。**
- 如果一个节点没有子节点或者父节点，则该节点相应的指针属性为Nil，这些Nil视为叶节点，为黑色。
- 如果某一个节点是红色，那么它的子节点必须是黑色(**不能出现两个红色节点相连的情况**)。
- **对每一个节点，从该节点到其所有后代叶节点的简单路径上，均包含相同数目的黑色节点。**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0acea7dafcf760ac4e5a437d9e631458.png)

**添加节点**

- 添加的节点的颜色，可以是红色的，也可以是黑色的。
- 默认用红色效率高。

**红黑树增删改查的性能都很好**



## 5. List系列集合

### 5.1 List集合特点、特有API

**List系列集合特点**

- ArrayList、LinekdList ：有序，可重复，有索引
- 有序：存储和取出的元素顺序一致
- 有索引：可以通过索引操作元素
- 可重复：存储的元素可以重复

**List集合特有方法**

- List集合因为支持索引，所以多了很多索引操作的独特api，其他Collection的功能List也都继承了。

|           方法名称            |                  说明                  |
| :---------------------------: | :------------------------------------: |
| void add(int index,E element) |   在此集合中的指定位置插入指定的元素   |
|      E remove(int index)      | 删除指定索引处的元素，返回被删除的元素 |
|  E set(int index,E element)   | 修改指定索引处的元素，返回被修改的元素 |
|       E get(int index)        |          返回指定索引处的元素          |



### 5.2 List集合的遍历方式

- 迭代器 
- 增强for循环
- Lambda表达式
- for循环（因为List集合存在索引）



### 5.3 ArrayList集合的底层原理

**ArrayList集合底层原理**

- ArrayList底层是基于数组实现的：根据索引定位元素快，增删需要做元素的移位操作。
- 第一次创建集合并添加第一个元素的时候，在底层创建一个默认长度为10的数组。

- ArrayList中有一个变量size，记录当前元素的个数，同时每当使用add添加一个元素的时候，size就往后移一位
- 当发现插入的元素位置与当前容量相等时，会扩容，扩大到原来的1.5倍，再把之前的元素迁移到新的集合里



### 5.4 LinkedList集合的底层原理

**LinkedList的特点**

- 底层数据结构是双链表，查询慢，首尾操作的速度是极快的，所以多了很多首尾操作的特有API

**LinkedList集合的特有功能**

|         方法名称          |               说明               |
| :-----------------------: | :------------------------------: |
| public void addFirst(E e) |    在该列表开头插入指定的元素    |
| public void addLast(E e)  |  将指定的元素追加到此列表的末尾  |
|    public E getFirst()    |     返回此列表中的第一个元素     |
|    public E getLast()     |    返回此列表中的最后一个元素    |
|  public E removeFirst()   |  从此列表中删除并返回第一个元素  |
|   public E removeLast()   | 从此列表中删除并返回最后一个元素 |

**示例：做一个栈和队列**

```java
public class ListDemo03 {
    public static void main(String[] args) {
        // LinkedList可以完成队列结构，和栈结构 （双链表）
        // 1、做一个队列：
        LinkedList<String> queue = new LinkedList<>();
        // 入队
        queue.addLast("1号");
        queue.addLast("2号");
        queue.addLast("3号");
        System.out.println(queue);
        // 出队
        // System.out.println(queue.getFirst());
        System.out.println(queue.removeFirst());        //1号
        System.out.println(queue.removeFirst());        //2号
        System.out.println(queue.removeFirst());        //3号
        System.out.println(queue);      //[]

        // 2、做一个栈
        LinkedList<String> stack = new LinkedList<>();
        // 入栈 压栈 (push),其实push内部是addFirst()
        stack.push("第1颗子弹");
        stack.push("第2颗子弹");
        stack.push("第3颗子弹");
        stack.push("第4颗子弹");
        System.out.println(stack);

        // 出栈  弹栈 (pop),其实pop内部是removeFirst()
        System.out.println(stack.pop());        //第4颗子弹
        System.out.println(stack.pop());        //第3颗子弹
        System.out.println(stack.pop());        //第2颗子弹
        System.out.println(stack);        //[第1颗子弹]
    }
}
```



总结：

- 如果查询多而增删少用ArrayList集合。(用的最多的)
- 如果查询少而增删首尾较多用LinkedList集合。



## 6. 补充知识：集合的并发修改异常问题

- 当我们从集合中找出某个元素并删除的时候可能出现一种并发修改异常问题。

**哪些遍历存在问题？**

- 迭代器遍历集合且直接用**集合删除元素**的时候可能出现。
- 增强for循环遍历集合且直接用**集合删除元素**的时候可能出现。

**哪种遍历且删除元素不出问题**

- 迭代器遍历集合但是用**迭代器自己的删除方法**操作可以解决。
- 使用for循环遍历并删除元素不会存在这个问题。

```java
public class Test {
    public static void main(String[] args) {
        // 1、准备数据
        ArrayList<String> list = new ArrayList<>();
        list.add("黑马");
        list.add("Java");
        list.add("Java");
        list.add("赵敏");
        list.add("赵敏");
        list.add("素素");
        System.out.println(list);
        // [黑马, Java, Java, 赵敏, 赵敏, 素素]
        //     it

        // 需求：删除全部的Java信息。
        // 1、迭代器遍历删除
        Iterator<String> it = list.iterator();
//        while (it.hasNext()){
//            String ele = it.next();
//            if("Java".equals(ele)){
//                // 删除Java
//                // list.remove(ele); 		//使用集合删除，会出现问题
//                it.remove(); // 使用迭代器删除当前所在元素，并且it不会后移(内部做了it--)
//            }
//        }
		// 集合删除会出问题，因为每删除一个，后面的元素往前移，it指向也会往后移，所以如果有两个连着的字符相同，当刚好又是删除的元素，就会有bug

//        System.out.println(list);

        // 2、foreach遍历删除 (会出现问题，这种无法解决的，foreach不能边遍历边删除，会出bug)
//        for (String s : list) {
//            if("Java".equals(s)){
//                list.remove(s);
//            }
//        }

        // 3、lambda表达式(会出现问题，这种无法解决的，Lambda遍历不能边遍历边删除，会出bug)
//        list.forEach(s -> {
//            if("Java".equals(s)){
//                list.remove(s);
//            }
//        });

        // 4、for循环(边遍历边删除集合没毛病，但是必须从后面开始遍历删除才不会出现漏掉应该删除的元素)
        for (int i = list.size() - 1; i >= 0 ; i--) {
            String ele = list.get(i);
            if("Java".equals(ele)){
                list.remove(ele);
            }
        }
        
        //非得从前往后删也可以，每次删完让i自减回到原来位置即可
        for(int i=0;i<list.size();i++){
            String ele=list.get(i);
            if("Java".equals(ele)){
                list.remove(ele);
                i--;
            }
        }
        
        System.out.println(list);
    }
}
```



## 7. 补充知识：泛型深入

**泛型概述**

- 泛型：是JDK5中引入的特性，可以在编译阶段约束操作的数据类型，并进行检查。
- 泛型的格式：<数据类型>; 注意：**泛型只能支持引用数据类型**。
- **集合体系的全部接口和实现类都是支持泛型的使用的。**

**泛型的好处：**

- 统一数据类型。
- 把运行时期的问题提前到了编译期间，避免了强制类型转换可能出现的异常，因为编译阶段类型就能确定下来。

**若实在是要用任意类型的集合，将泛型的数据类型写成Object即可**



**泛型可以在很多地方进行定义:**

- 类后面 —— 泛型类
- 方法申明上 —— 泛型方法
- 接口后面 —— 泛型接口



### 7.1 自定义泛型类

**泛型类的概述:**

- 定义类时同时定义了泛型的类就是泛型类。

  泛型类的格式：`修饰符 class 类名<泛型变量>{ }`

  示例：`public class MyArrayList<T> { }`

- 此处泛型变量**T**可以随便写为任意标识，常见的如**E、T、K、V**等。

- **作用**：编译阶段可以指定数据类型，类似于集合的作用。

**泛型类的原理**：

- 把出现泛型变量的地方全部替换成传输的真实数据类型。



**案例：模拟ArrayList集合自定义一个集合MyArrayList集合,完成添加和删除功能的泛型设计即可。**

MyArrayList类：

```java
public class MyArrayList<E> {
    private ArrayList baseList=new ArrayList();     //定义一个ArrayList的变量，完成下面功能

    public void add(E e){
        baseList.add(e);        //自己写一个add()比较困难，直接调用官方的
    }

    public void remove(E e){
        baseList.remove(e);
    }

    @Override
    public String toString() {
        return baseList.toString();
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        MyArrayList<Integer> mal=new MyArrayList<>();
        mal.add(10);
        mal.add(20);
        mal.add(30);
//      mal.add("abc");     //报错，指定类型为Integer了，只能添加int类型的数据
        System.out.println(mal);    //[10, 20, 30]
        mal.remove(20);
        System.out.println(mal);    //[10, 30]
    }
}
```



### 7.2 自定义泛型方法

**泛型方法的概述：**

- 定义方法时同时定义了泛型的方法就是泛型方法。

- 泛型方法的格式：`修饰符 <泛型变量> 方法返回值 方法名称(形参列表){}`

  范例：`public <T> void show(T t) { }`

- **作用**：方法中可以使用泛型接收一切实际类型的参数，方法更具备通用性。

**泛型方法的原理：**

- 把出现泛型变量的地方全部替换成传输的真实数据类型。



**案例：给你任何一个类型的数组，都能返回它的内容。也就是实现Arrays.toString(数组)的功能**



```java
public class GenericMethod {
    public static void main(String[] args) {
        String[] names={"小路","芙蓉","小何"};
        printArray(names);      //[小路, 芙蓉, 小何]

        Integer[] ages={18,20,25};
        printArray(ages);       //[18, 20, 25]
    }

    public static <T> void printArray(T[] arr){
        if(arr!=null){
            StringBuilder sb=new StringBuilder("[");
            for (int i = 0; i < arr.length; i++) {
                sb.append(arr[i]).append(i==arr.length-1?"":", ");
            }
            sb.append("]");
            System.out.println(sb);
        }else{
            System.out.println(arr);
        }
    }
}
```

**也可以有返回值，这样传进去什么类型的数据，就可以返回什么类型的数据出来：**

```java
String[] names2=getArray(names);
Integer[] ages2=getArray(ages);
    
public static <T> T[] getArray(T[] arr){
    return arr;
}
```



### 7.3 自定义泛型接口

**泛型接口的概述：**

- 使用了泛型定义的接口就是泛型接口。

- 泛型接口的格式：`修饰符 interface 接口名称<泛型变量>{}`

  范例：`public interface Data<E>{}`

- **作用**：泛型接口可以让实现类选择当前功能需要操作的数据类型。可以约束实现类，实现类可以在实现接口的时候传入自己操作的数据类型这样重写的方法都将是针对于该类型的操作。

**泛型接口的原理：**

- 实现类可以在实现接口的时候传入自己操作的数据类型，这样重写的方法都将是针对于该类型的操作。



**案例：教务系统，提供一个接口可约束一定要完成数据（学生，老师）的增删改查操作**

数据接口类：Data

```java
public interface Data<E> {
    void add(E e);
    void delete(int id);
    void update(E e);
    E queryById();
}
```

学生实现类：StudentData

```java
public class StudentData implements Data<Student>{

    @Override
    public void add(Student student) {

    }

    @Override
    public void delete(int id) {

    }

    @Override
    public void update(Student student) {

    }

    @Override
    public Student queryById() {
        return null;
    }
}
```

老师实现类：TeacherData

```java
public class TeacherData implements Data<Teacher>{

    @Override
    public void add(Teacher teacher) {

    }

    @Override
    public void delete(int id) {

    }

    @Override
    public void update(Teacher teacher) {

    }

    @Override
    public Teacher queryById() {
        return null;
    }
}
```



### 7.4 泛型通配符、上下限

**通配符：**?

- ? 可以在 “使用泛型” 的时候代表一切类型。

- E T K V 是在定义泛型的时候使用的。

**泛型通配符：案例导学**

- 开发一个极品飞车的游戏，所有的汽车都能一起参与比赛。

  ```java
  /**
      目标：泛型通配符。?
  
      需求：开发一个极品飞车的游戏，所有的汽车都能一起参与比赛。
  
      注意：
          虽然BMW和BENZ都继承了Car
          但是ArrayList<BMW>和ArrayList<BENZ>与ArrayList<Car>没有关系的！!
      通配符：？
          ？可以在“使用泛型”的时候代表一切类型。
          E T K V 是在定义泛型的时候使用的。
      泛型的上下限：
          ？ extends Car : ?必须是Car或者其子类  泛型上限
          ? super Car ：？必须是Car或者其父类   泛型下限
      小结：
          通配符：？
          ？可以在“使用泛型”的时候代表一切类型。
  
   */
  public class GenericDemo {
      public static void main(String[] args) {
          ArrayList<BMW> bmws = new ArrayList<>();
          bmws.add(new BMW());
          bmws.add(new BMW());
          bmws.add(new BMW());
          go(bmws);
  
          ArrayList<BENZ> benzs = new ArrayList<>();
          benzs.add(new BENZ());
          benzs.add(new BENZ());
          benzs.add(new BENZ());
          go(benzs);
  
          /*
          ArrayList<Dog> dogs = new ArrayList<>();
          dogs.add(new Dog());
          dogs.add(new Dog());
          dogs.add(new Dog());
          go(dogs);
          */
      }
  
      /**
         所有车比赛
       */
      //若不设置泛型上限，则狗也传可以进来
      public static void go(ArrayList<? extends Car> cars){
          
      }
  }
  
  class Dog{
  
  }
  
  class BENZ extends Car{
  }
  
  class BMW extends Car{
  }
  
  // 父类
  class Car{
  }
  ```



**注意：**虽然BMW和BENZ都继承了Car但是ArrayList<BMW>和ArrayList<BENZ>与ArrayList<Car>没有关系的！!

**泛型的上下限：**

- <? **extends** **Car**> ： ?必须是Car或者其子类  泛型上限
- <? **super** **Car**> ： ?必须是Car或者其父类  泛型下限



## 8. Set系列集合

### 8.1 Set系列集系概述

**Set系列集合特点**

- 无序：存取顺序不一致
- 不重复：可以去除重复
- 无索引：没有带索引的方法，所以不能使用普通for循环遍历，也不能通过索引来获取元素

**Set集合实现类特点**

- HashSet : 无序、不重复、无索引。
- LinkedHashSet：**有序**、不重复、无索引。
- TreeSet：**排序**、不重复、无索引。

**Set集合的功能上基本上与Collection的API一致。**

```java
public class SetDemo1 {
    public static void main(String[] args) {
        // 看看Set系列集合的特点： HashSet LinkedHashSet TreeSet
        //
        Set<String> sets = new HashSet<>(); // 一行经典代码  无序不重复，无索引
        // Set<String> sets = new LinkedHashSet<>(); // 有序  不重复 无索引
        sets.add("MySQL");
        sets.add("MySQL");
        sets.add("Java");
        sets.add("Java");
        sets.add("HTML");
        sets.add("HTML");
        sets.add("SpringBoot");
        sets.add("SpringBoot");
        System.out.println(sets);       //[Java, MySQL, HTML, SpringBoot]
    }
}
```



### 8.2 HashSet元素无序的底层原理：哈希表

**HashSet底层原理**

- HashSet集合底层采取**哈希表**存储的数据。
- 哈希表是一种对于增删改查数据性能都较好的结构。

**哈希表的组成**

- **JDK8之前**，底层使用**数组+链表**组成
- **JDK8开始后**，底层采用**数组+链表+红黑树**组成。

**哈希值**

- 是JDK根据对象的**地址**，按照某种规则计算出来的int类型的**数值**。

**Object类的API**

- `public int hashCode()：返回对象的哈希值`

**对象的哈希值特点**

- 同一个对象多次调用hashCode()方法返回的哈希值是相同的
- 默认情况下，不同对象的哈希值是不同的

```java
public class SetDemo2 {
    public static void main(String[] args) {
        // 目标：学会获取对象的哈希值，并确认一下
        String name = "itheima";
        System.out.println(name.hashCode());        //2118746965

        String name1 = "itheima1";
        System.out.println(name1.hashCode());       //1256646524
    }
}
```



**HashSet 1.7版本原理解析：数组** **+** **链表 +（结合哈希算法)**

1. 创建一个默认长度16的数组，数组名table
2. 根据元素的哈希值跟数组的长度求余(0-15)计算出应存入的位置（哈希算法）
3. 判断当前位置是否为null，如果是null直接存入
4. 如果位置不为null，表示有元素，则调用equals方法比较(内容不重复）
5. 比较后如果一样，则不存
6. 比较后如果不一样，则存入数组：
   - JDK 7新元素占老元素位置，指向老元素（形成链表）
   - JDK 8中新元素挂在老元素下面
7. 当数组存满到16*0.75=12时，就自动扩容，每次扩容原先的两倍

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/df594772193684029d394e87c04613e7.png)

**结论：哈希表是一种对于增删改查数据性能都较好的结构。**



**JDK1.8版本开始HashSet原理解析**

- 底层结构：哈希表（数组、链表、红黑树的结合体）
- 当挂在元素下面的数据过多时，查询性能降低，从JDK8开始后，当链表长度超过**8**的时候，自动转换为红黑树。



### 8.3 HashSet元素去重复的底层原理

上面的第4步可以说明。

**结论：如果希望Set集合认为2个内容一样的对象是重复的，必须重写对象的hashCode()和equals()方法。（IDEA可以直接一键自动生成）**

因为内容相同的2个对象，有可能他们的地址不一样，所以根据HashCode所获取的哈希值就不一样，计算出来的位置也就不一样，则还没有到equals进行比较时，该对象就已经存进去了，就可能与之前的对象内容重复。

**重写hashCode()的原因：**保证相同对象所获取的哈希值相同。

**重写equals()的原因：**不同对象的地址不同，而equals方法默认比较的是地址，所以重写保证内容相同的对象就是重复的。



**示例：**

```java
public class Student {
    private String name;
    private int age;
    private char sex;

    public Student() {
    }

    public Student(String name, int age, char sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    //······get/set()方法

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age && sex == student.sex && Objects.equals(name, student.name);
    }

    @Override
    public int hashCode() {
        //当调用重写的hashCode时，只要保证传入的参数一样，则返回的哈希值肯定是一样的
        return Objects.hash(name, age, sex);
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex=" + sex +
                '}';
    }
}
```

```java
public class SetDemo3 {
    public static void main(String[] args) {
        // Set集合去重复原因：先判断哈希值算出来的存储位置是否相同 再判断equals
        Set<Student> sets = new HashSet<>();

        Student s1 = new Student("无恙", 20, '男');
        Student s2 = new Student("无恙", 20, '男');
        Student s3 = new Student("周雄", 21, '男');
        System.out.println(s1.hashCode());      //800712763
        System.out.println(s2.hashCode());      //800712763
        System.out.println(s3.hashCode());      //680875133

        sets.add(s1);
        sets.add(s2);
        sets.add(s3);
        System.out.println(sets);
        //[Student{name='无恙', age=20, sex=男}, Student{name='周雄', age=21, sex=男}]
    }
}
```



### 8.4 实现类：LinkedHashSet

**LinkedHashSet集合概述和特点**

- **有序**、不重复、无索引。
- 这里的有序指的是保证存储和取出的元素顺序一致
- **原理**：底层数据结构是依然**哈希表**，只是每个元素又额外的多了一个**双链表的机制记录存储的顺序**。



### 8.5 实现类：TreeSet

**TreeSet集合概述和特点**

- **不重复、无索引、可排序**。
- 可排序：按照元素的大小默认**升序**（有小到大）排序。
- TreeSet集合底层是基于**红黑树**的数据结构实现排序的，增删改查性能都较好。
- **注意：TreeSet集合是一定要排序的，可以将元素按照指定的规则进行排序。**

**TreeSet集合默认的规则**

- 对于数值类型：Integer , Double，官方默认按照大小进行升序排序。
- 对于字符串类型：默认按照首字符的编号升序排序。
- 对于自定义类型如Student对象，TreeSet无法直接排序。

**结论：想要使用TreeSet存储自定义类型，需要制定排序规则**

**自定义排序规则：**

- 方式一：让自定义的类（如学生类）实现Comparable接口重写里面的compareTo方法来定制比较规则

  ```java
  //实现Comparable接口
  public class Apple implements Comparable<Apple>{
      ·········
      ·········
      ·········
      
          //重写compareTo方法
      @Override
      public int compareTo(Apple o) {
          // 按照重量进行升序比较的
          return this.weight - o.weight ; // 去重重量重复的元素
          // return this.weight - o.weight >= 0 ? 1 : -1; // 保留重量重复的元素
      }
  }
  ```

  

- 方式二：TreeSet集合有参数构造器，可以设置Comparator接口对应的比较器对象，来定制比较规则。

  ```java
  // 方式二：集合自带比较器对象进行规则定制
          //
  //        Set<Apple> apples = new TreeSet<>(new Comparator<Apple>() {
  //            @Override
  //            public int compare(Apple o1, Apple o2) {
  //                // return o1.getWeight() - o2.getWeight(); // 升序
  //                // return o2.getWeight() - o1.getWeight(); // 降序
  //                // 注意：浮点型建议直接使用Double.compare进行比较
  //                // return Double.compare(o1.getPrice() , o2.getPrice()); // 升序
  //                return Double.compare(o2.getPrice() , o1.getPrice()); // 降序
  //            }
  //        });
          //简化:
          Set<Apple> apples = new TreeSet<>(( o1,  o2) ->  Double.compare(o2.getPrice() , o1.getPrice())  );
  ```

  

**两种方式中，关于返回值的规则：**

- 如果认为第一个元素大于第二个元素返回正整数即可。
- 如果认为第一个元素小于第二个元素返回负整数即可。
- 如果认为第一个元素等于第二个元素返回0即可，此时Treeset集合只会保留一个元素，认为两者重复。

**注意：如果TreeSet集合存储的对象有实现比较规则，集合也自带比较器，默认使用集合自带的比较器排序（就近原则）。**

**完整示例：**

Apple类：

```java
public class Apple implements Comparable<Apple>{
    private String name;
    private String color;
    private double price;
    private int weight;

    public Apple() {
    }

    public Apple(String name, String color, double price, int weight) {
        this.name = name;
        this.color = color;
        this.price = price;
        this.weight = weight;
    }

     //·····get/set()方法

    @Override
    public String toString() {
        return "Apple{" +
                "name='" + name + '\'' +
                ", color='" + color + '\'' +
                ", price=" + price +
                ", weight=" + weight +
                '}';
    }

    /**
      方式一：类自定义比较规则
      o1.compareTo(o2)
     * @param o
     * @return
     */
    @Override
    public int compareTo(Apple o) {
        // 按照重量进行升序比较的
        return this.weight - o.weight ; // 去重重量重复的元素
        // return this.weight - o.weight >= 0 ? 1 : -1; // 保留重量重复的元素
    }
}
```

```java
public class SetDemo5 {
    public static void main(String[] args) {
        Set<Integer> sets = new TreeSet<>(); // 不重复 无索引 可排序
        sets.add(23);
        sets.add(24);
        sets.add(12);
        sets.add(8);
        System.out.println(sets);

        Set<String> sets1 = new TreeSet<>(); // 不重复 无索引 可排序
        sets1.add("Java");
        sets1.add("Java");
        sets1.add("angela");
        sets1.add("黑马");
        sets1.add("Java");
        sets1.add("About");
        sets1.add("Python");
        sets1.add("UI");
        sets1.add("UI");
        System.out.println(sets1);

        System.out.println("------------------------------");
        // 方式二：集合自带比较器对象进行规则定制
        //
//        Set<Apple> apples = new TreeSet<>(new Comparator<Apple>() {
//            @Override
//            public int compare(Apple o1, Apple o2) {
//                // return o1.getWeight() - o2.getWeight(); // 升序
//                // return o2.getWeight() - o1.getWeight(); // 降序
//                // 注意：浮点型建议直接使用Double.compare进行比较
//                // return Double.compare(o1.getPrice() , o2.getPrice()); // 升序
//                return Double.compare(o2.getPrice() , o1.getPrice()); // 降序
//            }
//        });
        //简化:
        Set<Apple> apples = new TreeSet<>(( o1,  o2) ->  Double.compare(o2.getPrice() , o1.getPrice())  );

        apples.add(new Apple("红富士", "红色", 9.9, 500));
        apples.add(new Apple("青苹果", "绿色", 15.9, 300));
        apples.add(new Apple("绿苹果", "青色", 29.9, 400));
        apples.add(new Apple("黄苹果", "黄色", 9.8, 500));
        System.out.println(apples);
    }
}
```



## 9. Collection体系的特点、使用场景总结

- 如果希望元素可以重复，又有索引，索引查询要快：
  - 用ArrayList集合，基于数组的。（用的最多）

- 如果希望元素可以重复，又有索引，增删首尾操作快：
  - 用LinkedList集合，基于链表的。

- 如果希望增删改查都快，但是元素不重复、无序、无索引：
  - 用HashSet集合，基于哈希表的。

- 如果希望增删改查都快，但是元素不重复、有序、无索引：
  - 用LinkedHashSet集合，基于哈希表和双链表。

- 如果要对对象进行排序：
  - 用TreeSet集合，基于红黑树。后续也可以用List集合实现排序。



## 10. 补充知识：可变参数

- 可变参数用在**形参**中可以**接收多个数据**（类型是一样的）。
- 可变参数的格式：``数据类型...参数名称``

**可变参数的作用：**

- 传输参数非常灵活，方便。可以不传输参数，可以传输1个或者多个，也可以传输一个数组
- 可变参数在**方法内部本质上就是一个数组**。

**可变参数的注意事项：**

1. 一个形参列表中可变参数只能有一个
2. 可变参数必须放在形参列表的最后面

**示例：**

```java
public class MethodDemo {
    public static void main(String[] args) {

        sum(); // 1、不传参数
        sum(10); // 2、可以传输一个参数
        sum(10, 20, 30); // 3、可以传输多个参数
        sum(new int[]{10, 20, 30, 40, 50}); // 4、可以传输一个数组
    }

    /**
       注意：一个形参列表中只能有一个可变参数,可变参数必须放在形参列表的最后面
     * @param nums
     */
    public static void sum(  int...nums){
        // 注意：可变参数在方法内部其实就是一个数组。 nums
        System.out.println("元素个数：" + nums.length);
        System.out.println("元素内容：" + Arrays.toString(nums));
    }
}
```

输出：

```
元素个数：0
元素内容：[]
元素个数：1
元素内容：[10]
元素个数：3
元素内容：[10, 20, 30]
元素个数：5
元素内容：[10, 20, 30, 40, 50]
```



## 11. 补充知识：集合工具类Collections

**Collections集合工具类**

- java.utils.Collections:是集合工具类
- 作用：Collections并不属于集合，是用来操作集合的工具类

**Collections常用的API**

|                            方法名                            |          说明          |
| :----------------------------------------------------------: | :--------------------: |
| public static <T> boolean addAll(Collection<? super T> c, T... elements) | 给集合对象批量添加元素 |
|           public static void shuffle(List<?> list)           | 打乱List集合元素的顺序 |

```java
public class CollectionsDemo01 {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        //names.add("楚留香");
        //names.add("胡铁花");
        //names.add("张无忌");
        //names.add("陆小凤");
        //使用Collections.addAll批量添加：
        Collections.addAll(names, "楚留香","胡铁花", "张无忌","陆小凤");
        System.out.println(names);          //[楚留香, 胡铁花, 张无忌, 陆小凤]

        // 2、public static void shuffle(List<?> list) :打乱集合顺序。
        Collections.shuffle(names);
        System.out.println(names);      //[胡铁花, 陆小凤, 楚留香, 张无忌]

        // 3、 public static <T> void sort(List<T> list):将集合中元素按照默认规则排序。 （排值特性的元素）
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 12, 23, 2, 4);
        System.out.println(list);       //[12, 23, 2, 4]
        Collections.sort(list);
        System.out.println(list);       //[2, 4, 12, 23]
    }
}
```



**Collections排序相关API**

- 使用范围：只能对于List集合的排序

排序方式1：

|                  方法名                   |             说明             |
| :---------------------------------------: | :--------------------------: |
| public static <T> void sort(List<T> list) | 将集合中元素按照默认规则排序 |

注意：本方式不可以直接对自定义类型的List集合排序，除非自定义类型实现了比较规则Comparable接口（List集合存储相同大小的元素 会保留！TreeSet不会保留相同的）。

排序方式2：

|                            方法名                            |             说明             |
| :----------------------------------------------------------: | :--------------------------: |
| public static <T> void sort(List<T> list，Comparator<? super T> c) | 将集合中元素按照指定规则排序 |

Apple类

```java
public class Apple implements Comparable<Apple>{
    private String name;
    private String color;
    private double price;
    private int weight;

    public Apple() {
    }

    public Apple(String name, String color, double price, int weight) {
        this.name = name;
        this.color = color;
        this.price = price;
        this.weight = weight;
    }

    //·······get/set()方法

    @Override
    public String toString() {
        return "Apple{" +
                "name='" + name + '\'' +
                ", color='" + color + '\'' +
                ", price=" + price +
                ", weight=" + weight +
                '}';
    }

    /**
      方式一：类自定义比较规则
      o1.compareTo(o2)
     * @param o
     * @return
     */
    @Override
    public int compareTo(Apple o) {
        // 按照重量进行比较的
        return this.weight - o.weight ; // List集存储相同大小的元素 会保留！
    }
}
```

```java
public class CollectionsDemo02 {
    public static void main(String[] args) {
        List<Apple> apples = new ArrayList<>(); // 可以重复！
        apples.add(new Apple("红富士", "红色", 9.9, 500));
        apples.add(new Apple("青苹果", "绿色", 15.9, 300));
        apples.add(new Apple("绿苹果", "青色", 29.9, 400));
        apples.add(new Apple("黄苹果", "黄色", 9.8, 500));

//        Collections.sort(apples); // 方法一：可以的，Apple类已经重写了比较规则
//        System.out.println(apples);

        // 方式二：sort方法自带比较器对象
//        Collections.sort(apples, new Comparator<Apple>() {
//            @Override
//            public int compare(Apple o1, Apple o2) {
//                return Double.compare(o1.getPrice() , o2.getPrice()); // 按照价格排序！！
//            }
//        });

        //简化：
        Collections.sort(apples, ( o1,  o2) ->  Double.compare(o1.getPrice() , o2.getPrice()) );
        System.out.println(apples);
    }
}
```



## 12. Collection体系的综合案例

**需求：**

- 在启动游戏房间的时候，应该提前准备好54张牌，完成洗牌、发牌、牌排序、逻辑

**分析：**

1. 当系统启动的同时需要准备好数据的时候，就可以用静态代码块了
2. 洗牌就是打乱牌的顺序
3. 定义三个玩家、依次发出51张牌
4. 给玩家的牌进行排序(拓展)
5. 输出每个玩家的牌数据

Card类：

```java
public class Card {
    private String size;
    private String color;
    private int index;  //牌的真正大小，便于排序

    public Card() {
    }

    public Card(String size, String color, int index) {
        this.size = size;
        this.color = color;
        this.index = index;
    }

    public String getSize() {
        return size;
    }

    public void setSize(String size) {
        this.size = size;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getIndex() {
        return index;
    }

    public void setIndex(int index) {
        this.index = index;
    }

    @Override
    public String toString() {
        return size+color;
    }
}
```

```java
public class CardTest {
    //1、定义一个静态的集合存储牌
    public static List<Card> allCards=new ArrayList<>();

    //2、做牌：定义静态代码块初始化数据
    static{
        //3、定义点数：个数确定，类型确定，使用数组
        String[] sizes={"3","4","5","6","7","8","9","10","J","Q","K","A","2"};

        //4、定义花色：个数确定，类型确定，使用数组
        String[] colors={"♠", "♥", "♣", "♦"};

        //5、组合点数和花数
        int index = 0;      //记录牌的大小
        for (String size : sizes) {
            index++;
            for (String color : colors) {
                //6、封装成一个牌对象
                Card c=new Card(size, color, index);
                allCards.add(c);
            }
        }
        //添加大小王
        Collections.addAll(allCards, new Card("", "小👲", ++index),new Card("", "大🃏", ++index ));
        System.out.println("新牌："+allCards);
    }

    public static void main(String[] args) {
        //7、洗牌
        Collections.shuffle(allCards);
        System.out.println("洗牌后："+allCards);

        //8、发牌，定义三个玩家，每个玩家也是一个集合容器
        List<Card> zhangSan=new ArrayList<>();
        List<Card> liSi=new ArrayList<>();
        List<Card> wangWu=new ArrayList<>();

        //9、开始发牌（发51张，留三张地主牌）
        for (int i = 0; i < allCards.size()-3; i++) {
            //获取当前牌对象
            Card c=allCards.get(i);

            //对3取余(0-2)，轮流发给三个玩家
            if(i%3==0){
                //发给张三
                zhangSan.add(c);
            }else if(i%3==1){
                //发给李四
                liSi.add(c);
            }else if(i%3==2){
                //发给王五
                wangWu.add(c);
            }
        }
        //10、拿到最后三张牌，截取成一个子集合，方便给地主
        List<Card> lastThreeCards=allCards.subList(allCards.size()-3,allCards.size());

        //11、给玩家的牌排序(从大到小)
        sortCards(zhangSan);
        sortCards(liSi);
        sortCards(wangWu);

        //12、输出玩家的牌
        System.out.println("张三："+zhangSan);
        System.out.println("李四："+liSi);
        System.out.println("王五："+wangWu);
        System.out.println("地主牌："+lastThreeCards);
    }

    //牌的排序方法
    private static void sortCards(List<Card> cards) {
        Collections.sort(cards, (o1, o2) -> o2.getIndex()-o1.getIndex());
    }
}
```

测试案例：

```
新牌：[3♠, 3♥, 3♣, 3♦, 4♠, 4♥, 4♣, 4♦, 5♠, 5♥, 5♣, 5♦, 6♠, 6♥, 6♣, 6♦, 7♠, 7♥, 7♣, 7♦, 8♠, 8♥, 8♣, 8♦, 9♠, 9♥, 9♣, 9♦, 10♠, 10♥, 10♣, 10♦, J♠, J♥, J♣, J♦, Q♠, Q♥, Q♣, Q♦, K♠, K♥, K♣, K♦, A♠, A♥, A♣, A♦, 2♠, 2♥, 2♣, 2♦, 小👲, 大🃏]
洗牌后：[J♣, 7♥, 9♦, A♠, 2♦, A♦, 6♣, 3♦, 大🃏, 8♥, Q♦, 2♣, 10♠, K♦, Q♠, 4♣, 8♠, 8♣, Q♥, 3♠, 2♥, 4♦, 4♠, K♥, J♦, 10♦, 2♠, 9♣, J♠, A♣, J♥, 5♦, 7♣, 5♣, 10♣, 6♠, 小👲, 10♥, 9♥, 5♥, 8♦, 3♣, 7♠, 6♦, 6♥, A♥, K♣, 7♦, K♠, 3♥, 9♠, 5♠, Q♣, 4♥]
张三：[小👲, A♠, A♥, K♠, Q♥, J♣, J♦, J♥, 10♠, 9♣, 8♥, 7♠, 6♣, 5♣, 5♥, 4♣, 4♦]
李四：[2♦, K♦, K♣, Q♦, J♠, 10♦, 10♣, 10♥, 8♠, 8♦, 7♥, 6♦, 5♦, 4♠, 3♦, 3♠, 3♥]
王五：[大🃏, 2♣, 2♥, 2♠, A♦, A♣, K♥, Q♠, 9♦, 9♥, 9♠, 8♣, 7♣, 7♦, 6♠, 6♥, 3♣]
地主牌：[5♠, Q♣, 4♥]
```



## 13. Map集合体系

**Map集合概述和使用：**

- Map集合是一种双列集合，每个元素包含两个数据。
- Map集合的每个元素的格式：key=value(键值对元素)。
- Map集合也被称为 **“键值对集合”**。

**Map集合整体格式：**

- Collection集合的格式: [元素1,元素2,元素3..]
- Map集合的完整格式：`{key1=value1 , key2=value2 , key3=value3 , ...}`



### 13.1 Map集合体系特点

- Map(接口)
  - HashMap(实现类)
    - LinkedHashMap(实现类)
  - HashTable(实现类)
    - Properties(实现类)
  - ······(接口)
    - TreeMap(实现类)

**说明**

- 使用最多的Map集合是HashMap。
- 重点掌握HashMap , LinkedHashMap , TreeMap。其他的后续理解。

**Map集合体系特点**

- Map集合的特点都是由键决定的。
- Map集合的键是无序,不重复的，无索引的，值不做要求（可以重复）。
- Map集合后面重复的键对应的值会覆盖前面重复键的值。
- Map集合的键值对都可以为null。

**Map集合实现类特点**

- HashMap:元素按照键是无序，不重复，无索引，值不做要求。（与Map体系一致）
- LinkedHashMap:元素按照键是**有序**，不重复，无索引，值不做要求。
- TreeMap：元素按照建是**排序**，不重复，无索引的，值不做要求。



```java
public class MapDemo1 {
    public static void main(String[] args) {
        // 1、创建一个Map集合对象
        Map<String, Integer> maps1 = new HashMap<>();
        maps1.put("鸿星尔克", 3);
        maps1.put("Java", 1);
        maps1.put("枸杞", 100);
        maps1.put("Java", 100); // 覆盖前面的数据
        maps1.put(null, null);
        System.out.println(maps1);      //{null=null, Java=100, 枸杞=100, 鸿星尔克=3}

        Map<String, Integer> maps2 = new LinkedHashMap<>();
        maps2.put("鸿星尔克", 3);
        maps2.put("Java", 1);
        maps2.put("枸杞", 100);
        maps2.put("Java", 100); // 覆盖前面的数据，位置还是在前面的位置上
        maps2.put(null, null);
        System.out.println(maps2);      //{鸿星尔克=3, Java=100, 枸杞=100, null=null}
    }
}
```



### 13.2 Map集合常用API

- Map是双列集合的祖宗接口，它的功能是全部双列集合都可以继承使用的。



**MapAPI如下:**

|              方法名称               |                           说明                           |
| :---------------------------------: | :------------------------------------------------------: |
|        V put(K key,V value)         |                         添加元素                         |
|        V remove(Object key)         |                   根据键删除键值对元素                   |
|            void clear()             |                   移除所有的键值对元素                   |
|   boolean containsKey(Object key)   |                 判断集合是否包含指定的键                 |
| boolean containsValue(Object value) |                 判断集合是否包含指定的值                 |
|          boolean isEmpty()          |                     判断集合是否为空                     |
|             int size()              |           集合的长度，也就是集合中键值对的个数           |
|       public Set<K> keySet()        |   获取全部键的集合，返回Set集合类型，因为键不可以重复    |
|       Collection<V> values()        | 获取全部值的集合，返回Collection集合类型，因为值可以重复 |

| public V get(Object key) | 根据键获取对应值 |
| :----------------------- | :--------------- |



```java
public class MapDemo {
    public static void main(String[] args) {
        // 1.添加元素: 无序，不重复，无索引。
        Map<String , Integer> maps = new HashMap<>();
        maps.put("iphoneX",10);
        maps.put("娃娃",20);
        maps.put("iphoneX",100);//  Map集合后面重复的键对应的元素会覆盖前面重复的整个元素！
        maps.put("huawei",100);
        maps.put("生活用品",10);
        maps.put("手表",10);
        // {huawei=100, 手表=10, 生活用品=10, iphoneX=100, 娃娃=20}
        System.out.println(maps);

        // 2.清空集合
//        maps.clear();
//        System.out.println(maps);

        // 3.判断集合是否为空，为空返回true ,反之！
        System.out.println(maps.isEmpty());

        // 4.根据键获取对应值:public V get(Object key)
        Integer value = maps.get("huawei");
        System.out.println(value);      //100
        System.out.println(maps.get("生活用品")); // 10
        System.out.println(maps.get("生活用品2")); // null

        // 5.根据键删除整个元素。(删除键会返回键的值)
        System.out.println(maps.remove("iphoneX"));     //100
        System.out.println(maps);       //{huawei=100, 手表=10, 生活用品=10, 娃娃=20}

        // 6.判断是否包含某个键 ，包含返回true ,反之
        System.out.println(maps.containsKey("娃娃"));  // true
        System.out.println(maps.containsKey("娃娃2"));  // false
        System.out.println(maps.containsKey("iphoneX")); // false

        // 7.判断是否包含某个值。
        System.out.println(maps.containsValue(100));  //true
        System.out.println(maps.containsValue(10));  //true
        System.out.println(maps.containsValue(22)); //true

        // {huawei=100, 手表=10, 生活用品=10, 娃娃=20}
        // 8.获取全部键的集合：public Set<K> keySet()
        Set<String> keys = maps.keySet();   //返回Set集合类型，因为键不可以重复
        System.out.println(keys);       //[huawei, 手表, 生活用品, 娃娃]

        System.out.println("------------------------------");
        // 9.获取全部值的集合：Collection<V> values();
        Collection<Integer> values = maps.values();     //返回Collection集合类型，因为值可以重复
        System.out.println(values);     //[100, 10, 10, 20]

        // 10.集合的大小
        System.out.println(maps.size());     // 4

        // 11.合并其他Map集合。(拓展)
        Map<String , Integer> map1 = new HashMap<>();
        map1.put("java1", 1);
        map1.put("java2", 100);
        Map<String , Integer> map2 = new HashMap<>();
        map2.put("java2", 1);
        map2.put("java3", 100);
        map1.putAll(map2); // 把集合map2的元素拷贝一份到map1中去
        System.out.println(map1);       //{java3=100, java2=1, java1=1}
        System.out.println(map2);       //{java3=100, java2=1}
    }
}
```



### 13.3 Map集合的遍历方式

Map集合的遍历方式有3种:

- 方式一：键找值的方式遍历：先获取Map集合全部的键，再根据遍历键找值。
- 方式二：键值对的方式遍历，把“键值对“看成一个整体，难度较大。
- 方式三：JDK 1.8开始之后的新技术：Lambda表达式。

#### 13.3.1 方式一：键找值

- 先获取Map集合的全部键的Set集合。
- 遍历键的Set集合，然后通过键提取对应值。

**键找值涉及到的API:**

|     方法名称      |       说明       |
| :---------------: | :--------------: |
|  Set<K> keySet()  | 获取所有键的集合 |
| V get(Object key) |   根据键获取值   |

```java
public class MapDemo01 {
    public static void main(String[] args) {
        Map<String , Integer> maps = new HashMap<>();
        // 1.添加元素: 无序，不重复，无索引。
        maps.put("娃娃",30);
        maps.put("iphoneX",100);
        maps.put("huawei",1000);
        maps.put("生活用品",10);
        maps.put("手表",10);
        System.out.println(maps);
        // maps = {huawei=1000, 手表=10, 生活用品=10, iphoneX=100, 娃娃=30}

        // 1、键找值：第一步：先拿到集合的全部键。
        Set<String> keys = maps.keySet();
        // 2、第二步：遍历每个键，根据键提取值
        for (String key : keys) {
            int value = maps.get(key);
            System.out.println(key + "===>" + value);
        }
    }
}
```



#### 13.3.2 方式二：键值对

- 先把Map集合转换成Set集合，Set集合中每个元素都是键值对实体类型了。
- 接下来就可以用foreach遍历Set集合，然后提取键以及提取值。

**键值对涉及到的API:**

|            方法名称            |           说明           |
| :----------------------------: | :----------------------: |
| Set<Map.Entry<K,V>> entrySet() | 获取所有键值对对象的集合 |
|           K getKey()           |          获得键          |
|          V getValue()          |          获取值          |

```java
public class MapDemo02 {
    public static void main(String[] args) {
        Map<String , Integer> maps = new HashMap<>();
        // 1.添加元素: 无序，不重复，无索引。
        maps.put("娃娃",30);
        maps.put("iphoneX",100);
        maps.put("huawei",1000);
        maps.put("生活用品",10);
        maps.put("手表",10);
        System.out.println(maps);
        // maps = {huawei=1000, 手表=10, 生活用品=10, iphoneX=100, 娃娃=30}
        /**
            maps = {huawei=1000, 手表=10, 生活用品=10, iphoneX=100, 娃娃=30}
                👇
            使用foreach遍历map集合.发现Map集合的键值对元素直接是没有类型的。所以不可以直接foreach遍历集合。
                👇
            可以通过调用Map的方法 entrySet把Map集合转换成Set集合形式  maps.entrySet();
                👇
            Set<Map.Entry<String,Integer>> entries =  maps.entrySet();  //将键值作为一个整体对象
             [(huawei=1000), (手表=10), (生活用品=10), (iphoneX=100), (娃娃=30)]
                              entry
                👇
            此时可以使用foreach遍历
       */
       // 1、把Map集合转换成Set集合
        Set<Map.Entry<String, Integer>> entries = maps.entrySet();
        // 2、开始遍历
        for(Map.Entry<String, Integer> entry : entries){
            String key = entry.getKey();
            int value = entry.getValue();
            System.out.println(key + "====>" + value);
        }
    }
}
```



#### 13.3.3 方式三：lambda表达式

- 得益于JDK 8开始的新技术Lambda表达式，提供了一种更简单、更直接的遍历集合的方式。



**Map结合Lambda遍历的API**

|                           方法名称                           |         说明          |
| :----------------------------------------------------------: | :-------------------: |
| default void forEach(BiConsumer<? super K, ? super V> action) | 结合lambda遍历Map集合 |

**BiConsumer方法内部用的是方式二的遍历方式**

```java
public class MapDemo03 {
    public static void main(String[] args) {
        Map<String , Integer> maps = new HashMap<>();
        // 1.添加元素: 无序，不重复，无索引。
        maps.put("娃娃",30);
        maps.put("iphoneX",100);//  Map集合后面重复的键对应的元素会覆盖前面重复的整个元素！
        maps.put("huawei",1000);
        maps.put("生活用品",10);
        maps.put("手表",10);
        System.out.println(maps);

        //  maps = {huawei=1000, 手表=10, 生活用品=10, iphoneX=100, 娃娃=30}

//        maps.forEach(new BiConsumer<String, Integer>() {
//            @Override
//            public void accept(String key, Integer value) {
//                System.out.println(key + "--->" + value);
//            }
//        });

        //简化:
        maps.forEach((k, v) -> System.out.println(k + "--->" + v));
    }
}
```



### 13.4 Map集合案例 - 统计投票人数

需求：

- 某个班级80名学生，现在需要组成秋游活动，班长提供了四个景点依次是（A、B、C、D）,每个学生只能选择一个景点，请统计出最终哪个景点想去的人数最多。

分析：

1. 将80个学生选择的数据拿到程序中去。
2. 定义Map集合用于存储最终统计的结果。
3. 遍历80个学生选择的数据，看Map集合中是否存在，不存在存入“数据=1“，存在则其对应值+1,

```java
public class MapTest1 {
    public static void main(String[] args) {
         // 1、模拟把80个学生选择的数据拿进来（从ABCD里面随机拿）
        String[] selects = {"A" , "B", "C", "D"};
        StringBuilder sb = new StringBuilder();
        Random r = new Random();
        for (int i = 0; i < 80; i++) {
            sb.append(selects[r.nextInt(selects.length)]);
        }
        System.out.println(sb);

        // 2、定义一个Map集合记录最终统计的结果： A=30 B=20 C=20 D=10  键是景点 值是选择的数量
        Map<Character, Integer> infos = new HashMap<>();    //景点类型为char

        // 3、遍历80个学生选择的数据
        for (int i = 0; i < sb.length(); i++) {
            // 4、提取当前选择景点字符(键)
            char ch = sb.charAt(i);
            // 5、判断Map集合中是否存在这个键
            if(infos.containsKey(ch)){
                 // 让其值 + 1
                infos.put(ch , infos.get(ch) + 1);      //get()方法是根据键返回值
            }else {
                // 说明此景点是第一次被选
                infos.put(ch , 1);
            }
        }

        // 4、输出集合
        System.out.println(infos);
    }
}
```



### 13.5 Map集合的实现类HashMap

**HashMap的特点**

- HashMap是Map里面的一个实现类。特点都是由键决定的：无序、不重复、无索引
- 没有额外需要学习的特有方法，直接使用Map里面的方法就可以了。
- HashMap跟HashSet底层原理是一模一样的，都是哈希表结构，只是HashMap的每个元素包含两个值而已。

**实际上：Set系列集合的底层就是Map实现的，只是Set集合中的元素只要键数据，不要值数据而已。**

HashSet底层：

```
public HashSet() {
	map = new HashMap<>();
}
```



**HashMap的底层原理**

- HashMap底层是哈希表结构的。
- 依赖hashCode方法和equals方法保证**键**的唯一。
- 如果**键**要存储的是自定义对象，需要重写hashCode和equals方法。
- 基于哈希表。增删改查的性能都较好。





### 13.6 Map集合的实现类LinkedHashMap

**LinkedHashMap集合概述和特点**

- 由键决定：有序、不重复、无索引。
- 这里的有序指的是保证存储和取出的元素顺序一致
- 原理：**底层数据结构是依然哈希表，只是每个键值对元素又额外的多了一个双链表的机制记录存储顺序**。



### 13.7 Map集合的实现类TreeMap

**TreeMap集合概述和特点**

- 由键决定特性：不重复、无索引、可排序
- 可排序：按照键数据的大小默认升序（有小到大）排序。**只能对键排序。**
- **注意：TreeMap集合是一定要排序的，可以默认排序，也可以将键按照指定的规则进行排序**
- TreeMap跟TreeSet一样底层原理是一样的，基于红黑树实现排序，增删改查性能较好。



**TreeMap集合自定义排序规则有2种(与TreeSet一样)**

- 类实现Comparable接口，重写比较规则。
- 集合自定义Comparator比较器对象，重写比较规则。



## 14. 补充知识：集合的嵌套

Map集合案例-统计投票人数

需求：

- 某个班级多名学生，现在需要组成秋游活动，班长提供了四个景点依次是（A、B、C、D）,每个学生可以**选择多个景点**，请统计出最终哪个景点想去的人数最多。

分析：

- 将80个学生选择的数据拿到程序中去，需要记住每个学生选择的情况。
- 定义Map集合用于存储最终统计的结果。

```java
public class MapTest4 {
    public static void main(String[] args) {
        // 1、要求程序记录每个学生选择的情况。
        // 使用一个Map集合存储。
        Map<String, List<String>> data = new HashMap<>();

        // 2、把学生选择的数据存入进去（模拟3个学生）。
        List<String> selects = new ArrayList<>();
        Collections.addAll(selects, "A", "C");
        data.put("罗勇", selects);

        List<String> selects1 = new ArrayList<>();
        Collections.addAll(selects1, "B", "C" , "D");
        data.put("胡涛", selects1);

        List<String> selects2 = new ArrayList<>();
        Collections.addAll(selects2 , "A",  "B", "C" , "D");
        data.put("刘军", selects2);

        System.out.println(data);       //{刘军=[A, B, C, D], 胡涛=[B, C, D], 罗勇=[A, C]}

        // 3、统计每个景点选择的人数。
        Map<String, Integer> infos = new HashMap<>(); 

        // 4、提取所有人选择的景点的信息。
        Collection<List<String>> values = data.values();
        System.out.println(values);     //[[A, B, C, D], [B, C, D], [A, C]]

        // values = [[A, B, C, D], [B, C, D], [A, C]]
        //             value
        for (List<String> value : values) {     //外层循环，遍历values整个集合
            for (String s : value) {            //内层循环，遍历value单个集合里的值
                // 有没有包含这个景点
                if(infos.containsKey(s)){
                    infos.put(s, infos.get(s) + 1);
                }else {
                    infos.put(s , 1);
                }
            }
        }
        System.out.println(infos);      //{A=2, B=2, C=3, D=2}
    }
}
```



## 15. 不可变集合

- 不可变集合，就是不可被修改的集合。
- 集合的数据项在创建的时候提供，并且在整个生命周期中都不可改变。否则报错。

**创建不可变集合的原因**

- 如果某个数据不能被修改，把它防御性地拷贝到不可变集合中是个很好的实践。
- 或者当集合对象被不可信的库调用时，不可变形式是安全的。

**创建不可变集合**

- 在List、Set、Map接口中，都存在of方法，可以创建一个不可变的集合。
- 这个集合不能添加，不能删除，不能修改。

|                  方法名                  |                说明                |
| :--------------------------------------: | :--------------------------------: |
|    static <E> List<E> of(E…elements)     | 创建一个具有指定元素的List集合对象 |
|     static <E> Set<E> of(E…elements)     | 创建一个具有指定元素的Set集合对象  |
| static <K , V>  Map<K，V> of(E…elements) | 创建一个具有指定元素的Map集合对象  |

```java
public class CollectionDemo {
    public static void main(String[] args) {
        // 1、不可变的List集合
        List<Double> lists = List.of(569.5, 700.5, 523.0,  570.5);
        // lists.add(689.0);        //报错
        // lists.set(2, 698.5);     //报错
        // System.out.println(lists);
        double score = lists.get(1);
        System.out.println(score);

        // 2、不可变的Set集合
        //Set<String> names = Set.of("迪丽热巴", "迪丽热九", "马尔扎哈", "卡尔眨巴", "迪丽热巴" );      //报错，不能有重复的元素
        Set<String> names = Set.of("迪丽热巴", "迪丽热九", "马尔扎哈", "卡尔眨巴" );
        // names.add("三少爷");        //报错
        System.out.println(names);

        // 3、不可变的Map集合
        Map<String, Integer> maps = Map.of("huawei",2, "Java开发", 1 , "手表", 1);
        // maps.put("衣服", 3);       //报错
        System.out.println(maps);
    }
}
```







# Stream流、异常、日志框架



## 1. Stream流

Stream流的概述：

- 在Java 8中，得益于Lambda所带来的函数式编程， 引入了一个全新的Stream流概念。
- 目的：用于简化集合和数组操作的API。



案例：体验Stream流的作用

需求：

- 创建一个集合，存储多个字符串元素
- 把集合中所有以"张"开头的元素存储到一个新的集合
- 把"张"开头的集合中的长度为3的元素存储到一个新的集合
- 遍历上一步得到的集合中的元素输出

```java
public class StreamTest {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        Collections.addAll(names, "张三丰","张无忌","周芷若","赵敏","张强");
        System.out.println(names);      //[张三丰, 张无忌, 周芷若, 赵敏, 张强]

        //一、使用传统方法：
        // 1、从集合中找出姓"张"的放到新集合
        List<String> zhangList = new ArrayList<>();
        for (String name : names) {
            if(name.startsWith("张")){
                zhangList.add(name);
            }
        }
        System.out.println(zhangList);      //[张三丰, 张无忌, 张强]

        // 2、找名称"长度是3"的姓名
        List<String> zhangThreeList = new ArrayList<>();
        for (String name : zhangList) {
            if(name.length() == 3){
                zhangThreeList.add(name);
            }
        }
        System.out.println(zhangThreeList);     //[张三丰, 张无忌]

        // 二、使用Stream实现
        names.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).forEach(s -> System.out.println(s));
    }
}
```

**Stream流支持链式编程**

**Stream流式思想的核心：**

1. 先得到集合或者数组的Stream流（就是一根传送带）
2. 把元素放上去
3. 然后就用这个Stream流简化的API来方便的操作元素。



### 1.1 Stream流的获取

**Stream流的三类方法**

- 获取Stream流
  - 创建一条流水线，并把数据放到流水线上准备进行操作
- 中间方法
  - 流水线上的操作。一次操作完毕之后，还可以继续进行其他操作
- 终结方法
  - 一个Stream流只能有一个终结方法，是流水线上的最后一个操作



**集合**获取Stream流的方式

- 可以使用Collection接口中的默认方法stream()生成流

  |           方法名           |            说明            |
  | :------------------------: | :------------------------: |
  | default Stream<E> stream() | 获取当前集合对象的Stream流 |

  

**数组**获取Stream流的方式

|                    方法名                     |              说明               |
| :-------------------------------------------: | :-----------------------------: |
| public static <T> Stream<T> stream(T[] array) |     获取当前数组的Stream流      |
|  public static<T> Stream<T> of(T... values)   | 获取当前数组/可变数据的Stream流 |

```java
public class StreamDemo02 {
    public static void main(String[] args) {
        /** --------------------Collection集合获取流-------------------------------   */
        Collection<String> list = new ArrayList<>();
        Stream<String> s =  list.stream();

        /** --------------------Map集合获取流-------------------------------   */
        Map<String, Integer> maps = new HashMap<>();
        // 键流
        Stream<String> keyStream = maps.keySet().stream();
        // 值流
        Stream<Integer> valueStream = maps.values().stream();
        // 键值对流（拿整体）
        Stream<Map.Entry<String,Integer>> keyAndValueStream =  maps.entrySet().stream();

        /** ---------------------数组获取流------------------------------   */
        String[] names = {"赵敏","小昭","灭绝","周芷若"};
        Stream<String> nameStream = Arrays.stream(names);
        Stream<String> nameStream2 = Stream.of(names);
    }
}
```



### 1.2 Stream流常用API（中间操作方法）

- 中间方法也称为非终结方法，调用完成后返回新的Stream流可以继续使用，支持链式编程。
- 在Stream流中**无法直接修改集合、数组中的数据。**

|                       名称                       |                             说明                             |
| :----------------------------------------------: | :----------------------------------------------------------: |
| Stream<T> filter(Predicate<? super T> predicate) |                用于对流中的数据进行**过滤。**                |
|          Stream<T> limit(long maxSize)           |                        获取前几个元素                        |
|              Stream<T> skip(long n)              |                        跳过前几个元素                        |
|               Stream<T> distinct()               |        去除流中重复的元素。依赖(hashCode和equals方法)        |
| static <T> Stream<T> concat(Stream a, Stream b)  | **合并**a和b两个流为一个流(合并两个不同类型的流要用Object流接收) |



**Stream流的常见终结方法：**

|             名称              |             说明             |
| :---------------------------: | :--------------------------: |
| void forEach(Consumer action) | 对此流的每个元素执行遍历操作 |
|         long count()          |      返回此流中的元素数      |

**注意：终结操作方法，调用完成后流就无法继续使用了，原因是不会返回Stream了。**

```java
public class StreamDemo03 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("周芷若");
        list.add("赵敏");
        list.add("张强");
        list.add("张三丰");
        list.add("张三丰");

        // Stream<T> filter(Predicate<? super T> predicate)
        list.stream().filter(s -> s.startsWith("张")).forEach(s -> System.out.println(s));

        long size = list.stream().filter(s -> s.length() == 3).count();
        System.out.println(size);

       // list.stream().filter(s -> s.startsWith("张")).limit(2).forEach(s -> System.out.println(s));
        //简化print，方法引用(::) (->前的对象要和print()中的对象相同)：
        list.stream().filter(s -> s.startsWith("张")).limit(2).forEach(System.out::println);

        list.stream().filter(s -> s.startsWith("张")).skip(2).forEach(System.out::println);

        // map加工方法: ->前：原材料  ->后:加工后的结果。
        // 给集合元素的前面都加上一个：黑马的：
        list.stream().map(s -> "黑马的：" + s).forEach(a -> System.out.println(a));

        // 需求：把所有的名称 都加工成一个学生对象。
        list.stream().map(s -> new Student(s)).forEach(s -> System.out.println(s));
        //用引用符号:: 简化(对象要相同)：
//        list.stream().map(Student::new).forEach(System.out::println); // 构造器引用  方法引用

        // 合并流。
        Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
        Stream<String> s2 = Stream.of("java1", "java2");
        // public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
        Stream<String> s3 = Stream.concat(s1 , s2);

        //去除流中重复的元素
        s3.distinct().forEach(s -> System.out.println(s));
    }
}
```

Student类：

```java
public class Student {
    private String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                '}';
    }
}
```



### 1.3 Stream流案例

需求：某个公司的开发部门，分为开发一部和二部，现在需要进行年中数据结算。

分析：

1. 员工信息至少包含了(名称、性别、工资、奖金、处罚记录)
2. 开发一部有4个员工、开发二部有5名员工
3. 分别筛选出2个部门的最高工资的员工信息，封装成优秀员工对象Topperformer
4. 分别统计出2个部门的平均月收入，要求去掉最高和最低工资
5. 统计2个开发部门整体的平均工资，去掉最低和最高工资的平均值



Employee类：

```java
public class Employee {
    private String name;
    private char sex;
    private double salary;
    private double bonus;
    private String punish; // 处罚信息

    public Employee(){
    }

    public Employee(String name, char sex, double salary, double bonus, String punish) {
        this.name = name;
        this.sex = sex;
        this.salary = salary;
        this.bonus = bonus;
        this.punish = punish;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public double getBonus() {
        return bonus;
    }

    public void setBonus(double bonus) {
        this.bonus = bonus;
    }

    public String getPunish() {
        return punish;
    }

    public void setPunish(String punish) {
        this.punish = punish;
    }


    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", sex=" + sex +
                ", salary=" + salary +
                ", bonus=" + bonus +
                ", punish='" + punish + '\'' +
                '}'+"\n";
    }
}
```

TopperFormer类：

```java
public class TopperFormer {
    private String name;
    private double money;

    public TopperFormer() {
    }

    public TopperFormer(String name, double money) {
        this.name = name;
        this.money = money;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "TopperFormer{" +
                "name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}
```



```java
public class StreamDemo {
    public static double allMoney1=0;        //静态变量记录开发一部去掉最高最低工资的总和
    public static double allMoney2=0;        //静态变量记录开发二部去掉最高最低工资的总和
    public static double allMoneyTotal=0;    //静态变量记录两个部门去掉最高最低工资的总和

    public static void main(String[] args) {
        List<Employee> one = new ArrayList<>();
        one.add(new Employee("猪八戒",'男',30000 , 25000, null));
        one.add(new Employee("孙悟空",'男',25000 , 1000, "顶撞上司"));
        one.add(new Employee("沙僧",'男',20000 , 20000, null));
        one.add(new Employee("小白龙",'男',20000 , 25000, null));

        List<Employee> two = new ArrayList<>();
        two.add(new Employee("武松",'男',15000 , 9000, null));
        two.add(new Employee("李逵",'男',20000 , 10000, null));
        two.add(new Employee("西门庆",'男',50000 , 100000, "被打"));
        two.add(new Employee("潘金莲",'女',3500 , 1000, "被打"));
        two.add(new Employee("武大郎",'女',20000 , 0, "下毒"));

        //1、开发一部的最高工资员工
        //用一个TopperFormer类型的map容器接收，max是stream流里获取最大值的方法（需要自己写规则），get是获取对象的方法
        TopperFormer t1=one.stream().max((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
                .map(o-> new TopperFormer(o.getName(),o.getSalary()+o.getBonus())).get();
        System.out.println("开发一部的最高工资员工为："+ t1);

        //2、统计平均工资，要去掉最高最低工资
        //sorter是stream流里排序方法，需要自己制定规则，先用skip去掉最低工资，再用limit获取除了最高工资的元素，最后用forEach遍历，获取总工资
        one.stream().sorted((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
                .skip(1).limit(one.size()-2).forEach(o->{
                    //o表示当前获得的对象
                    allMoney1 +=(o.getSalary() + o.getBonus());
                });

        //浮点型运算用总工资除人数有可能会出现失真问题，或者除不尽的情况，用大数据对象处理
        BigDecimal bdAllMoney1=BigDecimal.valueOf(allMoney1);
        BigDecimal bdSize1=BigDecimal.valueOf(one.size()-2);
        //结果保留2位小数，除不尽采用四舍五入法
        BigDecimal average1=bdAllMoney1.divide(bdSize1, 2, RoundingMode.HALF_UP);
        System.out.println("开发一部的平均工资是："+ average1);


        //开发二部获取最高工资员工和统计平均工资的方法跟上面相同
        TopperFormer t2=two.stream().max((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
                .map(o->new TopperFormer(o.getName(),o.getSalary()+o.getBonus())).get();
        System.out.println("开发二部的最高工资员工为："+ t2);

        one.stream().sorted((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
                .skip(1).limit(two.size()-2).forEach(o->{
                    allMoney2 +=(o.getSalary() + o.getBonus());
                });

        BigDecimal bdAllMoney2=BigDecimal.valueOf(allMoney2);
        BigDecimal bdSize2=BigDecimal.valueOf(two.size()-2);
        BigDecimal average2=bdAllMoney2.divide(bdSize2, 2, RoundingMode.HALF_UP);
        System.out.println("开发二部的平均工资是："+ average2);

        //3、合并两个集合流，再统计
        Stream<Employee> s1=one.stream();
        Stream<Employee> s2=two.stream();
        Stream<Employee> s3=Stream.concat(s1, s2);
        s3.sorted((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
                .skip(1).limit(one.size()+two.size()-2).forEach(o->{
                    allMoneyTotal+=(o.getSalary()+o.getBonus());
                });
        BigDecimal bdAllMoneyTotal=BigDecimal.valueOf(allMoneyTotal);
        BigDecimal bdSizeTotal=BigDecimal.valueOf(one.size()+two.size()-2);
        BigDecimal averageTotal=bdAllMoneyTotal.divide(bdSizeTotal, 2, RoundingMode.HALF_UP);
        System.out.println("两个开发部整体的平均工资是："+ averageTotal);
    }
}
```



### 1.4 收集Stream流

**Stream流的收集操作：**

- 收集Stream流的含义：就是**把Stream流操作后的结果数据转回到集合或者数组中去**。
- Stream流：方便操作集合/数组的**手段**。
- 集合/数组：才是开发中的**目的**。

**Stream流的收集方法**

|              名称              |             说明             |
| :----------------------------: | :--------------------------: |
| R collect(Collector collector) | 开始收集Stream流，指定收集器 |

**Collectors工具类提供了具体的收集方式**

|                             名称                             |          说明          |
| :----------------------------------------------------------: | :--------------------: |
|             public static <T> Collector toList()             | 把元素收集到List集合中 |
|             public static <T> Collector toSet()              | 把元素收集到Set集合中  |
| public static Collector toMap(Function keyMapper , Function valueMapper) | 把元素收集到Map集合中  |

也可以收集到数组中去，具体看下面示例代码。

==注意注意注意：“流只能使用一次”==

**示例：**

```java
public class StreamDemo05 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("张无忌");
        list.add("周芷若");
        list.add("赵敏");
        list.add("张强");
        list.add("张三丰");
        list.add("张三丰");

        //收集到List集合
        Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
//      List<String> zhangList = s1.collect(Collectors.toList()); // 可变集合

        //JDK16开始，转为List集合可以直接调用toList()，但是不能修改集合
        List<String> zhangList=s1.toList();     //不可变集合
//      zhangList.add("java1");       //报错，用新的toList得到的集合不能被修改
        System.out.println(zhangList);

        //收集到Set集合
        //注意注意注意：流只能使用一次，所以要创建一个新流再收集
        Stream<String> s2 = list.stream().filter(s -> s.startsWith("张"));
        Set<String> zhangSet = s2.collect(Collectors.toSet());
        System.out.println(zhangSet);       //[张强, 张三丰, 张无忌] （Set集合会去重复）

        
        //收集到Array数组里
        Stream<String> s3 = list.stream().filter(s -> s.startsWith("张"));
        Object[] arrs = s3.toArray();
        //String[] arrs = s3.toArray(String[]::new);  //将流收集到字符串数组(或者其他类型的数组)
        System.out.println("Arrays数组内容：" + Arrays.toString(arrs));
    }
}
```





## 2. 异常

### 2.1 异常概述、体系

**什么是异常？**

- 异常是程序在“编译”或者“执行”的过程中可能出现的问题，**注意**：语法错误不算在异常体系中
- 比如:数组索引越界、空指针异常、 日期格式化异常，等…

**为什么要学习异常?**

- 异常一旦出现了，如果没有提前处理，程序就会退出JVM虚拟机而终止
- 研究异常并且避免异常，然后提前处理异常，体现的是程序的安全, 健壮性



**异常体系：**

- Throwable(根类，不是异常类)
  - Error(错误)：系统级别问题、JVM退出等，代码无法控制。
  - Exception(异常类)：java.lang包下，称为异常类，它表示程序本身可以处理的问题
    - RuntimeException(运行阶段)及其子类：运行时异常，编译阶段不会报错。 (空指针异常，数组索引越界异常) 
    - 除RuntimeException之外的所有异常(编译阶段)：编译时异常，编译期必须处理的，否则程序不能通过编译。 (日期格式化异常)



**编译时异常和运行时异常**

- 编译时异常(javac.exe)：是在**编译成class文件**时必须要处理的异常，也称之为受检异常

- 运行时异常(java.exe)：在编译成class文件不需要处理，在**运行字节码文件**时可能出现的异常。



### 2.2 运行时异常

- 直接继承自RuntimeException或者其子类，编译阶段不会报错，运行时可能出现的错误。

**运行时异常示例**

- 数组索引越界异常: ArrayIndexOutOfBoundsException
- 空指针异常 : NullPointerException，直接输出没有问题，但是调用空指针的变量的功能就会报错
- 数学操作异常：ArithmeticException
- 类型转换异常：ClassCastException
- 数字转换异常： NumberFormatException

**运行时异常：一般是程序员业务没有考虑好或者是编程逻辑不严谨引起的程序错误，是自己的水平有问题！**

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        System.out.println("程序开始。。。。。。");
        /** 1.数组索引越界异常: ArrayIndexOutOfBoundsException。*/
        int[] arr = {1, 2, 3};
        System.out.println(arr[2]);
        // System.out.println(arr[3]); // 运行出错，程序终止

        /** 2.空指针异常 : NullPointerException。直接输出没有问题。但是调用空指针的变量的功能就会报错！！ */
        String name = null;
        System.out.println(name); // null
        // System.out.println(name.length()); // 运行出错，程序终止

        /** 3.类型转换异常：ClassCastException。 */
        Object o = 23;
        // String s = (String) o;  // 运行出错，程序终止

        /** 5.数学操作异常：ArithmeticException。 */
        //int c = 10 / 0;

        /** 6.数字转换异常： NumberFormatException。 */
        //String number = "23";
        String number = "23aabbc";
        Integer it = Integer.valueOf(number); // 运行出错，程序终止（String只有数字可以转）
        System.out.println(it + 1);

        System.out.println("程序结束。。。。。");
    }
}
```



### 2.3 编译时异常

- 不是RuntimeException或者其子类的异常，编译阶就报错，必须处理，否则代码不通过。

```java
public class ExceptionDemo {
    public static void main(String[] args) throws ParseException {
        String date = "2015-01-12 10:23:21";
        // 创建一个简单日期格式化类：
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM-dd HH:mm:ss");
        // 解析字符串时间成为日期对象
        Date d = sdf.parse(date);		//报错，格式不一致
        System.out.println(d);
    }
}
```



### 2.4 异常的默认处理流程

1. 默认会在出现异常的代码那里自动的创建一个异常对象：ArithmeticException。
2. 异常会从方法中出现的点这里抛出给调用者，调用者最终抛出给JVM虚拟机。
3. 虚拟机接收到异常对象后，先在控制台直接输出异常栈信息数据。
4. 直接从当前执行的异常点干掉当前程序。
5. 后续代码没有机会执行了，因为程序已经死亡。



**默认的异常处理机制并不好，一旦真的出现异常，程序立即死亡！**



### 2.5 编译时异常的处理机制

- 编译时异常是编译阶段就出错的，所以必须处理，否则代码根本无法通过

**编译时异常的处理形式有三种：**

1. 出现异常直接抛出去给调用者，调用者也继续抛出去。
2. 出现异常自己捕获处理，不麻烦别人。
3. 前两者结合，出现异常直接抛出去给调用者，调用者捕获处理。



**异常处理方式1 —— throws**

- throws：用在方法上，可以将方法内部出现的异常抛出去给本方法的调用者处理。
- 这种方式并不好，发生异常的方法自己不处理异常，如果异常最终抛出去给虚拟机将引起程序死亡。
- **抛出异常格式：**`方法 throws 异常1 ，异常2 ，异常3 ...{ }`
- **规范做法：**`方法 throws Exception{ }`，代表可以抛出一切异常

**异常处理方式2 —— try…catch…**

- 监视捕获异常，用在方法内部，可以将方法内部出现的异常直接捕获处理。

- 这种方式还可以，发生异常的方法自己独立完成异常的处理，程序可以继续往下执行。

- **格式：**

  ```
  try{
  	// 监视可能出现异常的代码！
  }catch(异常类型1 变量){
  	// 处理异常
  }catch(异常类型2 变量){
  	// 处理异常
  }...
  ```

- **建议格式：**

  ```
  try{
  	// 可能出现异常的代码！
  }catch (Exception e){
  	e.printStackTrace(); 	// 直接打印异常栈信息
  }
  
  Exception可以捕获处理一切异常类型！
  ```

```java
public class ExceptionDemo02 {
    public static void main(String[] args) {
        System.out.println("程序开始。。。。");
        parseTime("2011-11-11 11:11:11");
        System.out.println("程序结束。。。。");
    }

    public static void parseTime(String date) {
        try {
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM-dd HH:mm:ss");
            Date d = sdf.parse(date);
            System.out.println(d);

            InputStream is = new FileInputStream("E:/meinv.jpg");		//向指定目录获取一张图片
        } catch (Exception e) {
            e.printStackTrace(); // 打印异常栈信息
        }
    }
}
```

输出：

```
程序开始。。。。
java.text.ParseException: Unparseable date: "2011-11-11 11:11:11"
	at java.base/java.text.DateFormat.parse(DateFormat.java:399)
	at com.itheima_stream_yichang.d7_exception_handle.ExceptionDemo02.parseTime(ExceptionDemo02.java:48)
	at com.itheima_stream_yichang.d7_exception_handle.ExceptionDemo02.main(ExceptionDemo02.java:41)
程序结束。。。。

Process finished with exit code 0
```



**异常处理方式3 —— 前两者结合**

- 方法直接将异通过throws抛出去给调用者
- 调用者收到异常后不能再抛了，直接捕获处理。

```java
public class ExceptionDemo03 {
    public static void main(String[] args) {
        System.out.println("程序开始。。。。");
        try {
            parseTime("2011-11-11 11:11:11");		//调用者进行异常处理
            System.out.println("功能操作成功~~~");
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("功能操作失败~~~");
        }
        System.out.println("程序结束。。。。");
    }

    public static void parseTime(String date) throws Exception {		//方法将异常抛出
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy、MM-dd HH:mm:ss");
        Date d = sdf.parse(date);
        System.out.println(d);

        InputStream is = new FileInputStream("D:/meinv.jpg");
    }
}
```

输出：

```
程序开始。。。。
java.text.ParseException: Unparseable date: "2011-11-11 11:11:11"
	at java.base/java.text.DateFormat.parse(DateFormat.java:399)
	at com.itheima_stream_yichang.d7_exception_handle.ExceptionDemo03.parseTime(ExceptionDemo03.java:37)
	at com.itheima_stream_yichang.d7_exception_handle.ExceptionDemo03.main(ExceptionDemo03.java:26)
功能操作失败~~~
程序结束。。。。

Process finished with exit code 0
```

将` InputStream is = new FileInputStream("D:/meinv.jpg");`删除，将`SimpleDateFormat sdf = new SimpleDateFormat("yyyy、MM-dd HH:mm:ss");`中的日期格式改正确后，输出:

```
程序开始。。。。
Fri Nov 11 11:11:11 CST 2011
功能操作成功~~~
程序结束。。。。

Process finished with exit code 0
```



**异常处理的总结：**

- 在开发中按照规范来说第三种方式是最好的：底层的异常抛出去给最外层，最外层集中捕获处理。（理论）
- 实际应用中，只要代码能够编译通过，并且功能能完成，那么每一种异常处理方式似乎也都是可以的。（实际）



### 2.6 运行时异常的处理机制

**运行时异常的处理形式**

- 运行时异常编译阶段不会出错，是运行时才可能出错的，所以编译阶段不处理也可以。
- 按照规范建议还是处理：建议在最外层调用处集中捕获处理即可。 



```java
public class Test {
    public static void main(String[] args) {
        System.out.println("程序开始。。。。。。。。。。");
        try {
            chu(10, 0);
        } catch (Exception e) {
            e.printStackTrace();
        }
        System.out.println("程序结束。。。。。。。。。。");
    }

    public static void chu(int a , int b) { // throws RuntimeException(运行时异常默认抛){
        System.out.println(a);
        System.out.println(b);
        int c = a / b;
        System.out.println(c);
    }
}
```

输出：

```
程序开始。。。。。。。。。。
10
0
程序结束。。。。。。。。。。
java.lang.ArithmeticException: / by zero
	at com.itheima_stream_yichang.d8_exception_handle_runtime.Test.chu(Test.java:23)
	at com.itheima_stream_yichang.d8_exception_handle_runtime.Test.main(Test.java:13)

Process finished with exit code 0
```



### 2.7 异常处理使代码更稳健的案例

**需求**

- 键盘录入一个合理的价格为止（必须是数值，值必须大于0）。

**分析：**

- 定义一个死循环，让用户不断的输入价格。

```java
public class Test2 {
    public static void main(String[] args) {
        Scanner sc  = new Scanner(System.in);
        while (true) {
            //写成异常处理，就算用户输入的数据有问题，也不会崩
            try {
                System.out.println("请您输入合法的价格：");
                String priceStr = sc.nextLine();
                // 转换成double类型的价格
                double price = Double.valueOf(priceStr);    //价格转double类型的时候，如果用户出入13.2.3，则程序会崩

                // 判断价格是否大于 0
                if(price > 0) {
                    System.out.println("定价：" + price);
                    break;
                }else {
                    System.out.println("价格必须是正数~~~");
                }
            } catch (Exception e) {
                System.out.println("用户输入的数据有毛病，请您输入合法的数值，建议为正数~~");
            }
        }
    }
}
```

测试：

```
请您输入合法的价格：
-12.2
价格必须是正数~~~
请您输入合法的价格：
12.2.1
用户输入的数据有毛病，请您输入合法的数值，建议为正数~~
请您输入合法的价格：
abs
用户输入的数据有毛病，请您输入合法的数值，建议为正数~~
请您输入合法的价格：
12.99
定价：12.99

Process finished with exit code 0
```





### 2.8 自定义异常

**自定义异常的必要**

- Java无法为这个世界上全部的问题提供异常类。
- 如果企业想通过异常的方式来管理自己的某个业务问题，就需要自定义异常类了。

**自定义异常的好处：**

- 可以使用异常的机制管理业务问题，如提醒程序员注意。
- 同时一旦出现bug，可以用异常的形式清晰的指出出错的地方。

**自定义异常的分类**

1. 自定义编译时异常（编译时异常是编译阶段就报错，提醒更加强烈，一定需要处理）
   - 定义一个异常类继承Exception.
   - 重写构造器。
   - 在出现异常的地方用throw new 自定义对象抛出。
2. 自定义运行时异常（提醒不强烈，编译阶段不报错！！运行时才可能出现）
   - 定义一个异常类继承RuntimeException.
   - 重写构造器。
   - 在出现异常的地方用throw new 自定义对象抛出!



**示例：**

需求：认为年龄小于0岁，大于200岁就是一个异常。

自定义编译时异常类：

```java
public class ItheimaAgeIlleagalException extends Exception{
    public ItheimaAgeIlleagalException() {
    }

    public ItheimaAgeIlleagalException(String message) {
        super(message);
    }
}
```

自定义运行时异常类：

```java
public class ItheimaAgeIlleagalRuntimeException extends RuntimeException{
    public ItheimaAgeIlleagalRuntimeException() {
    }

    public ItheimaAgeIlleagalRuntimeException(String message) {
        super(message);
    }
}
```

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        //编译时异常
//        try {
//            checkAge1(-34);
//        } catch (ItheimaAgeIlleagalException e) {
//            e.printStackTrace();
//        }

        //运行时异常
        try {
            checkAge2(-23);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //运行时异常
    public static void checkAge2(int age)  {	//运行时异常会自动抛
        if(age < 0 || age > 200){
            // 抛出去一个异常对象给调用者
            // throw ：在方法内部直接创建一个异常对象，并从此点抛出
            // throws : 用在方法申明上的，抛出方法内部的异常
            throw new ItheimaAgeIlleagalRuntimeException(age + " is illeagal!");
        }else {
            System.out.println("年龄合法：推荐商品给其购买~~");
        }
    }

    //编译时异常
    public static void checkAge1(int age) throws ItheimaAgeIlleagalException {
        if(age < 0 || age > 200){
            // 抛出去一个异常对象给调用者
            // throw ：在方法内部直接创建一个异常对象，并从此点抛出
            // throws : 用在方法申明上的，抛出方法内部的异常
            throw new ItheimaAgeIlleagalException(age + " is illeagal!");
        }else {
            System.out.println("年龄合法：推荐商品给其购买~~");
        }
    }
}
```





## 3. 日志框架

### 3.1 日志技术的概述

- 程序中的日志： 程序中的日志可以用来记录程序运行过程中的信息，并可以进行永久存储

**之前用输出语句记录日志方式的弊端：**

- 信息只能展示在控制台
- 不能将其记录到其他的位置（文件，数据库）
- 想取消记录的信息需要修改代码才可以完成

**日志技术具备的优势**

- 可以将系统执行的信息选择性的记录到指定的位置（控制台、文件中、数据库中）。
- 可以随时以开关的形式控制是否记录日志，无需修改源代码。



### 3.2 日志技术体系

**体系结构**

日志规范接口：

- Commons Logging(JCL)
- Simple Logging Facade for Java(slf4j)

日志实现框架：

- Log4j
- JUL(java.util.logging)
- Logback(基于slf4j接口)
- 其他实现

**日志规范：**一些接口，提供给日志的实现框架设计的标准。

**日志框架：**牛人或者第三方公司已经做好的日志记录实现代码，后来者直接可以拿去使用。



### 3.3 Logback概述

**Logback日志框架**

- Logback是由log4j创始人设计的另一个开源日志组件，性能比log4j要好
- 官方网站：[Logback官网](https://logback.qos.ch/index.html)
- Logback是基于slf4j的日志规范实现的框架。

**Logback主要分为三个技术模块：**

- logback-core： logback-core 模块为其他两个模块奠定了基础，必须有。
- logback-classic：它是log4j的一个改良版本，同时它完整实现了slf4j API。
- logback-access 模块与 Tomcat 和 Jetty 等 Servlet 容器集成，以提供 HTTP 访问日志功能



### 3.4 Logback快速入门

导入Logback日志技术到项目中，用于纪录系统的日志信息：

1. 在项目下新建文件夹lib，导入Logback的相关jar包到该文件夹下，并添加到项目依赖库中去

2. 将Logback的核心配置文件logback.xml直接拷贝到src目录下（必须是src下）

3. 在代码中获取日志的对象：

   `public static final Logger LOGGER = LoggerFactory.getLogger("类对象");`

4. 使用日志对象LOGGER调用其方法输出不能的日志信息

```java
public class Test {
    public static final Logger LOGGER = LoggerFactory.getLogger("Test.class");
    public static void main(String[] args) {
        try {
            LOGGER.debug("main方法开始执行~~~");
            LOGGER.info("开始记录第二行日志，准备开始做除法了");
            int a = 10;
            int b = 0;
            LOGGER.trace("a=" + a);
            LOGGER.trace("b=" + b);
            System.out.println(a / b);
        }catch (Exception e){
            e.printStackTrace();
            LOGGER.error("功能出现异常，" + e);
        }
    }
}
```

运行后，控制台输出：

```
2022-01-11 00:39:22.959 [DEBUG]  Test.class [main] : main方法开始执行~~~
2022-01-11 00:39:22.962 [INFO ]  Test.class [main] : 开始记录第二行日志，准备开始做除法了
2022-01-11 00:39:22.966 [TRACE]  Test.class [main] : a=10
2022-01-11 00:39:22.966 [TRACE]  Test.class [main] : b=0
2022-01-11 00:39:22.967 [ERROR]  Test.class [main] : 功能出现异常，java.lang.ArithmeticException: / by zero
java.lang.ArithmeticException: / by zero
	at com.run.logback.Test.main(Test.java:20)

Process finished with exit code 0
```

日志文件中：

```
2022-01-11 00:39:22.959 [main] DEBUG Test.class - main方法开始执行~~~
2022-01-11 00:39:22.962 [main] INFO  Test.class - 开始记录第二行日志，准备开始做除法了
2022-01-11 00:39:22.966 [main] TRACE Test.class - a=10
2022-01-11 00:39:22.966 [main] TRACE Test.class - b=0
2022-01-11 00:39:22.967 [main] ERROR Test.class - 功能出现异常，java.lang.ArithmeticException: / by zero
```



### 3.5 Logback配置详解 - 输出位置、格式设置

Logback日志系统的特性都是通过核心配置文件logback.xml控制的。

**Logback日志输出位置、格式设置：**

- 通过logback.xml 中的**<append>**标签可以设置输出位置和日志信息的详细格式。
- 通常可以设置2个日志输出位置：**一个是控制台、一个是系统文件中**

**输出到控制台的配置标志:**

`<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">`

**输出到系统文件的配置标志:**

`<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">`



附：logback.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--
        CONSOLE ：表示当前的日志信息是可以输出到控制台的。
    -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <!--输出流对象 默认 System.out 改为 System.err-->
        <target>System.out</target>
        <encoder>
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度
                %msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level]  %c [%thread] : %msg%n</pattern>
        </encoder>
    </appender>

    <!-- File是输出的方向通向文件的 -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            <charset>utf-8</charset>
        </encoder>
        <!--日志输出路径-->
        <file>D:/JavaLogback/aruni-data.log</file>
        <!--指定日志文件拆分和压缩规则-->      <!--当文件过大时自动压缩-->
        <rollingPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <!--通过指定压缩文件名称，来确定分割文件方式-->
            <fileNamePattern>D:/JavaLogback/aruni-data2-%d{yyyy-MMdd}.log%i.gz</fileNamePattern>
            <!--文件拆分大小-->
            <maxFileSize>1MB</maxFileSize>       <!--差不多到1MB就压缩-->
        </rollingPolicy>
    </appender>

    <!--
    level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
   ， 默认debug
    <root>可以包含零个或多个<appender-ref>元素，标识这个输出位置将会被本日志级别控制。
    -->

    <root level="ALL">
        <!--注意：此处是开关，如果不配置关联打印位置，则该位置不会记录日志-->
        <appender-ref ref="CONSOLE"/>       <!--在控制台位置记录日志-->
        <appender-ref ref="FILE" />         <!--在文件位置记录日志-->
    </root>
</configuration>
```



### 3.6 Logback配置详解-日志级别设置

如果系统上线后只想记录一些错误的日志信息或者不想记录日志了，可以通过**设置日志的输出级别**来控制哪些日志信息输出或者不输出。

**日志级别**

- 级别程度依次是：TRACE< DEBUG< INFO<WARN<ERROR ; 默认级别是debug（忽略大小写），对应其方法。
- 作用：用于控制系统中哪些日志级别是可以输出的，**只输出级别不低于设定级别**的日志信息。
- ALL  和 OFF分别是打开全部日志信息，关闭全部日志信息。

**具体在`<root level="ERROR">`标签的level属性中设置日志级别：**

```xml
<!--
    level:用来设置打印级别，大小写无关：TRACE, DEBUG, INFO, WARN, ERROR, ALL 和 OFF
   ， 默认debug
    <root>可以包含零个或多个<appender-ref>元素，标识这个将会在对应的位置记录日志。
    -->

<root level="ERROR">
        <!--注意：此处是开关，如果不配置关联打印位置，则该位置不会记录日志-->
        <appender-ref ref="CONSOLE"/>       <!--在控制台位置记录日志-->
        <appender-ref ref="FILE" />         <!--在文件位置记录日志-->
</root>
```









# 阶段项目实战 - 影院系统

## 1. 日志框架搭建、系统分析

系统分析：

1. 集成日志框架、用于后期记录日志信息。
2. 定义一个电影类Movie类，Movie类包含：片名、主演、评分、时长、票价、余票
3. 系统包含2个用户角色：客户、商家。存在大量相同属性信息。
4. 定义User类作为父类，属性：登录名称、密码、真实名称、性别、电话、账户金额
5. 定义Business类代表商家角色，独有属性：店铺名称、地址。
6. 定义Customer类代表客户角色，独有属性：
7. 定义集合List<User>用户存放系统注册的用户对象信息。
8. 定义集合Map<Business, List<Movie>>存放商家和其排片信息。
9. 准备一些测试数据。

## 2. 首页设计、登录、商家界面、用户界面实现

1. 首页需要包含登录，商家入驻，客户注册功能。
2. 商家和客户可以共用一个登录功能。
3. 判断登录成功的用户的真实类型，根据用户类型完成对应的操作界面设计

## 3. 商家 - 详情页设计、影片上架、退出

1. 展示本商家的信息和其排片情况。
2. 提供影片上架功能：就是创建一个影片对象，存入到商家的集合中去。
3. 退出，需要回到登录的首页。

## 4. 商家 - 影片下架、影片修改

1. 提供影片下架功能：其实就是从商家的集合中删除影片对象。
2. 影片修改功能：拿到需要修改的影片对象，修改里面的数据。

## 5. 用户 - 展示全部影片

1. 其实就是 遍历全部商家和其排片信息并展示出来。


## 6. 用户 - 购票功能

1. 用户可以选择需要购买票的商家和其电影信息。
2. 可以选择购买的数量。
3. 购买成功后需要支付金额，并更新商家金额和客户金额

## 7. 用户 - 评分功能

1. 用户只能对自己已经购买过的电影进行评分。
2. 需要在当前用户对象中记录购买的电影信息（包括是否已经评价过）。
3. 每部电影的评分信息应该是系统所有用户评分的平均值。
4. 应该在系统定义一个集合存储每部电影的评分详情。





## 8. 用户 - 评分降序展示(学生作业)

已完成







## 9. 用户 - 根据片名查询全部影片信息（学生作业）

已完成



项目地址：[影院系统](https://pan.baidu.com/s/1meuZAhTlZ0pVwFNVPrcS9g?pwd=rkf9 
提取码：rkf9)







# File、非规律化递归、IO流

## 1. File

### 1.1 File类概述

- File类在包java.io.File下，代表操作系统的文件对象（文件、文件夹）。
- File类提供了诸如：定位文件，获取文件本身的信息、删除文件、创建文件（文件夹）等功能。

**File类创建对象**

|                   方法名称                   |                        说明                        |
| :------------------------------------------: | :------------------------------------------------: |
|       **public** File(String pathname)       |              根据文件路径创建文件对象              |
| **public** File(String parent, String child) |    从父路径名字符串和子路径名字符串创建文件对象    |
|  **public** File(File parent, String child)  | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

- File对象可以定位文件和文件夹
- File封装的对象仅仅是一个路径名，这个路径可以是存在的，也可以是不存在的。

```java
public class Test {
    public static void main(String[] args) {
        // 1、创建File对象（指定了文件的路径）
        // 路径写法：1、D:\FileTest\resources.txt
        //          2、D:/FileTest/resources.txt
        //          3、File.separator
        //File f = new File("D:\\FileTest\\resources.txt");
        //File f = new File("D:/FileTest/resources.txt");
        File f = new File("D:" + File.separator + "FileTest" + File.separator + "resources.txt");
        long size = f.length(); // 是文件的字节大小
        System.out.println(size);       //6(文件中的内容: abc123)

        // 2、File创建对象，支持绝对路径 支持相对路径（重点）
        File f1 = new File("D:\\FileTest\\resources.txt"); // 绝对路径
        System.out.println(f1.length());

        // 相对路径：一般定位模块中的文件的。 相对到工程下！！
        File f2 = new File("JavaSE/src/data.txt");
        System.out.println(f2.length());

        // 3、File创建对象 ，可以是文件也可以是文件夹
        File f3 = new File("D:\\FileTest");
        System.out.println(f3.exists()); // 判断这个路径是否存在，这个文件夹存在否
    }
}
```



### 1.2 File类的常用API

#### 1.2.1 判断文件类型、获取文件信息

|            方法名称             |                    说明                    |
| :-----------------------------: | :----------------------------------------: |
|  public boolean isDirectory()   |   测试此抽象路径名表示的File是否为文件夹   |
|     public boolean isFile()     |    测试此抽象路径名表示的File是否为文件    |
|     public boolean exists()     |     测试此抽象路径名表示的File是否存在     |
| public String getAbsolutePath() |        返回此抽象路径名的绝对路径名        |
|     public String getPath()     |   将此抽象路径名转换为定义时使用的路径名   |
|     public String getName()     | 返回由此抽象路径名表示的文件或文件夹的名称 |
|   public long lastModified()    |        返回文件最后修改的时间毫秒值        |

```java
public class Test1 {
    public static void main(String[] args) {
        // 1.绝对路径创建一个文件对象
        File f1 = new File("D:\\FileTest\\resources.txt");
        // a.获取它的绝对路径。
        System.out.println(f1.getAbsolutePath());
        // b.获取文件定义的时候使用的路径。
        System.out.println(f1.getPath());
        // c.获取文件的名称：带后缀。
        System.out.println(f1.getName());
        // d.获取文件的大小：字节个数。
        System.out.println(f1.length()); // 字节大小
        // e.获取文件的最后修改时间
        long time = f1.lastModified();
        System.out.println("最后修改时间：" + new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(time));
        // f、判断文件是文件还是文件夹
        System.out.println(f1.isFile()); // true
        System.out.println(f1.isDirectory()); // false

        System.out.println("-------------------------");

        File f2 = new File("JavaSE/src/data.txt");
        // a.获取它的绝对路径。
        System.out.println(f2.getAbsolutePath());
        // b.获取文件定义的时候使用的路径。
        System.out.println(f2.getPath());
        // c.获取文件的名称：带后缀。
        System.out.println(f2.getName());
        // d.获取文件的大小：字节个数。
        System.out.println(f2.length()); // 字节大小
        // e.获取文件的最后修改时间
        long time1 = f2.lastModified();
        System.out.println("最后修改时间：" + new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(time1));
        // f、判断文件是文件还是文件夹
        System.out.println(f2.isFile()); // true
        System.out.println(f2.isDirectory()); // false
        System.out.println(f2.exists()); // true

        File file = new File("D:/");
        System.out.println(file.isFile()); // false
        System.out.println(file.isDirectory()); // true
        System.out.println(file.exists()); // true

        File file1 = new File("D:/aaa");
        System.out.println(file1.isFile()); // false
        System.out.println(file1.isDirectory()); // false
        System.out.println(file1.exists()); // false
    }
}
```



#### 1.2.2 创建文件、删除文件功能

|            方法名称            |                  说明                  |
| :----------------------------: | :------------------------------------: |
| public boolean createNewFile() |          创建一个新的空的文件          |
|     public boolean mkdir()     |           只能创建一级文件夹           |
|    public boolean mkdirs()     |           可以创建多级文件夹           |
|    public boolean delete()     | 删除由此抽象路径名表示的文件或空文件夹 |

- delete方法默认只能删除文件和**空文件夹**。
- delete方法直接删除不走回收站

```java
public class Test2 {
    public static void main(String[] args) throws IOException {
        File f = new File("JavaSE\\src\\data.txt");
        // a.创建新文件，创建成功返回true ,反之false。 不需要这个，以后文件写出去的时候都会自动创建
        System.out.println(f.createNewFile());
        File f1 = new File("JavaSE\\src\\data02.txt");
        System.out.println(f1.createNewFile()); // （几乎不用的，因为以后文件都是自动创建的！）

        // b.mkdir创建一级目录
        File f2 = new File("D:/FileTest/aaa");
        System.out.println(f2.mkdir());

        // c.mkdirs创建多级目录(重点)
        File f3 = new File("D:/FileTest/ccc/ddd/eee/ffff");
//        System.out.println(f3.mkdir());
        System.out.println(f3.mkdirs()); // 支持多级创建

        // d.删除文件或者空文件夹(系统中的也可以)
        System.out.println(f1.delete());
        File f4 = new File("D:/resources/xueshan.jpeg");
        System.out.println(f4.delete()); // 占用一样可以删除

        // 只能删除空文件夹,不能删除非空文件夹.
        File f5 = new File("D:/resources/aaa");
        System.out.println(f5.delete());

    }
}
```



#### 1.2.3 遍历文件夹

|            方法名称             |                             说明                             |
| :-----------------------------: | :----------------------------------------------------------: |
|     public String[] list()      | 获取当前目录下所有的"一级文件名称"到一个字符串数组中去返回。 |
| public File[] listFiles()(常用) | 获取当前目录下所有的"一级文件对象"到一个文件对象数组中去返回（重点） |



**listFiles方法注意事项：**

- 当调用者不存在时，返回null
- 当调用者是一个文件时，返回null（返回一级文件对象，调用者必须是一个文件夹）
- 当调用者是一个空文件夹时，返回一个长度为0的数组
- 当调用者是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
- 当调用者是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，**包含隐藏内容**
- 当调用者是一个**需要权限**才能进入的文件夹时，返回null

```java
public class Test3 {
    public static void main(String[] args) {
        // 1、定位一个目录
        File f1 = new File("D:/FileTest");
        String[] names = f1.list();
        for (String name : names) {
            System.out.println(name);
        }

        System.out.println("---------------------");
        // 2.一级文件对象
        // 获取当前目录下所有的"一级文件对象"到一个文件对象数组中去返回（重点）
        File[] files = f1.listFiles();
        for (File f : files) {
            System.out.println(f.getAbsolutePath());
        }
    }
}
```

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6f4f65bec4a90f78a9ce82ac39543c33.png)



## 2. 非规律化递归

### 2.1 案例 -- 文件搜索

需求：文件搜索、从D：盘中，搜索出某个文件名称并输出绝对路径。

分析：

1. 先定位出的应该是一级文件对象
2. 遍历全部一级文件对象，判断是否是文件
3. 如果是文件，判断是否是自己想要的
4. 如果是文件夹，需要继续递归进去重复上述过程

```java
public class unRegularRecursion {
    public static void main(String[] args) {
        searchFile(new File("D:/"), "JavaSE.md");
    }

    //搜索某个目录下的全部文件，找到想要的文件
    //dir -- 被搜索的源目录(文件夹)；fileName -- 被搜索的文件名
    public static void searchFile(File dir, String fileName){
        //1. 判断dir是否是目录
        if(dir != null && dir.isDirectory()){
            //2. 提取当前目录下的一级文件对象
            File[] files = dir.listFiles();
            //3. 判断是否存在一级文件对象，存在才能遍历
            if(files != null && files.length > 0){
                for (File file : files) {
                    //4. 判断当前遍历的一级文件对象是文件还是目录
                    if(file.isFile()){
                        //5. 是文件，判断是不是要找的(只要包含都找出来)
                        if(file.getName().contains(fileName)){
                            System.out.println("找到了：" + file.getAbsolutePath());
                        }
                    }else{
                        //是目录(文件夹)，继续 递归查找
                        searchFile(file, fileName);
                    }
                }
            }
        }else{
            System.out.println("对不起，当前搜索的位置不是文件夹！");
        }
    }
}

输出：找到了：D:\Typaro_Notes\JavaSE\JavaSE.md
```



### 2.2 案例 -- 啤酒问题

需求：啤酒2元1瓶，4个盖子可以换一瓶，2个空瓶可以换一瓶，请问10元钱可以喝多少瓶酒，剩余多少空瓶和盖子。

答案：15瓶 3盖子 1瓶子 



```java
public class beerProblem {
    // 定义一个静态的成员变量用于存储可以买的酒数量
    public static int totalNumber; // 总数量
    public static int lastBottleNumber; // 记录每次剩余的瓶子个数
    public static int lastCoverNumber; // 记录每次剩余的盖子个数


    public static void main(String[] args) {
        // 1、拿钱买酒
        buy(10);
        System.out.println("总数：" + totalNumber);
        System.out.println("剩余盖子数：" + lastCoverNumber);
        System.out.println("剩余瓶子数：" + lastBottleNumber);
    }

    public static void buy(int money){
        // 2、看可以立马买多少瓶
        int buyNumber = money / 2; // 5
        totalNumber += buyNumber;

        // 3、把盖子 和瓶子换算成钱
        // 统计本轮总的盖子数  和 瓶子数
        int coverNumber = lastCoverNumber + buyNumber;
        int bottleNumber = lastBottleNumber + buyNumber;

        // 统计可以换算的钱
        int allMoney = 0;
        if(coverNumber >= 4){
            allMoney += (coverNumber / 4) * 2;
        }
        lastCoverNumber = coverNumber % 4;

        if(bottleNumber >= 2){
            allMoney += (bottleNumber / 2) * 2;
        }
        lastBottleNumber = bottleNumber % 2;

        if(allMoney >= 2){
            buy(allMoney);
        }

        Integer[] arr2 = new Integer[]{11, 22, 33};
        Arrays.sort(arr2);
    }
}
```



### 2.3 扩展 - 删除非空文件夹

- File默认不可以删除非空文件夹
- 我们需要遍历文件夹，先删除里面的内容，再删除自己。

```java
public class RecursionDemo07 {
    public static void main(String[] args) {
        deleteDir(new File("D:/new"));
    }

    /**
       删除文件夹，无所谓里面是否有内容，都可以删除
     * @param dir
     */
    public static void deleteDir(File dir){
        // 1、判断dir存在且是文件夹
        if(dir != null && dir.exists() && dir.isDirectory()){
            // 2、提取一级文件对象。
            File[] files = dir.listFiles();
            // 3、判断是否存在一级文件对象，存在则遍历全部的一级文件对象去删除
            if(files != null && files.length > 0){
                // 里面有内容
                for (File file : files) {
                    // 4、判断file是文件还是文件夹，文件直接删除
                    if(file.isFile()){
                        file.delete();
                    }else {
                        // 递归删除
                        deleteDir(file);
                    }
                }
            }
            // 删除自己
            dir.delete();
        }
    }
}
```



## 3. 字符集

### 3.1 常见字符集介绍

- 计算机底层不可以直接存储字符的。计算机中底层只能存储二进制(0、1)
- 二进制是可以转换成十进制的
- 计算机底层可以表示十进制编号。计算机可以给人类字符进行编号存储，这套编号规则就是字符集



**ASCII字符集：**

- ASCII(American Standard Code for Information Interchange，美国信息交换标准代码)：包括了数字、英文、符号。
- ASCII使用1个字节存储一个字符，一个字节是8位，总共可以表示128个字符信息，对于英文，数字来说是够用的。



**GBK：**

- window系统默认的码表。兼容ASCII码表，也包含了几万个汉字，并支持繁体汉字以及部分日韩文字。
- 注意：GBK是中国的码表，**一个中文以两个字节**的形式存储。但不包含世界上所有国家的文字。



**Unicode码表：**

- unicode（又称统一码、万国码、单一码）是计算机科学领域里的一项业界字符编码标准。
- 容纳世界上大多数国家的所有常见文字和符号。
- 由于Unicode会先通过UTF-8，UTF-16，以及 UTF-32的编码成二进制后再存储到计算机，其中最为常见的就是UTF-8。

**注意：**

- Unicode是万国码，以UTF-8编码后**一个中文**一般以**三个字节**的形式存储。
- UTF-8也要兼容ASCII编码表。
- 技术人员都应该使用UTF-8的字符集编码。
- 编码前和编码后的字符集需要一致，否则会出现中文乱码。





### 3.2 字符集的编码、解码操作

**String编码**

|              方法名称               |                             说明                             |
| :---------------------------------: | :----------------------------------------------------------: |
|          byte[] getBytes()          | 使用平台的默认字符集将该 String编码为一系列字节，将结果存储到新的字节数组中 |
| byte[] getBytes(String charsetName) | 使用指定的字符集将该 String编码为一系列字节，将结果存储到新的字节数组中 |

**String解码**

|                  构造器                  |                            说明                             |
| :--------------------------------------: | :---------------------------------------------------------: |
|           String(byte[] bytes)           | 通过使用平台的默认字符集解码指定的字节数组来构造新的 String |
| String(byte[] bytes, String charsetName) |     通过指定的字符集解码指定的字节数组来构造新的 String     |

```java
public class Test {
    public static void main(String[] args) throws Exception {
        // 1、编码：把文字转换成字节（使用指定的编码）
        String name = "abc我爱你中国";
        // byte[] bytes = name.getBytes(); // 以当前代码默认字符集进行编码 （UTF-8）
        byte[] bytes = name.getBytes("GBK"); // 指定编码
        System.out.println(bytes.length);
        System.out.println(Arrays.toString(bytes));

        // 2、解码：把字节转换成对应的中文形式（编码前 和 编码后的字符集必须一致，否则乱码 ）
        // String rs = new String(bytes); // 默认的UTF-8，乱码
        String rs = new String(bytes, "GBK"); // 指定GBK解码
        System.out.println(rs);
    }
}

```



## 4. IO流

**IO流概述**

- I表示intput，是数据从硬盘文件读入到内存的过程，称之输入，负责读。
- O表示output，是内存程序的数据从内存到写出到硬盘文件的过程，称之输出，负责写。

输入输出是相对于内存来说的。



**IO流体系**

- 字节流(音视频/图片等)
  - 字节输入流：InputStream
    - 实现类：FileInputStream
  - 字节输出流：OutputStream
    - 实现类：FileOutputStream
- 字符流(java文件/txt文件等)
  - 字符输入流：Reader
    - 实现类：FileReader
  - 字符输出流：Writer
    - 实现类：FileWriter



## 5.  字节流的使用

### 5.1  文件字节输入流：FileInputStream

#### 5.1.1 **每次读取一个字节**



- 作用：以内存为基准，把磁盘文件中的数据以字节的形式读取到内存中去。

构造器：

|                 构造器                  |                说明                |
| :-------------------------------------: | :--------------------------------: |
|    public FileInputStream(File file)    | 创建字节输入流管道与源文件对象接通 |
| public FileInputStream(String pathname) | 创建字节输入流管道与源文件路径接通 |

相关方法：

|            方法名称            |                            说明                            |
| :----------------------------: | :--------------------------------------------------------: |
|       public int read()        |   每次读取一个**字节**返回，如果字节已经没有可读的返回-1   |
| public int read(byte[] buffer) | 每次读取一个**字节数组**返回，如果字节已经没有可读的返回-1 |



```java
public class FileInputStreamDemo {
    public static void main(String[] args) throws Exception {
        //InputStream is = new FileInputStream(new File("JaveSE\\src\\data.txt"));
        //简化写法：
        InputStream is = new FileInputStream("JavaSE\\src\\data.txt");

        //读取一个字符(文件里的内容: aa11)
//        int b1 = is.read();
//        System.out.println((char)b1);   //a
//        int b2 = is.read();
//        System.out.println((char)b2);   //a
//        int b3 = is.read();
//        System.out.println((char)b3);   //1
//        int b4 = is.read();
//        System.out.println((char)b4);   //1
//        int b5 = is.read();
//        System.out.println(b5);         //-1

        //使用循环读取
        int b;
        while((b = is.read()) != -1){
            System.out.println((char) b);
        }
    }
}
```



每次读取一个字节存在的问题：

- 性能较慢
- 读取中文字符输出无法避免乱码问题。只有读一个字节的符号没问题。



#### 5.1.2 **每次读取一个字节数组**

```java
public class FileInputStreamDemo02 {
    public static void main(String[] args) throws Exception {
        InputStream is = new FileInputStream("JavaSE\\src\\data.txt");

        //文件中的数据：aa11b
/*        byte[] buffer = new byte[2];    //2B
        int len = is.read(buffer);      //读两个字符
        System.out.println("读取的字符数：" + len);
        String rs = new String(buffer);     //解码
        System.out.println(rs);     //aa

        int len2 = is.read(buffer);
        System.out.println("读取的字符数：" + len2);
        String rs2 = new String(buffer);
        System.out.println(rs2);    //11

//        int len3 = is.read(buffer);
//        System.out.println("读取的字符数：" + len2);     //1
//        String rs3 = new String(buffer);
//        System.out.println(rs3);    //b1

//        注意：当最后一次读的时候，文件中还剩下一个字符b，
//        他会只读取1个字节，但是字节数组是2个字节，
//        第二个字符就会用上一次读取的第二个字符补上

        //为了防止以上情况，需要确定从什么地方开始读，读取了多少个字节：
        int len3 = is.read(buffer);
        System.out.println("读取的字符数：" + len3);       //1
        String rs3 = new String(buffer, 0, len3);
        System.out.println(rs3);        //b
*/

        //使用循环读取
        byte[] buffer = new byte[2];
        int len; //记录每次读取的字节数
        while ((len = is.read(buffer)) != -1){
            System.out.print(new String(buffer, 0, len));   //aa11b
        }
    }
}
```

每次读取一个字节数组存在的问题：

- 读取的性能得到了提升。
- 读取中文字符输出无法避免乱码问题。



#### 5.1.3 一次读完全部字节

**方式一：**自己定义一个字节数组与文件的大小一样大，然后使用读取字节数组的方法，一次性读取完成。

**方式二：**官方为字节输入流InputStream提供了如下API可以直接把文件的全部数据读取到一个字节数组中

|                     方法名                      |                             说明                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
| public byte[] readAllBytes() throws IOException | 直接将当前字节输入流对应的文件对象的字节数据装到一个字节数组返回 |

```java
public class FileInputStreamDemo03 {
    public static void main(String[] args) throws Exception {
        File f = new File("JavaSE\\src\\data.txt");
        InputStream is = new FileInputStream(f);

        //方式1、定义一个字节数组与文件的大小一样
//        byte[] buffer = new byte[(int) f.length()];
//        int len = is.read(buffer);
//        System.out.println("读取的字节个数：" + len);
//        System.out.println("文件大小：" + f.length());
//        System.out.println(new String(buffer));

        //方式2、用官方的API
        byte[] buffer = is.readAllBytes();
        System.out.println(new String(buffer));     //aa11b
    }
}
```



**不会乱码，但是文件过大，字节数组可能引起内存溢出(一般不会，除非文件非常大)。**





### 5.2 文件字节输出流：FileOutputStream

- 作用：以内存为基准，把内存中的数据以字节的形式写出到磁盘文件中去的流。

|                          构造器                          |                      说明                      |
| :------------------------------------------------------: | :--------------------------------------------: |
|            public FileOutputStream(File file)            |       创建字节输出流管道与源文件对象接通       |
|    public FileOutputStream(File file，boolean append)    | 创建字节输出流管道与源文件对象接通，可追加数据 |
|         public FileOutputStream(String filepath)         |       创建字节输出流管道与源文件路径接通       |
| public FileOutputStream(String filepath，boolean append) | 创建字节输出流管道与源文件路径接通，可追加数据 |



**文件字节输出流（FileOutputStream）写数据出去的API**

|                       方法名称                       |             说明             |
| :--------------------------------------------------: | :--------------------------: |
|               public void write(int a)               |        写一个字节出去        |
|           public void write(byte[] buffer)           |      写一个字节数组出去      |
| public void write(byte[] buffer , int pos , int len) | 写一个字节数组的一部分出去。 |

**流的关闭与刷新 **

|  方法   |                             说明                             |
| :-----: | :----------------------------------------------------------: |
| flush() |                   刷新流，还可以继续写数据                   |
| close() | 关闭流，释放资源，但是在关闭之前会先刷新流。一旦关闭，就不能再写数据 |

```java
public class OutputStreamDemo {
    public static void main(String[] args) throws Exception {
        //若该目录没有该文件会自动创建
        //这种方式创建会先清空之前的数据，再写入新的数据：
        //OutputStream os = new FileOutputStream("JavaSE\\src\\data2.txt");

        //以追加的方式写数据(在文件位置后面加个参数true)：
        OutputStream os = new FileOutputStream("JavaSE\\src\\data2.txt", true);

        //从内存写数据出去(到文件)
        //1. 写一个字节出去
        os.write('a');
        os.write(98);
        os.write("\r\n".getBytes());    //写换行出去

        //2. 写一个字节数组出去
        byte[] buffer = {'a', 97, 98, 99};
        os.write(buffer);

        byte[] buffer2 = "我是中国人".getBytes();    //将中文字符串转为字节类型
        os.write(buffer2);

        //3. 写一个字节数组的一部分出去
        byte[] buffer3 = {'a', 97, 98, 99};
        os.write(buffer, 0, 3);

        //写数据必须刷新数据
        //os.flush();     //刷新数据后可以继续使用流
        os.close();     //  释放资源，包含了刷新，关闭后流不可以使用
    }
}
```



**字节输出流实现数据追加写法**

- `public FileOutputStream(String filepath，boolean append)`

- 创建字节输出流管道与源文件路径接通，可追加数据

**字节输出流实现写出去的数据能换行**

- `os.write(“\r\n”.getBytes())`
- "\n"是Linux和Unix的换行符，Windows下是"\r\n"

**让写出去的数据能成功生效**

- flush()刷新数据，刷新后流还可以继续使用
- close()方法是关闭流，关闭包含刷新，关闭后流不可以继续使用了。



### 5.3 文件拷贝

- 磁盘 - 内存 - 磁盘  的一个过程

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/97dc338ce56dcbad737da5c57a5a8c38.png)

**使用字节流完成文件的复制，字节流支持一切文件类型的复制**

- 任何文件的底层都是字节，拷贝是一字不漏的转移字节，只要前后文件格式、编码一致没有任何问题。

思路：

1. 根据数据源创建字节输入流对象
2. 根据目的地创建字节输出流对象
3. 读写数据，复制视频
4. 释放资源



```java
public class CopyDemo05 {
    public static void main(String[] args) {
        try {
            // 1、创建一个字节输入流管道与原视频接通
            InputStream is = new FileInputStream("file-io-app/src/out04.txt");

            // 2、创建一个字节输出流管道与目标文件接通
            OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");

            // 3、定义一个字节数组转移数据
            byte[] buffer = new byte[1024];		//每次转移1KB
            int len; // 记录每次读取的字节数。
            while ((len = is.read(buffer)) != -1){
                os.write(buffer, 0 , len);
            }
            System.out.println("复制完成了！");

            // 4、关闭流。
            os.close();
            is.close();
        } catch (Exception e){
            e.printStackTrace();
        }
    }
}
```





## 6. 资源释放的方式

### 6.1 try-catch-finally

- **finally**：在异常处理时提供finally块来执行所有清除操作，比如说IO流中的释放资源

- **特点：被finally控制的语句最终一定会执行，除非JVM退出，就算是return也会执行**

- 异常处理标准格式：`try….catch…finally`

  ```java
  //先定义输入输出流
  InputStream is = null ;
  OutputStream os = null;
  try{
      //在这再给输入输出流跟文件连接：
      // 1、创建一个字节输入流管道与原视频接通
      is = new FileInputStream("file-io-app/src/out04.txt");
      // 2、创建一个字节输出流管道与目标文件接通
      os = new FileOutputStream("file-io-app/src/out05.txt");
  }catch (Exception e){
      e.printStackTrace();
  } finally {
      // 关闭资源！
      try {
      if(os != null) os.close();
      } catch (Exception e) {
      e.printStackTrace();
      }
      try {
      if(is != null) is.close();
      } catch (Exception e) {
      e.printStackTrace();
      }
  }
  ```

- 作用：finally代码块是最终一定要执行的，可以在代码执行完毕的最后用于释放资源。

### 6.2 try-with-resource

- finally虽然可以用于释放资源，但是释放资源的代码过于繁琐，简化的方法：

  ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/08c21379e92af99e37350e6f4e472cdb.png)

**注意：**

- **JDK 7 以及 JDK 9**的 () 中**只能放置资源对象**，否则报错

- 资源都是**实现了Closeable/AutoCloseable接口的类对象**

  - InputStream/OutputStream都实现了Closeable的接口：

    `public abstract class InputStream implements Closeable {}`

    `public abstract class OutputStream implements Closeable, Flushable{} `



**推荐使用JDK7的方案：**

```java
public class TryCatchResouceDemo2 {
    public static void main(String[] args) {

        try (
                // 这里面只能放置资源对象，用完会自动关闭：自动调用资源对象的close方法关闭资源（即使出现异常也会做关闭操作）
                // 1、创建一个字节输入流管道与原视频接通
               InputStream is = new FileInputStream("file-io-app/src/out04.txt");
                // 2、创建一个字节输出流管道与目标文件接通
               OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");

               // int age = 23; // 这里只能放资源，放变量会自动报错。
	       //最终会自动调用资源的close方法
                ) {

            // 3、定义一个字节数组转移数据
            byte[] buffer = new byte[1024];
            int len; // 记录每次读取的字节数。
            while ((len = is.read(buffer)) != -1){
                os.write(buffer, 0 , len);
            }
            System.out.println("复制完成了！");

        } catch (Exception e){
            e.printStackTrace();
        }
    }
}
```





## 7. 字符流的使用

**字节流**读取中文输出会**乱码**或者**内存溢出**，使用**字符流**更合适，**最小单位是按照单个字符读取**。



### 7.1 文件字符输入流一次读取一个字符

**文件字符输入流：Reader**

- 作用：以内存为基准，把磁盘文件中的数据以字符的形式**读取到内存**中去。

|               构造器               |                说明                |
| :--------------------------------: | :--------------------------------: |
|    public FileReader(File file)    | 创建字符输入流管道与源文件对象接通 |
| public FileReader(String pathname) | 创建字符输入流管道与源文件路径接通 |

|            方法名称            |                             说明                             |
| :----------------------------: | :----------------------------------------------------------: |
|       public int read()        |    每次读取一个**字符**返回，如果字符已经没有可读的返回-1    |
| public int read(char[] buffer) | 每次读取一个**字符数组**，返回读取的字符个数，如果字符已经没有可读的返回-1 |

好处：

- 读取中文字符不会出现乱码（如果代码文件编码一致）
- 性能较慢

```java
public class FileReaderDemo01 {
    public static void main(String[] args) throws Exception {
        Reader fr = new FileReader("JavaSE/src/data.txt");
        //文件中的数据：aa11b
        //读取一个字符返回
//        int code = fr.read();
//        System.out.println((char) code);    //a
//
//        int code1 = fr.read();
//        System.out.println((char) code1);   //a

        //使用循环读
        int code;
        while ((code = fr.read()) != -1){
            System.out.print((char) code);  //aa11b
        }
    }
}
```



### 7.2 文件字符输入流一次读取一个字符数组

```java
public class FileReaderDemo02 {
    public static void main(String[] args) throws Exception {
        Reader fr = new FileReader("JavaSE/src/data2.txt");
        //用循环读
        char[] buffer = new char[1024];     //一次读1K字符
        int len;
        while ((len = fr.read(buffer)) != -1){
            String rs = new String(buffer, 0, len);
            System.out.println(rs);
        }
    }
}
```



### 7.3 文件字符输出流

**文件字符输出流：FileWriter**

- 作用：以内存为基准，把内存中的数据以字符的形式**写出到磁盘文件中去**的流。

|                       构造器                       |                      说明                      |
| :------------------------------------------------: | :--------------------------------------------: |
|            public FileWriter(File file)            |       创建字符输出流管道与源文件对象接通       |
|    public FileWriter(File file，boolean append)    | 创建字符输出流管道与源文件对象接通，可追加数据 |
|         public FileWriter(String filepath)         |       创建字符输出流管道与源文件路径接通       |
| public FileWriter(String filepath，boolean append) | 创建字符输出流管道与源文件路径接通，可追加数据 |

**文件字符输出流（FileWriter）写数据出去的API**

|                 方法名称                  |         说明         |
| :---------------------------------------: | :------------------: |
|             void write(int c)             |      写一个字符      |
|          void write(char[] cbuf)          |   写入一个字符数组   |
| void write(char[] cbuf, int off, int len) | 写入字符数组的一部分 |
|          void write(String str)           |     写一个字符串     |
| void write(String str, int off, int len)  | 写一个字符串的一部分 |

**流的关闭与刷新 **

|  方法   |                             说明                             |
| :-----: | :----------------------------------------------------------: |
| flush() |                   刷新流，还可以继续写数据                   |
| close() | 关闭流，释放资源，但是在关闭之前会先刷新流。一旦关闭，流就不能使用 |

```java
public class FileWriterDemo03 {
    public static void main(String[] args) throws Exception {
        // 1、创建一个字符输出流管道与目标文件接通
        // Writer fw = new FileWriter("JavaSE/src/data03.txt"); // 覆盖管道，每次启动都会清空文件之前的数据
        Writer fw = new FileWriter("JavaSE/src/data03.txt", true); // 覆盖管道，每次启动都会清空文件之前的数据

//      a.public void write(int c):写一个字符出去
        fw.write(98);
        fw.write('a');
        fw.write('徐'); // 不会出问题了
        fw.write("\r\n"); // 换行

//       b.public void write(String c)写一个字符串出去
        fw.write("abc我是中国人");
        fw.write("\r\n"); // 换行


//       c.public void write(char[] buffer):写一个字符数组出去
        char[] chars = "abc我是中国人".toCharArray();
        fw.write(chars);
        fw.write("\r\n"); // 换行


//       d.public void write(String c ,int pos ,int len):写字符串的一部分出去
        fw.write("abc我是中国人", 0, 5);
        fw.write("\r\n"); // 换行


//       e.public void write(char[] buffer ,int pos ,int len):写字符数组的一部分出去
        fw.write(chars, 3, 5);
        fw.write("\r\n"); // 换行


        // fw.flush();// 刷新后流可以继续使用
        fw.close(); // 关闭包含刷线，关闭后流不能使用

    }
}
```





# IO流（二）

**IO流体系**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/26dc18f3338750b030d01ac2f4761be3.png)

## 1. 缓冲流

### 1.1 缓冲流概述

- 缓冲流也称为高效流、或者高级流。之前学习的字节流可以称为原始流
- **作用：缓冲流自带缓冲区、可以提高原始字节流、字符流读写数据的性能**



- 字节缓冲流
  - 字节缓冲输入流： BufferedInputStream
  - 字节缓冲输出流：BufferedOutputStream
- 字符缓冲流
  - 字符缓冲输入流：BufferedReader
  - 字符缓冲输出流：BufferedWriter



### 1.2 字节缓冲流

**字节缓冲流性能优化原理：**

- 字节缓冲输入流**自带了8KB缓冲池**，以后我们直接从缓冲池读取数据，所以性能较好。
- 字节缓冲输出流**自带了8KB缓冲池**，数据就直接写入到缓冲池中去，写数据性能极高了。

|                    构造器                    |                             说明                             |
| :------------------------------------------: | :----------------------------------------------------------: |
|  public BufferedInputStream(InputStream is)  | 可以把低级的字节输入流包装成一个高级的缓冲字节输入流管道，从而提高字节输入流读数据的性能 |
| public BufferedOutputStream(OutputStream os) | 可以把低级的字节输出流包装成一个高级的缓冲字节输出流，从而提高写数据的性能 |

```java
public class Demo01 {
    public static void main(String[] args) {
        try (
                // 1、创建一个字节输入流管道与原视频接通
                InputStream is = new FileInputStream("file-io-app/src/out04.txt");

                //a、把原始的字节输入流包装成高级的缓冲字节输入流
                InputStream bis = new BufferedInputStream(is);

                // 2、创建一个字节输出流管道与目标文件接通
                OutputStream os = new FileOutputStream("file-io-app/src/out05.txt");

                //b、把原始的字节输出流包装成高级的缓冲字节输出流
                OutputStream bos = new BufferedOutputStream(os);
        ) {

            // 3、定义一个字节数组转移数据
            byte[] buffer = new byte[1024];
            int len; // 记录每次读取的字节数。
            while ((len = bis.read(buffer)) != -1){
                bos.write(buffer, 0 , len);
            }
            System.out.println("复制完成了！");

        } catch (Exception e){
            e.printStackTrace();
        }
    }
}
```



**建议使用字节缓冲输入流、字节缓冲输出流，结合字节数组(一般是1KB)的方式，目前来看是性能最优的组合。**



### 1.3 字符缓冲流

#### 1.3.1 字符缓冲输入流：BufferedReader

- **作用：提高字符输入流读取数据的性能，除此之外多了按照行读取数据的功能。**

|             构造器              |                             说明                             |
| :-----------------------------: | :----------------------------------------------------------: |
| public BufferedReader(Reader r) | 可以把低级的字符输入流包装成一个高级的缓冲字符输入流管道，从而提高字符输入流读数据的性能 |

字符缓冲输入流新增功能

|           方法           |                         说明                         |
| :----------------------: | :--------------------------------------------------: |
| public String readLine() | 读取一行数据返回，如果读取没有完毕，无行可读返回null |

```java
public class BufferedReaderDemo01 {
    public static void main(String[] args) {
        try(
                Reader fr = new FileReader("JavaSE/src/data2.txt");
                BufferedReader br = new BufferedReader(fr);
                ){
//            //每次读取一个字符数组的数据
//            char[] buffer = new char[1024];
//            int len;
//            while ((len = br.read(buffer)) != -1){
//                String rs = new String(buffer, 0, len);
//                System.out.print(rs);

            //每次读取一行数据
            String line;
            while((line = br.readLine()) != null){
                System.out.println(line);       //一次读一行，需要自己换行
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```



#### 1.3.2 字符缓冲输出流：BufferedWriter

- **作用：提高字符输出流写数据的性能，除此之外多了换行功能**

|             构造器              |                             说明                             |
| :-----------------------------: | :----------------------------------------------------------: |
| public BufferedWriter(Writer w) | 可以把低级的字符输出流包装成一个高级的缓冲字符输出流管道，从而提高字符输出流写数据的性能 |

|          方法           |   说明   |
| :---------------------: | :------: |
| public String newLine() | 换行操作 |

```java
public class BufferedWriterDemo01 {
    public static void main(String[] args) throws Exception {
        // 1、创建一个字符输出流管道与目标文件接通
        // Writer fw = new FileWriter("JavaSE/src/data03.txt"); // 覆盖管道，每次启动都会清空文件之前的数据
        Writer fw = new FileWriter("JavaSE/src/data03.txt", true); // 覆盖管道，每次启动都会清空文件之前的数据
        BufferedWriter bw = new BufferedWriter(fw);
//      a.public void write(int c):写一个字符出去
        bw.write(98);
        bw.write('a');
        bw.write('徐'); // 不会出问题了
        bw.newLine(); // 换行

//       b.public void write(String c)写一个字符串出去
        bw.write("abc我是中国人");
        bw.newLine(); // 换行


//       c.public void write(char[] buffer):写一个字符数组出去
        char[] chars = "abc我是中国人".toCharArray();
        bw.write(chars);
        bw.newLine(); // 换行


//       d.public void write(String c ,int pos ,int len):写字符串的一部分出去
        bw.write("abc我是中国人", 0, 5);
        bw.newLine(); // 换行


//       e.public void write(char[] buffer ,int pos ,int len):写字符数组的一部分出去
        bw.write(chars, 3, 5);
        bw.newLine(); // 换行


        // bw.flush();// 刷新后流可以继续使用
        bw.close(); // 关闭包含刷线，关闭后流不能使用
    }
}
```



### 1.4 案例：拷贝打乱顺序的出师表

需求：把《出师表》的文章顺序进行恢复到一个新文件中。

分析：

1. 定义一个缓存字符输入流管道与源文件接通。
2. 定义一个List集合存储读取的每行数据。
3. 定义一个循环按照行读取数据，存入到List集合中去。
4. 对List集合中的每行数据按照首字符编号升序排序。
5. 定义一个缓存字符输出管道与目标文件接通。
6. 遍历List集合中的每个元素，用缓冲输出管道写出并换行。

```java
public class CopyCSB {
    public static void main(String[] args) {
        try(
                //需要拷贝的源文件
                BufferedReader br = new BufferedReader(new FileReader("JavaSE/src/csb.txt"));

                //目标文件
                BufferedWriter bw = new BufferedWriter(new FileWriter("JavaSE/src/csb02.txt"));
                ){
            List<String> cSB = new ArrayList<>();
            String line;
            while ((line = br.readLine()) != null){
                cSB.add(line);      //按行读取，存到集合里去
            }

            //自定义排序规则（因为是中文的一二三... 如果是数字0-9就可以用sort方法直接排）
            List<String> sizes = new ArrayList<>();
            Collections.addAll(sizes, "一", "二", "三", "四", "五", "陆", "柒", "八", "九", "十", "十一");
            //集合的索引大小就是文字的大小
            Collections.sort(cSB, new Comparator<String>() {
                @Override
                public int compare(String o1, String o2) {
                    //o1   一. ····
                    //o2   二. ····
                    //截取开始位置到"."位置之间的字符，然后用indexOf获取它的索引
                    return sizes.indexOf(o1.substring(0, o1.indexOf("."))) - sizes.indexOf(o2.substring(0, o2.indexOf(".")));
                }
            });

            //将排好序的内容写到目标文件
            for (String s : cSB) {
                bw.write(s);
                bw.newLine();
            }

        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

```
源文件csb.txt：
十一.出师未捷身先死，长使英雄泪满襟。
三.侍中、侍郎郭攸之、费祎、董允等，此皆良实，志虑忠纯，是以先帝简拔以遗陛下。愚以为宫中之事，事无大小，悉以咨之，然后施行，必得裨补阙漏，有所广益。
八.愿陛下托臣以讨贼兴复之效，不效，则治臣之罪，以告先帝之灵。若无兴德之言，则责攸之、祎、允等之慢，以彰其咎；陛下亦宜自谋，以咨诹善道，察纳雅言，深追先帝遗诏，臣不胜受恩感激。
四.将军向宠，性行淑均，晓畅军事，试用之于昔日，先帝称之曰能，是以众议举宠为督。愚以为营中之事，悉以咨之，必能使行阵和睦，优劣得所。
二.宫中府中，俱为一体，陟罚臧否，不宜异同。若有作奸犯科及为忠善者，宜付有司论其刑赏，以昭陛下平明之理，不宜偏私，使内外异法也。
一.先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。然侍卫之臣不懈于内，忠志之士忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气，不宜妄自菲薄，引喻失义，以塞忠谏之路也。
九.今当远离，临表涕零，不知所言。
十.今当远离，临表涕零，不知所言。
陆.臣本布衣，躬耕于南阳，苟全性命于乱世，不求闻达于诸侯。先帝不以臣卑鄙，猥自枉屈，三顾臣于草庐之中，咨臣以当世之事，由是感激，遂许先帝以驱驰。后值倾覆，受任于败军之际，奉命于危难之间，尔来二十有一年矣。
柒.先帝知臣谨慎，故临崩寄臣以大事也。受命以来，夙夜忧叹，恐付托不效，以伤先帝之明，故五月渡泸，深入不毛。今南方已定，兵甲已足，当奖率三军，北定中原，庶竭驽钝，攘除奸凶，兴复汉室，还于旧都。此臣所以报先帝而忠陛下之职分也。至于斟酌损益，进尽忠言，则攸之、祎、允之任也。
五.亲贤臣，远小人，此先汉所以兴隆也；亲小人，远贤臣，此后汉所以倾颓也。先帝在时，每与臣论此事，未尝不叹息痛恨于桓、灵也。侍中、尚书、长史、参军，此悉贞良死节之臣，愿陛下亲之信之，则汉室之隆，可计日而待也。

目标文件csb02.txt：
一.先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。然侍卫之臣不懈于内，忠志之士忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气，不宜妄自菲薄，引喻失义，以塞忠谏之路也。
二.宫中府中，俱为一体，陟罚臧否，不宜异同。若有作奸犯科及为忠善者，宜付有司论其刑赏，以昭陛下平明之理，不宜偏私，使内外异法也。
三.侍中、侍郎郭攸之、费祎、董允等，此皆良实，志虑忠纯，是以先帝简拔以遗陛下。愚以为宫中之事，事无大小，悉以咨之，然后施行，必得裨补阙漏，有所广益。
四.将军向宠，性行淑均，晓畅军事，试用之于昔日，先帝称之曰能，是以众议举宠为督。愚以为营中之事，悉以咨之，必能使行阵和睦，优劣得所。
五.亲贤臣，远小人，此先汉所以兴隆也；亲小人，远贤臣，此后汉所以倾颓也。先帝在时，每与臣论此事，未尝不叹息痛恨于桓、灵也。侍中、尚书、长史、参军，此悉贞良死节之臣，愿陛下亲之信之，则汉室之隆，可计日而待也。
陆.臣本布衣，躬耕于南阳，苟全性命于乱世，不求闻达于诸侯。先帝不以臣卑鄙，猥自枉屈，三顾臣于草庐之中，咨臣以当世之事，由是感激，遂许先帝以驱驰。后值倾覆，受任于败军之际，奉命于危难之间，尔来二十有一年矣。
柒.先帝知臣谨慎，故临崩寄臣以大事也。受命以来，夙夜忧叹，恐付托不效，以伤先帝之明，故五月渡泸，深入不毛。今南方已定，兵甲已足，当奖率三军，北定中原，庶竭驽钝，攘除奸凶，兴复汉室，还于旧都。此臣所以报先帝而忠陛下之职分也。至于斟酌损益，进尽忠言，则攸之、祎、允之任也。
八.愿陛下托臣以讨贼兴复之效，不效，则治臣之罪，以告先帝之灵。若无兴德之言，则责攸之、祎、允等之慢，以彰其咎；陛下亦宜自谋，以咨诹善道，察纳雅言，深追先帝遗诏，臣不胜受恩感激。
九.今当远离，临表涕零，不知所言。
十.今当远离，临表涕零，不知所言。
十一.出师未捷身先死，长使英雄泪满襟。
```



## 2. 转换流

问题引出：不同编码读取乱码问题。文件编码和读取的编码必须一致才不会乱码。

### 2.1 字符输入转换流：InputStreamReader

- 可以把原始的字节流按照指定编码转换成字符输入流。

|                          构造器                           |                            构造器                            |
| :-------------------------------------------------------: | :----------------------------------------------------------: |
|         public InputStreamReader(InputStream is)          | 可以把原始的字节流按照代码默认编码转换成字符输入流。几乎不用，与默认的FileReader一样。 |
| public InputStreamReader(InputStream is ，String charset) | 可以把原始的字节流按照指定编码转换成字符输入流，这样字符流中的字符就不乱码了(**重点**) |

```java
public class InputStreamReaderDemo01 {
    public static void main(String[] args) throws Exception {
        // 代码UTF-8   文件 GBK  "D:\\resources\\data.txt"
        // 1、提取GBK文件的原始字节流。   abc 我
        InputStream is = new FileInputStream("D:\\resources\\data.txt");
        // 2、把原始字节流转换成字符输入流
        // Reader isr = new InputStreamReader(is); // 默认以UTF-8的方式转换成字符流。 还是会乱码的  跟直接使用FileReader是一样的
        Reader isr = new InputStreamReader(is , "GBK"); // 以指定的GBK编码转换成字符输入流  完美的解决了乱码问题

        BufferedReader br = new BufferedReader(isr);
        String line;
        while ((line = br.readLine()) != null){
            System.out.println(line);
        }
    }
}
```



### 2.2 字符输出转换流：OutputStreamWriter

需要控制写出去的字符使用的编码的方法:

- 可以把字符以指定编码获取字节后再使用字节输出流写出去：
  - `“我爱你中国”.getBytes(编码)`
- 也可以使用字符输出转换流实现

|                           构造器                           |                             说明                             |
| :--------------------------------------------------------: | :----------------------------------------------------------: |
|         public OutputStreamWriter(OutputStream os)         | 可以把原始的字节输出流按照代码默认编码转换成字符输出流。几乎不用。 |
| public OutputStreamWriter(OutputStream os，String charset) | 可以把原始的字节输出流按照指定编码转换成字符输出流(**重点**) |

```java
public class OutputStreamWriterDemo02 {
    public static void main(String[] args) throws Exception {
        // 1、定义一个字节输出流
        OutputStream os = new FileOutputStream("JavaSE/src/out03.txt");

        // 2、把原始的字节输出流转换成字符输出流
        // Writer osw = new OutputStreamWriter(os); // 以默认的UTF-8写字符出去 跟直接写FileWriter一样
        Writer osw = new OutputStreamWriter(os , "GBK"); // 指定GBK的方式写字符出去

        // 3、把低级的字符输出流包装成高级的缓冲字符输出流。
        BufferedWriter bw = new BufferedWriter(osw);

        bw.write("我爱中国1~~");
        bw.write("我爱中国2~~");
        bw.write("我爱中国3~~");

        bw.close();
    }
}
```



## 3. 序列化对象

### 3.1 对象序列化

- 作用：**以内存为基准，把内存中的对象存储到磁盘文件中去**，称为对象序列化。
- 使用到的流是**对象字节输出流**：ObjectOutputStream

|                   构造器                    |                    说明                    |
| :-----------------------------------------: | :----------------------------------------: |
| public ObjectOutputStream(OutputStream out) | 把低级字节输出流包装成高级的对象字节输出流 |

**ObjectOutputStream序列化方法**

|                 方法名称                  |                 说明                 |
| :---------------------------------------: | :----------------------------------: |
| public final void writeObject(Object obj) | 把对象写出去到对象序列化流的文件中去 |



**序列化对象的要求：对象必须实现序列化接口: `public class 类名 implements Serializable`**

**注意：**

- **transient修饰的成员变量不参与序列化**

- **序列化的版本号与反序列化的版本号必须一致才不会出错！**

Student类：

```
/**
  对象如果要序列化，必须实现Serializable序列化接口。
 */
public class Student implements Serializable {
    // 申明序列化的版本号码
    // 序列化的版本号与反序列化的版本号必须一致才不会出错！
    private static final long serialVersionUID = 1;
    private String name;
    private String loginName;
    // transient修饰的成员变量不参与序列化了
    private transient String passWord;
    private int age ;

    public Student(){
    }

    public Student(String name, String loginName, String passWord, int age) {
        this.name = name;
        this.loginName = loginName;
        this.passWord = passWord;
        this.age = age;
    }

    //·····get/set方法

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", loginName='" + loginName + '\'' +
                ", passWord='" + passWord + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
public class ObjectOutputStreamDemo1 {
    public static void main(String[] args) throws Exception {
        // 1、创建学生对象
        Student s = new Student("陈磊", "chenlei","1314520", 21);

        // 2、对象序列化：使用对象字节输出流包装字节输出流管道
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("io-app2/src/obj.txt"));

        // 3、直接调用序列化方法
        oos.writeObject(s);

        // 4、释放资源
        oos.close();
        System.out.println("序列化完成了~~");

    }
}

```



### 3.2 对象反序列化

- 使用到的流是**对象字节输入流**：ObjectInputStream
- 作用：**以内存为基准，把存储到磁盘文件中去的对象数据恢复成内存中的对象**，称为对象反序列化。

|                   构造器                    |                    说明                    |
| :-----------------------------------------: | :----------------------------------------: |
| public ObjectIntputStream(IntputStream out) | 把低级字节输入流包装成高级的对象字节输入流 |

**ObjectIntputStream序列化方法**

|            方法名称            |                         说明                         |
| :----------------------------: | :--------------------------------------------------: |
| public final void readObject() | 把存储到磁盘文件中去的对象数据恢复成内存中的对象返回 |

```java
public class ObjectInputStreamDemo2 {
    public static void main(String[] args) throws Exception {
        // 1、创建对象字节输入流管道包装低级的字节输入流管道
        ObjectInputStream is = new ObjectInputStream(new FileInputStream("io-app2/src/obj.txt"));

        // 2、调用对象字节输入流的反序列化方法
        Student s = (Student) is.readObject();

        System.out.println(s);
    }
}
```



## 4. 打印流

- 作用：**打印流可以实现方便、高效的打印数据到文件中去**。打印流一般是指：PrintStream，PrintWriter两个类。
- 可以实现打印什么数据就是什么数据，例如打印整数97写出去就是97，打印boolean的true，写出去就是true。

## 4.1 PrintStream、PrintWriter

**PrintStream**

|               构造器                |             说明             |
| :---------------------------------: | :--------------------------: |
| public PrintStream(OutputStream os) | 打印流直接通向字节输出流管道 |
|     public PrintStream(File f)      |    打印流直接通向文件对象    |
| public PrintStream(String filepath) |    打印流直接通向文件路径    |

|           方法            |          说明          |
| :-----------------------: | :--------------------: |
| public void print(Xxx xx) | 打印任意类型的数据出去 |



**PrintWriter**

|                构造器                |             说明             |
| :----------------------------------: | :--------------------------: |
| public PrintWriter(OutputStream os)  | 打印流直接通向字节输出流管道 |
|    public PrintWriter (Writer w)     | 打印流直接通向字符输出流管道 |
|     public PrintWriter (File f)      |    打印流直接通向文件对象    |
| public PrintWriter (String filepath) |    打印流直接通向文件路径    |

|           方法            |          说明          |
| :-----------------------: | :--------------------: |
| public void print(Xxx xx) | 打印任意类型的数据出去 |



**PrintStream和PrintWriter的区别**

- 打印数据功能上是一模一样的，**都是使用方便，性能高效（核心优势）**
- PrintStream继承自字节输出流OutputStream，支持**写字节**。
- PrintWriter继承自字符输出流Writer，支持**写字符**

```java
public class PrintStreamDemo01 {
    public static void main(String[] args) throws Exception {
        //PrintStream ps = new PrintStream("JavaSE/src/ps.txt");
        
        //PrintStream流不支持直接以追加的方式写文件
        //可以使用低级流FileOutputStream来表示
        PrintStream ps = new PrintStream(new FileOutputStream("JavaSE/src/ps.txt", true));

        ps.print(97);
        ps.print('b');
        ps.println(1234.23);
        ps.println("我是打印流输出的，我是啥就打印啥！！！");
        ps.close();
        
        PrintWriter pw = new PrintWriter("JavaSE/src/ps2.txt");
        pw.println("sdfi");
        pw.println("我是");
        pw.println("中国人");
        pw.close();
    }
}
```



### 4.2 输出语句的重定向

- 属于打印流的一种应用，可以把输出语句的打印位置改到文件。

  ```
  PrintStream ps = new PrintStream("文件地址");
  System.setOut(ps);
  ```

  

```java
public class PrintDemo2 {
    public static void main(String[] args) throws Exception {
        System.out.println("锦瑟无端五十弦");
        System.out.println("一弦一柱思华年");		//流到控制台

        // 改变输出语句的位置（重定向）
        PrintStream ps = new PrintStream("io-app2/src/log.txt");
        System.setOut(ps); // 把系统打印流改成我们自己的打印流

        System.out.println("庄生晓梦迷蝴蝶");
        System.out.println("望帝春心托杜鹃");		//流到文件中
    }
}
```





## 5. 补充知识：Properties

Properties其实就是一个Map集合，但是我们一般不会当集合使用，因为HashMap更好用。

**Properties核心作用：**

- **Properties代表的是一个属性文件，可以把自己对象中的键值对信息存入到一个属性文件中去。**
- 属性文件：后缀是.properties结尾的文件,里面的内容都是 key=value，后续做系统配置信息的。

Properties的API:

|                       构造器                        |                             说明                             |
| :-------------------------------------------------: | :----------------------------------------------------------: |
|           void load(InputStream inStream)           |            从输入字节流读取属性列表（键和元素对）            |
|              void load(Reader reader)               |            从输入字符流读取属性列表（键和元素对）            |
|    void store(OutputStream out, String comments)    | 将此属性列表（键和元素对）写入此 Properties表中，以适合于使用 load(InputStream)方法的格式写入输出字节流 |
|     void store(Writer writer, String comments)      | 将此属性列表（键和元素对）写入此 Properties表中，以适合使用 load(Reader)方法的格式写入输出字符流 |
| public Object setProperty(String key, String value) |                      保存键值对（put）                       |
|        public String getProperty(String key)        |           使用此属性列表中指定的键搜索属性值 (get)           |
|      public Set<String> stringPropertyNames()       |                所有键的名称的集合 (keySet())                 |

```java
public class PropertiesDemo {
    public static void main(String[] args) throws Exception {
        Properties properties = new Properties();
        //setProperty内部其实就是put方法，用哪个加数据都可以
        properties.setProperty("admin", "123456");
        properties.setProperty("it", "123");
        properties.setProperty("run", "456");
        System.out.println(properties);     //{admin=123456, run=456, it=123}

        //将键值对写入到properties文件中
        //第二个参数是说明内容(用英文)
        properties.store(new FileWriter("JavaSE/src/users.properties"), "this is users!!");

        //将properties文件中的键值对读出来
        Properties properties2 = new Properties();
        properties2.load(new FileReader("JavaSE/src/users.properties"));
        System.out.println(properties2);    //{admin=123456, run=456, it=123}
        //根据键获取值
        String rs = properties2.getProperty("admin");
        System.out.println(rs);     //123456
        String rs2 = properties2.getProperty("run");
        System.out.println(rs2);    //456
    }
}
```

properties文件中的内容：users

```properties
#this is users!!
#Thu Jan 13 14:33:51 CST 2022
admin=123456
run=456
it=123
```



## 6. 补充知识：IO框架

**commons-io概述**

- commons-io是apache开源基金组织提供的一组有关IO操作的类库，可以提高IO功能开发的效率。
- commons-io工具包提供了很多有关io操作的类。有两个主要的类FileUtils, IOUtils

**FileUtils主要有如下方法:**

|                          方法名                          |             说明             |
| :------------------------------------------------------: | :--------------------------: |
|   String readFileToString(File file, String encoding)    | 读取文件中的数据, 返回字符串 |
|        void copyFile(File srcFile, File destFile)        |          复制文件。          |
| void copyDirectoryToDirectory(File srcDir, File destDir) |         复制文件夹。         |

步骤：

1. 下载commons-io相关jar包；http://commons.apache.org/proper/commons-io/
2. 把commons-io-2.6.jar包复制到指定的Module的lib目录中
3. 将commons-io-2.6.jar加入到classpath中，在jar文件上点右键，选择 Add as Library -> 点击OK
4. 在类中导包使用



```java
public class CommonsIODemo01 {
    public static void main(String[] args) throws Exception {

////        // 1.完成文件复制！
//        IOUtils.copy(new FileInputStream("D:\\resources\\hushui.jpeg"),
//                new FileOutputStream("D:\\resources\\hushui2.jpeg"));


////        // 2.完成文件复制到某个文件夹下！
//        FileUtils.copyFileToDirectory(new File("D:\\resources\\hushui.jpeg"), new File("D:/"));


          // 3.完成文件夹复制到某个文件夹下！
//          FileUtils.copyDirectoryToDirectory(new File("D:\\resources") , new File("D:\\new"));
//           FileUtils.deleteDirectory(new File("D:\\new"));

        
         // JDK1.7 自己也做了一些一行代码完成复制的操作:New IO的技术
         // Files.copy(Path.of("D:\\resources\\hushui.jpeg"), Path.of("D:\\resources\\hushui3.jpeg"));

        FileUtils.deleteDirectory(new File("D:\\new"));		//只能删除空文件夹
    }
}
```







# 多线程

**什么是线程？**

- 线程(thread)是一个程序内部的一条执行路径。
- 我们之前启动程序执行后，main方法的执行其实就是一条单独的执行路径。
- 程序中如果只有一条执行路径，那么这个程序就是单线程的程序。

**多线程是什么？**

- 多线程是指从软硬件上实现多条执行流程的技术。



## 1. 多线程的创建

### **方式一：继承Thread类**

1. 定义一个子类MyThread继承线程类java.lang.Thread，重写run()方法
2. 创建MyThread类的对象
3. 调用线程对象的start()方法启动线程（启动后还是执行run方法的）

**Thread类：**

- Java是通过java.lang.Thread 类来代表线程的。 
- 按照面向对象的思想，Thread类应该提供了实现多线程的方式。

**方式一优缺点：**

- 优点：编码简单
- 缺点：线程类已经继承Thread，无法继承其他类，不利于扩展。

**为什么不直接调用了run方法，而是调用start启动线程？**

- 直接调用run方法会当成普通方法执行，此时相当于还是单线程执行。
- 只有调用start方法才是启动一个新的线程执行。

**把主线程任务放在子线程之前跑会发生什么情况？**

- 这样主线程一直是先跑完的，相当于是一个单线程的效果了。

**示例：**

```java
public class ThreadDemo1 {
    public static void main(String[] args) {
        // 3、new一个新线程对象
        Thread t = new MyThread();
        // 4、调用start方法启动线程（执行的还是run方法）
        t.start();

        for (int i = 0; i < 5; i++) {
            System.out.println("主线程执行输出：" + i);
        }

    }
}

/**
   1、定义一个线程类继承Thread类
 */
class MyThread extends Thread{
    /**
       2、重写run方法，里面是定义线程以后要干啥
     */
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("子线程执行输出：" + i);
        }
    }
}
```



### 方式二：实现Runnable接口

1. 定义一个线程任务类MyRunnable实现Runnable接口，重写run()方法
2. 创建MyRunnable任务对象
3. 把MyRunnable任务对象交给Thread处理。
4. 调用线程对象的start()方法启动线程



**Thread的构造器**

|                    构造器                     |                     说明                     |
| :-------------------------------------------: | :------------------------------------------: |
|          public Thread(String name)           |            可以为当前线程指定名称            |
|        public Thread(Runnable target)         |         封装Runnable对象成为线程对象         |
| public Thread(Runnable target ，String name ) | 封装Runnable对象成为线程对象，并指定线程名称 |



**方式二优缺点：**

- 优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强。
- 缺点：编程多一层对象包装，如果线程有执行结果是不可以直接返回的。

**示例：**

```java
public class ThreadDemo2 {
    public static void main(String[] args) {
        // 3、创建一个任务对象
        Runnable target = new MyRunnable();
        // 4、把任务对象交给Thread处理
        Thread t = new Thread(target);
        // Thread t = new Thread(target, "1号");
        // 5、启动线程
        t.start();

        for (int i = 0; i < 10; i++) {
            System.out.println("主线程执行输出：" + i);
        }
    }
}

/**
   1、定义一个线程任务类 实现Runnable接口
 */
class MyRunnable  implements Runnable {
    /**
       2、重写run方法，定义线程的执行任务的
     */
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println("子线程执行输出：" + i);
        }
    }
}

```



**多线程的实现方案二的另一种写法：匿名内部类形式**

1. 可以创建Runnable的匿名内部类对象。
2. 交给Thread处理。
3. 调用线程对象的start()启动线程。

**示例：**

```java
public class ThreadDemo2Other {
    public static void main(String[] args) {
        Runnable target = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    System.out.println("子线程1执行输出：" + i);
                }
            }
        };
        Thread t = new Thread(target);
        t.start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    System.out.println("子线程2执行输出：" + i);
                }
            }
        }).start();

        //Lambda简化：
        new Thread(() -> {
                for (int i = 0; i < 10; i++) {
                    System.out.println("子线程3执行输出：" + i);
            }
        }).start();

        for (int i = 0; i < 10; i++) {
            System.out.println("主线程执行输出：" + i);
        }
    }
}
```



### 方式三：JDK 5.0新增：实现Callable接口

前2种线程创建方式都存在一个问题：

- 他们重写的run方法均不能直接返回结果。
- 不适合需要返回线程执行结果的业务场景。



**多线程的实现方案三：利用Callable、FutureTask接口实现:**

1. 得到任务对象
   1. 定义类实现Callable接口，重写call方法，封装要做的事情。
   2. 用FutureTask把Callable对象封装成线程任务对象。
2. 把线程任务对象交给Thread处理。
3. 调用Thread的start方法启动线程，执行任务
4. 线程执行完毕后、通过FutureTask的get方法去获取任务执行的结果。



**FutureTask的API**

|              方法名称              |                 说明                 |
| :--------------------------------: | :----------------------------------: |
| public FutureTask<>(Callable call) | 把Callable对象封装成FutureTask对象。 |
|  public V get() throws Exception   |   获取线程执行call方法返回的结果。   |

**方式三优缺点：**

- 优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强。
- 可以在线程执行完毕后去获取线程执行的结果。
- 缺点：编码复杂一点。



**示例：**

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        // 3、创建Callable任务对象
        Callable<String> call = new MyCallable(100);
        // 4、把Callable任务对象 交给 FutureTask 对象
        //  FutureTask对象的作用1： 是Runnable的对象（实现了Runnable接口），可以交给Thread了
        //  FutureTask对象的作用2： 可以在线程执行完毕之后通过调用其get方法得到线程执行完成的结果
        FutureTask<String> f1 = new FutureTask<>(call);
        // 5、交给线程处理
        Thread t1 = new Thread(f1);
        // 6、启动线程
        t1.start();


        Callable<String> call2 = new MyCallable(200);
        FutureTask<String> f2 = new FutureTask<>(call2);
        Thread t2 = new Thread(f2);
        t2.start();

        try {
            // 如果f1任务没有执行完毕，这里的代码会等待，直到线程1跑完才提取结果。
            String rs1 = f1.get();
            System.out.println("第一个结果：" + rs1);
        } catch (Exception e) {
            e.printStackTrace();
        }

        try {
            // 如果f2任务没有执行完毕，这里的代码会等待，直到线程2跑完才提取结果。
            String rs2 = f2.get();
            System.out.println("第二个结果：" + rs2);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

/**
    1、定义一个任务类 实现Callable接口  应该申明线程任务执行完毕后的结果的数据类型
 */
class MyCallable implements Callable<String>{
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }

    /**
       2、重写call方法（任务方法）
     */
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= n ; i++) {
            sum += i;
        }
        return "子线程执行的结果是：" + sum;
    }
}

```



### 三种方式对比

|       方式       |                             优点                             |                          缺点                          |
| :--------------: | :----------------------------------------------------------: | :----------------------------------------------------: |
|   继承Thread类   |          编程比较简单，可以直接使用Thread类中的方法          | 扩展性较差，不能再继承其他的类，不能返回线程执行的结果 |
| 实现Runnable接口 |        扩展性强，实现该接口的同时还可以继承其他的类。        |          编程相对复杂，不能返回线程执行的结果          |
| 实现Callable接口 | 扩展性强，实现该接口的同时还可以继承其他的类。可以得到线程执行的结果 |                      编程相对复杂                      |



## 2. Thread的常用方法

**Thread常用API说明**

- Thread常用方法：**获取线程名称getName()、设置名称setName()、获取当前线程对象currentThread()**。
- 至于Thread类提供的诸如：yield、join、interrupt、不推荐的方法 stop 、守护线程、线程优先级等线程的控制方法，在开发中很少使用，这些方法会在高级篇以及后续需要用到的时候再为大家讲解。

**Thread获取和设置线程名称**

|         方法名称          |                             说明                             |
| :-----------------------: | :----------------------------------------------------------: |
|     String getName()      |        获取当前线程的名称，默认线程名称是Thread-索引         |
| void setName(String name) | 将此线程的名称更改为指定的名称，通过构造器也可以设置线程名称 |



**Thread类获得当前线程的对象**

|                方法名称                |                说明                |
| :------------------------------------: | :--------------------------------: |
| public static Thread currentThread()： | 返回对当前正在执行的线程对象的引用 |

**注意**

1. 此方法是Thread类的静态方法，可以直接使用Thread类调用。
2. 这个方法是在哪个线程执行中调用的，就会得到哪个线程对象。

**Thread的构造器**

|                   方法名称                    |                     说明                     |
| :-------------------------------------------: | :------------------------------------------: |
|          public Thread(String name)           |            可以为当前线程指定名称            |
|        public Thread(Runnable target)         |         封装Runnable对象成为线程对象         |
| public Thread(Runnable target ，String name ) | 封装Runnable对象成为线程对象，并指定线程名称 |



**Thread类的线程休眠方法**

|              方法名称               |                        说明                        |
| :---------------------------------: | :------------------------------------------------: |
| public static void sleep(long time) | 让当前线程休眠指定的时间后再继续执行，单位为毫秒。 |





**总结：Thread常用方法、构造器**

|                方法名称                |                     说明                      |
| :------------------------------------: | :-------------------------------------------: |
|            String getName()            | 获取当前线程的名称，默认线程名称是Thread-索引 |
|       void setName(String name)        |                 设置线程名称                  |
| public static Thread currentThread()： |      返回对当前正在执行的线程对象的引用       |
|  public static void sleep(long time)   |      让线程休眠指定的时间，单位为毫秒。       |
|           public void run()            |                 线程任务方法                  |
|          public void start()           |                 线程启动方法                  |

|                    构造器                     |                    说明                    |
| :-------------------------------------------: | :----------------------------------------: |
|          public Thread(String name)           |           可以为当前线程指定名称           |
|        public Thread(Runnable target)         |         把Runnable对象交给线程对象         |
| public Thread(Runnable target ，String name ) | 把Runnable对象交给线程对象，并指定线程名称 |



**示例：**

MyThread类：

```java
public class MyThread extends Thread{
    public MyThread() {
    }

    public MyThread(String name) {
        // 为当前线程对象设置名称，送给父类的有参数构造器初始化名称
        super(name);
    }

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println( Thread.currentThread().getName() + "输出：" + i);
        }
    }
}
```

常用API：

```java
public class ThreadDemo01 {
    // main方法是由主线程负责调度的
    public static void main(String[] args) {
        Thread t1 = new MyThread("1号");
        // t1.setName("1号");
        t1.start();
        System.out.println(t1.getName());

        Thread t2 = new MyThread("2号");
        // t2.setName("2号");
        t2.start();
        System.out.println(t2.getName());

        // 哪个线程执行它，它就得到哪个线程对象（当前线程对象）
        // 主线程的名称就叫main
        Thread m = Thread.currentThread();
        System.out.println(m.getName());
        m.setName("最牛的线程");

        for (int i = 0; i < 5; i++) {
            System.out.println( m.getName() + "输出：" + i);
        }
    }
}
```

sleep方法：

```java
public class ThreadDemo02 {
    // main方法是由主线程负责调度的
    public static void main(String[] args) throws Exception {
        for (int i = 1; i <= 5; i++) {
            System.out.println("输出：" + i);
            if(i == 3){
                // 让当前线程进入休眠状态
                // 段子：项目经理让我加上这行代码，如果用户愿意交钱，我就注释掉。
                Thread.sleep(3000);		//休眠3秒后，再打印4、5
            }
        }
    }
}
```



## 3. 线程安全

**线程安全问题**

- ==多个线程同时操作同一个共享资源==的时候可能会出现业务安全问题，称为线程安全问题。



**线程安全问题案例模拟：**

案例：取钱业务

需求：小明和小红是一对夫妻，他们有一个共同的账户，余额是10万元，模拟2人同时去取钱10万。

分析：

1. 需要提供一个账户类，创建一个账户对象代表2个人的共享账户。
2. 需要定义一个线程类，线程类可以处理账户对象。
3. 创建2个线程对象，传入同一个账户对象。
4. 启动2个线程，去同一个账户对象中取钱10万。



Account类：

```java
@Setter
@Getter
@AllArgsConstructor
public class Account {
    private String cardId;
    private double money; // 账户的余额

    public Account(){

    }
    
    /**
       小明 小红
     */
    public void drawMoney(double money) {
        // 0、先获取是谁来取钱，线程的名字就是人名
        String name = Thread.currentThread().getName();
        // 1、判断账户是否够钱
        if(this.money >= money){
            // 2、取钱
            System.out.println(name + "来取钱成功，吐出：" + money);
            // 3、更新余额
            this.money -= money;
            System.out.println(name + "取钱后剩余：" + this.money);
        }else {
            // 4、余额不足
            System.out.println(name +"来取钱，余额不足！");
        }
    }
}
```

取钱线程类：

```java
public class DrawThread extends Thread {
    // 接收处理的账户对象
    private Account acc;
    public DrawThread(Account acc,String name){
        super(name);		//给线程起名
        this.acc = acc;
    }
    @Override
    public void run() {
        // 小明 小红：取钱
        acc.drawMoney(100000);
    }
}
```

```java
public class ThreadDemo {
    public static void main(String[] args) {
        // 1、定义线程类，创建一个共享的账户对象
        Account acc = new Account("ICBC-111", 100000);

        // 2、创建2个线程对象，代表小明和小红同时进来了。
        new DrawThread(acc, "小明").start();
        new DrawThread(acc, "小红").start();
    }
}
```



**出现的问题：**

- 小明小红都能把钱取成功，因为在判断账户是否够钱的时候，前一个取钱的线程对象虽然已经取钱成功了，但还没有更新余额，后一个对象就已经到判断这里了，导致都判断为够钱，以至出现问题。



## 4. 线程同步

- 为了解决线程的安全问题。



取钱案例出现问题的原因：**多个线程同时执行，发现账户都是够钱的**

解决方法：**让多个线程实现先后依次访问共享资源**



**线程同步的核心思想：**

- ==加锁==，把共享资源进行上锁，每次只能一个线程进入访问完毕以后解锁，然后其他线程才能进来。



### 方式一：同步代码块

- 作用：把出现线程安全问题的核心代码给上锁。

- 原理：每次只能一个线程进入，执行完毕后自动解锁，其他线程才可以进来执行。

  ```java
  synchronized(同步锁对象) {
  	操作共享资源的代码(核心代码)
  }
  ```

**锁对象要求**

- 理论上：锁对象只要对于当前同时执行的线程来说是同一个对象即可。

**锁对象用任意唯一的对象好不好呢?**

- 不好，会影响其他无关线程的执行。



**锁对象的规范要求：**

- 规范上：==建议使用共享资源作为锁对象==。
- 对于**实例方法**建议使用==this==作为锁对象。
- 对于**静态方法**建议使用==字节码（类名.class）==对象作为锁对象。

**示例：**

将上面案例Account类中的`public void drawMoney(double money) `方法进行改进：

```java
public void drawMoney(double money) {
        // 1、拿到是谁来取钱
        String name = Thread.currentThread().getName();
        // 同步代码块
        // 小明 小红
    
        // this == acc 共享账户
        synchronized (this) {
            // 2、判断余额是否足够
            if(this.money >= money){
                // 钱够了
                System.out.println(name+"来取钱，吐出：" + money);
                // 更新余额
                this.money -= money;
                System.out.println(name+"取钱后，余额剩余：" + this.money);
            }else{
                // 3、余额不足
                System.out.println(name+"来取钱，余额不足！");
            }
        }
    }
```

**此时，只有账户相同的取钱对象才会有一把共同的锁，不会影响其他账户。**



### 方式二：同步方法

格式：

```java
修饰符 synchronized 返回值类型 方法名称(形参列表) {
	操作共享资源的代码
}
```

**示例：将上面案例的drawMoney方法改为：`public synchronized void drawMoney(double money)`即可**



**同步方法底层原理**

- 同步方法其实底层也是有隐式锁对象的，只是锁的范围是整个方法代码。
- 如果方法是实例方法：同步方法**默认**用this作为的锁对象。但是代码要高度面向对象！
- 如果方法是静态方法：同步方法**默认**用类名.class作为的锁对象。

**总结：**

==同步代码块锁的范围更小，同步方法锁的范围更大。但是同步方法更加方便，使用的更多。==

### 方式三：Lock锁

- 为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock，更加灵活、方便。
- Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作。
- **Lock是接口不能直接实例化**，这里采用它的**实现类ReentrantLock来构建Lock锁对象**。

|        方法名称        |          说明          |
| :--------------------: | :--------------------: |
| public ReentrantLock() | 获得Lock锁的实现类对象 |

**Lock的API**

|   方法名称    |  说明  |
| :-----------: | :----: |
|  void lock()  | 获得锁 |
| void unlock() | 释放锁 |



**示例：改进上面案例的代码**

```java
@Setter
@Getter
@AllArgsConstructor
public class Account {
    private String cardId;
    private double money; // 余额 关键信息

    // final修饰后：锁对象是唯一和不可替换的，非常专业
    private final Lock lock = new ReentrantLock();

    public Account() {
    }

    /**
      小明 小红
     */
    public void drawMoney(double money) {
        // 1、拿到是谁来取钱
        String name = Thread.currentThread().getName();
        // 2、判断余额是否足够
        // 小明  小红
        
        lock.lock(); // 上锁
        try {
            if(this.money >= money){
                // 钱够了
                System.out.println(name+"来取钱，吐出：" + money);
                // 更新余额
                this.money -= money;
                System.out.println(name+"取钱后，余额剩余：" + this.money);
            }else{
                // 3、余额不足
                System.out.println(name+"来取钱，余额不足！");
            }
        } finally {
            lock.unlock(); // 解锁，放在finally里面，是担心在try时出异常，导致解不了锁。
        }
    }
}
```





## 5. 线程通信 [了解]

**什么是线程通信、如何实现？**

- 所谓线程通信就是线程间相互发送数据，线程间共享一个资源即可实现线程通信。

**线程通信常见形式**

- 通过共享一个数据的方式实现。
- 根据共享数据的情况决定自己该怎么做，以及通知其他线程怎么做。

**线程通信实际应用场景**

- 生产者与消费者模型：生产者线程负责生产数据，消费者线程负责消费生产者产生的数据。
- 要求：生产者线程生产完数据后唤醒消费者，然后等待自己，消费者消费完该数据后唤醒生产者，然后等待自己。

**线程通信案例模拟**

- 模拟客服系统，系统可以不断的接入电话 和 分发给客服。

  ![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0e44c8b309be543a76f13325cd0ebc24.png)

**线程通信的前提**：线程通信通常是在多个线程操作同一个共享资源的时候需要进行通信，且要保证线程安全。

**Object类的等待和唤醒方法：**

|     方法名称     |                             说明                             |
| :--------------: | :----------------------------------------------------------: |
|   void wait()    | 让当前线程等待并释放所占锁，直到另一个线程调用notify()方法或 notifyAll()方法 |
|  void notify()   |                    唤醒正在等待的单个线程                    |
| void notifyAll() |                    唤醒正在等待的所有线程                    |

**注意：**上述方法应该使用当前同步锁对象进行调用。

**示例：**

```java
public class Phone {
    //实现线程间通信问题，默认认为手机当前处于等待来电提醒
    private boolean flag = false;

    public void run(){
        //a、负责来电提醒的线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    while (true) {
                        //线程同步处理，使用当前手机对象
                        synchronized (Phone.this) {
                            if(flag) {
                                //此时应该是接电话
                            }else {
                                //代表要来电提醒了
                                System.out.println("您好，有新电话呼入，请接听~~~");
                                Thread.sleep(2000);
                                flag = true;
                                Phone.this.notify();        //唤醒正在等待的线程
                                Phone.this.wait();          //自己进入等待状态
                            }
                        }
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();

        //b、接电话线程，正式接听了
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    //不断的接听电话
                    while (true) {
                        //线程同步处理，使用当前手机对象
                        synchronized (Phone.this) {
                            if(flag) {
                                //可以接听电话了
                                System.out.println("电话接听中~~~");
                                System.out.println("通话结束了~~~");
                                Thread.sleep(4000);
                                flag = false;        //代表要继续等待电话呼入
                                Phone.this.notify();        //唤醒正在等待的线程
                                Phone.this.wait();          //自己进入等待状态
                            }else {
                                //还没有来电提醒，自己继续等待，唤醒别人
                                Phone.this.notify();
                                Phone.this.wait();
                            }
                        }
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }

    public static void main(String[] args) {
        //1、创建一部手机对象
        Phone huawei = new Phone();
        huawei.run();
    }
}
```





## 6. 线程池 [重点]

### 6.1 线程池概述

- 线程池就是一个可以==复用线程==的技术。

**不使用线程池的问题** 

- 如果用户每发起一个请求，后台就创建一个新线程来处理，下次新任务来了又要创建新线程，而**创建新线程的开销是很大的**，这样会严重影响系统的性能。

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d5613e4020fba0aae337ed2dcec54a1d.png)



### 6.2 线程池实现的API、参数说明

- JDK 5.0起提供了代表线程池的接口：ExecutorService

**如何得到线程池对象**

1. 使用ExecutorService的实现类ThreadPoolExecutor自创建一个线程池对象

   `ExecutorService` ----> `ThreadPoolExecutor`

2. 使用Executors（线程池的工具类）调用方法返回不同特点的线程池对象



**ThreadPoolExecutor**构造器的参数说明：

```java
public ThreadPoolExecutor(int corePoolSize,
					    int maximumPoolSize,
					    long keepAliveTime,
					    TimeUnit unit,
					    BlockingQueue<Runnable> workQueue,
					    ThreadFactory threadFactory,
					    RejectedExecutionHandler handler) 
```

参数一：指定线程池的线程数量（核心线程）： corePoolSize     ·················    不能小于0

参数二：指定线程池可支持的最大线程数： maximumPoolSize	·················    最大数量>=核心线程数量

参数三：指定临时线程的最大存活时间： keepAliveTime			  ·················    不能小于0

参数四：指定存活时间的单位(秒、分、时、天)： unit				  ··················    时间单位

参数五：指定任务队列： workQueue											··················    不能为null

参数六：指定用哪个线程工厂创建线程： threadFactory			   ·················     不能为null

参数七：指定线程忙，任务满的时候，新任务来了怎么办： handler	···········     不能为null





**线程池常见面试题：**

1. **临时线程什么时候创建啊？**
   - **新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程。**
2. **什么时候会开始拒绝任务？**
   - **核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始任务拒绝。**





### 6.3 线程池处理Runnable任务

**ThreadPoolExecutor创建线程池对象示例**

```java
ExecutorService pools = new ThreadPoolExecutor(3, 5
			, 8 , TimeUnit.SECONDS, new ArrayBlockingQueue<>(6),
			Executors.defaultThreadFactory() , new ThreadPoolExecutor.AbortPolicy());
```

**ExecutorService的常用方法**

|              方法名称              |                             说明                             |
| :--------------------------------: | :----------------------------------------------------------: |
|   void execute(Runnable command)   |  执行任务/命令，没有返回值，一般用来**执行 Runnable 任务**   |
| Future<T> submit(Callable<T> task) | 执行任务，返回未来任务对象获取线程结果，**一般拿来执行 Callable 任务** |
|          void shutdown()           |                  等任务执行完毕后关闭线程池                  |
|    List<Runnable> shutdownNow()    |    立刻关闭，停止正在执行的任务，并返回队列中未执行的任务    |

**新任务拒绝策略**

|                  策略                  |                             详解                             |
| :------------------------------------: | :----------------------------------------------------------: |
|     ThreadPoolExecutor.AbortPolicy     | 丢弃任务并抛出RejectedExecutionException异常。**是默认的策略** |
|   ThreadPoolExecutor.DiscardPolicy：   |          丢弃任务，但是不抛出异常 这是不推荐的做法           |
| ThreadPoolExecutor.DiscardOldestPolicy |      抛弃队列中等待最久的任务 然后把当前任务加入队列中       |
|  ThreadPoolExecutor.CallerRunsPolicy   |    由主线程负责调用任务的run()方法从而绕过线程池直接执行     |

**示例：**



```java
public class ThreadPoolDemo1 {
    public static void main(String[] args) {
        /*
        public ThreadPoolExecutor(int corePoolSize,
        int maximumPoolSize,
        long keepAliveTime,
        TimeUnit unit,
        BlockingQueue<Runnable> workQueue,
        ThreadFactory threadFactory,
        RejectedExecutionHandler handler)
        */
        //1、创建线程池对象
        ExecutorService pool = new ThreadPoolExecutor(3, 5,
                6, TimeUnit.SECONDS, new ArrayBlockingQueue<>(5),
                Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy() );

        //2、把任务给线程池处理
        Runnable target = new MyRunnable();
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);

        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        pool.execute(target);
        // ^此时还是不创建临时线程，因为3个核心线程处理前三个任务，而队列最多可以排5个，相当于3位客人已经进屋了，外面刚好还有5个位置，等3个核心线程忙完了就来接待。

        //此时最大的线程数不足，开始创建临时线程来处理任务。
        pool.execute(target);       //创建1个临时进程
        pool.execute(target);       //再创建1个临时进程（此时最大的线程数已满）

        //pool.execute(target);       //不创建临时线程，不会再执行此任务了，根据参数7的策略来处理该新任务

        //关闭线程池（开发中一般不会使用）
        //pool.shutdownNow();    //立即关闭，即使任务没有执行完（会丢失任务）
        pool.shutdown();         //等全部任务执行完毕之后再关闭
    }
}
```

输出：

```
pool-1-thread-3输出了：HelloWorld ==>0
pool-1-thread-4输出了：HelloWorld ==>0
pool-1-thread-1输出了：HelloWorld ==>0
pool-1-thread-5输出了：HelloWorld ==>0
pool-1-thread-2输出了：HelloWorld ==>0
pool-1-thread-5输出了：HelloWorld ==>1
pool-1-thread-1输出了：HelloWorld ==>1
pool-1-thread-4输出了：HelloWorld ==>1
pool-1-thread-3输出了：HelloWorld ==>1
pool-1-thread-4输出了：HelloWorld ==>2
pool-1-thread-1输出了：HelloWorld ==>2
pool-1-thread-5输出了：HelloWorld ==>2
pool-1-thread-2输出了：HelloWorld ==>1
pool-1-thread-5输出了：HelloWorld ==>3
pool-1-thread-1输出了：HelloWorld ==>3
pool-1-thread-4输出了：HelloWorld ==>3
pool-1-thread-3输出了：HelloWorld ==>2
pool-1-thread-4输出了：HelloWorld ==>4
pool-1-thread-1输出了：HelloWorld ==>4
pool-1-thread-5输出了：HelloWorld ==>4
pool-1-thread-2输出了：HelloWorld ==>2
pool-1-thread-2输出了：HelloWorld ==>3
pool-1-thread-3输出了：HelloWorld ==>3
pool-1-thread-2输出了：HelloWorld ==>4
pool-1-thread-4输出了：HelloWorld ==>0
pool-1-thread-5输出了：HelloWorld ==>0
pool-1-thread-4输出了：HelloWorld ==>1
pool-1-thread-2输出了：HelloWorld ==>0
pool-1-thread-3输出了：HelloWorld ==>4
pool-1-thread-2输出了：HelloWorld ==>1
pool-1-thread-4输出了：HelloWorld ==>2
pool-1-thread-5输出了：HelloWorld ==>1
pool-1-thread-4输出了：HelloWorld ==>3
pool-1-thread-2输出了：HelloWorld ==>2
pool-1-thread-3输出了：HelloWorld ==>0
pool-1-thread-1输出了：HelloWorld ==>0
pool-1-thread-3输出了：HelloWorld ==>1
pool-1-thread-2输出了：HelloWorld ==>3
pool-1-thread-4输出了：HelloWorld ==>4
pool-1-thread-5输出了：HelloWorld ==>2
pool-1-thread-2输出了：HelloWorld ==>4
pool-1-thread-3输出了：HelloWorld ==>2
pool-1-thread-1输出了：HelloWorld ==>1
pool-1-thread-3输出了：HelloWorld ==>3
pool-1-thread-5输出了：HelloWorld ==>3
pool-1-thread-3输出了：HelloWorld ==>4
pool-1-thread-1输出了：HelloWorld ==>2
pool-1-thread-5输出了：HelloWorld ==>4
pool-1-thread-1输出了：HelloWorld ==>3
pool-1-thread-1输出了：HelloWorld ==>4

Process finished with exit code 0
```



### 6.4 线程池处理Callable任务

- 使用ExecutorService的方法：
- `Future<T> submit(Callable<T> command)`

**示例：**

MyCallable类

```Java
//定义一个人物类，实现Callable接口，应该声明线程任务执行完后结果的数据类型
class MyCallable implements Callable<String>{
    private int n;
    public MyCallable(int n){
        this.n = n;
    }

    //重写call方法（此处示例为求某数的前n项和）
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 0; i <= n ; i++) {
            sum +=i;
        }
        return Thread.currentThread().getName() + "执行 1+" +n + "的结果是：" + sum;
    }
}
```

```java
public class ThreadPoolDemo2 {
    public static void main(String[] args) throws Exception {
        // 1、创建线程池对象
        /**
         public ThreadPoolExecutor(int corePoolSize,
         int maximumPoolSize,
         long keepAliveTime,
         TimeUnit unit,
         BlockingQueue<Runnable> workQueue,
         ThreadFactory threadFactory,
         RejectedExecutionHandler handler)
         */
        ExecutorService pool = new ThreadPoolExecutor(3, 5 ,
                6, TimeUnit.SECONDS, new ArrayBlockingQueue<>(5) , Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy() );

        // 2、给任务线程池处理。
        Future<String> f1 = pool.submit(new MyCallable(100));
        Future<String> f2 = pool.submit(new MyCallable(200));
        Future<String> f3 = pool.submit(new MyCallable(300));
        Future<String> f4 = pool.submit(new MyCallable(400));
        Future<String> f5 = pool.submit(new MyCallable(500));

//        String rs = f1.get();
//        System.out.println(rs);

        System.out.println(f1.get());
        System.out.println(f2.get());
        System.out.println(f3.get());
        System.out.println(f4.get());
        System.out.println(f5.get());

    }
}
```

输出：

```
pool-1-thread-1执行 1+100的结果是：5050
pool-1-thread-2执行 1+200的结果是：20100
pool-1-thread-3执行 1+300的结果是：45150
pool-1-thread-3执行 1+400的结果是：80200
pool-1-thread-2执行 1+500的结果是：125250
```



### 6.5 Executors工具类实现线程池

**Executors得到线程池对象的常用方法**

- Executors：线程池的工具类通过调用方法返回不同类型的线程池对象。 

|                           方法名称                           |                             说明                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newCachedThreadPool() | 线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了一段时间则会被回收掉。 |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newFixedThreadPool(int nThreads) | 创建固定线程数量的线程池，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程替代它。 |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newSingleThreadExecutor () | 创建只有一个线程的线程池对象，如果该线程出现异常而结束，那么线程池会补充一个新线程。 |
| public static [ScheduledExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ScheduledExecutorService.html) newScheduledThreadPool(int corePoolSize) | 创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务。 |

==注意：Executors的底层其实也是基于线程池的实现类ThreadPoolExecutor创建线程池对象的。==

**示例：**

MyRunnable类使用6.3的示例



```java
public class ThreadPoolDemo3 {
    public static void main(String[] args) throws Exception {
        // 1、创建固定线程数据的线程池
        ExecutorService pool = Executors.newFixedThreadPool(3);

        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable());
        pool.execute(new MyRunnable()); // 已经没有多余线程了，不再处理此任务
    }
}
```



**Executors使用可能存在的陷阱**

- 大型并发系统环境中使用Executors如果不注意可能会出现系统风险。

| 方法名称                                                     | 存在问题                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newFixedThreadPool(int nThreads) | 允许请求的任务队列长度是Integer.MAX_VALUE，可能出现OOM错误（ java.lang.OutOfMemoryError ） |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newSingleThreadExecutor() | 允许请求的任务队列长度是Integer.MAX_VALUE，可能出现OOM错误（ java.lang.OutOfMemoryError ） |
| public static [ExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ExecutorService.html) newCachedThreadPool() | 创建的线程数量最大上限是Integer.MAX_VALUE，线程数可能会随着任务1:1增长，也可能出现OOM错误（ java.lang.OutOfMemoryError ） |
| public static [ScheduledExecutorService](mk:@MSITStore:D:\course\jdk-11中文api修订版.CHM::/java.base/java/util/concurrent/ScheduledExecutorService.html) newScheduledThreadPool(int corePoolSize) | 创建的线程数量最大上限是Integer.MAX_VALUE，线程数可能会随着任务1:1增长，也可能出现OOM错误（ java.lang.OutOfMemoryError ） |



![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5399b5ce260e990db30f26f84c45f81a.png)

- Executors是否适合做大型互联网场景的线程池方案？
  - 不合适。建议使用ThreadPoolExecutor来指定线程池参数，这样可以明确线程池的运行规则，规避资源耗尽的风险。



### 补充知识：定时器

- 定时器是一种控制任务**延时调用**，或者**周期调用**的技术。
- **作用：闹钟、定时邮件发送。**

**定时器的实现方式**

1. 方式一：Timer
2. 方式二： ScheduledExecutorService



#### **Timer定时器**

|     构造器     |        说明         |
| :------------: | :-----------------: |
| public Timer() | 创建Timer定时器对象 |

|                             方法                             |                   说明                    |
| :----------------------------------------------------------: | :---------------------------------------: |
| public void schedule([TimerTask](file:///D:/course/%E5%9F%BA%E7%A1%80%E9%98%B6%E6%AE%B5/API%E6%96%87%E6%A1%A3/docs/api/java.base/java/util/TimerTask.html) task, long delay, long period) | 开启一个定时器，按照计划处理TimerTask任务 |

**Timer定时器的特点和存在的问题**

1. Timer是单线程，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入。
2. 可能因为其中的某个任务的异常使Timer线程死掉，从而影响后续任务执行。

**示例：**

```java
public class TimerDemo1 {
    public static void main(String[] args) {
        // 1、创建Timer定时器
        Timer timer = new Timer();  // 定时器本身就是一个单线程。
        // 2、调用方法，处理定时任务
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行AAA~~~" + new Date());
//                try {
//                    Thread.sleep(5000);
//                } catch (InterruptedException e) {
//                    e.printStackTrace();
//                }
            }
        }, 1000, 2000);		//延迟1秒，每隔2秒执行一次

        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行BBB~~~"+ new Date());
                System.out.println(10/0);		//让B线程挂掉，整个定时器都出现异常
            }
        }, 0, 2000);			//B任务要等A任务睡眠5秒后才执行

        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行CCC~~~"+ new Date());
            }
        }, 0, 3000);
    }
}
```



#### **ScheduledExecutorService定时器**（推荐使用）

- ScheduledExecutorService是 jdk1.5中引入了并发包，目的是为了弥补Timer的缺陷, ScheduledExecutorService内部为线程池。

|                       Executors的方法                        |      说明      |
| :----------------------------------------------------------: | :------------: |
| public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) | 得到线程池对象 |

|                ScheduledExecutorService的方法                |     说明     |
| :----------------------------------------------------------: | :----------: |
| public ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit) | 周期调度方法 |

**ScheduledExecutorService的优点**

- 基于线程池，某个任务的执行情况**不会影响其他定时任务的执行**。

**示例：**

```java
public class TimerDemo2 {
    public static void main(String[] args) {
        // 1、创建ScheduledExecutorService线程池，做定时器
        ScheduledExecutorService pool = Executors.newScheduledThreadPool(3);

        // 2、开启定时任务
        pool.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行输出：AAA  ==》 " + new Date());
                try {
                    Thread.sleep(100000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, 0, 2, TimeUnit.SECONDS);	//每隔2秒执行一次


        pool.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行输出：BBB  ==》 " + new Date());
                System.out.println(10 / 0);		//此线程挂了，不影响其他线程
            }
        }, 0, 2, TimeUnit.SECONDS);


        pool.scheduleAtFixedRate(new TimerTask() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行输出：CCC  ==》 " + new Date());
            }
        }, 0, 2, TimeUnit.SECONDS);

    }
}
```





### 补充知识：并发、并行

**并发与并行**

- 正在运行的程序（软件）就是一个独立的进程， 线程是属于进程的，多个线程其实是并发与并行同时进行的。



**并发的理解：**

- CPU同时处理线程的数量有限。
  CPU会轮询为系统的每个线程服务，由于CPU切换的速度很快，给我们的感觉这些线程在同时执行，这就是并发。

**并行的理解：**

- 在同一个时刻上，同时有多个线程在被CPU处理并执行。



### 补充知识：线程的生命周期

**线程的状态**

- 线程的状态：也就是线程从生到死的过程，以及中间经历的各种状态及状态转换。
- 理解线程的状态有利于提升并发编程的理解能力。

Java**线程的状态**

- Java总共定义了6种状态。
- 6种状态都定义在Thread类的内部枚举类中。

```java
public class Thread{
     ...
     public enum State {
             NEW,
             RUNNABLE,
             BLOCKED,
             WAITING,
             TIMED_WAITING,
             TERMINATED;
     }
     ...
}
```

|        线程状态         |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|        NEW(新建)        |                 线程刚被创建，但是并未启动。                 |
|    Runnable(可运行)     |               线程已经调用了start()等待CPU调度               |
|     Blocked(锁阻塞)     | 线程在执行的时候未竞争到锁对象，则该线程进入Blocked状态；。  |
|    Waiting(无限等待)    | 一个线程进入Waiting状态，另一个线程调用notify或者notifyAll方法才能够唤醒 |
| Timed Waiting(计时等待) | 同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。带有超时参数的常用方法有Thread.sleep 、Object.wait。 |
|    Teminated(被终止)    | 因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。 |

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/adc75ae302a00116363c577c60751cbe.png)







# 网络编程

- 网络编程可以让程序与网络上的其他设备中的程序进行数据交互。

**网络通信基本模式**

- 常见的通信模式有如下2种形式：Client-Server(CS) 、 Browser/Server(BS)
  - Client客户端：
    - 需要程序员开发实现。
    - 用户需要安装客户端。
  - Browser浏览器：
    - 不需要程序员开发实现。
    - 用户需要安装浏览器。



## 1. 网络通信三要素

**实现网络编程关键的三要素**

- ==IP地址==：设备在网络中的地址，是唯一的标识。
- ==端口==：应用程序在设备中唯一的标识。
- ==协议==:   数据在网络中传输的规则，常见的协议有UDP协议和TCP协议。



### 要素一：IP地址

- IP（Internet Protocol）：全称”互联网协议地址”，是分配给上网设备的唯一标志。
- 常见的IP分类为：IPv4和IPv6
  - IPv4：
    - 32bit(4字节) ---> 110000000 10101000 00000001 01000010 --> 192.168.1.66(点分十进制表示法)
  - IPv6：
    - 128位（16个字节），号称可以为地球每一粒沙子编号。
    - IPv6分成8个整数，每个整数用四个十六进制位表示， 数之间用冒号（：）分开。
    - ABCD:EF01:2345:6789:ABCD:EF01:2345:6789（冒分十六进制表示法）



**IP地址形式：**

- 公网地址、和私有地址(局域网使用)。
- 192.168. 开头的就是常见的局域网地址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用。 

**IP常用命令：**

- ipconfig：查看本机IP地址
- ping IP地址(or域名)：检查网络是否连通

**特殊IP地址：**

- 本机IP: 127.0.0.1(IP地址)或者localhost(域名)：称为回送地址也可称本地回环地址，只会寻找当前所在本机。



### IP地址操作类-InetAddress

**InetAddress的使用**

- 此类表示Internet协议（IP）地址。

**InetAddressAPI如下**

|                       名称                       |                       说明                       |
| :----------------------------------------------: | :----------------------------------------------: |
|     public static InetAddress getLocalHost()     |               返回本主机的地址对象               |
| public static InetAddress getByName(String host) |  得到指定主机的IP地址对象，参数是域名或者IP地址  |
|           public String getHostName()            |               获取此IP地址的主机名               |
|          public String getHostAddress()          |                 返回IP地址字符串                 |
|     public boolean isReachable(int timeout)      | 在指定毫秒内连通该IP地址对应的主机，连通返回true |

```java
public class InetAddressDemo1 {
    public static void main(String[] args) throws Exception {
        //1、获取本机地址对象
        InetAddress ip1 = InetAddress.getLocalHost();
        System.out.println(ip1.getHostName());      //MateBook-14
        System.out.println(ip1.getHostAddress());       //192.168.1.106

        //2、获取域名ip对象
        InetAddress ip2 = InetAddress.getByName("www.baidu.com");
        System.out.println(ip2.getHostName());      //www.baidu.com
        System.out.println(ip2.getHostAddress());       //183.232.231.174

        //3、获取公网ip对象
        InetAddress ip3 = InetAddress.getByName("112.80.248.174");
        System.out.println(ip3.getHostName());
        System.out.println(ip3.getHostAddress());

        //4、判断是否能通：ping 5s之前测试是否可连通
        System.out.println(ip3.isReachable(5000));      //true
    }
}
```





### 要素二：端口

**端口号**

- **标识正在计算机设备上运行的进程（程序）**，被规定为一个 16 位的二进制，范围是 0~65535。

**端口类型**

- 周知端口：0~1023，被预先定义的知名应用占用（如：HTTP占用 80，FTP占用21） 
- 注册端口：1024~49151，分配给用户进程或某些应用程序。（如：Tomcat占 用8080，MySQL占用3306）
- 动态端口：49152到65535，之所以称为动态端口，是因为它 一般不固定分配某种进程，而是动态分配。

**注意：我们自己开发的程序选择注册端口，且一个设备中不能出现两个程序的端口号一样，否则出错。**



### 要素三：协议

**通信协议**

- 连接和通信数据的规则被称为网络通信协议

**网络通信协议有两套参考模型**

- OSI参考模型：世界互联协议标准，全球通信规范，由于此模型过于理想化，未能在因特网上进行广泛推广。 
- TCP/IP参考模型(或TCP/IP协议)：事实上的国际标准。

| OSI参考模型 | TCP/IP参考模型  |       各层对应        |                         面向操作                         |
| :---------: | :-------------: | :-------------------: | :------------------------------------------------------: |
|   应用层    |     应用层      | HTTP、FTP、DNS、SMTP… | 应用程序需要关注的：浏览器，邮箱。程序员一般在这一层开发 |
|   表示层    |     应用层      | HTTP、FTP、DNS、SMTP… | 应用程序需要关注的：浏览器，邮箱。程序员一般在这一层开发 |
|   会话层    |     应用层      | HTTP、FTP、DNS、SMTP… | 应用程序需要关注的：浏览器，邮箱。程序员一般在这一层开发 |
|   传输层    |     传输层      |     **TCP、UDP**…     |                 选择使用的TCP , UDP协议                  |
|   网络层    |     网络层      |       IP、ICMP…       |               封装源和目标IP，进行路径选择               |
| 数据链路层  | 数据链路层+物理 |   物理寻址、比特流…   |                      物理设备中传输                      |
|   物理层    | 数据链路层+物理 |   物理寻址、比特流…   |                      物理设备中传输                      |

**传输层的2个常见协议**

- TCP(Transmission Control Protocol) ：传输控制协议
- UDP(User Datagram Protocol)：用户数据报协议



#### TCP协议

**TCP协议特点**

- 使用TCP协议，必须双方先建立连接，它是一种**面向连接**的**可靠通信**协议。
- 传输前，采用“**三次握手**”方式建立连接，所以是可靠的 。
- 在连接中可进行大数据量的传输 。
- 连接、发送数据都需要确认，且传输完毕后，还需释放已建立的连接，通信效率较低。 

**TCP协议通信场景**

- 对信息安全要求较高的场景，例如：文件下载、金融等数据通信。

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/23c8e22ddc91ca7e0eab2da2a9beba2f.png)

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/57baf0f237f46d70ca21bda2f1800dc0.png)



#### UDP协议

- UDP是一种**无连接、不可靠传输**的协议。
- 将数据源IP、目的地IP和端口封装成数据包，不需要建立连接 
- 每个数据包的大小限制在64KB内 
- 发送不管对方是否准备好，接收方收到也不确认，故是不可靠的 
- 可以广播发送 ，发送数据结束时无需释放资源，开销小，速度快。

**UDP协议通信场景**

- 语音通话，视频会话等。



## 2. UDP通信

### 2.1 UDP通信：快速入门

**UDP协议的特点**

- UDP是一种**无连接、不可靠传输**的协议。
- 将数据源IP、目的地IP和端口以及数据封装成数据包，大小限制在64KB内，直接发送出去即可。

**DatagramPacket：数据包对象（韭菜盘子）**

| 构造器                                                       | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public DatagramPacket(byte[] buf, int length, InetAddress address, int port) | 创建发送端数据包对象<br/>buf：要发送的内容，字节<br/>length：要发送内容的字节<br/>address：接收端的IP地址对<br/>port：接收端的端口号 |
| public DatagramPacket(byte[] buf, int length)                | 创建接收端的数据包对象<br/>buf：用来存储接收的内容<br/>length：能够接收内容的长度 |

**DatagramPacket常用方法**

|          方法          |           说明           |
| :--------------------: | :----------------------: |
| public int getLength() | 获得实际接收到的字节个数 |



**DatagramSocket：发送端和接收端对象（人）**

|             构造器              |                        说明                        |
| :-----------------------------: | :------------------------------------------------: |
|     public DatagramSocket()     | 创建发送端的Socket对象，系统会随机分配一个端口号。 |
| public DatagramSocket(int port) |         创建接收端的Socket对象并指定端口号         |

**DatagramSocket类成员方法**

|                 方法                  |    说明    |
| :-----------------------------------: | :--------: |
|  public void send(DatagramPacket dp)  | 发送数据包 |
| public void receive(DatagramPacket p) | 接收数据包 |



**客户端实现步骤**

1. 创建DatagramSocket对象（发送端对象）                      扔韭菜的人
2. 创建DatagramPacket对象封装需要发送的数据（数据包对象）                 韭菜盘子
3. 使用DatagramSocket对象的send方法传入DatagramPacket对象                 开始抛出韭菜
4. 释放资源

**接收端实现步骤**

1. 创建DatagramSocket对象并指定端口（接收端对象）                      接韭菜的人
2. 创建DatagramPacket对象接收数据（数据包对象）                 韭菜盘子
3. 使用DatagramSocket对象的receive方法传入DatagramPacket对象                 开始接收韭菜
4. 释放资源



**示例：**

客户端：

```java
//客户端，一发一收
public class ClientDemo1 {
    public static void main(String[] args) throws Exception {
        System.out.println("=====客户端启动=====");
        //1、创建发送端对象，发送端自带默认的端口
        DatagramSocket socket = new DatagramSocket();

        //2、创建一个数据包对象封装数据（韭菜盘子）
        //public DatagramPacket(byte buf[], int length, InetAddress address, int port)
        byte[] buffer = "我是一颗美味的韭菜，你愿意吃吗？".getBytes();
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getLocalHost(), 8888);

        //3、发送数据出去
        socket.send(packet);

        socket.close();
    }
}
```

服务端：

```java
//服务端
public class ServerDemo2 {
    public static void main(String[] args) throws Exception {
        System.out.println("=====服务端启动=====");
        //1、创建接收端对象：注册端口（人）
        DatagramSocket socket = new DatagramSocket(8888);

        //2、创建一个数据包对象接收数据（韭菜盒子）
        byte[] buffer = new byte[1024 * 64];        //64KB的盒子
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        //3、等待接收数据
        socket.receive(packet);

        //4、输出数据（读多书倒多少）
        int len = packet.getLength();
        String rs = new String(buffer, 0, len);
        System.out.println("收到了：" + rs);

        //获取发送端的ip和端口
        String ip = packet.getSocketAddress().toString();
        System.out.println("对方地址：" + ip);
        int port = packet.getPort();
        System.out.println("对方端口：" + port);
        
        socket.close();
    }
}
```



服务端先启动：

```
=====服务端启动=====
```

客户端再启动：

```
=====客户端启动=====
```

客户端启动后，服务端接收到的信息：

```
=====服务端启动=====
收到了：我是一颗美味的韭菜，你愿意吃吗？
对方地址：/192.168.1.106:56562
对方端口：56562

Process finished with exit code 0
```



### 2.2 UDP通信：多发多收

**发送端可以反复发送数据**

客户端实现步骤：

1. 创建DatagramSocket对象（发送端对象）                      扔韭菜的人
2. 使用while死循环不断的接收用户的数据输入，如果用户输入的exit则退出程序
3. 如果用户输入的不是exit,  把数据封装成DatagramPacket                      韭菜盘子
4. 使用DatagramSocket对象的send方法将数据包对象进行发送                   开始抛出韭菜
5. 释放资源

**接收端可以反复接收数据**

接收端实现步骤：

1. 创建DatagramSocket对象并指定端口（接收端对象）                      接韭菜的人
2. 创建DatagramPacket对象接收数据（数据包对象）                 韭菜盘子
3. 使用while死循环不断的进行第4步
4. 使用DatagramSocket对象的receive方法传入DatagramPacket对象                 开始接收韭菜



**示例：**

客户端：

```java
//客户端，多发多收
public class ClientDemo1 {
    public static void main(String[] args) throws Exception {
        System.out.println("=====客户端启动=====");
        //1、创建发送端对象，发送端自带默认的端口，也可以自定义
        DatagramSocket socket = new DatagramSocket();

        Scanner sc = new Scanner(System.in);
        //2、创建一个数据包对象封装数据（韭菜盘子）
        //public DatagramPacket(byte buf[], int length, InetAddress address, int port)
        while (true) {
            System.out.println("请说：");
            String msg = sc.nextLine();

            if("exit".equals(msg)) {
                System.out.println("离线成功~~");
                socket.close();
                break;
            }
            byte[] buffer = msg.getBytes();
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, InetAddress.getLocalHost(), 8888);

            //3、发送数据出去
            socket.send(packet);
        }
    }
}
```

服务端：

```java
//服务端
public class ServerDemo2 {
    public static void main(String[] args) throws Exception {
        System.out.println("=====服务端启动=====");
        //1、创建接收端对象：注册端口（人）
        DatagramSocket socket = new DatagramSocket(8888);

        //2、创建一个数据包对象接收数据（韭菜盒子）
        byte[] buffer = new byte[1024 * 64];        //64KB的盒子
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        while (true) {
            //3、等待接收数据
            socket.receive(packet);

            //4、输出数据（读多书倒多少）
            int len = packet.getLength();
            String rs = new String(buffer, 0, len);
            System.out.println("收到了来自：" + packet.getAddress() + "，对方端口是" + packet.getPort() + "的消息：" + rs);
        }
    }
}
```

```
=====服务端启动=====
```

```
=====客户端启动=====
请说：
你好，在吗？
请说：
我是彭于晏
请说：
exit
离线成功~~

Process finished with exit code 0
```

```
=====服务端启动=====
收到了来自：/192.168.1.106，对方端口是56502的消息：你好，在吗？
收到了来自：/192.168.1.106，对方端口是56502的消息：我是彭于晏
```

**此服务端可以接收来自不同端口的信息**



UDP的接收端为什么可以接收很多发送端的消息？

- 接收端只负责接收数据包，无所谓是哪个发送端的数据包。



### 2.3 UDP通信-广播、组播

**UDP的三种通信方式**

- 单播：单台主机与单台主机之间的通信。
- 广播：当前主机与所在网络中的所有主机通信。
- 组播：当前主机与选定的一组主机的通信。



**UDP如何实现广播**

- 使用广播地址：255.255.255.255

- 具体操作：

  1. **发送端**发送的数据包的目的地写的是广播地址、且指定端口。 **（255.255.255.255  ,   9999）**
  2. 本机所在网段的**其他主机**的程序只要注册对应端口就可以收到消息了。**（9999）**

  

**UDP如何实现组播**

- 使用组播地址：224.0.0.0 ~ 239.255.255.255
- 具体操作：
  1. **发送端**的数据包的目的地是组播IP  **(例如：224.0.1.1,  端口：9999)**
  2. **接收端**必须绑定该组播IP**(224.0.1.1)**，端口还要注册发送端的目的端口**9999** ，这样即可接收该组播消息。
  3. DatagramSocket的子类**MulticastSocket**可以在接收端绑定组播IP。



## 3. TCP通信

**TCP协议回顾：** 

- TCP是一种面向连接，安全、可靠的传输数据的协议 
- 传输前，采用“三次握手”方式，点对点通信，是可靠的 
- 在连接中可进行大数据量的传输 

**TCP通信模式演示：** 

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ca7090ad4ad447b78faf202ef6bbca21.png)

==注意：在java中只要是使用java.net.Socket类实现通信，底层即是使用了TCP协议==

### 3.1 TCP通信-快速入门

**Socket**（客户端）

|                构造器                 |                             说明                             |
| :-----------------------------------: | :----------------------------------------------------------: |
| public Socket(String host , int port) | 创建发送端的Socket对象与服务端连接，参数为服务端程序的ip和端口。 |

**Socket类成员方法**

|              方法              |        说明        |
| :----------------------------: | :----------------: |
| OutputStream getOutputStream() | 获得字节输出流对象 |
|  InputStream getInputStream()  | 获得字节输入流对象 |



**ServerSocket(服务端)**

|            构造器             |      说明      |
| :---------------------------: | :------------: |
| public ServerSocket(int port) | 注册服务端端口 |

**ServerSocket类成员方法**

|          方法          |                             说明                             |
| :--------------------: | :----------------------------------------------------------: |
| public Socket accept() | 等待接收客户端的Socket通信连接，连接成功返回Socket对象与客户端建立端到端通信 |



**客户端实现步骤：**

1. 创建客户端的Socket对象，请求与服务端的连接。
2. 使用socket对象调用getOutputStream()方法得到字节输出流。
3. 使用字节输出流完成数据的发送。
4. 释放资源：关闭socket管道。

**服务端实现步骤**

1. 创建ServerSocket对象，注册服务端端口。
2. 调用ServerSocket对象的accept()方法，等待客户端的连接，并得到Socket管道对象。
3. 通过Socket对象调用getInputStream()方法得到字节输入流、完成数据的接收。
4. 释放资源：关闭socket管道



**示例：**

客户端：

```java
//一发一收
public class ClientDemo1 {
    public static void main(String[] args) {
        try {
            System.out.println("=====客户端启动=====");
            //1、创建Socket通信管道请求有服务端连接
            //public Socket(String host, int port)
            Socket socket = new Socket("127.0.0.1", 6666);

            //2、从socket通信管道中得到一个字节输出流，负责发送数据
            OutputStream os = socket.getOutputStream();
            //3、把低级字节流包装成打印流
            PrintStream ps = new PrintStream(os);
            //4、发送信息
            ps.println("我是TCP的客户端，我已经与你对接，并发出邀请！！");    //发一行，因为服务端是按行接收的，否则会报错
            ps.flush();

            //关闭资源（一般不建议关，需要exit退出的时候再关）
            //ps.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

服务端：

```java
public class ServerDemo2 {
    public static void main(String[] args) {
        try {
            System.out.println("=====服务端启动=====");
            //1、注册端口
            ServerSocket serverSocket = new ServerSocket(6666);
            //2、必须调用accept方法：等待接收克服的的Socket连接请求，建立Socket通信管道
            Socket socket = serverSocket.accept();
            //3、从socket通信导管中得到一个字节输入流
            InputStream is = socket.getInputStream();
            //4、将低级字节输入流包装成缓冲字符输入流(先包装成字符输入流）
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            //5、按照行读取信息
            String msg;
            if ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress() + "说了：" + msg);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

先启动服务端，再启动客户端

```
=====服务端启动=====
/127.0.0.1:57416说了：我是TCP的客户端，我已经与你对接，并发出邀请！！

Process finished with exit code 0
```



### 3.2 TCP通信-多发多收消息

需求：

1. 可以使用死循环控制服务端收完消息继续等待接收下一个消息。
2. 客户端也可以使用死循环等待用户不断输入消息。
3. 客户端一旦输入了exit，则关闭客户端程序，并释放资源。



**示例：**

客户端：

```java
public class ClientDemo1 {
    public static void main(String[] args) {
        try {
            System.out.println("=====客户端启动=====");
            //1、创建Socket通信管道请求有服务端连接
            //public Socket(String host, int port)
            Socket socket = new Socket("127.0.0.1", 6666);

            //2、从socket通信管道中得到一个字节输出流，负责发送数据
            OutputStream os = socket.getOutputStream();
            //3、把低级字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true) {
                System.out.println("请说：");
                String msg = sc.nextLine();
                //4、发送信息
                ps.println(msg);    //发一行，因为服务端是按行接收的，否则会报错
                ps.flush();
            }

            //关闭资源（一般不建议关，需要exit退出的时候再关）
            //ps.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

服务端：

```java
public class ServerDemo2 {
    public static void main(String[] args) {
        try {
            System.out.println("=====服务端启动=====");
            //1、注册端口
            ServerSocket serverSocket = new ServerSocket(6666);
            //2、必须调用accept方法：等待接收克服的的Socket连接请求，建立Socket通信管道
            Socket socket = serverSocket.accept();
            //3、从socket通信导管中得到一个字节输入流
            InputStream is = socket.getInputStream();
            //4、将低级字节输入流包装成缓冲字符输入流(先包装成字符输入流）
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            //5、按照行读取信息
            String msg;
            while ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress() + "说了：" + msg);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

输出：

```
=====客户端启动=====
请说：
你好
请说：
我是彭于晏
请说：
exit

Process finished with exit code 0
```

```
=====服务端启动=====
/127.0.0.1:57518说了：你好
/127.0.0.1:57518说了：我是彭于晏

Process finished with exit code 0
```



==注意：此案例不可以同时接收多个客户端的消息，因为服务端现在只有一个线程，单线程每次只能处理一个客户端的Socket通信，所以只能与一个客户端进行通信。==



### 3.3 TCP通信-同时接受多个客户端消息 [重点]

**同时处理多个客户端消息**

​											服务端

​											主线程（while死循环接收socket连接）     

客户端1	------------------>	socket	子线程1

客户端2	------------------>	socket	子线程2

客户端3	------------------>	socket	子线程3



**示例：**

ServerReaderThread 线程类：

```java
public class ServerReaderThread extends Thread{
    private Socket socket;
    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            //3、从socket通信导管中得到一个字节输入流
            InputStream is = socket.getInputStream();
            //4、将低级字节输入流包装成缓冲字符输入流(先包装成字符输入流）
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            //5、按照行读取信息
            String msg;
            while ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress() + "说了：" + msg);
            }
        } catch (Exception e) {
            //下线提示（因为上面while循环在不断的接收信息，当客户端退出后，接不到信息就会抛异常，所以在这里提示）
            System.out.println(socket.getRemoteSocketAddress() + "已下线，退出了聊天室！");
        }
    }
}
```

客户端：

```java
public class ClientDemo1 {
    public static void main(String[] args) {
        try {
            System.out.println("=====客户端启动=====");
            //1、创建Socket通信管道请求有服务端连接
            //public Socket(String host, int port)
            Socket socket = new Socket("127.0.0.1", 6666);

            //2、从socket通信管道中得到一个字节输出流，负责发送数据
            OutputStream os = socket.getOutputStream();
            //3、把低级字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true) {
                System.out.println("请说：");
                String msg = sc.nextLine();
                //4、发送信息
                if("exit".equals(msg)) {
                    ps.close();
                    break;
                }else {
                    ps.println(msg);    //发一行，因为服务端是按行接收的，否则会报错
                    ps.flush();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

服务端：

```java
public class ServerDemo2 {
    public static void main(String[] args) {
        try {
            System.out.println("=====服务端启动=====");
            //1、注册端口
            ServerSocket serverSocket = new ServerSocket(6666);
            //a. 定义一个死循环由主线程负责不断的接收客户端的Socket管道连接
            while (true) {
                //2、每接收到一个客户端的Socket管道，交给一个独立的子线程负责读取消息
                Socket socket = serverSocket.accept();
                //提示谁上线了
                System.out.println(socket.getRemoteSocketAddress() + "上线了，进入了聊天室！");
                //3、开始创建独立线程处理socket
                new ServerReaderThread(socket).start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

客户端启动两个线程任务：

```
=====客户端启动=====
请说：
大家好，我是渣渣辉
请说：
我要下线咯
请说：

Process finished with exit code 130
```

```
=====客户端启动=====
请说：
你们好，我是贪玩官方
请说：
渣渣辉走了，我也溜咯！
请说：

Process finished with exit code 130
```

服务端输出：

```
=====服务端启动=====
/127.0.0.1:57697上线了，进入了聊天室！
/127.0.0.1:57697说了：大家好，我是渣渣辉
/127.0.0.1:57697说了：我要下线咯
/127.0.0.1:57697已下线，退出了聊天室！
/127.0.0.1:57704上线了，进入了聊天室！
/127.0.0.1:57704说了：你们好，我是贪玩官方
/127.0.0.1:57704说了：渣渣辉走了，我也溜咯！
/127.0.0.1:57704已下线，退出了聊天室！

Process finished with exit code 130
```



本次是如何实现服务端接收多个客户端的消息的：

- **主线程**定义了**循环负责接收客户端Socket**管道连接
- 每接收到一个Socket通信管道后**分配一个独立的线程**负责处理它。



### 3.4 TCP通信-使用线程池优化

目前的通信架构存在什么问题？

- 客户端与服务端的线程模型是： N-N的关系。
- 客户端并发越多，系统瘫痪的越快。

**引入线程池处理多个客户端消息**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2348cbed0b391b75abfdc923ce7bd70c.png)



**示例：**

ServerReaderRunnable线程类：

```java
public class ServerReaderRunnable implements Runnable{
    private Socket socket;
    public ServerReaderRunnable(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try {
            //3、从socket通信导管中得到一个字节输入流
            InputStream is = socket.getInputStream();
            //4、将低级字节输入流包装成缓冲字符输入流(先包装成字符输入流）
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            //5、按照行读取信息
            String msg;
            while ((msg = br.readLine()) != null) {
                System.out.println(socket.getRemoteSocketAddress() + "说了：" + msg);
            }
        } catch (Exception e) {
            //下线提示（因为上面while循环在不断的接收信息，当客户端退出后，接不到信息就会抛异常，所以在这里提示）
            System.out.println(socket.getRemoteSocketAddress() + "已下线，退出了聊天室！");
        }
    }
}
```

客户端：

```java
public class ClientDemo1 {
    public static void main(String[] args) {
        try {
            System.out.println("=====客户端启动=====");
            //1、创建Socket通信管道请求有服务端连接
            //public Socket(String host, int port)
            Socket socket = new Socket("127.0.0.1", 6666);

            //2、从socket通信管道中得到一个字节输出流，负责发送数据
            OutputStream os = socket.getOutputStream();
            //3、把低级字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc = new Scanner(System.in);
            while (true) {
                System.out.println("请说：");
                String msg = sc.nextLine();
                //4、发送信息
                if("exit".equals(msg)) {
                    ps.close();
                    break;
                }else {
                    ps.println(msg);    //发一行，因为服务端是按行接收的，否则会报错
                    ps.flush();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

服务端

```java
public class ServerDemo2 {
    //使用静态变量记录一个线程池对象
    private static ExecutorService pool = new ThreadPoolExecutor(3, 5, 6,
            TimeUnit.SECONDS, new ArrayBlockingQueue<>(2), Executors.defaultThreadFactory(),
            new ThreadPoolExecutor.AbortPolicy());
    public static void main(String[] args) {
        try {
            System.out.println("=====服务端启动=====");
            //1、注册端口
            ServerSocket serverSocket = new ServerSocket(6666);
            //a. 定义一个死循环由主线程负责不断的接收客户端的Socket管道连接
            Socket socket = serverSocket.accept();
            System.out.println(socket.getRemoteSocketAddress() + "上线了，进入了聊天室！");
            //2、交给线程池处理
            Runnable target = new ServerReaderRunnable(socket);     //封装成任务对象
            pool.execute(target);       //丢进线程池排队
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



**本次使用线程池的优势：**

- 服务端可以复用线程处理多个客户端，可以避免系统瘫痪。
- ==不过只合适客户端通信时长较短的场景==。因为当核心线程都在工作，任务数量也达到了最大线程数，并且任务队列的数量超过了最大队列数量时，后面的任务就不会处理了，只有等前面的任务处理完后，才有线程来处理后面的任务。



### 3.5 TCP通信实战案例-即时通信

- 即时通信，是指一个客户端的消息发出去，其他客户端可以接收到。
- 之前我们的消息都是发给服务端的。



**怎样设计即时通信**

- 即时通信需要进行端口转发的设计思想。
- 服务端需要把在线的Socket管道存储起来。
- 一旦收到一个消息要推送给其他管道。

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d2f8184c8095bf5c968a4e024c1a1f6b.png)



**代码：**

ClientReaderThread 客户端线程类，负责接收消息：

```java
public class ClientReaderThread extends Thread{
    private Socket socket;
    public ClientReaderThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try {
            // 3、从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();
            // 4、把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            // 5、按照行读取消息
            String msg;
            while ((msg = br.readLine()) != null){
                System.out.println(socket.getRemoteSocketAddress() + "收到了: " + msg);
            }
        } catch (Exception e) {
            System.out.println("服务端把你踢出去了~~");
        }
    }
}
```

客户端类：

```java
/**
    拓展：即时通信

    客户端：发消息的同时，随时有人发消息过来。
    服务端：接收消息后，推送给其他所有的在线socket
 */
public class ClientDemo1 {
    public static void main(String[] args) {
        try {
            System.out.println("====客户端启动===");
            // 1、创建Socket通信管道请求有服务端的连接
            // public Socket(String host, int port)
            Socket socket = new Socket("127.0.0.1", 6868);

            // 马上为客户端分配一个独立的线程负责读取它收到的消息
            new ClientReaderThread(socket).start();

            // 2、从socket通信管道中得到一个字节输出流 负责发送数据
            OutputStream os = socket.getOutputStream();

            // 3、把低级的字节流包装成打印流
            PrintStream ps = new PrintStream(os);

            Scanner sc =  new Scanner(System.in);
            while (true) {
                System.out.println("请说：");
                String msg = sc.nextLine();
                // 4、发送消息
                ps.println(msg);
                ps.flush();
            }
            // 关闭资源。
            // socket.close();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



ServerReaderThread 服务端线程类，负责发送信息：

```java
public class ServerReaderThread extends Thread{
    private Socket socket;
    public ServerReaderThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try {
            // 3、从socket通信管道中得到一个字节输入流
            InputStream is = socket.getInputStream();
            // 4、把字节输入流包装成缓冲字符输入流进行消息的接收
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            // 5、按照行读取消息
            String msg;
            while ((msg = br.readLine()) != null){
                System.out.println(socket.getRemoteSocketAddress() + "说了：: " + msg);
                // 把这个消息发给当前所有在线socket
                sendMsgToAll(msg);
            }
        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress() + "下线了！！！");
            // 从在线集合中抹掉本客户端socket
            ServerDemo2.onLineSockets.remove(socket);
        }
    }

    private void sendMsgToAll(String msg) {
        try {
            // 遍历全部的在线 socket给他们发消息
            for (Socket onLineSocket : ServerDemo2.onLineSockets) {
                // 除了自己的socket，其他socket我都发！！
                if(onLineSocket != socket){
                    PrintStream ps = new PrintStream(socket.getOutputStream());
                    ps.println(msg);
                    ps.flush();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

服务端类：

```java
/**
   目标: 即时通信
 */
public class ServerDemo2 {
	//定义一个静态的List集合存储当前全部在线的socket管道
    public static List<Socket> onLineSockets = new ArrayList<>();

    public static void main(String[] args) {
        try {
            System.out.println("===服务端启动成功===");
            // 1、注册端口
            ServerSocket serverSocket = new ServerSocket(6868);
            // a.定义一个死循环由主线程负责不断的接收客户端的Socket管道连接。
            while (true) {
                // 2、每接收到一个客户端的Socket管道，交给一个独立的子线程负责读取消息
                Socket socket = serverSocket.accept();
                System.out.println(socket.getRemoteSocketAddress()+ "它来了，上线了！");
                // 把当前客户端管道Socket加入到在线集合中去
                onLineSockets.add(socket);

                // 3、开始创建独立线程处理socket
                new ServerReaderThread(socket).start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



### 3.6 TCP通信实战案例-模拟BS系统 [了解]

- 之前的客户端都是**CS架构，客户端实需要我们自己开发实现的。**

- **BS结构是浏览器访问服务端，不需要开发客户端。**

**实现BS开发**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cfc90fcfcf4a924a784380dcfaf11ca1.png)





**注意：服务器必须给浏览器响应HTTP协议格式的数据，否则浏览器不识别。**

**代码：**

ServerReaderRunnable 服务端线程类：

```java
public class ServerReaderRunnable implements Runnable{
    private Socket socket;
    public ServerReaderRunnable(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try {
            // 浏览器 已经与本线程建立了Socket管道
            // 响应消息给浏览器显示
            PrintStream ps = new PrintStream(socket.getOutputStream());
            // 必须响应HTTP协议格式数据，否则浏览器不认识消息
            ps.println("HTTP/1.1 200 OK"); // 协议类型和版本 响应成功的消息！
            ps.println("Content-Type:text/html;charset=UTF-8"); // 响应的数据类型：文本/网页

            ps.println(); // 必须发送一个空行

            // 才可以响应数据回去给浏览器
            ps.println("<span style='color:red;font-size:90px'>《最牛的149期》 </span>");
            ps.close();
        } catch (Exception e) {
            System.out.println(socket.getRemoteSocketAddress() + "下线了！！！");
        }
    }
}
```

```java
/**
    了解：BS-浏览器-服务器基本了解。

    引入：
        之前客户端和服务端都需要自己开发。也就是CS架构。
        接下来模拟一下BS架构。

    客户端：浏览器。（无需开发）
    服务端：自己开发。
    需求：在浏览器中请求本程序，响应一个网页文字给浏览器显示
 */
public class BSserverDemo {
    // 使用静态变量记住一个线程池对象
    private static ExecutorService pool = new ThreadPoolExecutor(3,
            5, 6, TimeUnit.SECONDS,
            new ArrayBlockingQueue<>(2)
            , Executors.defaultThreadFactory(), new ThreadPoolExecutor.AbortPolicy());

    public static void main(String[] args) {
        try {
            // 1.注册端口
            ServerSocket ss = new ServerSocket(8080);
            // 2.创建一个循环接收多个客户端的请求。
            while(true){
                Socket socket = ss.accept();
                // 3.交给一个独立的线程来处理！
                pool.execute(new ServerReaderRunnable(socket));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

启动服务端后，在浏览器输入本地ip地址和端口号

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4e451353b6689f8b9b899e3f26e9340e.png)







# 单元测试、反射、注解、动态代理

## 1. 单元测试

### 1.1 单元测试概述

- 单元测试就是针对最小的功能单元编写测试代码，Java程序最小的功能单元是方法，因此，单元测试就是针对Java方法的测试，进而检查方法的正确性。

**目前测试方法是怎么进行的，存在什么问题**

- 只有一个main方法，如果一个方法的测试失败了，其他方法测试会受到影响。
- 无法得到测试的结果报告，需要程序员自己去观察测试是否成功。
- 无法实现自动化测试。



**Junit单元测试框架**

- JUnit是使用Java语言实现的单元测试框架，它是开源的，Java开发者都应当学习并使用JUnit编写单元测试。
- 此外，几乎所有的IDE工具都集成了JUnit，这样我们就可以直接在IDE中编写并运行JUnit测试，JUnit目前最新版本是5。

**JUnit优点**

- JUnit可以灵活的选择执行哪些测试方法，可以一键执行全部测试方法。
- Junit可以生成全部方法的测试报告。
- 单元测试中的某个方法测试失败了，不会影响其他测试方法的测试。



### 1.2 单元测试快速入门

使用单元测试进行业务方法预期结果、正确性测试的快速入门

步骤：

1. 将JUnit的jar包导入到项目的lib目录中
   - IDEA通常整合好了Junit框架，一般不需要导入。
   - 如果IDEA没有整合好，需要自己手工导入如下2个JUnit的jar包到模块
2. 编写测试方法：该测试方法必须是公共的无参数无返回值的非静态方法。
3. 在测试方法上使用@Test注解：标注该方法是一个测试方法
4. 在测试方法中完成被测试方法的预期正确性测试。
5. 选中测试方法，选择“JUnit运行” ，如果测试良好则是绿色；如果测试失败，则是红色



### 1.3 单元测试常用注解

**Junit常用注解(Junit 4.xxxx版本)**

|     注解     |                             说明                             |
| :----------: | :----------------------------------------------------------: |
|    @Test     |                           测试方法                           |
|   @Before    | 用来修饰实例方法，该方法会在每一个测试方法执行之前执行一次。 |
|    @After    | 用来修饰实例方法，该方法会在每一个测试方法执行之后执行一次。 |
| @BeforeClass |   用来静态修饰方法，该方法会在所有测试方法之前只执行一次。   |
| @AfterClass  |   用来静态修饰方法，该方法会在所有测试方法之后只执行一次。   |

**Junit常用注解(Junit 5.xxxx版本)**

|    注解     |                             说明                             |
| :---------: | :----------------------------------------------------------: |
|    @Test    |                           测试方法                           |
| @BeforeEach | 用来修饰实例方法，该方法会在每一个测试方法执行之前执行一次。 |
| @AfterEach  | 用来修饰实例方法，该方法会在每一个测试方法执行之后执行一次。 |
| @BeforeAll  |   用来静态修饰方法，该方法会在所有测试方法之前只执行一次。   |
|  @AfterAll  |   用来静态修饰方法，该方法会在所有测试方法之后只执行一次。   |



- 开始执行的方法: 初始化资源。
- 执行完之后的方法: 释放资源。



**示例：**

模拟类：

```java
/**
   业务方法
 */
public class UserService {
    public String loginName(String loginName , String passWord){
        if("admin".equals(loginName) && "123456".equals(passWord)){
            return "登录成功";
        }else {
            return "用户名或者密码有问题";
        }
    }

    public void selectNames(){
        System.out.println(10/2);
        System.out.println("查询全部用户名称成功~~");
    }
}
```

测试类：

```java
/**
   测试类
 */
public class TestUserService {

    // 修饰实例方法的
    @Before
    public void before(){
        System.out.println("===before方法执行一次===");
    }

    @After
    public void after(){
        System.out.println("===after方法执行一次===");
    }

    // 修饰静态方法
    @BeforeClass
    public static void beforeClass(){
        System.out.println("===beforeClass方法执行一次===");
    }

    @AfterClass
    public static void afterClass(){
        System.out.println("===afterClass方法执行一次===");
    }


    /**
       测试方法
       注意点：
            1、必须是公开的，无参数 无返回值的方法
            2、测试方法必须使用@Test注解标记。
     */
    @Test
    public void testLoginName(){
        UserService userService = new UserService();
        String rs = userService.loginName("admin","123456");

        // 进行预期结果的正确性测试：断言。
        Assert.assertEquals("您的登录业务可能出现问题", "登录成功", rs );
    }

    @Test
    public void testSelectNames(){
        UserService userService = new UserService();
        userService.selectNames();
    }
}
```

测试结果：

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4a237652b7c574b07f3692b83761cace.png)



## 2. 反射

### 2.1 反射概述

- 反射是指对于任何一个Class类，在 =="运行的时候"== 都可以直接==得到这个类全部成分==。
- 在运行时,可以直接得到这个类的构造器对象：Constructor
- 在运行时,可以直接得到这个类的成员变量对象：Field
- 在运行时,可以直接得到这个类的成员方法对象：Method
- 这种==运行时动态获取类信息==以及==动态调用类中成分==的能力称为Java语言的==反射==机制。

**反射的关键：**

- 反射的第一步都是先得到编译后的Class类对象，然后就可以得到Class的全部成分。

  ```java
  HelloWorld.java -> javac -> HelloWorld.class
  Class c = HelloWorld.class;
  ```



### 2.2 反射获取类对象

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/94448a60a6c3bf4b0dbc0ec31b2e1675.png)

**获取Class类的对象的三种方式**

- 方式一：`Class c1 = Class.forName(“全类名”);`
- 方式二：`Class c2 = 类名.class;`
- 方式三：`Class c3 = 对象.getClass();`

**示例：**

```java
public class Test {
    public static void main(String[] args) throws Exception {
        // 1、Class类中的一个静态方法：forName(全限名：包名 + 类名)
        Class c = Class.forName("com.d2_reflect_class.Student");
        System.out.println(c);      // Student.class

        // 2、类名.class
        Class c1 = Student.class;
        System.out.println(c1);

        // 3、对象.getClass() 获取对象对应类的Class对象。
        Student s = new Student();
        Class c2 = s.getClass();
        System.out.println(c2);
    }
}
```

**一个类在内存中只有一个class对象；一个类被加载后，类的整个结构都会被封装在class对象中**



**哪些类型可以有class对象？**

```java
	Class c1 = Object.class;    //类
        Class c2 = Comparable.class;    //接口
        Class c3 = String[].class;    //一维数组
        Class c4 = int[][].class;    //二维数组
        Class c5 = Override.class;    //注解
        Class c6 = ElementType.class;    //枚举
        Class c7 = Integer.class;    //基本数据类型
        Class c8 = void.class;    //void
        Class c9 = Class.class;    //Class

        System.out.println(c1);     //class java.lang.Object
        System.out.println(c2);     //interface java.lang.Comparable
        System.out.println(c3);     //class [Ljava.lang.String;
        System.out.println(c4);     //class [[I
        System.out.println(c5);     //interface java.lang.Override
        System.out.println(c6);     //class java.lang.annotation.ElementType
        System.out.println(c7);     //class java.lang.Integer
        System.out.println(c8);     //void
        System.out.println(c9);     //class java.lang.Class

	int[] a1 = new int[2];
        int[] a2 = new int[20];
        System.out.println(a1.getClass().hashCode());       //1956725890
        System.out.println(a2.getClass().hashCode());       //1956725890
```

**只要类型与维度一样，就是同一个Class对象**



### 2.3 反射获取构造器对象

**使用反射技术获取构造器对象并使用**

- 反射的第一步是先得到类对象，然后从类对象中获取类的成分对象
- Class类中用于获取构造器的方法

|                             方法                             |                    说明                    |
| :----------------------------------------------------------: | :----------------------------------------: |
|              Constructor<?>[] getConstructors()              | 返回所有构造器对象的数组（只能拿public的） |
|          Constructor<?>[] getDeclaredConstructors()          |   返回所有构造器对象的数组，存在就能拿到   |
|  Constructor<T> getConstructor(Class<?>... parameterTypes)   |    返回单个构造器对象（只能拿public的）    |
| Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes) |      返回单个构造器对象，存在就能拿到      |

**示例：**

Student类

```java
public class Student {
    private String name;
    private int age;

    private Student(){
        System.out.println("无参数构造器执行！");
    }

    public Student(String name, int age) {
        System.out.println("有参数构造器执行！");
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
/**
    目标：反射_获取Constructor构造器对象.

    反射的第一步是先得到Class类对象。（Class文件）

    小结：
        获取类的全部构造器对象： Constructor[] getDeclaredConstructors()
            -- 获取所有申明的构造器，只要你写我就能拿到，无所谓权限。建议使用！！
        获取类的某个构造器对象：Constructor getDeclaredConstructor(Class... parameterTypes)
            -- 根据参数匹配获取某个构造器，只要申明就可以定位，不关心权限修饰符，建议使用！

 */
public class TestStudent01 {
    // 1. getConstructors:
    // 获取全部的构造器：只能获取public修饰的构造器。
    // Constructor[] getConstructors()
    @Test
    public void getConstructors(){
        // a.第一步：获取类对象
        Class c = Student.class;
        // b.提取类中的全部的构造器对象(这里只能拿public修饰)
        Constructor[] constructors = c.getConstructors();
        // c.遍历构造器
        for (Constructor constructor : constructors) {
            System.out.println(constructor.getName() + "===>" + constructor.getParameterCount());
        }
    }


    // 2.getDeclaredConstructors():
    // 获取全部的构造器：只要你敢写，这里就能拿到，无所谓权限是否可及。
    @Test
    public void getDeclaredConstructors(){
        // a.第一步：获取类对象
        Class c = Student.class;
        // b.提取类中的全部的构造器对象
        Constructor[] constructors = c.getDeclaredConstructors();
        // c.遍历构造器
        for (Constructor constructor : constructors) {
            System.out.println(constructor.getName() + "===>" + constructor.getParameterCount());
        }
    }

    // 3.getConstructor(Class... parameterTypes)
    // 获取某个构造器：只能拿public修饰的某个构造器
    @Test
    public void getConstructor() throws Exception {
        // a.第一步：获取类对象
        Class c = Student.class;
        // b.定位单个构造器对象 (按照参数定位无参数构造器 只能拿public修饰的某个构造器)
        Constructor cons = c.getConstructor();
        System.out.println(cons.getName() + "===>" + cons.getParameterCount());
    }


    // 4.getConstructor(Class... parameterTypes)
    // 获取某个构造器：只要你敢写，这里就能拿到，无所谓权限是否可及。
    @Test
    public void getDeclaredConstructor() throws Exception {
        // a.第一步：获取类对象
        Class c = Student.class;
        // b.定位单个构造器对象 (按照参数定位无参数构造器)
        Constructor cons = c.getDeclaredConstructor();
        System.out.println(cons.getName() + "===>" + cons.getParameterCount());

        // c.定位某个有参构造器
        Constructor cons1 = c.getDeclaredConstructor(String.class, int.class);
        System.out.println(cons1.getName() + "===>" + cons1.getParameterCount());

    }
}
```

输出：

```
com.d3_reflect_constructor.Student===>0
com.d3_reflect_constructor.Student===>2
com.d3_reflect_constructor.Student===>2
com.d3_reflect_constructor.Student===>0
com.d3_reflect_constructor.Student===>2
com.d3_reflect_constructor.Student===>2

Process finished with exit code 0
```



- 获取构造器的作用依然是初始化一个对象返回。

|                  符号                   |                   说明                    |
| :-------------------------------------: | :---------------------------------------: |
|    T newInstance(Object... initargs)    |         根据指定的构造器创建对象          |
| public void setAccessible(boolean flag) | 设置为true,表示取消访问检查，进行暴力反射 |

```java
/**
    目标: 反射_获取Constructor构造器然后通过这个构造器初始化对象。

    反射获取Class中的构造器对象Constructor作用：
            也是初始化并得到类的一个对象返回。
 
    小结：
        可以通过定位类的构造器对象。
        如果构造器对象没有访问权限可以通过：void setAccessible(true)打开权限
        构造器可以通过T newInstance(Object... initargs)调用自己，传入参数！
 */
public class TestStudent02 {
    // 1.调用构造器得到一个类的对象返回。
    @Test
    public void getDeclaredConstructor() throws Exception {
        // a.第一步：获取类对象
        Class c = Student.class;
        // b.定位单个构造器对象 (按照参数定位无参数构造器)
        Constructor cons = c.getDeclaredConstructor();
        System.out.println(cons.getName() + "===>" + cons.getParameterCount());

        // 如果遇到了私有的构造器，可以暴力反射
        cons.setAccessible(true); // 权限被打开

        Student s = (Student) cons.newInstance();
        System.out.println(s);

        System.out.println("-------------------");

        // c.定位某个有参构造器
        Constructor cons1 = c.getDeclaredConstructor(String.class, int.class);
        System.out.println(cons1.getName() + "===>" + cons1.getParameterCount());

        Student s1 = (Student) cons1.newInstance("孙悟空", 1000);
        System.out.println(s1);
    }
}
```

输出：

```
com.d3_reflect_constructor.Student===>0
无参数构造器执行！
Student{name='null', age=0}
-------------------
com.d3_reflect_constructor.Student===>2
有参数构造器执行！
Student{name='孙悟空', age=1000}

Process finished with exit code 0
```



总结：

1. 利用反射技术获取构造器对象的方式
   - `getDeclaredConstructors()`
   - `getDeclaredConstructor (Class<?>... parameterTypes)`
2. 反射得到的构造器可以做什么？
   - 依然是创建对象
     - `public newInstance(Object... initargs)`
   - 如果是非public的构造器，需要打开权限（暴力反射），然后再创建对象
     - setAccessible(boolean)
     - **反射可以破坏封装性，私有的也可以执行了。**



### 2.4 反射获取成员变量对象

**使用反射技术获取成员变量对象并使用**

- 反射的第一步是先得到类对象，然后从类对象中获取类的成分对象。
- Class类中用于获取成员变量的方法

|                方法                 |                     说明                     |
| :---------------------------------: | :------------------------------------------: |
|         Field[] getFields()         | 返回所有成员变量对象的数组（只能拿public的） |
|     Field[] getDeclaredFields()     |   返回所有成员变量对象的数组，存在就能拿到   |
|     Field getField(String name)     |    返回单个成员变量对象（只能拿public的）    |
| Field getDeclaredField(String name) |      返回单个成员变量对象，存在就能拿到      |

**示例：**

```java
public class Student {
    private String name;
    private int age;
    public static String schoolName;
    public static final String  COUNTTRY = "中国";

    public Student(){
        System.out.println("无参数构造器执行！");
    }

    public Student(String name, int age) {
        System.out.println("有参数构造器执行！");
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
/**
     目标：反射_获取Field成员变量对象。

     反射的第一步是先得到Class类对象。
 
     小结：
        获取全部成员变量：getDeclaredFields
        获取某个成员变量：getDeclaredField
 */
public class FieldDemo01 {
    /**
     * 1.获取全部的成员变量。
     * Field[] getDeclaredFields();
     *  获得所有的成员变量对应的Field对象，只要申明了就可以得到
     */
    @Test
    public void getDeclaredFields(){
        // a.定位Class对象
        Class c = Student.class;
        // b.定位全部成员变量
        Field[] fields = c.getDeclaredFields();
        // c.遍历一下
        for (Field field : fields) {
            System.out.println(field.getName() + "==>" + field.getType());
        }
    }

    /**
        2.获取某个成员变量对象 Field getDeclaredField(String name);
     */
    @Test
    public void getDeclaredField() throws Exception {
        // a.定位Class对象
        Class c = Student.class;
        // b.根据名称定位某个成员变量
        Field f = c.getDeclaredField("age");
        System.out.println(f.getName() +"===>" + f.getType());
    }
}
```

输出：

```
age===>int
name==>class java.lang.String
age==>int
schoolName==>class java.lang.String
COUNTTRY==>class java.lang.String

Process finished with exit code 0
```



- 获取成员变量的作用依然是在某个对象中取值、赋值

|                 符号                 |  说明  |
| :----------------------------------: | :----: |
| void set(Object obj, Object value)： |  赋值  |
|        Object get(Object obj)        | 获取值 |

```java
/**
    目标：反射获取成员变量: 取值和赋值。

    Field的方法：给成员变量赋值和取值
        void set(Object obj, Object value)：给对象注入某个成员变量数据
        Object get(Object obj):获取对象的成员变量的值。
        void setAccessible(true);暴力反射，设置为可以直接访问私有类型的属性。
        Class getType(); 获取属性的类型，返回Class对象。
        String getName(); 获取属性的名称。
 */
public class FieldDemo02 {
    @Test
    public void setField() throws Exception {
        // a.反射第一步，获取类对象
        Class c = Student.class;
        // b.提取某个成员变量
        Field ageF = c.getDeclaredField("age");

        ageF.setAccessible(true); // 暴力打开权限

        // c.赋值
        Student s = new Student();
        ageF.set(s , 18);  // s.setAge(18);
        System.out.println(s);

        // d、取值
        int age = (int) ageF.get(s);
        System.out.println(age);
    }
}
```

输出：

```
无参数构造器执行！
Student{name='null', age=18}
18

Process finished with exit code 0
```



### 2.5 反射获取方法对象

**使用反射技术获取方法对象并使用**

- 反射的第一步是先得到类对象，然后从类对象中获取类的成分对象。
- Class类中用于获取成员方法的方法

|                             方法                             |                     说明                     |
| :----------------------------------------------------------: | :------------------------------------------: |
|                    Method[] getMethods()                     | 返回所有成员方法对象的数组（只能拿public的） |
|                Method[] getDeclaredMethods()                 |   返回所有成员方法对象的数组，存在就能拿到   |
|  Method getMethod(String name, Class<?>... parameterTypes)   |    返回单个成员方法对象（只能拿public的）    |
| Method getDeclaredMethod(String name, Class<?>... parameterTypes) |      返回单个成员方法对象，存在就能拿到      |

- 获取成员方法的作用依然是在某个对象中进行执行此方法

**Method类中用于触发执行的方法**

|                   符号                    | 说明                                                         |
| :---------------------------------------: | :----------------------------------------------------------- |
| Object invoke(Object obj, Object... args) | 运行方法，<br>参数1：用obj对象调用该方法<br>参数2：调用方法的传递的参数（如果没有就不写）<br>返回值：方法的返回值（如果没有就不写） |



**示例：**

Dog类：

```java
public class Dog {
    private String name ;
    public Dog(){
    }

    public Dog(String name) {
        this.name = name;
    }

    public void run(){
        System.out.println("狗跑的贼快~~");
    }

    private void eat(){
        System.out.println("狗吃骨头");
    }

    private String eat(String name){
        System.out.println("狗吃" + name);
        return "吃的很开心！";
    }

    public static void inAddr(){
        System.out.println("在黑马学习Java!");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

```java
/**
    目标：反射——获取Method方法对象

    Method的方法执行：
        Object invoke(Object obj, Object... args)
          参数一：触发的是哪个对象的方法执行。
          参数二： args：调用方法时传递的实际参数
 */
public class MethodDemo01 {
    /**
     * 1.获得类中的所有成员方法对象
     */
    @Test
    public void getDeclaredMethods(){
        // a.获取类对象
        Class c = Dog.class;
        // b.提取全部方法；包括私有的
        Method[] methods = c.getDeclaredMethods();
        // c.遍历全部方法
        for (Method method : methods) {
            System.out.println(method.getName() +" 返回值类型：" + method.getReturnType() + " 参数个数：" + method.getParameterCount());
        }
    }

    /**
     * 2. 获取某个方法对象
     */
    @Test
    public void getDeclardMethod() throws Exception {
        // a.获取类对象
        Class c = Dog.class;
        // b.提取单个方法对象
        Method m = c.getDeclaredMethod("eat");
        Method m2 = c.getDeclaredMethod("eat", String.class);

        // 暴力打开权限了
        m.setAccessible(true);
        m2.setAccessible(true);

        // c.触发方法的执行
        Dog d = new Dog();
        // 注意：方法如果是没有结果回来的，那么返回的是null.
        Object result = m.invoke(d);
        System.out.println(result);

        Object result2 = m2.invoke(d, "骨头");
        System.out.println(result2);
    }
}
```

输出：

```
getName 返回值类型：class java.lang.String 参数个数：0
run 返回值类型：void 参数个数：0
setName 返回值类型：void 参数个数：1
eat 返回值类型：void 参数个数：0
eat 返回值类型：class java.lang.String 参数个数：1
inAddr 返回值类型：void 参数个数：0

狗吃骨头
null
狗吃骨头
吃的很开心！

Process finished with exit code 0
```



### 2.6 反射的作用-绕过编译阶段为集合添加数据

- 反射是作用在运行时的技术，此时集合的泛型将不能产生约束了，此时是可以==为集合存入其他任意类型的元素的。==

  

- 泛型只是在编译阶段可以约束集合只能操作某种数据类型，在==编译成Class文件进入运行阶段==的时候，其真实类型都是ArrayList了，泛型相当于被擦除了。



反射为何可以给约定了泛型的集合存入其他类型的元素？

- ==编译成Class文件进入运行阶段的时候，泛型会自动擦除。==
- 反射是作用在运行时的技术，此时已经不存在泛型了。



**示例：**

```java
public class ReflectDemo {
    public static void main(String[] args) throws Exception {
        // 需求：反射实现泛型擦除后，加入其他类型的元素
        ArrayList<String> lists1 = new ArrayList<>();
        ArrayList<Integer> lists2 = new ArrayList<>();

        System.out.println(lists1.getClass());
        System.out.println(lists2.getClass());	 // ArrayList.class

        System.out.println(lists1.getClass() ==  lists2.getClass()); 		//true

        System.out.println("---------------------------");
        ArrayList<Integer> lists3 = new ArrayList<>();
        lists3.add(23);
        lists3.add(22);
        // lists3.add("黑马");		//报错

        Class c = lists3.getClass(); // ArrayList.class  ===> public boolean add(E e)
        // 定位c类中的add方法
        Method add = c.getDeclaredMethod("add", Object.class);
        boolean rs = (boolean) add.invoke(lists3, "黑马");
        System.out.println(rs);

        System.out.println(lists3);

        //另一种方法：
        ArrayList list4 = lists3;
        list4.add("白马");
        list4.add(false);
        System.out.println(lists3);
    }
}
```

输出：

```
class java.util.ArrayList
class java.util.ArrayList
true
---------------------------
true
[23, 22, 黑马]
[23, 22, 黑马, 白马, false]

Process finished with exit code 0
```



### 2.7 反射的作用-通用框架的底层原理

**需求：**

- 给你任意一个对象，在不清楚对象字段的情况可以，可以把对象的字段名称和对应值存储到文件中去。

**分析：**

1. 定义一个方法，可以接收任意类的对象。
2. 每次收到一个对象后，需要解析这个对象的全部成员变量名称。
3. 这个对象可能是任意的，那么怎么样才可以知道这个对象的全部成员变量名称呢？
4. 使用反射获取对象的Class类对象，然后获取全部成员变量信息。
5. 遍历成员变量信息，然后提取本成员变量在对象中的具体值
6. 存入成员变量名称和值到文件中去即可。

**代码：**

Teacher和Student类：

```java
@Setter
@Getter
@AllArgsConstructor
@NoArgsConstructor
public class Teacher {
    private String name;
    private char sex;
    private double salary;
    }
```

```java
@Setter
@Getter
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private String name;
    private char sex;
    private int age;
    private String className;
    private String hobby;
}
```

MybatisUtil类：

```java
public class MybatisUtil {
    /**
     保存任意类型的对象
     * @param obj
     */
    public static void save(Object obj){
        try (
                PrintStream ps = new PrintStream(new FileOutputStream("JUnit-reflect-annotation-proxy\\src\\data.txt", true));
        ){
            // 1、提取这个对象的全部成员变量：只有反射可以解决
            Class c = obj.getClass();  //   c.getSimpleName()获取当前类名   c.getName获取全限名：包名+类名
            ps.println("================" + c.getSimpleName() + "================");

            // 2、提取它的全部成员变量
            Field[] fields = c.getDeclaredFields();
            // 3、获取成员变量的信息
            for (Field field : fields) {
                String name = field.getName();
                // 提取本成员变量在obj对象中的值（取值）
                field.setAccessible(true);
                String value = field.get(obj) + "";
                ps.println(name  + "=" + value);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
/**
   目标：提供一个通用框架，支持保存所有对象的具体信息。
 */
public class ReflectDemo {
    public static void main(String[] args) throws Exception {
        Student s = new Student();
        s.setName("猪八戒");
        s.setClassName("西天跑路1班");
        s.setAge(1000);
        s.setHobby("吃，睡");
        s.setSex('男');
        MybatisUtil.save(s);

        Teacher t = new Teacher();
        t.setName("波仔");
        t.setSex('男');
        t.setSalary(6000);
        MybatisUtil.save(t);
    }
}
```

运行后，在相应的data.txt文件里有插入的任意对象

```txt
================Student================
name=猪八戒
sex=男
age=1000
className=西天跑路1班
hobby=吃，睡
================Teacher================
name=波仔
sex=男
salary=6000.0

```



### 2.8 反射作用总结

- 可以在运行时得到一个类的全部成分然后操作
- 可以破坏封装性。（很突出）
- 也可以破坏泛型的约束性。（很突出）
- 更重要的用途是适合：做Java高级框架
- 基本上主流框架都会基于反射设计一些通用技术功能





## 3. 注解

### 3.1 注解概述

- Java 注解（Annotation）又称 Java 标注，是 JDK5.0 引入的一种注释机制。
- Java 语言中的类、构造器、方法、成员变量、参数等都可以被注解进行标注。

**注解的作用**

- ==对Java中类、方法、成员变量做标记，然后进行特殊处理==，至于到底做何种处理由业务需求来决定。
- 例如：JUnit框架中，标记了注解@Test的方法就可以被当成测试方法执行，而没有标记的就不能当成测试方法执行。





### 3.2 自定义注解

**自定义注解 --- 格式**

- 自定义注解就是自己做一个注解来使用。

```java
public @interface   注解名称   {
		public 属性类型 属性名() default 默认值 ;
}
```

属性类型：Java支持的数据类型基本上都支持



**特殊属性**

- value属性，如果==只有一个value属性==的情况下，使用value属性的时候可以省略value名称不写
- 但是如果有多个属性,  且多个属性没有默认值，那么value名称是不能省略的



**示例：**

相关注解类：

```java
public @interface MyBook {
    String name();		//这不是一个方法，是注解的属性
    String[] authors();
    double price();
}
```

```java
public @interface Book {
    String value();		 // 特殊属性
    double price() ;
    //double price() default 9.9;		//默认值，注解在使用时该属性可以不用赋值
}
```



```java
/**
   目标：学会自定义注解。掌握其定义格式和语法。
 */
@MyBook(name="《精通JavaSE》",authors = {"黑马", "dlei"} , price = 199.5)
//@Book(value = "/delete")
// @Book("/delete")		//只有value() 一个值时可以直接写
@Book(value = "/delete", price = 23.5)
//@Book("/delete")
public class AnnotationDemo1 {

    @MyBook(name = "《精通JavaSE2》", authors = {"黑马", "dlei"}, price = 199.5)
    private AnnotationDemo1() {

    }

    @MyBook(name = "《精通JavaSE1》", authors = {"黑马", "dlei"}, price = 199.5)
    public static void main(String[] args) {
        @MyBook(name = "《精通JavaSE2》", authors = {"黑马", "dlei"}, price = 199.5)
        int age = 21;
    }
}
```



### 3.3 元注解

- 元注解：就是注解注解的注解。 

**元注解有两个：**

- ==@Target==: 约束自定义注解只能在哪些地方使用，
- ==@Retention==：申明注解的生命周期
- ==@Documented==：表示是否将注解生成在JAVAdoc文档中
- ==@Inherited==：子类可以继承父类的注解



**@Target中可使用的值定义在ElementType枚举类中，常用值如下**

```java
public enum ElementType {
 
    TYPE,	 // 类、接口、枚举类
 
    FIELD, 		// 成员变量（包括：枚举常量）
 
    METHOD,		 // 成员方法
 
    PARAMETER,	 	// 方法参数
 
    CONSTRUCTOR, 	// 构造方法
 
    LOCAL_VARIABLE,		 // 局部变量
 
    ANNOTATION_TYPE, 	// 注解类
 
    PACKAGE, 	// 可用于修饰：包
 
    TYPE_PARAMETER, 	// 类型参数，JDK 1.8 新增
 
    TYPE_USE 	// 使用类型的任何地方，JDK 1.8 新增
 
}
```



**@Retention中可使用的值定义在RetentionPolicy枚举类中，常用值如下**

- SOURCE： 注解只作用在源码阶段，生成的字节码文件中不存在
- CLASS：  注解作用在源码阶段，字节码文件阶段，运行阶段不存在，默认值.
- RUNTIME：注解作用在源码阶段，字节码文件阶段，运行阶段（开发常用）



**示例：**

注解类：

```java
@Target({ElementType.METHOD, ElementType.FIELD}) // 元注解
@Retention(RetentionPolicy.RUNTIME) // 一直活着，在运行阶段这个注解也不消失
public @interface MyTest {
}
```

```java
/**
   目标：认识元注解
 */
//@MyTest	 // 只能注解方法和成员变量
public class AnnotationDemo2 {

    @MyTest
    private String name;

    @MyTest
    public void test(){

    }

    public static void main(String[] args) {

    }
}
```







### 3.4 注解解析

**注解的解析**

- 注解的操作中经常需要进行解析，注解的解析就是判断是否存在注解，存在注解就解析出内容。

**与注解解析相关的接口**

- Annotation：注解的顶级接口，注解都是Annotation类型的对象
- AnnotatedElement：该接口定义了与注解解析相关的解析方法

|                             方法                             |                             说明                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|            Annotation[]  getDeclaredAnnotations()            |         获得当前对象上使用的所有注解，返回注解数组。         |
|      T getDeclaredAnnotation(Class<T> annotationClass)       |                 根据注解类型获得对应注解对象                 |
| boolean isAnnotationPresent(Class<Annotation> annotationClass) | 判断当前对象是否使用了指定的注解，如果使用了则返回true，否则false |

- 所有的类成分Class, Method , Field , Constructor，都实现了AnnotatedElement接口他们都拥有解析注解的能力



**解析注解的技巧**

- 注解在哪个成分上，我们就先拿哪个成分对象。
- 比如注解作用成员方法，则要获得该成员方法对应的Method对象，再来拿上面的注解
- 比如注解作用在类上，则要该类的Class对象，再来拿上面的注解
- 比如注解作用在成员变量上，则要获得该成员变量对应的Field对象，再来拿上面的注解



案例：注解解析

分析：

1. 定义注解Book，要求如下：
   - 包含属性：String value()  书名
   - 包含属性：double price() 价格，默认值为 100
   - 包含属性：String[] authors() 多位作者
   - 限制注解使用的位置：类和成员方法上
   - 指定注解的有效范围：RUNTIME
2. 定义BookStore类，在类和成员方法上使用Book注解
3. 定义AnnotationDemo01测试类获取Book注解上的数据

**代码：**

注解Book类：

```java
@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Bookk {
    String value();
    double price() default 100;
    String[] author();
}
```

```java
/**
   目标：完成注解的解析
 */
public class AnnotationDemo3 {
    @Test
    public void parseClass(){
        // a.先得到类对象
        Class c = BookStore.class;
        // b.判断这个类上面是否存在这个注解
        if(c.isAnnotationPresent(Bookk.class)){
            //c.直接获取该注解对象
            Bookk book = (Bookk) c.getDeclaredAnnotation(Bookk.class);
            System.out.println(book.value());
            System.out.println(book.price());
            System.out.println(Arrays.toString(book.author()));     //author是数组
        }
    }

    @Test
    public void parseMethod() throws NoSuchMethodException {
        // a.先得到类对象
        Class c = BookStore.class;

        Method m = c.getDeclaredMethod("test");

        // b.判断这个类上面是否存在这个注解
        if(m.isAnnotationPresent(Bookk.class)){
            //c.直接获取该注解对象
            Bookk book = (Bookk) m.getDeclaredAnnotation(Bookk.class);
            System.out.println(book.value());
            System.out.println(book.price());
            System.out.println(Arrays.toString(book.author()));
        }
    }
}

@Bookk(value = "《情深深雨濛濛》", price = 99.9, author = {"琼瑶", "dlei"})
class BookStore{

    @Bookk(value = "《三少爷的剑》", price = 399.9, author = {"古龙", "熊耀华"})
    public void test(){
    }
}
```

输出：

```
《情深深雨濛濛》
99.9
[琼瑶, dlei]
《三少爷的剑》
399.9
[古龙, 熊耀华]

Process finished with exit code 0
```



### 3.5 注解的应用场景一：junit框架

案例：**模拟Junit框架**

需求：定义若干个方法，只要加了MyTest注解，就可以在启动时被触发执行

分析：

- 定义一个自定义注解MyTest，只能注解方法，存活范围是一直都在。
- 定义若干个方法，只要有@MyTest注解的方法就能在启动时被触发执行，没有这个注解的方法不能执行。



**代码：**

MyTest注解类：

```java
@Target({ElementType.METHOD}) 		// 元注解
@Retention(RetentionPolicy.RUNTIME) 		// 一直活着，在运行阶段这个注解也不消失
public @interface MyTest {
}
```

```java
public class AnnotationDemo4 {
    //未定义注解的不被调用
    public void test1(){
        System.out.println("===test1===");
    }

    @MyTest
    public void test2(){
        System.out.println("===test2===");
    }

    @MyTest
    public void test3(){
        System.out.println("===test3===");
    }

    /**
      启动菜单：有注解的才被调用。
     */
    public static void main(String[] args) throws Exception {
        AnnotationDemo4 t = new AnnotationDemo4();
        // a.获取类对象
        Class c = AnnotationDemo4.class;
        // b.提取全部方法
        Method[] methods = c.getDeclaredMethods();
        // c.遍历方法，看是否有MyTest注解，有就跑它
        for (Method method : methods) {
            if(method.isAnnotationPresent(MyTest.class)){
                // 跑它
                method.invoke(t);
            }
        }
    }
}
```

输出：

```
===test2===
===test3===

Process finished with exit code 0
```





## 4. 动态代理

### 4.1 准备案例、提出问题

**案例：**模拟企业业务功能开发，并完成每个功能的性能统计

**需求：**模拟某企业用户管理业务，需包含用户登录，用户删除，用户查询功能，并要统计每个功能的耗时。

**分析：**

1. 定义一个UserService表示用户业务接口，规定必须完成用户登录，用户删除，用户查询功能。
2. 定义一个实现类UserServiceImpl实现UserService，并完成相关功能，且统计每个功能的耗时。
3. 定义测试类，创建实现类对象，调用方法。



**代码：**

UserService接口类：

```java
public interface UserService {
    String login(String loginName , String passWord) ;
    void selectUsers();
    boolean deleteUsers();
    void updateUsers();
}
```

UserServiceImpl实现类：

```java
public class UserServiceImpl implements UserService{
    @Override
    public String login(String loginName, String passWord)  {
        long startTimer = System.currentTimeMillis();
        try {
            Thread.sleep(1000);
            if("admin".equals(loginName) && "1234".equals(passWord)) {
                return "success";
            }
            return "登录名和密码可能有误";
        } catch (Exception e) {
            e.printStackTrace();
            return "error";
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("login方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }



    }

    @Override
    public void selectUsers() {
        long startTimer = System.currentTimeMillis();
        System.out.println("查询了100个用户数据！");
        try {
            Thread.sleep(2000);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("selectUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }

    @Override
    public boolean deleteUsers() {
        long startTimer = System.currentTimeMillis();
        try {
            System.out.println("删除100个用户数据！");
            Thread.sleep(500);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("deleteUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }

    @Override
    public void updateUsers() {
        long startTimer = System.currentTimeMillis();
        try {
            System.out.println("修改100个用户数据！");
            Thread.sleep(2500);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            long endTimer = System.currentTimeMillis();
            System.out.println("updateUsers方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");
        }
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        UserService userService = new UserServiceImpl();
        System.out.println(userService.login("admin", "1234"));
        System.out.println(userService.deleteUsers());
        userService.selectUsers();
        userService.updateUsers();
    }
}
```

输出：

```
login方法耗时：1.0s
success
删除100个用户数据！
deleteUsers方法耗时：0.501s
true
查询了100个用户数据！
selectUsers方法耗时：2.001s
修改100个用户数据！
updateUsers方法耗时：2.5s

Process finished with exit code 0
```



**本案例存在的问题**

- 业务对象的的每个方法都要进行时间性能统计，存在大量重复的代码。



### 4.2 使用动态代理解决问题

**动态代理**

- 代理就是被代理者没有能力或者不愿意去完成某件事情，需要找个人代替自己去完成这件事，动态代理就是用来对业务功能（方法）进行代理的。

**关键步骤**

1. 必须有接口，实现类要实现接口（代理通常是基于接口实现的）。
2. 创建一个实现类的对象，该对象为业务对象，紧接着为业务对象做一个代理对象。



**案例优化：**

UserService接口类不变

UserServiceImpl实现类中有关性能分析的代码全都删掉，只保留真正的业务功能

```java
public class UserServiceImpl implements UserService{
    @Override
    public String login(String loginName, String passWord)  {
        try {
            Thread.sleep(1000);
        } catch (Exception e) {
            e.printStackTrace();
        }
        if("admin".equals(loginName) && "1234".equals(passWord)) {
            return "success";
        }
        return "登录名和密码可能有毛病";
    }

    @Override
    public void selectUsers() {
        System.out.println("查询了100个用户数据！");
        try {
            Thread.sleep(2000);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public boolean deleteUsers() {
        try {
            System.out.println("删除100个用户数据！");
            Thread.sleep(500);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    @Override
    public void updateUsers() {
        try {
            System.out.println("修改100个用户数据！");
            Thread.sleep(2500);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

新增ProxyUtil代理类

```java
/**
    public static Object newProxyInstance(ClassLoader loader,  Class<?>[] interfaces, InvocationHandler h)
    参数一：类加载器，负责加载代理类到内存中使用。
    参数二：获取被代理对象实现的全部接口。代理要为全部接口的全部方法进行代理
    参数三：代理的核心处理逻辑
 */
public class ProxyUtil {
    /**
      生成业务对象的代理对象。
     * @param obj
     * @return
     */
    
    //写成泛型，任何类型的对象都能通用
    public static <T> T  getProxy(T obj) {
        // 返回了一个代理对象了
        return (T)Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                obj.getClass().getInterfaces(),
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        // 参数一：代理对象本身。一般不管
                        // 参数二：正在被代理的方法
                        // 参数三：被代理方法，应该传入的参数
                       long startTimer = System .currentTimeMillis();
                        // 马上触发方法的真正执行。(触发真正的业务功能)
                        Object result = method.invoke(obj, args);

                        long endTimer = System.currentTimeMillis();
                        System.out.println(method.getName() + "方法耗时：" + (endTimer - startTimer) / 1000.0 + "s");

                        // 把业务功能方法执行的结果返回给调用者
                        return result;
                    }
                });
    }
}
```

调用代理执行：

```java
public class Test {
    public static void main(String[] args) {
        // 1、把业务对象，直接做成一个代理对象返回，代理对象的类型也是 UserService类型
        UserService userService = ProxyUtil.getProxy(new UserServiceImpl());
        //一调用方法就走代理
        System.out.println(userService.login("admin", "1234"));
        System.out.println(userService.deleteUsers());
        userService.selectUsers();
        userService.updateUsers();
    }
}
```

输出结果与上面一致



**动态代理的优点：**

- 非常的灵活，支持任意接口类型的实现类对象做代理，也可以直接为接本身做代理。
- 可以为被代理对象的所有方法做代理。
- 可以在不改变方法源码的情况下，实现对方法功能的增强。
- 不仅简化了编程工作、提高了软件系统的可扩展性，同时也提高了开发效率。



**动态代理的思想：**

AOP是Aspect Oriented Programming，即面向切面编程。

那什么是AOP？

我们先回顾一下OOP：Object Oriented Programming，OOP作为面向对象编程的模式，获得了巨大的成功，OOP的主要功能是数据封装、继承和多态。

而AOP是一种新的编程方式，它和OOP不同，OOP把系统看作多个对象的交互，AOP把系统分解为不同的关注点，或者称之为切面（Aspect）。





# XML、XML解析、设计模式等

## 1. XML

### 1.1 XML概述

- XML是可扩展标记语言（eXtensible Markup Language）的缩写，它是是一种数据表示格式，可以描述非常复杂的数据结构，常用于传输和存储数据。

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <data>
          <sender>张三</sender>
          <receiver>李四</receiver>    
          <src>
                   <addr>北京</addr>
                   <date>2022-11-11 11:11:11</date>
          </src>
          <current>武汉</current>
          <dest>广州</dest>
  </data>
  ```

  



**XML的几个特点和使用场景**

- 一是纯文本，默认使用UTF-8编码；二是可嵌套；
- 如果把XML内容存为文件，那么它就是一个XML文件。
- XML的使用场景：**XML内容经常被当成消息进行网络传输，或者作为配置文件用于存储系统的信息。 **



### 1.2 XML的语法规则

- XML文件的后缀名为：==xml==
- 文档声明必须是==第一行==

```
<?xml version="1.0" encoding="UTF-8" ?>
version：XML默认的版本号码、该属性是必须存在的
encoding：本XML文件的编码
```

**XML的其他组成**

- XML文件中可以定义注释信息：<!-- 注释内容 -->

- XML文件中可以存在以下特殊字符

  ```
  &lt;    <  小于
  &gt;    >  大于
  &amp;   &  和号
  &apos;  '  单引号
  &quot;  "  引号
  ```

- XML文件中可以存在CDATA区: ==<![CDATA[   …内容…  ]]>==



### 1.3 XML文档约束方式

- DTD约束
  - 可以约束XML文件的编写。
  - ==不能约束具体的数据类型。==

- schema约束
  - schema可以约束==具体的数据类型==，约束能力上更强大。
  - schema==本身也是一个xml文件==，本身也受到其他约束文件的要求，所以编写的更加严谨



## 2. XML解析技术

- 使用程序读取XML中的数据

### 2.1 XML解析技术概述

XML的数据的作用是什么，最终需要怎么处理？

- 存储数据、做配置信息、进行数据传输。
- 最终需要被程序进行读取，解析里面的信息。

**两种解析方式**

- SAX解析
- ==DOM解析==

**Dom常见的解析工具**

|   名称    |                             说明                             |
| :-------: | :----------------------------------------------------------: |
|   JAXP    |               SUN公司提供的一套XML的解析的API                |
|   JDOM    | JDOM是一个开源项目，它基于树型结构，利用纯JAVA的技术对XML文档实现解析、生成、序列化以及多种操作。 |
| ==dom4j== | 是JDOM的升级品，用来读写XML文件的。具有性能优异、功能强大和极其易使用的特点，它的性能超过sun公司官方的dom 技术，同时它也是一个开放源代码的软件，Hibernate也用它来读写配置文件。 |
|   jsoup   |    功能强大DOM方式的XML解析开发包，尤其对HTML解析更加方便    |



**DOM解析解析文档对象模型**

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ab9ac8c3d86712e61c6c3a1109e534e0.png)



### 2.2 Dom4J解析XML文件

**Dom4j解析XML-得到Document对象**

|        构造器/方法        |            说明             |
| :-----------------------: | :-------------------------: |
|    public SAXReader()     |    创建Dom4J的解析器对象    |
| Document read(String url) | 加载XML文件成为Document对象 |

**Document类**

|          方法名          |      说明      |
| :----------------------: | :------------: |
| Element getRootElement() | 获得根元素对象 |

**示例：**

```java
public class Dom4jDemo1 {
    @Test
    public void parseXMLdata() throws Exception {
        //1、创建一个Dom4j的解析器对象，代表了整个dom4j框架
        SAXReader saxReader = new SAXReader();
        //2、把XML文件加载到内存中成为一个Document文档对象
        //Document document = saxReader.read(new File("xml\\src\\Contacts.xml")); //这样并不好，需要通过模块名去定位文件位置

        //用类下的API：getResourceAsStream，该API中的 / 是直接去src下寻找文件（即使改了模块名也能找到）
        InputStream is = Dom4jDemo1.class.getResourceAsStream("/Contacts.xml");
        Document document = saxReader.read(is);
        //3、获取根元素对象
        Element root = document.getRootElement();
        System.out.println(root.getName());     //contactList
    }
}
```



### 2.3 Dom4J解析XML文件中的各种节点

**Dom4j解析XML的元素、属性、文本**

|               方法名                |                             说明                             |
| :---------------------------------: | :----------------------------------------------------------: |
|      List<Element> elements()       |                   得到当前元素下所有子元素                   |
| List<Element> elements(String name) |            得到当前元素下指定名字的子元素返回集合            |
|    Element element(String name)     | 得到当前元素下指定名字的子元素,如果有很多名字相同的返回第一个 |
|          String getName()           |                         得到元素名字                         |
| String attributeValue(String name)  |                   通过属性名直接得到属性值                   |
|    String elementText(子元素名)     |                  得到指定名称的子元素的文本                  |
|          String getText()           |                           得到文本                           |



**示例：**

```java
public class Dom4jDemo1 {
    @Test
    public void parseXMLdata() throws Exception {
        //1、创建一个Dom4j的解析器对象，代表了整个dom4j框架
        SAXReader saxReader = new SAXReader();
        //2、把XML文件加载到内存中成为一个Document文档对象
        //Document document = saxReader.read(new File("xml\\src\\Contacts.xml")); //这样并不好，需要通过模块名去定位文件位置

        //用类下的API：getResourceAsStream，该API中的 / 是直接去src下寻找文件（即使改了模块名也能找到）
        InputStream is = Dom4jDemo1.class.getResourceAsStream("/Contacts.xml");
        Document document = saxReader.read(is);
        //3、获取根元素对象
        Element root = document.getRootElement();
        System.out.println(root.getName());     //contactList

        //4、获取根元素下的全部子元素对象(一级)
        List<Element> sonEles = root.elements();
        for (Element sonEle : sonEles) {
            System.out.println(sonEle.getName());
        }       //contact contact contact user

        //a. 获取某个子元素
        Element userEle = root.element("user");
        System.out.println(userEle.getName());      //user

        //b. 获取子元素对象（默认提取第一个）
        Element contact = root.element("contact");
        System.out.println(contact.elementText("name"));    //   潘金莲
        //前后去空格
        System.out.println(contact.elementTextTrim("name"));    //潘金莲

        //c. 根据元素获取属性值
        Attribute idAttr = contact.attribute("vip");
        System.out.println(idAttr.getName() + "-->" + idAttr.getValue());   //vip-->true
        //也可以直接获取属性值
        System.out.println(contact.attributeValue("id"));   //1

        //d. 获取当前元素下子元素对象
        Element email = contact.element("email");
        System.out.println(email.getText());        //panpan@itcast.cn
    }
}
```





### 2.4 Dom4J解析XML文件-案例实战

需求：利用Dom4J的知识，将Contact.xml文件中的联系人数据封装成List集合，其中每个元素是实体类Contact。打印输出 List 中的每个元素。

**代码：**

Contact类

```java
/*
    <contact id="1" vip="true">
    <name>   潘金莲  </name>
    <gender>女</gender>
    <email>panpan@itcast.cn</email>
    </contact>
*/

    @Getter
    @Setter
    @AllArgsConstructor
    @NoArgsConstructor
    @ToString
public class Contact {
    private String name;
    private int id;
    private boolean vip;
    private char gender;
    private String email;
}
```

```java
public class Dom4jTest2 {
    @Test
    public void parseToList() throws Exception{
        //1、创建SaxReader对象
        SAXReader saxReader = new SAXReader();
        //2、加载XML文件成为文档对象Document对象
        Document document = saxReader.read(Dom4jTest2.class.getResourceAsStream("/Contacts.xml"));
        //3、先拿根元素
        Element root = document.getRootElement();
        //4、提取contact子元素
        List<Element> contactEles = root.elements("contact");
        //5、准备ArrayList集合封装联系人信息
        List<Contact> contacts = new ArrayList<>();
        //6、遍历contact子元素
        for (Element contactEle : contactEles) {
            //7、每个子元素都是一个联系人对象
            Contact contact = new Contact();
            contact.setId(Integer.valueOf(contactEle.attributeValue("id")));
            contact.setVip(Boolean.valueOf(contactEle.attributeValue("vip")));
            contact.setName(contactEle.attributeValue("name"));
            contact.setGender(contactEle.elementTextTrim("gender").charAt(0));
            contact.setEmail(contactEle.elementText("email"));
            //8、把联系人对象数据加入到List集合
            contacts.add(contact);
        }
        //9、遍历List集合
        for (Contact contact : contacts) {
            System.out.println(contact);
        }
    }
}
```

输出：

```
Contact(name=null, id=1, vip=true, gender=女, email=panpan@itcast.cn)
Contact(name=null, id=2, vip=false, gender=男, email=wusong@itcast.cn)
Contact(name=null, id=3, vip=false, gender=男, email=wuda@itcast.cn)

Process finished with exit code 0
```





## 3. XML检索技术：Xpath

**XPath介绍**

- XPath在解析XML文档方面提供了一独树一帜的路径思想，更加优雅，高效
- XPath使用==路径表达式==来定位XML文档中的元素节点或属性节点。

**示例**

- /元素/子元素/孙元素
- //子元素//孙元素

**Document中与XPath相关的API：**

|              方法名              |           说明           |
| :------------------------------: | :----------------------: |
| Node selectSingleNode("表达式")  | 获取符合表达式的唯一元素 |
| List<Node> selectNodes("表达式") | 获取符合表达式的元素集合 |



**XPath的四大检索方案**

- 绝对路径

  - 采用绝对路径获取从根节点开始逐层的查找/contactList/contact/name节点列表并打印信息

    |        方法名         |                   说明                   |
    | :-------------------: | :--------------------------------------: |
    | /根元素/子元素/孙元素 | 从根元素开始，一级一级向下查找，不能跨级 |

    

- 相对路径

  - 先得到根节点contactList

  - 再采用相对路径获取下一级contact 节点的name子节点并打印信息

    |     方法名      |                    说明                    |
    | :-------------: | :----------------------------------------: |
    | ./子元素/孙元素 | 从当前元素开始，一级一级向下查找，不能跨级 |

    

- 全文检索

  - 直接全文搜索所有的name元素并打印

    |     方法名      |                            说明                            |
    | :-------------: | :--------------------------------------------------------: |
    |    //contact    |               找contact元素，无论元素在哪里                |
    | //contact/name  |    找contact，无论在哪一级，但name一定是contact的子节点    |
    | //contact//name | contact无论在哪一种，name只要是contact的子孙元素都可以找到 |

    

- 属性查找

  - 在全文中搜索属性，或者带属性的元素

    |         方法名         |                            说明                            |
    | :--------------------: | :--------------------------------------------------------: |
    |       //@属性名        |     查找属性对象，无论是哪个元素，只要有这个属性即可。     |
    |    //元素[@属性名]     |         查找元素对象，全文搜索指定元素名和属性名。         |
    | //元素//[@属性名=‘值’] | 查找元素对象，全文搜索指定元素名和属性名，并且属性值相等。 |

    

```java
public class XPathDemo {
    /**
     1.绝对路径: /根元素/子元素/子元素。
     */
    @Test
    public void parse01() throws Exception {
        // a、创建解析器对象
        SAXReader saxReader = new SAXReader();
        // b、把XML加载成Document文档对象
        Document document =
                saxReader.read(XPathDemo.class.getResourceAsStream("/Contacts2.xml"));
        // c、检索全部的名称
        List<Node> nameNodes = document.selectNodes("/contactList/contact/name");
        for (Node nameNode : nameNodes) {
            Element  nameEle = (Element) nameNode;
            System.out.println(nameEle.getTextTrim());
        }
    }
    /*
    输出：
        潘金莲
        武松
        武大狼
    */


    /**
     2.相对路径： ./子元素/子元素。 (.代表了当前元素)
     */
    @Test
    public void parse02() throws Exception {
        // a、创建解析器对象
        SAXReader saxReader = new SAXReader();
        // b、把XML加载成Document文档对象
        Document document =
                saxReader.read(XPathDemo.class.getResourceAsStream("/Contacts2.xml"));
        Element root = document.getRootElement();
        // c、检索全部的名称
        List<Node> nameNodes = root.selectNodes("./contact/name");
        for (Node nameNode : nameNodes) {
            Element  nameEle = (Element) nameNode;
            System.out.println(nameEle.getTextTrim());
        }
    }
    /*
    输出：
        潘金莲
        武松
        武大狼
    */


    /**
     3.全文搜索：
     //元素  在全文找这个元素
     //元素1/元素2  在全文找元素1下面的一级元素2
     //元素1//元素2  在全文找元素1下面的全部元素2
     */
    @Test
    public void parse03() throws Exception {
        // a、创建解析器对象
        SAXReader saxReader = new SAXReader();
        // b、把XML加载成Document文档对象
        Document document =
                saxReader.read(XPathDemo.class.getResourceAsStream("/Contacts2.xml"));
        // c、检索数据
        //List<Node> nameNodes = document.selectNodes("//name");    //找所有name
        // List<Node> nameNodes = document.selectNodes("//contact/name");   //找contact下所有的一级name元素
        List<Node> nameNodes = document.selectNodes("//contact//name");     //找contact下所有name元素
        for (Node nameNode : nameNodes) {
            Element  nameEle = (Element) nameNode;
            System.out.println(nameEle.getTextTrim());
        }
    }
    /*
    输出：
        潘金莲
        武松
        武大狼
        我是西门庆
    */

    
    /**
     4.属性查找。
     //@属性名称  在全文检索属性对象。
     //元素[@属性名称]  在全文检索包含该属性的元素对象。
     //元素[@属性名称=值]  在全文检索包含该属性的元素且属性值为该值的元素对象。
     */
    @Test
    public void parse04() throws Exception {
        // a、创建解析器对象
        SAXReader saxReader = new SAXReader();
        // b、把XML加载成Document文档对象
        Document document =
                saxReader.read(XPathDemo.class.getResourceAsStream("/Contacts2.xml"));
        // c、检索数据
        List<Node> nodes = document.selectNodes("//@id");
        for (Node node : nodes) {
            Attribute attr = (Attribute) node;
            System.out.println(attr.getName() + "===>" + attr.getValue());
        }

        // 查询name元素（包含id属性的）
//      Node node = document.selectSingleNode("//name[@id]");
        Node node = document.selectSingleNode("//name[@id=888]");   //name下包含id=888的元素
        Element ele = (Element) node;
        System.out.println(ele.getTextTrim());      //我是西门庆
    }
    /*
    输出：
        id===>1
        id===>2
        id===>3
        id===>888
        我是西门庆
    */
}
```





## 4. 设计模式

### 4.1 工厂模式

**什么是工厂设计模式？**

- 之前我们创建类对象时, 都是使用new 对象的形式创建,在很多业务场景下也提供了不直接new的方式 。
- 工厂模式（Factory Pattern）是 Java 中最常用的设计模式之一，  这种类型的设计模式属于创建型模式，它提供了一种获取对象的方式。



**工厂设计模式的作用：**

- 工厂的方法可以封装对象的创建细节，比如：为该对象进行加工和数据注入。
- ==可以实现类与类之间的解耦操作（核心思想）。==





**示例：**

定义一个Computer抽象类：

```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public abstract class Computer {
    private String name;
    private double price;

    public abstract void start();
}
```

定义Huawei和Mac类，继承Computer类，并重写里面的方法：

```java
public class Huawei extends Computer{
    @Override
    public void start() {
        System.out.println(getName() + "开机了，展示了华为的菊花图标~~~~");
    }
}
```

```java
public class Mac extends Computer{
    @Override
    public void start() {
        System.out.println(getName() + "以非常优雅的方法启动了，展示了一个苹果logo");
    }
}
```

设计一个工厂类，负责生产电脑，用户需要什么直接调用即可：

```java
public class FactoryPattern {
    /**
       定义一个方法，创建对象返回
     */
    public static Computer createComputer(String info){
        switch (info){
            case "huawei":
                Computer c = new Huawei();
                c.setName("huawei pro 16");
                c.setPrice(5999);
                return c;
            case "mac":
                Computer c2 = new Mac();
                c2.setName("MacBook pro");
                c2.setPrice(11999);
                return c2;
            default:
                return null;
        }
    }
}
```

```java
public class FactoryDemo {
    public static void main(String[] args) {
        Computer c1 = FactoryPattern.createComputer("huawei");
        c1.start();

        Computer c2 = FactoryPattern.createComputer("mac");
        c2.start();
    }
}
```



==工厂模式的思想是提供一个工厂方法返回对象，用户需要哪个对象就直接调用方法返回。==





### 4.2 装饰模式

**什么是装饰设计模式？**

- 创建一个新类，包装原始类，从而在新类中提升原来类的功能。

**装饰设计模式的作用：**

- 作用：装饰模式指的是在不改变原类的基础上, 动态地扩展一个类的功能。

- 例如：

  ```
  InputStream(抽象父类)
  FileInputStream(实现子类，读写性能较差)
  BufferedInputStream(实现子类，装饰类，读写性能高)
  ```

**基本思路：**

1. 定义父类。
2. 定义原始类，继承父类，定义功能。
3. 定义装饰类，继承父类，包装原始类，增强功能！！



**示例：**

定义抽象父类InputStream

```java
/**
   共同父类
 */
public abstract class InputStream {
    public abstract int read();
    public abstract int read(byte[] buffer);
}
```

定义原始类FileInputStream，继承父类InputStream，定义基本功能

```java
/**
   原始类
 */
public class FileInputStream extends InputStream{
    @Override
    public int read() {
        System.out.println("低性能的方式读取了一个字节a");
        return 97;
    }

    @Override
    public int read(byte[] buffer) {
        buffer[0] = 97;
        buffer[1] = 98;
        buffer[2] = 99;
        System.out.println("低性能的方式读取了一个字节数组：" + Arrays.toString(buffer));
        return 3;
    }
}
```

定义装饰类BufferedInputStream，继承父类InputStream，包装原始类，增强功能

```java
/**
   装饰类：继承InputStream 拓展原始类的功能
 */
public class BufferedInputStream extends InputStream{
    private InputStream is;
    public BufferedInputStream(InputStream is){
        this.is = is;
    }
    @Override
    public int read() {
        System.out.println("提供8KB的缓冲区，提高读数据性能~~~~");
        return is.read();
    }

    @Override
    public int read(byte[] buffer) {
        System.out.println("提供8KB的缓冲区，提高读数据性能~~~~");
        return is.read(buffer);
    }
}
```

```java
public class DecoratorPattern {
    public static void main(String[] args) {
        InputStream is = new BufferedInputStream(new FileInputStream());
        System.out.println(is.read());
        System.out.println(is.read(new byte[3]));
    }
}
```

输出：

```
提供8KB的缓冲区，提高读数据性能~~~~
低性能的方式读取了一个字节a
97
提供8KB的缓冲区，提高读数据性能~~~~
低性能的方式读取了一个字节数组：[97, 98, 99]
3

Process finished with exit code 0
```







