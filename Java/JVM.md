# 1. 初识JVM

## 1.1 什么是JVM

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





### 1）运行时常量池：

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





## 2.6 直接内存 Direct Memory

**定义**：

- 常见于NIO操作时，用于数据缓冲区
- 分配回收成本较高，但读写性能高
- 不受JVM内存回收管理

**使用直接内存的好处**：
文件读写流程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210208180041113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

因为 java 不能直接操作文件管理，需要切换到内核态，使用本地方法进行操作，然后读取磁盘文件，会在系统内存中创建一个缓冲区，将数据读到系统缓冲区， 然后在将系统缓冲区数据，复制到 java 堆内存中。

缺点是数据存储了两份，在系统内存中有一份，java 堆中有一份，造成了不必要的复制。

使用了 DirectBuffer 文件读取流程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210208181022863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

直接内存是操作系统和 Java 代码都可以访问的一块区域，无需将代码从系统内存复制到 Java 堆内存，从而提高了效率。

**直接内存的回收原理**：

```java
public class DirectMemoryTest {

    public static int _1GB = 1024 * 1024 * 1024;

    public static void main(String[] args) throws IOException, NoSuchFieldException, IllegalAccessException {
//        method();
        method1();
    }

    // 演示 直接内存是被 unsafe 创建与回收
    private static void method1() throws IOException, NoSuchFieldException, IllegalAccessException {

        // 通过反射获取 Unsafe 对象
        Field field = Unsafe.class.getDeclaredField("theUnsafe");
        field.setAccessible(true);
        Unsafe unsafe = (Unsafe)field.get(Unsafe.class);
	
        // 分配内存
        long base = unsafe.allocateMemory(_1GB);
        unsafe.setMemory(base, _1GB,  (byte)0);
        System.in.read();
        
	// 释放内存
        unsafe.freeMemory(base);
        System.in.read();
    }

    // 演示 直接内存 被释放
    private static void method() throws IOException {
        ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1GB);
        System.out.println("分配完毕");
        System.in.read();
        System.out.println("开始释放");
        byteBuffer = null;
        System.gc(); 	// 手动 gc
        System.in.read();
    }

}
```

直接内存的回收不是通过 JVM 的垃圾回收来释放的，而是通过`unsafe.freeMemory` 来手动释放。

> 回收过程（源码）

1. `allocateDirect` 方法的实现，底层是创建了一个 `DirectByteBuffer` 对象：

   ```java
   public static ByteBuffer allocateDirect(int capacity) {
       return new DirectByteBuffer(capacity);
   }
   ```

2. `DirectByteBuffer`类：

   ```java
   DirectByteBuffer(int cap) {   // package-private
      
       super(-1, 0, cap, cap);
       boolean pa = VM.isDirectMemoryPageAligned();
       int ps = Bits.pageSize();
       long size = Math.max(1L, (long)cap + (pa ? ps : 0));
       Bits.reserveMemory(size, cap);
   
       long base = 0;
       try {
           base = unsafe.allocateMemory(size); 	// 申请内存分配
       } catch (OutOfMemoryError x) {
           Bits.unreserveMemory(size, cap);
           throw x;
       }
       unsafe.setMemory(base, size, (byte) 0);
       if (pa && (base % ps != 0)) {
           // Round up to page boundary
           address = base + ps - (base & (ps - 1));
       } else {
           address = base;
       }
       // 通过虚引用，来实现直接内存的释放，this为虚引用的实际对象, 第二个参数是一个回调，实现了 runnable 接口，run 方法中通过 unsafe 释放内存。
       cleaner = Cleaner.create(this, new Deallocator(base, size, cap)); 
       att = null;
   }
   
   ```

   这里用到了一个 `Cleaner`（虚引用）来监测对象，如果虚引用的实际对象（这里是 `DirectByteBuffer` ）被回收以后，就会调用 `Cleaner` 的 `clean` 方法，来清除直接内存中占用的内存。

   ```java
   public void clean() {
       if (remove(this)) {
           try {
               // 都用函数的 run 方法, 释放内存
               this.thunk.run();
           } catch (final Throwable var2) {
               AccessController.doPrivileged(new PrivilegedAction<Void>() {
                   public Void run() {
                       if (System.err != null) {
                           (new Error("Cleaner terminated abnormally", var2)).printStackTrace();
                       }
   
                       System.exit(1);
                       return null;
                   }
               });
           }
   
       }
   }
   ```

   可以看到关键的一行代码， `this.thunk.run()`，thunk 是 Runnable 对象。run 方法就是回调 Deallocator 中的 run 方法：

   ```java
       public void run() {
           if (address == 0) {
               // Paranoia
               return;
           }
           // 释放内存
           unsafe.freeMemory(address);
           address = 0;
           Bits.unreserveMemory(size, capacity);
       }
   ```

