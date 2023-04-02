## 1. Java 字节码（.class）

什么是字节码：

- 在 Java 中，JVM 可以理解的代码叫做字节码（即扩展名为 `.class` 的文件）；

- 由于字节码并不针对一种特定的机器，Java 程序 **无须重新编译** 便可在多种不同操作系统的计算机上运行；

- **Java 程序从源代码到运行的过程如下图所示**：

  ![Java程序转变为机器代码的过程](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210171945116.png)



采用字节码的好处：

- `.class->机器码` 这一步 JVM 类加载器 **首先加载字节码文件，然后通过解释器逐行解释执行**，这种方式的执行速度会相对比较慢；
- 而且，有些方法和代码块是需要被频繁调用的 (也就是所谓的热点代码)，所以后面引进了 **JIT（just-in-time compilation） 编译器**，而 JIT 属于 **运行时编译**。
- 当 JIT 编译器完成第一次编译后，其会 **将字节码对应的机器码保存下来**，下次可以直接使用。而机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 **Java 是编译与解释共存的语言** 。



## 2. 为什么说 Java 是编译与解释共存的语言？

编译型语言和解释型语言的区别：

- **编译型**：通过编译器将源代码一次性翻译成可被该平台执行的机器码。执行速度比较快，开发效率比较低；
- **解释型**：解释型语言会通过解释器一句一句的将代码解释（interpret）为机器代码后再执行。开发效率比较快，执行速度比较慢；

![编译型语言和解释型语言](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210171950541.png)

因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class` 文件），这种字节码必须由 Java 解释器来解释执行。所以说 **Java 语言 “编译与解释并存”**。



## 3. 基本数据类型

### 3.1 八种数据类型

Java 中有 8 种基本数据类型，分别为：

- 6 种数字类型：
  - 4 种整数型：`byte`、`short`、`int`、`long`；
  - 2 种浮点型：`float`、`double`；
- 1 种字符型：`char`（`String` 不是基本数据类型）；
- 1 种布尔型：`boolean`；

这 8 种基本数据类型的默认值以及所占空间的大小如下：

| 基本类型  | 位数 | 字节 | 默认值  | 取值范围                                   |
| :-------- | :--- | :--- | :------ | ------------------------------------------ |
| `byte`    | 8    | 1    | 0       | -128 ~ 127                                 |
| `short`   | 16   | 2    | 0       | -32768 ~ 32767                             |
| `int`     | 32   | 4    | 0       | -2147483648 ~ 2147483647                   |
| `long`    | 64   | 8    | 0L      | -9223372036854775808 ~ 9223372036854775807 |
| `char`    | 16   | 2    | 'u0000' | 0 ~ 65535                                  |
| `float`   | 32   | 4    | 0f      | 1.4E-45 ~ 3.4028235E38                     |
| `double`  | 64   | 8    | 0d      | 4.9E-324 ~ 1.7976931348623157E308          |
| `boolean` | 1    |      | false   | true、false                                |

这八种基本类型都有对应的包装类分别为：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 。

### 3.2 基本类型和包装类型的区别

- 包装类型 **默认值是 `null`** ，而基本类型有默认值且不是 `null`。
- 包装类型可用于 **泛型**，而基本类型不可以。
- 基本数据类型的 **局部变量** 存放在 Java 虚拟机 **栈** 中的局部变量表，基本数据类型的 **成员变量**（未被 `static` 修饰&在方法外面的变量 ）存放在 Java 虚拟机的 **堆** 中。**包装类型属于对象类型**，我们知道几乎所有对象实例都存在于 **堆** 中。
- 相比于对象类型， 基本数据类型占用的空间非常小。



### 3.3 包装类型的缓存机制

Java 基本数据类型的 **包装类型** 的大部分都用到了 **缓存机制来提升性能**。

`Byte`，`Short`，`Integer`，`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的 **缓存数据**，`Character` 创建了数值在 **[0,127]** 范围的 **缓存数据**。

`Float`、`Double` 没有缓存数据，`Boolean` 直接返回 `True` or `False`，也没有缓存数据。



什么时候会用到包装类型的缓存：

- 只有在发生 **自动装箱** 的过程，才会使用缓存；
- 自动装箱（就是编译器自动调用包装类的 `valueOf` 方法）；

而直接 `new` 出来的包装类型数据，无论值是多少，都不会用到缓存。不过 JDK 9 之后就弃用包装类型的构造方法了，所以现在一般都是直接基本类型进行赋值或使用 `valueOf()` 方法。所以一定要注意这个缓存问题；

```java
class Test {
    public static void main(String[] args) {
        Integer a = 200;
        Integer b = 200;
        System.out.println(a == b);     // false

        Integer c = 100;
        Integer d = 100;
        System.out.println(c == d);     // true
    }
}
```



在自动装箱创建包装类时：

- 如果数值在 **缓存数据的范围内**，则直接从缓存（常量池）中取值，**不会创建对象**；

- 如果数值 **超出对应范围**，则会去 **创建新的对象**，**故所有整型包装类对象之间值的比较，全部使用 equals 方法比较**。

  ![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210172027854.png)

原理：

- 当包装类加载时，该包装类中的内部类 `xxCache` 会初始化一个 **包装类类型数组**，最小值（固定值）为 -128，而最大值（默认值）为 127【可修改】，这个长度的缓存值放在 **方法区的常量池** 中，是所有 **线程共享** 的。
- 当发生自动包装的时候，调用 `valueOf()` 方法，对需要包装的基本类型的值进行判断，如果在缓存值的范围内，则返回缓存的对象，否则创建一个新的对象返回。



