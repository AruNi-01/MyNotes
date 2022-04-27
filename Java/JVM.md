# 1. 初识JVM

## 2.1 什么是JVM

**定义**：Java Virtual Machine，Java程序的运行环境（Java 二进制 字节码的运行环境）。

**好处**：

- 一次编译，处处执行
- 自动的内存管理，垃圾回收机制
- 组数下标越界检查

**比较**：JVM、JRE、JDK的关系如下图所示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210207154634171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

## 1.2 JVM的组成

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210207155820178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

- ClassLoader：Java代码编译成二进制后，会经过类加载器，这样才能加载到JVM中运行。
- Method Area：类是放在方法区中。
- Heap：类的实例对象大部分存放在堆区。
- 当类调用方法时，会用到JVM Stack、PC Register、本地方法栈。
- 方法执行时的每行代码是由执行引擎中的解释器逐行执行，方法中的热点代码频繁调用的方法，由JIT编译器优化后执行，GC会对堆中不用的对象进行回收。
- 需要和操作系统打交道就需要使用到本地方法接口。

# 2. 内存结构

## 2.1 程序计数器 PC Register

**定义**：Program Counter Register  程序计数器（寄存器）

**作用**：记录下一条JVM指令的执行地址行号

**特点**：

- 线程私有的
- 不会存在内存溢出

**示例**：

```bash
0: getstatic 		#20 // PrintStream out = System.out; 
3: astore_1 		// -- 
4: aload_1 			// out.println(1); 
5: iconst_1 		// -- 
6: invokevirtual 	  #26 // -- 
9: aload_1 		      // out.println(2); 
10: iconst_2	       // -- 
11: invokevirtual 	   #26 // -- 
14: aload_1  		// out.println(3); 
15: iconst_3 		// -- 
16: invokevirtual 	   #26 // -- 
19: aload_1 	       // out.println(4); 
20: iconst_4 	         // -- 
21: invokevirtual         #26 // -- 
24: aload_1 		// out.println(5); 
25: iconst_5 		 // -- 
26: invokevirtual 	    #26 // -- 
29: return
```

- 解释器会解释JVM指令为机器码交给CPU执行，程序计数器会记录下一条指令的地址行号，这样下一次解释器会从程序计数器拿到指令，然后进行解释执行。
- 多线程的环境下，如果两个线程发生了上下文切换，那么程序计数器会记录线程下一行指令的地址行号，以便于下次线程切换回来时接着往下执行。

## 2.2 虚拟机栈 JVM Stacks

**定义**：

- 每个线程运行需要的内存空间，成为虚拟机栈。
- 每个栈由多个栈帧（Frame）组成，对应着每次调用方法时所占用的内存。
- 每次线程只能由一个活动栈帧，就是当前正在执行的方法。

**问题**：

1. 垃圾回收是否涉及栈内存？
   - 不涉及；栈内存是方法调用产生的，方法调用结束后会弹出栈。
2. 栈内存分配越大越好吗？
   - 不是；因为物理内存是一定的，栈内存越大，可以支持更多的递归调用，但是可执行的线程数就会减少。
3. 方法中的局部变量是否线程安全？
   - 如果方法内部的变量没有逃离方法的作用访问，它是线程安全的。
   - 如果是**局部变量引用了对象**，并**逃离了方法的访问**，那就要考虑线程安全问题。

下面对问题3进行具体的示例说明：

示例1：

```java
public static void method1(){
    StringBuffer s1 = new StringBuffer();
    s1.append("A");
    s1.append("B");
    ...
}
```

> 多个线程同时执行此方法，每个线程都会产生一个自己的变量，每个线程之间互不干扰，不会对其他线程的变量产生影响

如果s1是一个静态变量，上面的代码还是线程安全的吗？

- 当变量是一个静态变量时，多个线程执行此方法，每个方法都会操作这同一个变量，是共享的；如果不对它进行保护，那么存在线程安全问题。

示例2：

```java
public static void method2(StringBuiler sBuilder){
    sBuilder.append("A");
    sBuilder.append("B");
    ...
}
```

> sBuilder对象是作为参数传递进去的，这就意味着它不是线程私有的，多个线程都可以同时访问它；所以不是线程安全的

示例3：

```java
public static StringBuilder method3(){
    StringBuilder s1 = new StringBuilder();
    s1.append("A");
    s1.append("B");
    ...
    return s1;
}
```

> 虽然s1对象是在方法内生成的对象，但是s1作为一个返回变量返回，其他线程可以获取它，对他进行并发的操作；所以不是线程安全的

示例4：

```java
public static String method4(){
    StringBuilder s1 = new StringBuilder();
    s1.append("A");
    s1.append("B");
    ...
    return s1.toString();
}  
```