**总结**：

- 使用了 Unsafe 类来完成直接内存的分配回收，回收需要主动调用freeMemory 方法
- ByteBuffer 的实现内部使用了 Cleaner（虚引用）来检测 ByteBuffer 。一旦ByteBuffer 被垃圾回收，那么会由 ReferenceHandler（守护线程） 来调用 Cleaner 的 clean 方法调用 freeMemory 来释放内存

**注意**：

一般用 JVM 调优时，会加上下面的参数：

`-XX:+DisableExplicitGC  // 禁止显示的 GC`

```java
/**
     * -XX:+DisableExplicitGC 禁用显示的垃圾回收
     */
    private static void method() throws IOException {
        ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1GB);
        System.out.println("分配完毕");
        System.in.read();
        System.out.println("开始释放");
        byteBuffer = null;
        System.gc(); 		// 手动 gc 失效，没有触发Java中的垃圾回收，内存没有被回收掉，只有当真正的垃圾回收发生时，才会被清理，内存才会被释放。可以使用手动freeMemory来对直接内存进行管理
        System.in.read();
    }
```

禁止我们手动的 GC，比如手动 `System.gc()` 无效，它是一种 full gc，会回收新生代、老年代，会造成程序执行的时间比较长。所以我们就像上面的演示那样，通过 unsafe 对象手动调用 freeMemory 的方式释放内存。





# 3. 垃圾回收

## 3.1 如何判断对象可以回收

### 1）引用计数法

当一个对象被引用时，就将引用对象的值加一，当值为 0 时，就表示该对象不被引用，可以被垃圾收集器回收。

弊端：

- 循环引用时，两个对象的计数都为1，导致两个对象都无法被释放，但没有其他对象引用他们。如下图所示：

![image-20220428141119784](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428141119784.png)



### 2）可达性分析算法

- JVM中的垃圾回收器采用可达性分析来探索所有存活的对象
- 扫描堆中的对象，看能否沿着 GC Root 对象为起点的引用链找到该对象，如果找不到，则表示可以回收





> 哪些对象可以作为 GC Root ？

```java
public static void main(String[] args) throws IOException {
    ArrayList<Object> list = new ArrayList<>();
    list.add("a");
    list.add("b");
    list.add(1);
    System.out.println(1);
    
    System.in.read();

    list = null;
    System.out.println(2);
    System.in.read();
    System.out.println("end");
}
```

对于以上代码，可以使用如下命令将堆内存信息转储成一个文件，然后使用
Eclipse Memory Analyzer 工具进行分析。

1、使用jps命令，查看程序的进程：

![image-20220428144825113](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428144825113.png)

2、使用 `jmap -dump:format=b,live,file=1.bin 9892` 命令转储文件

- dump：转储文件
- format=b：二进制文件
- live：抓快照时只抓存活的对象，live前会进行一次垃圾回收，得到哪些是存活对象
- file：文件名
- 9892：进程的id

![image-20220428145229190](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428145229190.png)

3、在程序的窗口输入回车，执行`list = null;`后再次执行步骤2的命令，得到文件`2.bin`：

![image-20220428145504932](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428145504932.png)

4、打开 Eclipse Memory Analyzer ，对生成的文件进行分析：

![image-20220428151009846](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428151009846.png)

查看文件1 GC Root 中的 Thread，可以发现此时ArrayList也是 GC Root：

![image-20220428151631197](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428151631197.png)

查看文件2 GC Root 中的 Thread，因为list被置为null了，所以会被回收掉，跟对象中无ArrayList了：

![image-20220428151829556](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428151829556.png)

### 3）四种引用

其实有以下五种：

- 强引用
- 软引用
- 弱引用
- 虚引用
- 终结器引用

通过下图来说明这五种引用被回收的情况，实线表示强引用：

![image-20220428152509364](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428152509364.png)