### 3.4 自动装箱与拆箱

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

**装箱** 其实就是调用了包装类的 `valueOf()` 方法，**拆箱** 其实就是调用了 `xxxValue()` 方法。

因此：

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`
- `int n = i` 等价于 `int n = i.intValue()`；（还可转为其他基本数据类型：`i.doubleValue()`、`i.shortValue()`）

`Xxx.parseXxx(String str)` 是用于将字符串转为对应的 `xxx` 类型（基本类型），若转后的数值与 `Xxx` 类型冲突，则会报错。



### 3.5 浮点数运算时精度丢失的风险

浮点数运算精度丢失代码演示：

```java
float a = 2.0f - 1.9f;
float b = 1.8f - 1.7f;
System.out.println(a);		// 0.100000024
System.out.println(b);		// 0.099999905
System.out.println(a == b);		// false
```

为什么会出现这个问题呢？

- 这个和计算机保存浮点数的机制有很大关系。**计算机是二进制** 的，而且计算机在表示一个数字时，**宽度是有限** 的，**无限循环的小数存储在计算机时，只能被截断**，所以就会导致 **小数精度发生损失** 的情况。这也就是解释了为什么 **浮点数没有办法用二进制精确表示**。



怎么解决这个问题呢？

- `BigDecimal` 可以实现对浮点数的运算，不会造成精度丢失。通常情况下，大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过 `BigDecimal` 来做的。

  ```java
  BigDecimal a = new BigDecimal("1.0");
  BigDecimal b = new BigDecimal("0.9");
  BigDecimal c = new BigDecimal("0.8");
  
  BigDecimal x = a.subtract(b);
  BigDecimal y = b.subtract(c);
  
  System.out.println(x);	 /* 0.1 */
  System.out.println(y);	 /* 0.1 */
  System.out.println(Objects.equals(x, y));	 /* true */
  ```



> BigDecimal 详解

在创建 `BigDecimal` 时的常见问题：

- 为了防止精度丢失，禁止使用构造方法 `BigDecimal(double)`，而推荐使用它的 `BigDecimal(String val)` 构造方法或者 `BigDecimal.valueOf(double val)` 静态方法来创建对象。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210172057316.png)



等值比较问题：

- `equals()` 方法不仅仅会比较值的大小（value）**还会比较精度**（scale）；
- 而 `compareTo()` 方法比较的时候会忽略精度。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210172058993.png)



### 3.6 超过 long 整型的数据应该如何表示

基本数值类型都有一个表达范围，如果超过这个范围就会有数值溢出的风险。

在 Java 中，64 位 long 整型是最大的整数类型。

```java
long l = Long.MAX_VALUE;
System.out.println(l + 1);	 // -9223372036854775808
System.out.println(l + 1 == Long.MIN_VALUE);	 // true
```

对于超过最大整数类型的数，可以使用 `BigInteger` ，它内部使用 **`int[]` 数组** 来存储 **任意大小** 的整形数据。

相对于常规整数类型的运算来说，`BigInteger` 运算的效率会相对较低。



## 4. 面向对象基础

### 4.1 多态

多态是指 **一个行为具有多种不同的表现形式或形态**。

多态存在的三个必要条件：

- 继承；
- 重写；
- **父类引用指向子类对象**。

例如：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202303081049083.jpeg)

多态中的成员特点：

- 成员 **变量**：**编译、运行都看左边**，即访问变量时，默认先访问父类中的变量值；
- 成员 **方法**：**编译看左边，运行看右边**，即访问方法时，真正运行过程中优先调用子类的方法。

多态的转型：

- 向上转型：多态本身就是一个向上转型的过程；
- 向下转型：需要使用强制类型转换：`Dog dog = (Dog) animal`。

### 4.2 构造方法有哪些特点？是否可被 override?

构造方法特点如下：

- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 生成类的对象时自动执行，无需调用。

构造方法 **不能被 override**（重写），但是 **可以 overload**（重载），所以可以看到一个类中有多个构造函数的情况。

### 4.3 重写和重载的区别

区别：

- 重载发生在 **一个类** 里面，发生在 **编译时**。**方法名相同，参数列表不同**（参数类型，个数、顺序不同），方法的返回值和修饰符可以不同（因此 **不能以返回值和修饰符来判断是否为重载**）。
- 重写发生在 **父类与子类或接口与实现类之间**，发生在 **运行时**。**方法名相同，参数列表相同**，返回值的范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。如果父类方法访问修饰符为 private 则不能重写该方法（因为都无法继承这个方法）。

|                        | 重载                           | 重写                                                         |
| ---------------------- | ------------------------------ | ------------------------------------------------------------ |
| 多态的表现形式         | 编译期间的活动                 | 运行期间的活动                                               |
| static 方法            | 可以                           | 不可以                                                       |
| final 方法             | 可以                           | 不可以                                                       |
| 跨包，修饰符缺省的方法 | 可以                           | 不可以                                                       |
| 参数列表               | 必须修改                       | 不能修改                                                     |
| 返回类型               | 无法以返回值类型作为区分标准   | 与被重写方法的返回类型可以不相同，但是 **必须是父类返回值的派生类** |
| 访问权限               | 被重载的方法可以改变访问修饰符 | 访问权限不能比父类中被重写的方法的访问权限更低               |

重载关心的只是 **参数**：有参无参，参数类型不同，参数数量不同，不同类型的参数顺序不同，都可以实现方法的重载。

重写的目的在于根据创造对象的类型不同而表现出多态。因为 **静态方法无需创建对象即可使用**，没有对象，重写所需要的 “对象类型” 这一要素不存在，而且静态方法在编译时就已经确定了，因此 **无法被重写**。

### 4.4 接口和抽象类有什么共同点和区别？

**接口** 的概念与特征：

- **一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为（功能）**；
- 接口中每一个方法也是隐式抽象的，接口中的方法会被 **隐式** 的指定为 **public abstract**；
- 接口中可以含有变量，但是接口中的变量会被隐式的指定为 **public static final** 变量；



**抽象类** 的使用原则如下：

1. 抽象方法必须为 public 或者 protected（因为如果为 private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为 public；
2. 抽象类不能直接实例化，需要依靠子类采用向上转型的方式处理；
3. 抽象类必须有子类，使用 extends 继承，一个子类只能继承一个抽象类；
4. 子类（如果不是抽象类）则必须覆写抽象类之中的全部抽象方法（如果子类没有实现父类的抽象方法，则必须将子类也定义为为 abstract 类。）；



**共同点**：

- 都 **不能被实例化**。
- 都可以 **包含抽象方法**。
- 都可以有 **默认实现的方法**（Java 8 可以用 `default` 关键字在接口中定义默认方法）。



**区别**：

- 接口主要用于对类的行为进行约束，实现了某个接口就具有了对应的行为；抽象类主要用于代码复用，强调的是所属关系。
- 一个类只能继承一个类，但是可以实现多个接口。
- **接口** 中的 **成员变量** 只能是 `public static final` 类型的，**不能被修改且必须有初始值**；而抽象类的成员变量默认 default，**可在子类中被重新定义**，也 **可被重新赋值**。
- 抽象类中的方法可以有方法体，就是能实现方法的具体功能，但是接口中的方法不行。
  - ***JDK 1.8 以后，接口里可以有静态方法和方法体了***。
  - ***JDK 1.8 以后，接口允许包含具体实现的方法，该方法称为 "默认方法"，默认方法使用 default 关键字修饰***。
  - ***JDK 1.9 以后，允许将接口中的方法定义为 private，使得某些复用的代码不会把方法暴露出去***。



### 4.5 hashCode 与 equals

什么是 HashCode：

- hashCode() 的作⽤是获取哈希码；它实际上是返回⼀个 int 整数，定义在 Object 类中， 是一个本地⽅法，这个⽅法通常⽤来将 **对象的内存地址转换为整数后返回**。
- 哈希码主要在哈希表这类集合映射的时候用到，哈希表存储的是键值对 (key-value)，它的特点是：能根据 “键” 快速的映射到对应的 “值”。这其中就利⽤到了哈希码。



不会创建 “类对应的散列表”：不存在重写 equals() 要重写 hashCode()：

- 当我们不在 HashSet，HashTable，HashMap 等等这些 **本质是散列表** 的数据结构中用到这个类，此时，equals() 用来比较该类的两个对象是否相等，而 **hashCode() 则根本没有任何作用**。
- 因为不是散列表的结构中，根本就不会调用什么 hashCode() 方法，重不重写都没关系；



会创建 “类对应的散列表”：**重写 equals() 一定要重写 hashCode()**：

- 不会创建 “类对应的散列表” 的情况下，该类的 “hashCode() 和 equals() ” 是有关系的：
  1. 如果两个对象相等，那么它们的 hashCode() 值一定相同。这里的 “对象相等” 是指通过 equals() 比较两个对象时返回 true。
  2. 如果两个对象 **hashCode() 相等**，它们的 **equals() 不一定相等**。因为在散列表中，两个键值对的哈希值相等，并不一定能得出键值对相等，因为会出现 **哈希冲突**。
- 这里所说的 “会创建类对应的散列表” 是说：我们会在 HashSet, HashTable, HashMap 等等这些本质是散列表的数据结构中用到该类。



在使用如 HashSet、HashMap 等需要使用哈希算法的数据结构时，**若只重写 equals()，而不重写 hashCode**，会有什么问题？

- 在添加对象时：如果 **只重写了 equals**，那么只要 **值相等**，我们就认为对象相等，但若这两个对象的 **哈希值不相等**，在 **HashSet 中根据哈希值** 来判断两个对象时就会 **认为这两个对象不相等**，因此将他们 **都装入集合中**，显然这是与我们的预期不符的。



### 4.6 深拷贝和浅拷贝区别？什么是引用拷贝？

深拷贝和浅拷贝的区别：

- **浅拷贝**：浅拷贝会在堆上创建一个新的对象，但是 **原对象内部的属性是引用类型** 的话，浅拷贝会直接 **复制内部对象的引用地址**，也就是说拷贝对象和原对象共用同一个内部对象；
- **深拷贝**：深拷贝会完全复制整个对象，**包括** 这个对象所包含的 **内部对象**；

> 浅拷贝对于拷贝基本数据类型来说和深拷贝没有区别，但是在对引用类型来说，区别就大了。



浅拷贝示例：

下面的 `Person` 类实现了 `Cloneable` 接口，并重写了 `clone()` 方法。

`clone()` 方法的实现很简单，直接调用的是父类 `Object` 的 `clone()` 方法。

```java
public class Person implements Cloneable {
    private Address address;	// 引用类型的对象
    // 省略构造函数、Getter&Setter方法
    @Override
    public Person clone() {
        try {
            Person person = (Person) super.clone();
            return person;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

public class Address implements Cloneable{
    private String name;	// 引用类型的对象
    // 省略构造函数、Getter&Setter方法
    @Override
    public Address clone() {
        try {
            return (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

测试 ：

```java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// true
System.out.println(person1.getAddress() == person1Copy.getAddress());
```

从输出结构就可以看出， `person1` 的克隆对象和 `person1` 使用的仍然是 **同一个 `Address` 对象**。



深拷贝示例：

简单对 `Person` 类的 `clone()` 方法进行修改，连带着要把 `Person` 对象内部的 `Address` 对象一起复制。

```java
@Override
public Person clone() {
    try {
        Person person = (Person) super.clone();
        person.setAddress(person.getAddress().clone());
        return person;
    } catch (CloneNotSupportedException e) {
        throw new AssertionError();
    }
}
```

测试：

```java
Person person1 = new Person(new Address("武汉"));
Person person1Copy = person1.clone();
// false
System.out.println(person1.getAddress() == person1Copy.getAddress());
```

从输出结构就可以看出，`person1` 的克隆对象和 `person1` 包含的 `Address` 对象已经是不同的了。

当然，他们包含的 `Address` 虽然已经不同，但是 `Address.name` 还是相同的，因为 `String` 也是引用类型，所以完整的深拷贝还需要将 `name` 属性也进行拷贝，但是 `String` 又是不可变类型，它没有实现 `Cloneable` 接口，所以不能用 `clone()` 方法进行拷贝，可以直接 new 一个新串返回：

```java
@Override
    protected Address clone() throws CloneNotSupportedException {
        Address address = (Address) super.clone();
        address.setName(new String(address.getName()));
        return address;
    }
```

此时它们的 `name` 属性也是不同的引用了：

```java
public static void main(String[] args) throws CloneNotSupportedException {
        Person person1 = new Person(new Address("haha"));
        Person person1Copy = person1.clone();
        System.out.println(person1Copy.getAddress().getName() == person1.getAddress().getName());
   	   // false
    }
```



另外，实现深拷贝的方法除了上面的重写克隆方法外，还可以采用 **序列化** 的方法，先将原对象序列化，再反序列化成拷贝对象，这样就可以创建一个新的对象了。

- Java 序列化就是指将对象转换为 **字节序列**（二进制序列）的过程，而反序列化则是只将字节序列转换成目标对象的过程。



什么是 **引用拷贝**：

- 引用拷贝就是 **两个不同的引用指向同一个对象**，如果对一个对象进行改变，其他对象也会跟着改变；



![浅拷贝、深拷贝、引用拷贝示意图](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210192135604.png)

## 5. Java 常见类

### 5.1 Object

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
    
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
    
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
    
/**
 * naitive 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
    
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
    
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
    
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
    
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
    
/**
 * 多了 nanos 参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 毫秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
    
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
    
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```



#### == 和 equals() 的区别

**`==`** 对于基本类型和引用类型的作用效果是不同的：

- 对于 **基本数据类型** ，`==` 比较的是值；
- 对于 **引用数据类型**，`==` 比较的是对象的 **内存地址**；

因为 Java 只有 **值传递**，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其 **本质比较的都是值**，只是引用类型变量存的值是 **对象的地址**。



`equals()` 用于判断两个对象是否相等。`equals()` 方法存在于 `Object` 类中，而 `Object` 类是所有类的直接或间接父类，因此 **所有的类都有`equals()`方法**。



`equals()` 方法存在两种使用情况：

- 类没有重写 `equals()` 方法：等价于 "=="，使用的是默认的 `Object` 类的 `equals()` 方法；
- 类重写了 `equals()` 方法：我们一般都重写 `equals()` 方法来比较两个对象中的 **属性** 是否相等；若它们的属性相等，则返回 true；

示例：

```java
String a = new String("ab"); 	// a 为一个引用
String b = new String("ab"); 	// b为另一个引用,对象的内容一样
String aa = "ab"; 	// 放在常量池中
String bb = "ab"; 	// 从常量池中查找
System.out.println(aa == bb);	// true
System.out.println(a == b);	// false
System.out.println(a.equals(b));	// true
```

**`String` 中的 `equals()` 方法是被重写过的**，因为 `Object` 的 `equals` 方法是比较的对象的内存地址，而 `String` 的 `equals` 方法比较的是对象的值。

当创建 `String` 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 `String` 对象。

`String`类`equals()`方法：

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```



#### hashCode() 有什么用？

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。

`hashCode() ` 定义在 JDK 的 `Object` 类中，这就意味着 Java 中的任何类都包含有 `hashCode()` 函数。另外需要注意的是： `Object` 的 `hashCode()` 方法是本地方法，也就是用 C 语言或 C++ 实现的，该方法通常用来 **将对象的内存地址转换为整数** 之后返回。

散列表存储的是键值对(key-value)，它的特点是：**能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）**

#### 为什么要有 hashCode

以 “`HashSet` 如何检查重复” 为例子来说明为什么要有 `hashCode`？

- 当把对象加入 `HashSet` 时，`HashSet` 会先计算对象的 `hashCode` 值来判断对象加入的位置，同时也会与其他已经加入的对象的 `hashCode` 值作比较；
- 如果没有相同的 `hashCode`，`HashSet` 会假设对象没有重复出现；
- 但是如果发现 **有相同 `hashCode` 值的对象**，这时会 **调用 `equals()` 方法** 来检查 `hashCode` 相等的对象是否真的相同：
  - 如果两者相同，`HashSet` 就不会让其加入操作成功。
  - 如果 **不同** 的话，就会 **重新散列到其他位置**。这样我们就大大减少了 `equals` 的次数，相应就大大提高了执行速度。



其实， `hashCode()` 和 `equals()` 都是用于比较两个对象是否相等。

**那为什么 JDK 还要同时提供这两个方法呢**？

这是因为在一些容器（比如 `HashMap`、`HashSet`）中，**有了 `hashCode()` 之后，判断元素是否在对应容器中的效率会更高**（参考添加元素进`HashSet`的过程）！

**那为什么不只提供 `hashCode()` 方法呢**？

这是因为 **两个对象的`hashCode` 值相等并不代表两个对象就相等**，存在 **哈希碰撞** 的可能。

总结下来就是：

- 如果两个对象的 `hashCode` 值相等，那这两个对象不一定相等（哈希碰撞）；
- 如果两个对象的 `hashCode` 值相等，且 `equals()` 也返回 `true`，才认为这两个对象相等；
- 如果两个对象的 `hashCode` 值不相等，那么直接肯定这两个对象不相等；



#### 重写 equals() 时必须重写 hashCode() ？

以 “**类的用途**” 来将 “`hashCode()` 和 `equals()` 的关系” 分 2 种情况来说明：

**1、不会创建 “类对应的散列表”：不存在重写 equals() 要重写 hashCode()**：

- 当我们不在 HashSet，HashTable，HashMap 等等这些 **本质是散列表** 的数据结构中用到这个类，此时，equals() 用来比较该类的两个对象是否相等，而 **hashCode() 则根本没有任何作用**。

- 因为不是散列表的结构中，根本就不会调用什么 hashCode() 方法，重不重写都没关系；



**2、会创建 “类对应的散列表”：重写 equals() 一定要重写 hashCode()**：

- 会创建 “类对应的散列表” 的情况下，该类的 “hashCode() 和 equals() ” 是有关系的：
  1. 如果两个对象相等，那么它们的 hashCode() 值一定相同。这里的 “对象相等” 是指通过 equals() 比较两个对象时返回 true。
  2. 如果两个对象 **hashCode() 相等**，它们的 **equals() 不一定相等**。因为在散列表中，两个键值对的哈希值相等，并不一定能得出键值对相等，因为会出现 **哈希冲突**。
- 这里所说的 “会创建类对应的散列表” 是说：我们会在 HashSet, HashTable, HashMap 等等这些本质是散列表的数据结构中用到该类。



在使用如 HashSet、HashMap 等需要使用哈希算法的数据结构时，**若只重写 equals()，而不重写 hashCode()**，会有什么问题？

- 在添加对象时：如果 **只重写了 equals**，那么只要 **值相等**，我们就认为对象相等，但若这两个对象的 **哈希值不相等**，在 **HashSet 中根据哈希值** 来判断两个对象时就会 **认为这两个对象不相等**，因此将他们 **都装入集合中**，显然这是与我们的预期不符的。



示例：

只重写了 equals()：

```java
class Test {
    public static void main(String[] args) {
        User user1 = new User(1, "aaa"), user2 = new User(1, "aaa");
        HashSet<User> set = new HashSet<>();
        set.add(user1); set.add(user2);
        for (User user : set) System.out.println(user);
    }
}
/* 输出：
User{id=1, name='aaa'}
User{id=1, name='aaa'}
*/

class User {
    int id;
    String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        User user = (User) o;
        return this.id == user.id && this.name.equals(user.name);
    }
    
    // 构造方法、toString() 略
}
```

equals() 和 hashCode() 都重写了：

```java
class Test {
    public static void main(String[] args) {
        User user1 = new User(1, "aaa"), user2 = new User(1, "aaa");
        HashSet<User> set = new HashSet<>();
        set.add(user1); set.add(user2);
        for (User user : set) System.out.println(user);
    }
}
/* 输出：
User{id=1, name='aaa'}
*/

class User {
    int id;
    String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        User user = (User) o;
        return this.id == user.id && this.name.equals(user.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
    
    // 构造方法、toString() 略
}
```

### 5.2 String

#### String、StringBuffer、StringBuilder 的区别

从可变性来说：

- `String` 是不可变的，它被 `final` 和 `private` 修饰；

- `StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类，在 `AbstractStringBuilder` 中也是使用 byte 数组保存字符串，不过没有使用 `final` 和 `private` 关键字修饰，最关键的是这个 **`AbstractStringBuilder` 类还提供了很多修改字符串的方法** 比如 `append` 方法。

  ```java
  abstract class AbstractStringBuilder implements Appendable, CharSequence {
      byte[] value;
      
      public AbstractStringBuilder append(String str) {
          if (str == null)
              return appendNull();
          int len = str.length();
          ensureCapacityInternal(count + len);
          str.getChars(0, len, value, count);
          count += len;
          return this;
      }
    	//...
  }
  ```



从线程安全性来说：

- `String` 中的对象是不可变的，也就可以理解为常量，线程安全。
- `AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法。
  - `StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。
  - `StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。



从使用性能来说：

- 每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。
- `StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。
- 相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。



对于三者的使用总结：

1. 操作少量的数据适用 `String`；
2. 单线程操作字符串缓冲区下操作大量数据适用 `StringBuilder`；
3. 多线程操作字符串缓冲区下操作大量数据适用 `StringBuffer`；



#### String 为什么是不可变的？

> 首先需要理解什么是不可变？

String 不可变很简单，如下图，给一个已有字符串 "abcd"，第二次赋值为 "abcdel"，不是在原内存地址上修改数据，而是重新指向一个新对象，新地址：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210222139469.webp)



> 探索 String 不可变的真正原因

首先来看看 `final` 的作用：

- 被 `final` 关键字修饰的 **类不能被继承**，修饰的 **方法不能被重写**；
- 修饰 **变量** 分两种情况（重要）：
  - 基本类型：值不能改变；
  - 引用类型：不能再指向其他对象；



接下来直接翻开 JDK 源码，`java.lang.String` 类起手前三行是这样的：

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
               Constable, ConstantDesc {

    @Stable
    private final byte[] value;
    // ......
}
```

从源码中可以得知一下信息：

- String 类用 `final` 修饰，说明 String 类 **不可继承**；
- String 类的主力成员字段 `value` 是一个 `byte[]` 数组（JDK 9 之前是 `char[]`），也是用 `final` 修饰，被 final 修饰的字段创建后就 **不可改变**。注意数组是引用类型，说明 `value` 只是不能再指向其他对象；
- 注意：是这个 `byte[]` 类型的 **变量 `value` 不可改变**，并不是这个数组不可变。也就是说只是 `value` **变量引用的地址不可改变**，而 **数组本身是可变的**；
- **`value` 变量** 只是 **stack 上的一个引用**，**数组的本体结构在 heap 中**。String 类里的 `value` 用 `final` 修饰，只是说 stack 里的这个叫 `value` 所引用的地址不可变，没有说 heap 中数组本身中的数据不可变；

举个例子：

```java
final int[] value = {1, 2, 3};
int[] another = {4, 5, 6};
value = another;  	// 编译报错：不能给 final 变量value 赋值。
```

因为把 value 重新指向了 another 指向的地址，而 value 用 final 修饰，所以自然是不允许的。

但是如果直接对数组本身进行修改，如下：

```java
final int[] value = {1, 2, 3};
value[2] = 10;	// 允许直接对数组本身进行修改
System.out.println(Arrays.toString(value)); 	// [1, 2, 10]
```

所以，并不是因为 `final` 修饰了 String 和 value 就是不可变的了，主要是编写这个类的程序员把 String 封装得很好，具体体现如下：

- String 被 `final` 修饰，不能被继承，这避免了其他人继承后破坏；
- `byte[] value` 被 `private`修饰，只能在本类中对 value 进行操作；
- String 类中没有提供任何能修改 `byte[]` 数组数据的方法；

所以 **String 不可变的关键都在底层的实现，而不是单单一个final**。



> String 为什么要设置为不可变

最简单地原因，就是为了**安全**。

例如，我们要在某个函数中对字符串进行修改，然后返回新的字符串，如下：

```java
public class Main {
    // 不可变的 String
    public static String updateStr(String s) {
        s += "bbb";     // 底层使用 StringBuilder 拼接，返回一个新对象
        return s;
    }

    // 可变的 StringBuilder
    public static StringBuilder updateSb(StringBuilder sb) {
        return sb.append("bbb");
    }

    public static void main(String[] args) {
        String origin = "aaa";
        // 不可变的 String 做参数
        String updatedStr = Main.updateStr(origin);
        System.out.println("String aaa ->" + origin);

        StringBuilder originSb = new StringBuilder("aaa");
        // 可变的 StringBuilder 做参数
        StringBuilder updatedSb = Main.updateSb(originSb);
        System.out.println("StringBuilder ->" + updatedSb);
    }
}

// output：
// String aaa ->aaa
// StringBuilder ->aaabbb
```

可以发现：若使用可变的 StringBuilder 做参数，因为 Java 是值传递，引用类型传递的是地址，所以把我们原来的 `originSb` 的地址传过去，然后进行改变了，但是我们本意肯定是不想改变它的，只是想把它作为参数进行修改，然后返回一个新的修改后的字符串给我们。

而不可变的 String 做参数的时候，就不会修改掉我们原来的 `origin` 字符串；



再看下面这个 **HashSet** 用 StringBuilder 做元素的场景，问题就更严重了，而且更隐蔽。

![image-20221022223540662](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210222235905.png)

StringBuilder 型变量 sb1 和 sb2 分别指向了堆内的字面量 "aaa" 和 "aaabbb"。把他们都插入一个HashSet。到这一步没问题。但如果后面不小心 **把变量 sb3 也指向 sb1 的地址**，再改变 sb3 的值，因为StringBuilder 没有不可变性的保护，**sb3 直接在原先 "aaa" 的地址上改，导致 sb1 的值也变了**。这时候，**HashSet 上就出现了两个相等的键值 "aaabbb”**。破坏了 HashSet 键值的唯一性。所以千万不要用可变类型做 HashMap 和 HashSet 键值。



还有一个原因是 String 会保存在字符串常量池中，这样在大量使用字符串的情况下，可以节省内存空间和提高效率。但之所以能实现这个特性，String 的不可变性是最基本的一个必要条件。要是内存里字符串内容能改来改去，这么做就完全没有意义了。



#### 字符串拼接用 "+" 还是 StringBuilder ？

Java 语言本身并不支持运算符重载，“+” 和 “+=” 是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

```java
String str1 = "he";
String str2 = "llo";
String str3 = "world";
String str4 = str1 + str2 + str3;
```

上面的代码对应的字节码如下：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210192318334.png)

可以看出，字符串对象通过 “+” 的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个新的 `String` 对象 。

不过，在循环内使用 “+” 进行字符串的拼接的话，存在比较明显的缺陷：**编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象**。

```java
String[] arr = {"he", "llo", "world"};
String s = "";
for (int i = 0; i < arr.length; i++) {
    s += arr[i];
}
System.out.println(s);
```

`StringBuilder` 对象是在循环内部被创建的，这意味着每循环一次就会创建一个 `StringBuilder` 对象。



如果直接使用 `StringBuilder` 对象进行字符串拼接的话，就不会存在这个问题了。

```java
String[] arr = {"he", "llo", "world"};
StringBuilder s = new StringBuilder();
for (String value : arr) {
    s.append(value);
}
System.out.println(s);
```

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210192319453.png)



#### 字符串常量池的作用

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

```java
//  在堆中创建字符串对象”ab“
//  将字符串对象”ab“的引用保存在字符串常量池中
String aa = "ab";
//  直接返回字符串常量池中字符串对象”ab“的引用
String bb = "ab";
System.out.println(aa==bb);	// true
```



#### String s = new String("abc") 和 String s = "abc" 的区别

两个语句都会去 **字符串常量池中检查是否已经存在 “abc”**，如果有则直接使用，如果没有则会在常量池中创建 “abc” 对象。

但是不同的是，`String str1 = new String("abc")` 还会通过 new String() 在 **堆** 里创建一个 "abc" 字符串对象实例。所以后者可以理解为被前者包含。

![堆与常量池中的String](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210192328333.png)

> String s = new String("abc") 创建了几个对象？

很明显，一个或两个。如果字符串常量池已经有 “abc”，则是一个；否则，两个。

当字符串常量池没有 “abc”，此时会创建如下两个对象：

- 一个是字符串字面量 "abc" 所对应的、字符串常量池中的实例
- 另一个是通过 new String() 创建并初始化的，内容与 "abc" 相同的实例，在堆中。

而常量池中有 "abc" 时，只需要在堆中创建即可；



#### intern 方法有什么作用

`String.intern()` 是一个 native（本地）方法，其作用是 **将指定的字符串对象的引用保存在字符串常量池中**。

可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。

示例：

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在 字符串常量池 中
String s1 = "Java";
// 直接返回 字符串常量池 中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在 堆 中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回 字符串常量池 中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```



#### String 类型的变量和常量做 "+" 运算时发生了什么？

先来看字符串不加 `final` 关键字拼接的情况（JDK1.8）：

```java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);	//false
System.out.println(str3 == str5);	//true
System.out.println(str4 == str5);	//false
```

对于 **编译期可以确定值的字符串**，也就是 **常量字符串** ，jvm 会将其 **存入字符串常量池**。并且，字符串常量 **拼接得到的字符串常量在编译阶段就已经被存放字符串常量池**，这个得益于编译器的优化。

对于 `String str3 = "str" + "ing";`  编译器会优化成 `String str3 = "string";` 。



不过，字符串使用 `final` 关键字声明之后，可以让编译器当做常量来处理：

```java
final String str1 = "str";
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";	// 常量池中的对象
String d = str1 + str2; 	// 常量池中的对象
System.out.println(c == d);	// true
```

被 `final` 关键字修改之后的 `String` 会被编译器当做常量来处理，编译器在程序编译期就可以确定它的值，其效果就相当于访问常量。



## 6. 序列化

序列化：**把 Java 对象转为二进制流**，方便存储和传输。

反序列化：**把二进制流恢复成对象**。

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270009172.png)

> Serializable 接口有什么用？

这个接口只是一个标记，没有具体的作用，但是如果不实现这个接口，在有些序列化场景会报错，所以一般建议，创建的 JavaBean 类都实现 Serializable。

> serialVersionUID 又有什么用？

```java
private static final long serialVersionUID = 1L;
```

我们经常会看到这样的代码，这个 ID 其实就是用来 **验证序列化的对象和反序列化对应的对象 ID 是否一致**。

如果没有显示指定 serialVersionUID ，则编译器会根据类的相关信息自动生成一个，可以认为是一个指纹。

> 怎么使变量不被序列化？

- **静态变量** 不会被序列化，因为静态变量不属于某个对象；
- 使用 `transient` 修饰的变量不会被序列化（`transient` 只能修饰变量）；



> 序列化有几种方式？

Java 常见的序列化方式：

![Java常见序列化方式](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270013711.png)

- **Java 对象流** 序列化 ：Java 原生序列化方法即通过 Java 原生流 (InputStream 和 OutputStream 之间的转化)的方式进行转化。一般是对象输出流 `ObjectOutputStream `和对象输入流 `ObjectInputStream`；
- **Json 序列化**：这个可能是我们 **最常用** 的序列化方式，Json 序列化的选择很多，一般会使用 jackson 包，通过 ObjectMapper 类来进行一些操作，比如将对象转化为 byte 数组或者将 Json 串转化为对象。
- **ProtoBuff 序列化**：ProtocolBuffer 是一种轻便高效的结构化数据存储格式，ProtoBuff 序列化对象可以很大程度上将其 **压缩**，可以大大减少数据传输大小，提高系统性能。





## 7. 异常

Java 异常类层次结构图概览：

![Java 异常类层次结构图](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210262125389.png)

### 6.1 Exception 和 Error 有什么区别

- Exception：程序本身可以处理的异常，可以通过 catch 捕获；其可以分为两类：
  - Checked Exception：受检查异常，必须处理；
  - Unchecked Exception：不受检查异常，可以不处理；
- Error：程序无法处理的错误，不建议通过 catch 捕获。这些错误发生时，JVM 一般会选择线程终止；



### 6.2 Checked Exception 和 Unchecked Exception 有什么区别

**Checked Exception**：

- **受检查异常** ，Java 代码在编译过程中，如果受检查异常没有被 `catch `或者 `throws` 关键字处理的话，就 **没办法通过编译**。
- 常见的受检查异常有：IO 相关的异常、`ClassNotFoundException` 、`SQLException` 等；

**Unchecked Exception**：

- **不受检查异常** ，Java 代码在编译过程中 ，即使不处理不受检查异常也可以正常通过编译。
- `RuntimeException` 及其子类都统称为非受检查异常，常见的有：
  - `NullPointerException`(空指针错误)
  - `IllegalArgumentException`(参数错误比如方法入参类型错误)
  - `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
  - `ArrayIndexOutOfBoundsException`（数组越界错误）
  - `ClassCastException`（类型转换错误）
  - `ArithmeticException`（算术错误）
  - `SecurityException` （安全错误比如权限不够）
  - `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)



### 6.3 try-catch-finally

- `try`块 ： 用于捕获异常。其后可接零个或多个 `catch` 块，如果没有 `catch` 块，则必须跟一个 `finally` 块。
- `catch`块 ： 用于处理 try 捕获到的异常。
- `finally` 块 ： 无论是否捕获或处理异常，`finally` 块里的语句都会被执行。当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将 **在方法返回之前被执行**。

**注意：不要在 finally 语句块中使用 return**：当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。

这是因为 try 语句中的 return 返回值会先被 **暂存在一个本地变量中**，当执行到 finally 语句中的 return 之后，这个 **本地变量的值** 就变为了 **finally 语句中的 return 返回值**。

示例：

```java
public static void main(String[] args) {
    System.out.println(f(2));
}

public static int f(int value) {
    try {
        return value * value;
    } finally {
        if (value == 2) {
            return 0;
        }
    }
}
// output：0
```



但如果是如下情况：

```java
try {
    i = 2;
    return i;
} finally {
    i = 3;
}
```

最终返回的结果还是 2，因为在执行 finally 之前 i = 2 已经暂存起来了，执行完 finally 就直接返回之前的暂存结果。



> finally 中的代码一定会执行？

不一定的！在某些情况下，finally 中的代码不会被执行。

就比如说 finally 之前虚拟机被终止运行的话，finally 中的代码就不会被执行。

```java
try {
    System.out.println("Try to do something");
    throw new RuntimeException("RuntimeException");
} catch (Exception e) {
    System.out.println("Catch Exception -> " + e.getMessage());
    // 终止当前正在运行的Java虚拟机
    System.exit(1);
} finally {
    System.out.println("Finally");
}

/* output：
Try to do something
Catch Exception -> RuntimeException
*/
```

另外，在以下 2 种特殊情况下，`finally` 块的代码也不会被执行：

1. 程序所在的线程死亡。
2. 关闭 CPU。



> try-with-resources

适用范围：必须是资源 — 任何实现了 `java.lang.AutoCloseable` 或 `java.io.Closeable` 的对象。

关闭资源和 finally 块的执行顺序：catch 和 finally 都是在 **关闭资源后** 运行。





## 8. IO 流

IO 流在 Java 中分为 **输入流 **和 **输出流**，而根据数据的处理方式又分为 **字节流 **和 **字符流**。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的：

- `InputStream`/`Reader`：所有的输入流的基类，前者是 **字节输入流**，后者是 **字符输入流**。
- `OutputStream`/`Writer`：所有输出流的基类，前者是 **字节输出流**，后者是 **字符输出流**。



### 7.1 为什么要分为字节流和字符流？

本质：不管是文件的读写还是网络的收发，信息的最小存储单元都是字节，为什么 IO 流还要分一个字符流出来？

- 字符流其实是由 Java 虚拟机将字节转换得到的，这个过程有些耗时；
- 如果不知道编码的类型，适用字节流很容易出现乱码；

所以， IO 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。

如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



### 7.2 BIO、NIO、AIO？

![BIO、NIO、AIO](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210262353601.png)

> BIO（Blocking IO）

BIO 属于同步阻塞 IO 模型，应用程序发起 read 后，会一直阻塞，直到内核把数据拷贝到用户空间。

![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210262356889.png)

在客户端连接数量不高的情况下，是没问题的。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。

> NIO（Non-blocking/New IO）

Java 中的 NIO 于 Java 1.4 中引入，对应 `java.nio` 包，提供了 `Channel` , `Selector`，`Buffer` 等抽象。

NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它是支持 **面向缓冲** 的，**基于通道** 的 I/O 操作方法。 对于高负载、高并发的（网络）应用，应使用 NIO 。

Java 中的 NIO 可以看作是 **I/O 多路复用模型**。也有很多人认为，Java 中的 NIO 属于同步非阻塞 IO 模型。

先来看看同步非阻塞 IO 模型：

![图源：《深入拆解Tomcat & Jetty》](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210262358052.png)

同步非阻塞 IO 模型中，应用程序会 **一直发起 read 调用**，等待数据从内核空间拷贝到用户空间的这段时间里，**线程依然是阻塞的**，直到在内核把数据拷贝到用户空间。

相比于同步阻塞 IO 模型，同步非阻塞 IO 模型确实有了很大改进。通过 **轮询** 操作，**避免了一直阻塞**。

但是，这种 IO 模型同样存在问题：**应用程序不断进行 IO 系统调用轮询数据是否已经准备好的过程是是非消耗 CPU 资源的**。

这个时候，**I/O 多路复用模型** 就上场了：

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270002048.png)

IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间 -> 用户空间）还是阻塞的。

**IO 多路复用模型，通过减少无效的系统调用，减少了对 CPU 资源的消耗**。



Java 中的 NIO ，有一个非常重要的 **选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270004715.png)

> AIO（Asynchronous IO）

AIO 也就是 NIO 2。Java 7 中引入了 NIO 的改进版 NIO 2，它是异步 IO 模型。

异步 IO 是 **基于事件和回调机制** 实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270005477.png)

目前来说 AIO 的应用还不是很广泛。Netty 之前也尝试使用过 AIO，不过又放弃了。这是因为，Netty 使用了 AIO 之后，在 Linux 系统上的性能并没有多少提升。



最后，简单总结一下 Java 中的 BIO、NIO、AIO：

![image-20221027000718917](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202210270007884.png)



