> 返回类型是String，String具有不变性，源码中String是被final修饰的；所以返回String类型是线程安全的

**总结**：

- 示例2、3发生了逃逸，作用域不止在方法内部了，不是线程安全的
- 示例1、4未发生逃逸，是线程安全的





**栈内存溢出**：

- 栈帧过大、过多、或者第三方类库操作，都有可能造成栈内存溢出 `java.lang.stackOverFlowError`
- 可以使用`-Xss1024k`指定栈内存大小，但是一般不会修改





**线程运行诊断**：

案例：cpu 占用过多
解决方法：Linux 环境下运行某些程序的时候，可能导致 CPU 的占用过高，这时需要定位占用 CPU 过高的线程

- top 命令，查看是哪个进程占用 CPU 过高
- ps H -eo pid, tid（线程id）, %cpu | grep 刚才通过 top 查到的进程号 通过 ps 命令进一步查看是哪个线程占用 CPU 过高
- jstack 进程 id 通过查看进程中的线程的 nid ，刚才通过 ps 命令看到的 tid 来对比定位，注意 jstack 查找出的线程 id 是 16 进制的，需要转换



## 2.3 本地方法栈 Native Method Stacks

一些带有 native 关键字的方法就是需要Java去调用本地的C或者C++方法，因为Java有时候没法直接和操作系统底层交互，所以需要用到本地方法栈，服务于带 native 关键字的方法。



## 2.4 堆 Heap

**定义**：Heap 堆

- 通过new关键字创建的对象都会被存放在堆内存中

**特点**：

- 线程共享，堆内存中的对象都需要考虑线程安全问题
- 有垃圾回收机制

**堆内存溢出**：

- `java.lang.OutOfMemoryError : Java heap space`
- 可以使用`Xmx8m`来指定堆内存大小
- 在测试的时候，可以把堆内存设置的小一点，避免测试时间过短，误以为没有堆内存溢出的风险

**堆内存诊断**：

运行后直接在控制台输入相关命令即可查看堆内存的情况：

1. `jps`：查看当前系统中有那些 java 进程
2. `jmap` ：查看堆内存占用情况 `jmap -heap 进程id`
3. `jconsole` ：会弹出来一个图形界面，多功能的监测工具，可以连续监测
4. `jvisualvm` ：可视化的展示虚拟机的信息



## 2.5 方法区 Method Area

**定义**：

- 方法区与Java堆一样，是各个线程共享的内存区域，**它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据**。

在HotSpot中，把方法区称为**永久代**，本质上两者并不等价，仅仅是因为HotSpot虚拟机的设计团队选择把GC分代收集至方法区，或者说用永久代来实现方法区而已。这样HotSpot的垃圾收集器可以像管理Java堆一样管理这部分内存，能省去专门为方法区编写内存管理代码的工作。

移除永久代的工作从JDK1.7就开始了。JDK1.7中，存储在永久代的部分数据就已经转移到了Java Heap或者是 Native  Heap。但永久代仍存在于JDK1.7中，并没完全移除，譬如符号引用(Symbols)转移到了native heap；字面量(interned  strings)转移到了java heap；类的静态变量(class statics)转移到了java heap。

JDK8开始，使用**元空间**取代了永久代。元空间本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于：**元空间并不在虚拟机中，而是使用本地内存**。因此默认情况下元空间的大小**仅受本地内存限制**。





> 为什么要将永久代 (PermGen) 替换为元空间 (MetaSpace) ?

1. 整个永久代有一个 JVM 本身设置的固定大小上限，无法进行调整，而元空间使用的是直接内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小
2. 元空间里面存放的是类的元数据，这样加载多少类的元数据就不由 `MaxPermSize` 控制了, 而由系统的实际可用空间来控制，这样能加载的类就更多了
3. 在 JDK8，合并 HotSpot 和 JRockit 的代码时, JRockit 从来没有一个叫永久代的东西, 合并之后就没有必要额外的设置这么一个永久代的地方了





**组成**：

Hotspot 虚拟机JDK1.6 1.8 内存结构图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210208112903305.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)





**方法区内存溢出**：

方法区的大小决定了系统可以保存多少个类，如果系统定义了太多的类，导致方法区溢出，虚拟机同样会抛出内存溢出错误：

- 1.8 之前会导致永久代内存溢出：`java.lang.OutOfMemoryError: PermGen space` 
- 1.8 开始后会导致元空间内存溢出：`java.lang.OutOfMemoryError: Metaspace` 
- 比如：
  - 加载大量的第三方的jar包
  - Tomcat部署的工程过多（30~50个）
  - 大量动态的生成反射类（Spring AOP使用cglib生成大量的动态代理类）

指定永久代内存大小：