- **强引用**：只有**所有 GC Roots 对象都不通过强引用引用该对象**，该对象才能被垃圾回收
- **软引用**：仅有软引用引用该对象时，在垃圾回收后，**内存仍不足时**会再次触发垃圾回收，回收软引用对象，可以配合引用队列来释放软引用自身(因为**软引用自身也是一个对象**)
- **弱引用**：仅有弱引用引用该对象时，在垃圾回收后，**无论内存是否充足**，都会回收弱引用对象，但是**弱引用可以存活到下一次垃圾回收发生之前**，也可以配合引用队列来释放弱引用自身(因为**弱引用自身也是一个对象**)
- **虚引用**：**必须配合引用队列**使用，主要配合 `ByteBuffer` 使用，**被引用对象回收时**，会**将虚引用放入队列**，由 `Reference Handler` 线程调用虚引Cleaner中的clean方法，该方法会根据记录的直接内存地址，调用`Unsafe.freeMemory`方法释放直接内存(在上面的直接内存有讲解)
- **终结器引用**：无需手动编码，但其内部配合引用队列使用，在**第一次 GC** 垃圾回收时，**终结器引用入队**（**被引用对象暂时没有被回收**），再由 `Finalizer` 线程通过**终结器引用找到被引用对象**并调用它的 `finalize` 方法，**第二次 GC** 时才能**回收被引用对象**

 



> 软引用演示

```java
/**
 * 演示 软引用
 * -Xmx20m -XX:+PrintGCDetails -verbose:gc
 */
public class SoftReferenceTest {

    public static int _4MB = 4 * 1024 * 1024;

    public static void main(String[] args) throws IOException {
        // method1();
        method2();
    }

    // 设置 -Xmx20m , 演示堆内存不足,
    public static void method1() throws IOException {
        ArrayList<byte[]> list = new ArrayList<>();

        for(int i = 0; i < 5; i++) {
            list.add(new byte[_4MB]);
        }
        System.in.read();
    }

    // 演示 软引用
    public static void method2() throws IOException {
        // list --> SoftReference --> byte[]
        
        // 现在 list 对 SoftReference 是强引用
        ArrayList<SoftReference<byte[]>> list = new ArrayList<>();
        for(int i = 0; i < 5; i++) {
            // 现在 SoftReference 对 byte 是软引用
            SoftReference<byte[]> ref = new SoftReference<>(new byte[_4MB]);
            System.out.println(ref.get());
            list.add(ref);
            System.out.println(list.size());
        }
        System.out.println("循环结束：" + list.size());
        for(SoftReference<byte[]> ref : list) {
            System.out.println(ref.get());
        }
    }
}
```

method1 方法解析：
首先会设置一个堆内存的大小为 20m，然后运行 mehtod1 方法，会抛异常，堆内存不足，因为 mehtod1 中的 list 都是强引用。
 ![image-20220428191029386](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428191029386.png)
method2 方法解析：
在 list 集合中存放了 软引用对象，当内存不足时，会触发 full gc，在垃圾回收后，**内存仍不足时**会再次触发垃圾回收，将软引用的对象回收。细节如图：

![image-20220428192437466](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428192437466.png) 

![image-20220428193249783](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428193249783.png)

上面的代码中，当软引用引用的对象被回收了，但是**软引用还存在**，为null值，所以一般**软引用需要搭配一个引用队列**一起使用，将软引用本身也做一次清理。
修改 method2 如下：

```java
    // 演示 软引用 + 引用队列
    public static void method2() throws IOException {
        // list --> SoftReference --> byte[]

        // 现在 list 对 SoftReference 是强引用
        ArrayList<SoftReference<byte[]>> list = new ArrayList<>();

        // 引用队列
        ReferenceQueue<byte[]> queue = new ReferenceQueue<>();

        for(int i = 0; i < 5; i++) {
            // 现在 SoftReference 对 byte 是软引用
            // 关联了引用队列，当软引用所关联的byte[]被回收时，软引用自身会加入到queue中去
            SoftReference<byte[]> ref = new SoftReference<>(new byte[_4MB], queue);
            System.out.println(ref.get());
            list.add(ref);
            System.out.println(list.size());
        }
        
        // 从队列中获取无用的 软引用对象，并移除
        Reference<? extends byte[]> poll = queue.poll();
        while (poll != null) {
            list.remove(poll);
            poll = queue.poll();
        }
        
        System.out.println("循环结束：" + list.size());
        for(SoftReference<byte[]> ref : list) {
            System.out.println(ref.get());
        }
    }
```

