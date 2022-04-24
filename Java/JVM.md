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

- `java.lang.OutofMemoryError : java heap space`
- 可以使用`Xmx8m`来指定堆内存大小

**堆内存诊断**：

1. jps 工具：查看当前系统中有那些 java 进程
2. jmap 工具：查看堆内存占用情况 `jmap - heap 进程id`
3. jconsole 工具：图形界面的，多功能的监测工具，可以连续监测
4. jvisualvm 工具



## 2.5 方法区 Method Area

**定义**：