- `-XX:MaxPermSize=8m`

指定原空间内存大小：

- `-XX:MaxMetaspaceSize=8m`





### 2.5.1 运行时常量池：

> 反编译Java字节码(Class)工具

在设置中找到External Tools，选择添加，Tool Settings中各值如下：

- Program：`$JDKPath$\bin\javap.exe`
- Arguments：`-v $FileClass$`
- Working directory：`$OutputPath$`

![image-20220426195700379](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426195700379.png)

添加完成后，直接在`.java`代码右键，打开此工具，就能获得该Java字节码反编译后的结果：

![image-20220426200008930](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426200008930.png)





二进制字节码包含（类的基本信息，常量池，类方法定义，包含了虚拟机的指令）
首先看看常量池是什么，编译如下代码：

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

然后进行反编译查看结果：

![image-20220426200713453](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426200713453.png)

每条指令都会对应常量池表中一个地址，常量池表中的地址可能对应着一个类名、方法名、参数类型等信息

![image-20220426200851531](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426200851531.png)

**常量池**：

- 就是一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量信息

**运行时常量词**：

- 常量池是 `*.class` 文件中的，当该类被加载以后，它的常量池信息就会放入运行时常量池，并把里面的符号地址变为真实地址

#### StringTable

> 什么是StringTable？

字符串池在**JDK1.6**时存在于**永久代**中，**1.8**时存在于**堆**中。

`String s1 = "bac";`这样声明的字符串会放入字符串池中，而`String s1 = new String("abcd");`会在字符串池中有1个 "abcd" 的字符串对象，堆中也有1个，2个是不同的。

- 字符串池可以避免重复创建字符串对象

  ```java
  String s1 = "a";
  String s2 = "a";
  boolean isEqual = (s1 == s2);		// true，因为都是字符串池中的一个对象
  ```

- 常量池中的字符串仅仅是符号，第一次使用时才会变成对象

- StringTable的结构为hashtable结构，相同的字符串在池中只存在一份





> 字符串拼接原理

- 字符串变量拼接：先 new 一个 StringBuilder 对象，再调用它的 append 方法拼接，最后调用 toString 方法返回整个字符串，实际上就是 new String；
- 字符串常量拼接：原理是 javac 在**编译期优化**，编译器认为是常量，拼接结果唯一，去字符串池中找

示例：

```java
public static void main(String[] args) throws InterruptedException {
    String s1 = "a";
    String s2 = "b";
    String s3 = "ab";
    // 字符串变量拼接：
    String s4 = s1 + s2;    // new StringBuilder().append("a").append("b").toString()   <==>   new String("ab")
    // 字符串常量拼接：
    String s5 = "a" + "b";     // javac 在编译期优化，编译器认为是常量，拼接结果唯一，去字符串池中找
    System.out.println(s3 == s4);       // false     s3在字符串池中，而s4在堆区中
    System.out.println(s3 == s5);       // true	    s5是从字符串池中拿到的
}
```





> 字符串延迟加载

- 字符串是延迟加载的，**执行到的时候才会加载**，不是一次性加载完

利用IDEA中的 Memory（查看运行时类对象个数）验证：

```java
public static void main(String[] args) throws InterruptedException {
    System.out.println();       // java.lang.String     2093

    System.out.println("1");    // java.lang.String     2094
    System.out.println("2");
    System.out.println("3");
    System.out.println("4");
    System.out.println("5");
    System.out.println("6");
    System.out.println("7");
    System.out.println("8");
    System.out.println("9");
    System.out.println("0");    // java.lang.String     2103

    // 以下的字符串与上面重复，直接从字符串池中获取，String对象的数量不会增加
    System.out.println("1");    // java.lang.String     2103
    System.out.println("2");    // java.lang.String     2103
    System.out.println("3");
    System.out.println("4");
    System.out.println("5");
    System.out.println("6");
    System.out.println("7");
    System.out.println("8");
    System.out.println("9");
    System.out.println("0");
}
```

![image-20220426212132221](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426212132221.png)





> `xxx.intern()`方法，将串池中还没有的字符串对象放入串池中

**intern()方法**：

调用字符串对象的 intern 方法，主动将字符串对象放入到串池中

- JDK1.8：将这个字符串对象尝试放入串池，**如果有则并不会放入**，如果没有则将该字符串放入串池， 最后把串池中的对象的引用返回
- JDK1.6：将这个字符串对象尝试放入串池，**如果有则并不会放入**，如果没有会把**此对象复制一份**， **将复制的新对象放入串池**， 原来的对象还是在堆中，最后把串池中的对象返回

**注意**：此时如果调用 `intern()` 方法成功，堆内存与串池中的字符串对象是同一个对象；如果失败，则不是同一个对象