可以看到，循环结束后，只剩下第五个byte数组对象：

![image-20220428194151918](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428194151918.png)





> 弱引用演示

```java
public class WeakReferenceTest {

    public static void main(String[] args) {
//        method1();
        method2();
    }

    public static int _4MB = 4 * 1024 *1024;

    // 演示 弱引用
    public static void method1() {
        // list --> WeakReference --> byte[]

        List<WeakReference<byte[]>> list = new ArrayList<>();
        for(int i = 0; i < 10; i++) {
            WeakReference<byte[]> weakReference = new WeakReference<>(new byte[_4MB]);
            list.add(weakReference);

            System.out.print("第" + (i + 1) + "次：");
            for(WeakReference<byte[]> wake : list) {
                System.out.print(wake.get() + ",");
            }
            System.out.println();
        }
        System.out.println("循环结束：" + list.size());
    }

    // 演示 弱引用 + 引用队列
    public static void method2() {
        List<WeakReference<byte[]>> list = new ArrayList<>();
        ReferenceQueue<byte[]> queue = new ReferenceQueue<>();

        for(int i = 0; i < 10; i++) {
            WeakReference<byte[]> weakReference = new WeakReference<>(new byte[_4MB], queue);
            list.add(weakReference);
            System.out.print("第" + (i + 1) + "次：");
            for(WeakReference<byte[]> wake : list) {
                System.out.print(wake.get() + ",");
            }
            System.out.println();
        }
        System.out.println("===========================================");
        Reference<? extends byte[]> poll = queue.poll();
        while (poll != null) {
            list.remove(poll);
            poll = queue.poll();
        }
        for(WeakReference<byte[]> wake : list) {
            System.out.print(wake.get() + ",");
        }
    }

}
```

method 1 分析：

![image-20220428200843046](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428200843046.png)

method 2 分析：

![image-20220428201411175](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428201411175.png)

## 3.2 垃圾回收算法

### 1）标记清除

定义：Mark Sweep

- 速度较快，不用管理被清除后的内存块
- 不移动对象，与保守式GC算法兼容。在保守式GC算法中对象是不能移动的
- 会产生内存碎片。因为对象不移动，所以导致块是不连续的，容易出现空闲内存很多，但分配大对象时找不到合适的块

![image-20220428202817497](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428202817497.png)

### 2）标记整理

Mark Compact

- 速度慢，在整理移动时会花费时间
- 没有内存碎片。在标记清理后，会进行整理，将后面的内存向前移动，不会出现像标记清除那样的碎片化块
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210209143504936.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

### 3）复制

Copy

- 不会有内存碎片
- 需要占用两倍内存空间

复制的过程如下：

在垃圾回收后，将没有被回收的对象从FROM区复制到TO区：![在这里插入图片描述](https://img-blog.csdnimg.cn/20210209144026784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

复制后交换FROM和TO的位置：

![image-20220428203525227](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428203525227.png)

这样即可保证TO区域一直处于空闲状态

## 3.3 分代垃圾回收

![image-20220428204857575](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428204857575.png)

> 整体过程

1. 新创建的对象首先分配在 eden区

   ![image-20220428205016053](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428205016053.png)

2. 新生代空间不足时，触发 minor gc ，eden区 和 from区存活的对象使用 - copy 复制到 to区 中，存活的对象年龄加1，然后交换 from 和 to 的位置

   ![image-20220428204742185](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428204742185.png)

3. minor gc 会引发STW（stop the world），暂停其他线程，native代码可以执行，但不能与JVM交互。等垃圾回收结束后，恢复用户线程运行

4. 当伊甸园中继续添加对象，再次发生 minor gc，重复步骤2。此时幸存区from中没用的对象也会被回收

   ![image-20220428205830075](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428205830075.png)

5. 当幸存区对象的寿命超过阈值时，会晋升到老年代，最大的寿命是 15（4bit）

   ![image-20220428205934967](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428205934967.png)

6. 当老年代空间不足时，会先触发 minor gc，如果空间仍然不足，那么就触发 full gc ，也会引发STW，停止的时间更长

   

### 1）相关 JVM 参数

| 含义                            | 参数                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| 堆初始大小                      | -Xms                                                         |
| 堆最大大小                      | -Xmx 或 -XX:MaxHeapSize=size                                 |
| 新生代大小                      | -Xmn 或 (-XX:NewSize=size + -XX:MaxNewSize=size )            |
| 幸存区比例（动态）              | -XX:InitialSurvivorRatio=ratio 和 -XX:+UseAdaptiveSizePolicy |
| 幸存区比例                      | -XX:SurvivorRatio=ratio                                      |
| 晋升阈值                        | -XX:MaxTenuringThreshold=threshold                           |
| 晋升详情                        | -XX:+PrintTenuringDistribution                               |
| GC详情                          | -XX:+PrintGCDetails -verbose:gc                              |
| FullGC 前执行 MinorGC(默认开启) | -XX:+ScavengeBeforeFullGC                                    |

### 2）GC 分析

```java
public class GCTest {

    private static final int _512KB = 512 * 1024;
    private static final int _1MB = 1024 * 1024;
    private static final int _2MB = 2 * 1024 * 1024;
    private static final int _6MB = 6 * 1024 * 1024;
    private static final int _7MB = 7 * 1024 * 1024;
    private static final int _8MB = 8 * 1024 * 1024;

    // -Xms20m -Xmx20m -Xmn10m -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc
    public static void main(String[] args) {
        List<byte[]> list = new ArrayList<>();
        list.add(new byte[_6MB]);   // 此时 eden 已经占用100%
        list.add(new byte[_512KB]);   // 触发GC
        list.add(new byte[_7MB]);   // 此时eden 占用90%，from 占用61%
        list.add(new byte[_512KB]);   // 触发第二次GC
        list.add(new byte[_2MB]);   //  tenured 占用90%
        list.add(new byte[_1MB]);   // 老年区内存不足，触发Full GC
    }

}
```

通过上面的代码，给 list 分配内存，来观察 新生代和老年代的情况，什么时候触发 minor gc，什么时候触发 full gc 等情况：

1. 当mian函数中没有代码时：

   ![image-20220428212932300](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428212932300.png)

2. 将`_6MB`和`_512KB`添加进 list 中：

   ![image-20220428213559160](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428213559160.png)

3. 再将`_7MB`和`_512MB`添加进 list 中：

   ![image-20220428214155354](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428214155354.png)

4. 再将`_2MB`和`_1MB`添加进 list 中，老年代不够存放，发生堆溢出异常：

   ![image-20220428214947733](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428214947733.png)

> 大对象的情况

若新生代空间不够当前对象的储存，老年代空间够时，会直接将该对象晋升到老年代去，不会发生GC。





> 子线程的堆内存溢出不会影响主线程

- 当一个线程抛出OOM异常后，它所占据的内存资源会全部被释放掉，从而不会影响其他线程的运行。进程无法恢复。

```java
public class Test {
    private static final int _8MB = 8 * 1024 * 1024;

    // -Xms20m -Xmx20m -Xmn10m -XX:+UseSerialGC -XX:+PrintGCDetails -verbose:gc
    public static void main(String[] args) throws InterruptedException {
        // 子线程发生堆内存溢出
        new Thread(()->{
            System.out.println("子线程执行：");
            ArrayList<byte[]> list = new ArrayList<>();
            list.add(new byte[_8MB]);   // 老年代只有10M
            list.add(new byte[_8MB]);
        }).start();

        Thread.sleep(2000);
        System.out.println("主线程执行");
    }
}
```

![image-20220428220257494](C:\Users\AruNi、\AppData\Roaming\Typora\typora-user-images\image-20220428220257494.png)

**注意**：

- 但是实际情况中（线上）大多数都是该线程创建的对象无法被全部释放（存在可达的GC链），导致其余线程申请堆内存失败，发生Full GC，直到内存溢出。这也就是为什么一般情况下，发生OOM，进程基本就凉凉了，是一种链式恶性反应
- 因为发生OOM之前要进行gc，就算其他线程能够正常工作，也会因为频繁gc产生较大的影响

## 4、垃圾回收器

**相关概念：**

- 并行收集：指多条垃圾收集线程并行工作，但此时用户线程仍处于等待状态。
- 并发收集：指用户线程与垃圾收集线程同时工作（不一定是并行的可能会交替执行）。用户程序在继续运行，而垃圾收集程序运行在另一个 CPU 上
- 吞吐量：即 CPU 用于运行用户代码的时间与 CPU 总消耗时间的比值（吞吐量 = 运行用户代码时间 / ( 运行用户代码时间 + 垃圾收集时间 )），也就是。例如：虚拟机共运行 100 分钟，垃圾收集器花掉 1 分钟，那么吞吐量就是 99% 。

### 1）串行

- 单线程
- 堆内存较少，适合个人电脑

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210092812153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

```java
-XX:+UseSerialGC=serial + serialOld
1
```

**安全点**：让其他线程都在这个点停下来，以免垃圾回收时移动对象地址，使得其他线程找不到被移动的对象
 因为是串行的，所以只有一个垃圾回收线程。且在该线程执行回收工作时，其他线程进入阻塞状态

**Serial 收集器**
 Serial 收集器是最基本的、发展历史最悠久的收集器
 **特点**：单线程、简单高效（与其他收集器的单线程相比），采用复制算法。对于限定单个 CPU 的环境来说，Serial  收集器由于没有线程交互的开销，专心做垃圾收集自然可以获得最高的单线程收集效率。收集器进行垃圾回收时，必须暂停其他所有的工作线程，直到它结束（Stop The World）！

**ParNew 收集器**
 ParNew 收集器其实就是 Serial 收集器的多线程版本
 **特点**：多线程、ParNew 收集器默认开启的收集线程数与CPU的数量相同，在 CPU 非常多的环境中，可以使用 -XX:ParallelGCThreads 参数来限制垃圾收集的线程数。和 Serial 收集器一样存在 Stop The World 问题

**Serial Old 收集器**
 Serial Old 是 Serial 收集器的老年代版本
 **特点**：同样是单线程收集器，采用标记-整理算法

### 2）吞吐量优先

- 多线程
- 堆内存较大，多核 cpu
- 让单位时间内，STW 的时间最短 0.2 0.2 = 0.4

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210094915306.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

```java
-XX:+UseParallelGC ~ -XX:+UsePrallerOldGC
-XX:+UseAdaptiveSizePolicy
-XX:GCTimeRatio=ratio // 1/(1+radio)
-XX:MaxGCPauseMillis=ms // 200ms
-XX:ParallelGCThreads=n
12345
```

**Parallel Scavenge 收集器**
 与吞吐量关系密切，故也称为吞吐量优先收集器
 **特点**：属于新生代收集器也是采用复制算法的收集器（用到了新生代的幸存区），又是并行的多线程收集器（与 ParNew 收集器类似）

该收集器的目标是达到一个可控制的吞吐量。还有一个值得关注的点是：GC自适应调节策略（与 ParNew 收集器最重要的一个区别）

GC自适应调节策略：
 Parallel Scavenge 收集器可设置 -XX:+UseAdptiveSizePolicy 参数。
 当开关打开时不需要手动指定新生代的大小（-Xmn）、Eden 与 Survivor 区的比例（-XX:SurvivorRation）、
 晋升老年代的对象年龄（-XX:PretenureSizeThreshold）等，虚拟机会根据系统的运行状况收集性能监控信息，动态设置这些参数以提供最优的停顿时间和最高的吞吐量，这种调节方式称为 GC 的自适应调节策略。

Parallel Scavenge 收集器使用两个参数控制吞吐量：

- XX:MaxGCPauseMillis=ms 控制最大的垃圾收集停顿时间（默认200ms）
- XX:GCTimeRatio=rario 直接设置吞吐量的大小

**Parallel Old 收集器**
 是 Parallel Scavenge 收集器的老年代版本
 **特点**：多线程，采用标记-整理算法（老年代没有幸存区）

### 3）响应时间优先

- 多线程
- 堆内存较大，多核 cpu
- 尽可能让 STW 的单次时间最短 0.1 0.1 0.1 0.1 0.1 = 0.5

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210104030390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

```java
-XX:+UseConcMarkSweepGC ~ -XX:+UseParNewGC ~ SerialOld
-XX:ParallelGCThreads=n ~ -XX:ConcGCThreads=threads
-XX:CMSInitiatingOccupancyFraction=percent
-XX:+CMSScavengeBeforeRemark
1234
```

**CMS 收集器**
 Concurrent Mark Sweep，一种以获取最短回收停顿时间为目标的**老年代收集器**
 **特点**：基于标记-清除算法实现。并发收集、低停顿，但是会产生内存碎片
 **应用场景**：适用于注重服务的响应速度，希望系统停顿时间最短，给用户带来更好的体验等场景下。如 web 程序、b/s 服务
 **CMS 收集器的运行过程分为下列4步：**
 **初始标记**：标记 GC Roots 能直接到的对象。速度很快但是仍存在 Stop The World 问题。
 **并发标记**：进行 GC Roots Tracing 的过程，找出存活对象且用户线程可并发执行。
 **重新标记**：为了修正并发标记期间因用户程序继续运行而导致标记产生变动的那一部分对象的标记记录。仍然存在 Stop The World 问题
 **并发清除**：对标记的对象进行清除回收，清除的过程中，可能任然会有新的垃圾产生，这些垃圾就叫浮动垃圾，如果当用户需要存入一个很大的对象时，新生代放不下去，老年代由于浮动垃圾过多，就会退化为 serial Old 收集器，将老年代垃圾进行标记-整理，当然这也是很耗费时间的！

CMS 收集器的内存回收过程是与用户线程一起并发执行的，可以搭配 ParNew 收集器（多线程，新生代，复制算法）与 Serial Old 收集器（单线程，老年代，标记-整理算法）使用。

### 4）G1 收集器

**定义：** Garbage First
 **适用场景：**

- 同时注重吞吐量和低延迟（响应时间）
- 超大堆内存（内存大的），会将堆内存划分为多个大小相等的区域
- 整体上是标记-整理算法，两个区域之间是复制算法

**相关参数：**
 JDK8 并不是默认开启的，所需要参数开启

```java
-XX:+UseG1GC
-XX:G1HeapRegionSize=size
-XX:MaxGCPauseMillis=time
123
```

#### G1 垃圾回收阶段

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210114932887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)
 Young Collection：对新生代垃圾收集
 Young Collection + Concurrent Mark：如果老年代内存到达一定的阈值了，新生代垃圾收集同时会执行一些并发的标记。
 Mixed Collection：会对新生代 + 老年代 + 幸存区等进行混合收集，然后收集结束，会重新进入新生代收集。