**JDK1.8**环境下的示例：

示例1：

```java
public class Main {
    public static void main(String[] args) {
        // "a"和"b" 被放入串池中，str 则存在于堆内存之中
        String str = new String("a") + new String("b");		// new String("ab");
        // 此时串池中没有 "ab" ，则会将该字符串对象放入到串池中，此时堆内存与串池中的 "ab" 是同一个对象
        String str2 = str.intern();
        // 给 str3 赋值，因为此时串池中已有 "ab" ，则直接将串池中的内容返回
        String str3 = "ab";
        // 堆内存与串池中的 "ab" 是同一个对象
        System.out.println(str == st2);		// true
        System.out.println(str2 == str3);	// true
    }
}
```

示例2，将str3放在前面：

```java
public class Main {
    public static void main(String[] args) {
        // 此处创建字符串对象 "ab" ，因为串池中还没有 "ab" ，所以将其放入串池中
        String str3 = "ab";
        // "a" 和 "b" 被放入串池中，str 则存在于堆内存之中
        String str = new String("a") + new String("b");
        // 此时"ab" 已存在与串池中，所以放入失败，str还是堆中的"ab"，但是会返回串池中的 "ab" 
        String str2 = str.intern();

        System.out.println(str == str2);	// false，堆中的str没有放入串池中，而str2是从串池的返回的

        System.out.println(str2 == str3);	 // true
    }
}
```

**JDK1.6**环境下的示例：

示例1，和JDK1.8情况**不同**：

```java
public class Main {
    public static void main(String[] args) {
        // "a"和"b" 被放入串池中，str 则存在于堆内存之中
        String str = new String("a") + new String("b");		// new String("ab");
        // 此时串池中没有 "ab" ，JDK1.6则会将该字符串复制一份，将复制的新对象放入到串池中，str本身还只是在堆中，最后也会返回串池中的"ab"对象，所以str2在串池中
        String str2 = str.intern();
        // 给 str3 赋值，因为此时串池中已有 "ab" ，则直接将串池中的内容返回
        String str3 = "ab";
        // str不在串池中
        System.out.println(str == st2);		// flase
        System.out.println(str2 == str3);	// true
    }
}
```

示例2，将str3放在前面，和JDK1.8情况相同：

```java
public class Main {
    public static void main(String[] args) {
        // 此处创建字符串对象 "ab" ，因为串池中还没有 "ab" ，所以将其放入串池中
        String str3 = "ab";
        // "a" 和 "b" 被放入串池中，str 则存在于堆内存之中
        String str = new String("a") + new String("b");
        // 此时"ab" 已存在与串池中，所以放入失败，str还是堆中的"ab"，但是会返回串池中的 "ab" 
        String str2 = str.intern();

        System.out.println(str == str2);	// false，堆中的str没有放入串池中，而str2是从串池的返回的

        System.out.println(str2 == str3);	 // true
    }
}
```





> StringTable 的位置

字符串池在**JDK1.6**时存在于**永久代**中，**1.8**时存在于**堆**中。

![image-20220426221311973](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220426221311973.png)





> StringTable 垃圾回收

- `-Xmx10m` 指定堆内存大小
- `-XX:+PrintStringTableStatistics` 打印字符串常量池信息
- `-XX:+PrintGCDetails`
- `-verbose:gc` 打印 gc 的次数，耗费时间等信息

演示 StringTable 的垃圾回收：

```java
// 配置 -Xmx10m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails -verbose:gc
public static void main(String[] args) {
    int i = 0;
    try {
        for(int j = 0; j < 10000; j++) { // j = 100, j = 10000
            String.valueOf(j).intern();
            i++;
        }
    }catch (Exception e) {
        e.printStackTrace();
    }finally {
        System.out.println(i);
    }
}
```

运行查看 StringTable 的数据统计：

- j = 100 时，键值数量为1854(还包括一些方法等)，指定的堆内存大小还够用，未进行垃圾回收：

![image-20220427230156382](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220427230156382.png)

- j = 10000 时，键值数量只有7226个，并没有全部加入进来，因为此时JVM将一些没有被引用的无用字符串垃圾收回了：

![image-20220427230725518](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220427230725518.png)

![image-20220427230736977](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220427230736977.png)





> StringTable 性能调优

1. StringTable的数据结构为 hashtable ，如果程序里字符串常量的个数非常多，可以适当修改虚拟机参数`-XX:StringTableSize(桶的个数最小为1009)的大小`
   - 原理是增大桶的数量，减小冲突，来减少字符串放入串池的过程中所需要的时间，提升效率
2. 考虑是否需要字符串对象入池；可以通过 `intern()` 方法减少重复入池，实现调优

