#### Young Collection

**新生代存在 STW：**
 分代是按对象的生命周期划分，分区则是将堆空间划分连续几个不同小区间，每一个小区间独立回收，可以控制一次回收多少个小区间，方便控制 GC 产生的停顿时间！
 E：eden，S：幸存区，O：老年代
 新生代收集会产生 STW ！
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210122339138.gif)

#### Young Collection + CM

在 Young GC 时会进行 GC Root 的初始化标记
 老年代占用堆空间比例达到阈值时，进行并发标记（不会STW），由下面的 JVM 参数决定 -XX:InitiatingHeapOccupancyPercent=percent （默认45%）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210122601873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### Mixed Collection

会对 E S O 进行**全面的回收**

- 最终标记会 STW
- 拷贝存活会 STW

-XX:MaxGCPauseMills=xxms 用于指定最长的停顿时间！
 问：为什么有的老年代被拷贝了，有的没拷贝？
 因为指定了最大停顿时间，如果对所有老年代都进行回收，耗时可能过高。为了保证时间不超过设定的停顿时间，会回收最有价值的老年代（回收后，能够得到更多内存）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210144216170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### Full GC

G1 在老年代内存不足时（老年代所占内存超过阈值）
 如果垃圾产生速度慢于垃圾回收速度，不会触发 Full GC，还是并发地进行清理
 如果垃圾产生速度快于垃圾回收速度，便会触发 Full GC，然后退化成 serial Old 收集器串行的收集，就会导致停顿的时候长。

#### Young Collection 跨代引用

- 新生代回收的跨代引用（老年代引用新生代）问题
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210154730275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)
- 卡表 与 Remembered Set   
  - Remembered Set 存在于E中，用于保存新生代对象对应的脏卡     
    - 脏卡：O 被划分为多个区域（一个区域512K），如果该区域引用了新生代对象，则该区域被称为脏卡
- 在引用变更时通过 post-write barried + dirty card queue
- concurrent refinement threads 更新 Remembered Set
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210154940579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### Remark

重新标记阶段
 在垃圾回收时，收集器处理对象的过程中

- 黑色：已被处理，需要保留的
- 灰色：正在处理中的
- 白色：还未处理的
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210161204728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

但是在并发标记过程中，有可能 A 被处理了以后未引用 C ，但该处理过程还未结束，在处理过程结束之前 A 引用了 C ，这时就会用到 remark 。
 过程如下

- 之前 C 未被引用，这时 A 引用了 C ，就会给 C 加一个写屏障，写屏障的指令会被执行，将 C 放入一个队列当中，并将 C 变为 处理中状态
- 在并发标记阶段结束以后，重新标记阶段会 STW ，然后将放在该队列中的对象重新处理，发现有强引用引用它，就会处理它，由灰色变成黑色。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210161559793.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210161527103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### JDK 8u20 字符串去重

过程

- 将所有新分配的字符串（底层是 char[] ）放入一个队列
- 当新生代回收时，G1 并发检查是否有重复的字符串
- 如果字符串的值一样，就让他们引用同一个字符串对象
- 注意，其与 String.intern() 的区别   
  - String.intern() 关注的是字符串对象
  - 字符串去重关注的是 char[]
  - 在 JVM 内部，使用了不同的字符串标

优点与缺点

- 节省了大量内存
- 新生代回收时间略微增加，导致略微多占用 CPU

```java
-XX:+UseStringDeduplication
1
```

#### JDK 8u40 并发标记类卸载

在并发标记阶段结束以后，就能知道哪些类不再被使用。如果一个类加载器的所有类都不在使用，则卸载它所加载的所有类

#### JDK 8u60 回收巨型对象

- 一个对象大于region的一半时，就称为巨型对象
- G1不会对巨型对象进行拷贝
- 回收时被优先考虑
- G1会跟踪老年代所有incoming引用，如果老年代incoming引用为0的巨型对象就可以在新生代垃圾回收时处理掉

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210210165555732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDI4MDU3Ng==,size_16,color_FFFFFF,t_70)

#### JDK 9 并发标记起始时间的调整

- 并发标记必须在堆空间占满前完成，否则退化为 FulGC
- JDK 9 之前需要使用 -XX:InitiatingHeapOccupancyPercent
- JDK 9 可以动态调整   
  - -XX:InitiatingHeapOccupancyPercent 用来设置初始值
  - 进行数据采样并动态调整
  - 总会添加一个安全的空挡空间

## 5、垃圾回收调优

查看虚拟机参数命令

```java
D:\JavaJDK1.8\bin\java  -XX:+PrintFlagsFinal -version | findstr "GC"
1
```

可以根据参数去查询具体的信息

### 1）调优领域

- 内存
- 锁竞争
- cpu 占用
- io
- gc

### 2）确定目标

低延迟/高吞吐量？ 选择合适的GC

- CMS G1 ZGC
- ParallelGC
- Zing

### 3）最快的 GC

首先排除减少因为自身编写的代码而引发的内存问题

- 查看 Full GC 前后的内存占用，考虑以下几个问题   
  - 数据是不是太多？     
    - resultSet = statement.executeQuery(“select * from 大表 limit n”)
  - 数据表示是否太臃肿     
    - 对象图
    - 对象大小 16 Integer 24 int 4
  - 是否存在内存泄漏     
    - static Map map …
    - 软
    - 弱
    - 第三方缓存实现

### 4）新生代调优

- 新生代的特点
  - 所有的 new 操作分配内存都是非常廉价的     
    - TLAB thread-lcoal allocation buffer
  - 死亡对象回收零代价
  - 大部分对象用过即死（朝生夕死）
  - Minor GC 所用时间远小于 Full GC
- 新生代内存越大越好么？
  - 不是     
    - 新生代内存太小：频繁触发 Minor GC ，会 STW ，会使得吞吐量下降
    - 新生代内存太大：老年代内存占比有所降低，会更频繁地触发 Full GC。而且触发 Minor GC 时，清理新生代所花费的时间会更长
  - 新生代内存设置为内容纳[并发量*(请求-响应)]的数据为宜
- 幸存区需要能够保存 当前活跃对象+需要晋升的对象
- 晋升阈值配置得当，让长时间存活的对象尽快晋升

```java
-XX:MaxTenuringThreshold=threshold
-XX:+PrintTenuringDistrubution
12
```

### 5）老年代调优

以 CMS 为例：

- CMS 的老年代内存越大越好
- 先尝试不做调优，如果没有 Full GC 那么已经，否者先尝试调优新生代。
- 观察发现 Full GC 时老年代内存占用，将老年代内存预设调大 1/4 ~ 1/3

```java
-XX:CMSInitiatingOccupancyFraction=percent
1
```

### 6）案例

案例1：Full GC 和 Minor GC 频繁
 案例2：请求高峰期发生 Full GC，单次暂停时间特别长（CMS）
 案例3：老年代充裕情况下，发生 Full GC（jdk1.7）



