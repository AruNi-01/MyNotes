## 1. 什么是JUC

JUC 就是 `java.util.concurrent` 下面的类包，专门用于多线程的开发。

## 2. 线程和进程

### 2.1 进程

一个在内存中运行的应用程序。每个进程都有自己独立的一块内存空间，一个进程可以有多个线程，比如在Windows 系统中，一个运行的 xx.exe 就是一个进程。

### 2.2 线程

线程是进程中的一个执行任务（控制单元），负责当前进程中程序的执行。

一个进程至少有一个线程，一个进程可以运行多个线程，多个线程可共享数据。

对于 Java 而言：Thread、Runable、Callable 进行开启线程的。

Java 默认有**2个线程**： main线程、GC线程。

> Java 真的可以开启线程吗？

Java 是没有权限去开启线程、操作硬件的。

线程`Thread` 类中的 `start()` 方法是一个 `native` 本地方法，它调用的底层的 `C++` 代码：

```java
public synchronized void start() {
        /**
         * This method is not invoked for the main method thread or "system"
         * group threads created/set up by the VM. Any new functionality added
         * to this method in the future may have to also be added to the VM.
         *
         * A zero status value corresponds to state "NEW".
         */
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

        /* Notify the group that this thread is about to be started
         * so that it can be added to the group's list of threads
         * and the group's unstarted count can be decremented. */
        group.add(this);

        boolean started = false;
        try {
            start0();
            started = true;
        } finally {
            try {
                if (!started) {
                    group.threadStartFailed(this);
                }
            } catch (Throwable ignore) {
                /* do nothing. If start0 threw a Throwable then
                  it will be passed up the call stack */
            }
        }
    }

    //这是一个C++底层，Java是没有权限操作底层硬件的
    private native void start0();
```

### 2.3 并发

多线程操作同一个资源。

- CPU 只有一核，模拟出来多条线程。可以使用 CPU 快速交替，来模拟多线程。
- 并发编程的本质：**充分利用CPU的资源**。

### 2.4 并行

- CPU 有多核，多个线程可以同时执行。 可以使用线程池去操作。
- 并行才是真正的同时执行。

**获取 cpu 的核数**：

```java
public class Test1 {
    public static void main(String[] args) {
        // 获取cpu的核数
        System.out.println(Runtime.getRuntime().availableProcessors());	// 12
    }
}
```

### 2.5 线程的状态

`Thread` 类中查看线程的状态枚举类：

```java
public enum State {

        // 新生
        NEW,

        // 运行
        RUNNABLE,

        // 阻塞
        BLOCKED,

        // 等待（死等）
        WAITING,

        // 超时等待（超时后就不等了）
        TIMED_WAITING,

        // 终止
        TERMINATED;
}
```

### 2.6 wait/sleep

> 区别

|       | 来源类 | 锁的释放   | 使用范围                 | 异常捕获 |
| ----- | ------ | ---------- | ------------------------ | -------- |
| wait  | Object | 会释放锁   | 同步代码块(Synchronized) | 不需要   |
| sleep | Thread | 不会释放锁 | 任何地方                 | 必须要   |

## 3. Lock

### 3.1 传统的 Synchronized

列举一个卖票的例子。

```java
package com.marchsoft.juctest;

import lombok.Synchronized;

/**
 * Description：synchronized
 *
 * @author jiaoqianjin
 * Date: 2020/8/10 21:36
 **/

public class Demo01 {
    public static void main(String[] args) {
        final Ticket ticket = new Ticket();

        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sale();
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sale();
            }
        },"B").start();
        new Thread(()->{
            for (int i = 0; i < 40; i++) {
                ticket.sale();
            }
        },"C").start();
    }
}
// 资源类 OOP 属性、方法
class Ticket {
    private int number = 30;

    //卖票的方式
    public synchronized void sale() {
        if (number > 0) {
            System.out.println(Thread.currentThread().getName() + "卖出了第" + (number--) + "张票剩余" + number + "张票");
        }
    }
}
```

当 `public void sale()` 没有加 `Synchronized` 关键字时，输出的结果是有误的：

![image-20220623224752999](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220623224752999.png)

加上 `Synchronized` 关键字后，输出结果正常：

![image-20220623224910963](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220623224910963.png)

`Synchronized` 本质：队列、锁。

### 3.2 Lock

`Lock` 接口的实现类：

![image-20220623231112473](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220623231112473.png)

- `ReentrantLock`：不传参数时，默认是一个**非公平锁**（可以插队）；传参数 true 时，是一个**公平锁**（先来先到）。

  ![image-20220623232021283](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220623232021283.png)

- `ReentrantReadWriteLock`：

  - `ReentrantReadWriteLock.ReadLock`：读锁
  - `ReentrantReadWriteLock.WriteLock`：写锁



卖票示例改进：

```java
public class SaleTicketDemo02 {
    public static void main (String[]args){
        Ticket2 ticket2 = new Ticket2();
        new Thread(() -> {
            for (int i = 0; i < 40; i++) {
                ticket2.sale();
            }
        }, "A").start();
        new Thread(() -> {
            for (int i = 0; i < 40; i++) {
                ticket2.sale();
            }
        }, "B").start();
        new Thread(() -> {
            for (int i = 0; i < 40; i++) {
                ticket2.sale();
            }
        }, "C").start();
    }
}

// lock 三部曲
// 1、Lock lock=new ReentrantLock();
// 2、lock.lock() 加锁
// 3、finally => 解锁：lock.unlock();
class Ticket2 {
    // 属性、方法
    private int number = 30;

    Lock lock = new ReentrantLock();

    // 卖票的方式
    public  void sale() {
        lock.lock();    // 加锁
        try {
            // 业务代码
            if (number > 0) {
                System.out.println(Thread.currentThread().getName() + "卖出了第 " + (number--) + " 张票，剩余 " + number + " 张票！");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();  // 解锁
        }
    }
}
```

### 3.3 Synchronized 与 Lock 的区别

1、`Synchronized` 内置的Java关键字，`Lock` 是一个 Java 类

2、`Synchronized` 无法判断获取锁的状态，`Lock` 可以判断

3、`Synchronized` 会自动释放锁，`Lock` 必须要手动加锁和手动释放锁！**可能会遇到死锁**

4、`Synchronized` 线程1(获得锁->阻塞)、线程2(一直等待)；`Lock` 就不一定会一直等待下去，**Lock 会有一个 `tryLock()` 方法去尝试获取锁**，不会造成长久的等待

5、`Synchronized` 是可重入锁，不可中断，非公平的；`Lock` 是可重入，可判断锁，可以自己设置公平锁和非公平锁（**可重入锁**：以线程为单位，当一个线程获取对象锁之后，这个线程可以再次获取本对象上的锁，而其他的线程是不可以的）

6、`Synchronized` 适合锁少量的代码同步问题；`Lock` 适合锁大量的同步代码

## 4. 生产者和消费者的关系

线程之间的通信问题。线程交替执行，例如多个线程操作同一个变量，不可能同时操作，所以线程之间需要通信。

### 4.1 Synchronzied 版本

```java
public class ProducerAndConsumer01 {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();
    }
}

// 判断等待 =》具体业务 =》通知
class Data {    // 数字 资源类
    private int number = 0;

    // +1操作
    public synchronized void increment() throws InterruptedException {
        // 判断等待，直到number=0再执行+1操作
        if (number != 0) {
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName() + "==>" + number);
        // 通知其他线程 +1执行完毕了
        this.notifyAll();
    }

    // -1操作
    public synchronized void decrement() throws InterruptedException {
        // 判断等待，直到number!=0再执行-1操作
        if (number == 0) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "==>" + number);
        // 通知其他线程 +1执行完毕了
        this.notifyAll();
    }
}
```

运行，交替的进行了加一减一的操作：

```text
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0

Process finished with exit code 0
```



> 存在问题（虚假唤醒）

**如果有2个以上的线程，会出现虚假唤醒**。

在上面代码中再增加两个线程，一个执行加一，一个执行减一。此时就有2个加一线程，2个减一线程。执行后输出不是在 0 和 1 之间交替：

```text
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
C==>1
B==>0
A==>1
B==>0
C==>1
A==>2
C==>3
D==>2
D==>1
D==>0
C==>1
D==>0
C==>1
D==>0

Process finished with exit code 0
```



> 解决方法：if 改为 while

**问题根源**：`wait()` 等待时如果被唤醒，代码是从 `wait()` 之后的代码开始执行的。

- 使用 `if` 判断等待时，唤醒后表明 `wait()` 这行代码执行完了，那么 if 分支也执行完了，就会直接执行后面的代码，若在被唤醒的过程中，其他线程抢先一步，对数字进行了修改，由于 if 分支执行完了，不会再次判断等待，直接执行后面的代码，结果导致出现虚假唤醒。

- 使用 `while` 循环判断等待时，在唤醒后 还是会循环执行一次判断等待，不会出现虚假唤醒。

代码如下：

```java
public class ProducerAndConsumer01 {
    public static void main(String[] args) {
        Data data = new Data();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

// 判断等待 =》具体业务 =》通知
class Data {    // 数字 资源类
    private int number = 0;

    // +1操作
    public synchronized void increment() throws InterruptedException {
        // 判断等待，直到number=0再执行+1操作
        while (number != 0) {
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName() + "==>" + number);
        // 通知其他线程 +1执行完毕了
        this.notifyAll();
    }

    // -1操作
    public synchronized void decrement() throws InterruptedException {
        // 判断等待，直到number!=0再执行-1操作
        while (number == 0) {
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName() + "==>" + number);
        // 通知其他线程 +1执行完毕了
        this.notifyAll();
    }
}
```

此时正常输出：

```text
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
A==>1
B==>0
C==>1
D==>0
C==>1
D==>0
C==>1
D==>0
C==>1
D==>0
C==>1
D==>0

Process finished with exit code 0
```

### 4.2 Lock 版

![image-20220627225019083](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220627225019083.png)

`Lock` 和 `Synchronized` 写法大同小异，`Lock` 需要先 `new` 一个 `Condition` 对象，然后调用 `Condition` 中的等待、唤醒方法。`Lock` 需要手动上锁，示例如下：

```java
public class ProducerAndConsumer02 {
    public static void main(String[] args) {
        Data2 data2 = new Data2();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data2.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data2.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data2.increment();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "C").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    data2.decrement();
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }, "D").start();
    }
}

class Data2 {
    private int number = 0;

    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
    // condition.await();   // 等待
    // condition.signalAll();   // 唤醒

    // +1操作
    public void increment() throws InterruptedException {
        lock.lock();        // 上锁
        try {              // 业务
            while (number != 0) {
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName() + "==>" + number);
            condition.signalAll();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {     // 解锁
            lock.unlock();
        }
    }

    // -1操作
    public void decrement() throws InterruptedException {
        lock.lock();    // 上锁
        try {       // 业务
            while (number == 0) {
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName() + "==>" + number);
            condition.signalAll();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {     // 解锁
            lock.unlock();
        }
    }
}
```

输出：

```text
A==>1
B==>0
A==>1
B==>0
C==>1
B==>0
C==>1
B==>0
C==>1
B==>0
C==>1
D==>0
C==>1
D==>0
A==>1
D==>0
A==>1
D==>0
A==>1
D==>0

Process finished with exit code 0
```

可以看到输出结果和使用 `Synchronized` 时差不多。

那么为什么要有 `Lock` 呢？ `Lock` 的优势在哪里？

### 4.3 Condition 的优势

`Condition` 可以**精准**的通知和唤醒线程。

在上面的示例中，线程的唤醒是随机的。接下来使用 `Condition` 来精准的顺序执行：

```java
public class ProducerAndConsumer03 {
    public static void main(String[] args) {
        Data3 data3 = new Data3();
        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                data3.printA();
            }
        }, "A").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                data3.printB();
            }
        }, "B").start();

        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                data3.printC();
            }
        }, "C").start();
    }
}

class Data3 {
    private Lock lock = new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int flag = 1; // 1:A  2:B  3:C

    public void printA() {
        lock.lock();
        try {
            // 业务代码  判断-》执行-》通知
            while (flag != 1) {
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName() + "==》AAA");
            flag = 2;       // 标志位置为B的
            condition2.signal();    // 指定唤醒condition2（B）
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printB() {
        lock.lock();
        try {
            // 业务代码  判断-》执行-》通知
            while (flag != 2) {
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName() + "==》BBB");
            flag = 3;       // 标志位置为C的
            condition3.signal();    // 指定唤醒condition3（C）
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printC() {
        lock.lock();
        try {
            // 业务代码  判断-》执行-》通知
            while (flag != 3) {
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName() + "==》CCC");
            flag = 1;       // 标志位置为C的
            condition1.signal();    // 指定唤醒condition2（C）
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

运行，可以发现每次都是按顺序执行的：

```text
A==》AAA
B==》BBB
C==》CCC
A==》AAA
B==》BBB
C==》CCC
A==》AAA
B==》BBB
C==》CCC
A==》AAA
B==》BBB
C==》CCC
A==》AAA
B==》BBB
C==》CCC

Process finished with exit code 0
```



## 5. 8 锁现象

如何判断锁的是谁！锁到底锁的是谁？

锁会锁住：对象、Class

8 锁就是关于锁的 8 个问题。



> 问题一

两个同步方法，先执行发短信还是打电话

```java
public class EightLock {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            phone.sendMs();
        }).start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(() -> {
            phone.call();
        }).start();
    }
}

class Phone {
    public synchronized void sendMs() {
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
}
```

输出结果：

```text
发短信
打电话

Process finished with exit code 0
```

输出顺序是因为 `sendMs()` 方法在前面吗？ 其实并不是。



> 问题2

**让发短信 延迟4s**

```java
public class EightLock {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            phone.sendMs();
        }).start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(() -> {
            phone.call();
        }).start();
    }
}

class Phone {
    public synchronized void sendMs() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
}
```

现在结果**还是先发短信，然后再打电话**。

**原因**：

- `synchronized` 锁住的对象是方法的调用者！对于两个方法用的是同一个锁，谁先拿到谁先执行，另外一个等待。上面示例中先调用 `sendMs` 方法，所以它先拿到锁。



> 问题三

加一个普通方法，让一个线程调用 `synchronized` 的方法，另一个调用普通方法

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            try {
                phone.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone.hello(); }).start();
    }
}

class Phone {
    public synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}

```

输出结果：

```text
hello
发短信

Process finished with exit code 0
```

**原因**：

- `hello` 是一个普通方法，不受 `synchronized` 锁的影响，不用等待锁的释放。



> 问题四

如果我们使用的是两个对象，一个调用发短信，一个调用打电话，那么整个顺序是怎么样的呢？

```java
public class EightLock {
    public static void main(String[] args) throws InterruptedException {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            phone1.sendMs();
        }).start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(() -> {
            phone2.call();
        }).start();
    }
}

class Phone {
    public synchronized void sendMs() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
```

输出结果：

```text
打电话
发短信

Process finished with exit code 0
```

**原因**：

- 两个对象**各自拥有一把锁**，互不影响，不会出现等待的情况，发短信睡了4s，所以先执行打电话。





> 问题五、六

**如果把 `synchronized` 的方法加上 `static` 变成静态方法！那么顺序又是怎么样的呢**？

（1）我们先来使用**一个对象**调用两个方法！

答案是：**先发短信，后打电话**

（2）如果我们使用**两个对象**调用两个方法！

答案是：**还是先发短信，后打电话**

**为什么加了 `static` 就始终前面一个对象先执行呢**？

**原因**：

- 加了 `static` 后，静态方法是类一加载就有了的，所以 `synchronized` 锁的是这 1 个 `Class` 类，而不是用 2 把锁去锁这两个方法。
- 不管有多少个对象，这些对象都是属于同一个类，都只有 1 把锁，谁先拿到这个锁就先执行。



> 问题七、八

如果使用一个**静态同步方法**、一个**普通同步方法**、**一个对象**调用顺序是什么？

```java
public class EightLock {
    public static void main(String[] args) throws InterruptedException {
        Phone phone1 = new Phone();

        new Thread(() -> {
            phone1.sendMs();
        }).start();

        TimeUnit.SECONDS.sleep(1);

        new Thread(() -> {
            phone1.call();
        }).start();
    }
}

class Phone {
    // 静态同步方法
    public static synchronized void sendMs() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("发短信");
    }
    // 普通同步方法
    public synchronized void call() {
        System.out.println("打电话");
    }
}
```

输出结果：

```text
打电话
发短信

Process finished with exit code 0
```

**原因**：

- 一个锁的是 **`Class` 类**，一个锁的是**对象的调用者**，两把锁互不影响。



如果使用两个对象分别调用静态同步方法和普通同步方法，调用顺序是什么？

先打电话，后发短信

**原因**：

- 两把锁锁的不是同一个东西，互不影响。

## 6. 集合不安全

### 6.1 List 不安全

```java
public class ListUnsafe {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            new Thread(() -> {
                list.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(list);
            }, String.valueOf(i)).start();
        }
    }
}
```

运行，可能会导致 `java.util.ConcurrentModificationException` 并发修改异常。**ArrayList 在并发情况下是不安全的**。

解决方案：

```java
public class ListUnsafe {
    public static void main(String[] args) {
        /**
         * 解决方案
         * 1. List<String> list = new Vector<>();	Synchronized实现
         * 2. List<String> list = Collections.synchronizedList(new ArrayList<>());	用集合工具类中的同步方法
         * 3. List<String> list = new CopyOnWriteArrayList<>();		Lock实现
         */

        List<String> list = new CopyOnWriteArrayList<>();
        for (int i = 1; i <= 10; i++) {
            new Thread(() -> {
                list.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(list);
            }, String.valueOf(i)).start();
        }
    }
}
```



> CopyOnWrite 思想

写入时复制（CopyOnWrite，简称COW）思想是计算机程序设计领域中的一种优化策略。

`CopyOnWriteArrayList` 在有写操作的时候会 copy 一份数据，然后写完再设置成新的数据。

原理：

- 大多数线程都是读取（假设这段时间里只发生读取操作）同一个容器中的数据时，这样大家读取到数据都是唯一、一致、安全的。
- 当往里面增加数据时，`CopyOnWriteArrayList` 会先 copy 出一个新容器（简称副本），再往副本容器里添加这个新的数据，最后把新的容器赋值给之前旧的容器。
- 在添加数据期间，其他线程如果要读取数据，仍然是读取旧的容器里的数据。因为写的时候不会锁住旧的 `CopyOnWriteArrayList`。



> CopyOnWriteArrayList 相比于 Vector 的优点

- `Vector` 底层是使用 `synchronized` 关键字实现，效率低

  ```java
      public synchronized void addElement(E obj) {
          modCount++;
          ensureCapacityHelper(elementCount + 1);
          elementData[elementCount++] = obj;
      }
  ```

- `CopyOnWriteArrayList` 使用的是 `Lock` 锁，效率更高效

  ```java
      public boolean add(E e) {
          final ReentrantLock lock = this.lock;
          lock.lock();
          try {
              Object[] elements = getArray();
              int len = elements.length;
              Object[] newElements = Arrays.copyOf(elements, len + 1);	// 写入时先copy一个副本
              newElements[len] = e;	// 写入到副本中
              setArray(newElements);	// 写入后将副本set回容器中
              return true;
          } finally {
              lock.unlock();
          }
      }
  ```

  

### 6.2 Set 不安全

**Set 和 List 同理**：多线程情况下，会出现并发修改异常，普通的 Set 集合也是线程不安全的。

解决方案还是两种：

- 使用 `Collections` 工具类的 `synchronized` 包装的Set类
- 使用 `CopyOnWriteArraySet` 写入复制的解决方案

```java
public class SetTest {
    public static void main(String[] args) {
        /**
         * 1. Set<String> set = Collections.synchronizedSet(new HashSet<>());
         * 2. Set<String> set = new CopyOnWriteArraySet<>();
         */
        Set<String> set = new CopyOnWriteArraySet<>();
        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            },String.valueOf(i)).start();
        }
    }
}
```



> HashSet 底层

- `HashSet` 底层就是一个 `HashMap`，元素都存到 `HashMap` 的 Key 上面，而 Value 有一个统一不变的值

- 当有新值加入时，底层的 `HashMap` 会判断 Key 值是否存在，如果不存在，则插入新值，如果存在就不插入

  ```java
      public HashSet() {
          map = new HashMap<>();
      }
  
      // add方法，本质就是map， map的 key不重复，value是一个统一不变的值
      public boolean add(E e) {
          return map.put(e, PRESENT)==null;
      }
  
      private static final Object PRESENT = new Object();	// 统一不变的值    
  ```

注意：

- 说白了，`HashSet` 就是限制了功能的 `HashMap`
- 

### 6.3 Map 不安全

同样的 `HashMap` 基础类也存在**并发修改异常**

```java
public class MapTest {
    public static void main(String[] args) {
        /**
         * 解决方案
         * 1. Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
         *  Map<String, String> map = new ConcurrentHashMap<>();
         */
        Map<String, String> map = new ConcurrentHashMap<>();
        for (int i = 1; i < 100; i++) {
            new Thread(()->{
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }
    }
}
```

Map 中没有 `CopyOnWrite`，而是使用 `ConcurrentHashMap`。



> new HashMap<>()

- `new HashMap<>();` 时，默认等价于 `new HashMap<>(16, 0.75);`

  - 参数一：初始化容量（哈希表中桶的数量）
  - 参数二：加载因子（哈希表在其容量自动扩容之前可以达到多满的一种度量）
  - 当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行扩容、`rehash` 操作（即重建内部数据结构），扩容后的哈希表将具有两倍的原容量。

  ```java
      /**
       * The default initial capacity - MUST be a power of two.
       */
      static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
  
      /**
       * The load factor used when none specified in constructor.
       */
      static final float DEFAULT_LOAD_FACTOR = 0.75f;
  ```

- 为了减少冲突的概率，当 `HashMap` 的数组长度到了一个临界值就会触发扩容，把所有元素 `rehash` 再放到扩容后的容器中，所以说 `rehash` 是一个非常耗时的操作。

而这个临界值是由加载因子和当前容器的容量大小来确定，默认情况下是 16 x 0.75 = 12 时，就会触发扩容操作。

加载因子初始化为 0.75 的原因：

- 加载因子过高，例如为1.0，这样会减少空间开销，提高空间利用率，但同时会增加查询时间的成本
- 加载因子过低，例如为0.5，虽然可以减少查询时间，但是空间利用率很低，同时提高了 `rehash` 操作的次数



> HashMap 底层

[HashMap底层](https://blog.csdn.net/zjdzka/article/details/112246992)



## 7. 多线程的三种创建方式

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



### 方式三：JUC—实现Callable接口

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



## 8. 常用的辅助类

### CountDownLatch（减法计数器）

`CountDownLatch` 类可以设置一个**减法计数器**，然后通过 `countDown()` 方法来进行 -1 的操作，使用 `await()` 方法等待计数器归零，然后再继续执行 `await()` 方法之后的语句。

**主要方法**：

- `countDown()`：减一操作
- `await()`：等待计数器归零，再向下执行

示例：

```java
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        // 总数是6
        CountDownLatch countDownLatch = new CountDownLatch(6);

        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " Go out");
                countDownLatch.countDown(); // 线程执行完后 数量-1
            }, String.valueOf(i)).start();
        }

        countDownLatch.await();     // 等待计数器归零后，再向下执行
        System.out.println("Close door");
    }
}
```

输出：

```text
1 Go out
5 Go out
4 Go out
2 Go out
3 Go out
6 Go out
Close door

Process finished with exit code 0
```



### CyclickBarrier（加法计数器）

`CyclicBarrier` 看英文单词就可以看出大概就是循环阻塞的意思。所以还常称为循环栅栏。

`CyclicBarrier` 主要方法有：

```java
public class CyclicBarrier {

    private int dowait(boolean timed, long nanos); // 供await方法调用 判断是否达到条件 可以往下执行吗
    
    // 创建一个新的CyclicBarrier，它将在给定数量的参与方（线程）等待时触发，每执行一次CyclicBarrier就累加1，达到了parties，就会触发barrierAction的执行
    public CyclicBarrier(int parties, Runnable barrierAction) ;
    
    // 创建一个新的CyclicBarrier ，参数就是目标障碍数，它将在给定数量的参与方（线程）等待时触发，每次执行 CyclicBarrier 一次障碍数会加一，如果达到了目标障碍数，才会执行 cyclicBarrier.await()之后的语句
    public CyclicBarrier(int parties) 
        
    // 返回触发此障碍所需的参与方数量。
    public int getParties()
	
    // 等待，直到所有各方都在此屏障上调用了await 。
    // 如果当前线程不是最后一个到达的线程，那么它会出于线程调度目的而被禁用并处于休眠状态。到所有线程都调用了或者被中断亦或者发生异常中断退出
    public int await()
	
    // 基本同上 多了个等待时间 等待时间内所有线程没有完成，将会抛出一个超时异常
    public int await(long timeout, TimeUnit unit)

    // 障碍重置为其初始状态
    public void reset()

}
```

`public CyclicBarrier(int parties)` 的构造方法第一个参数是目标障碍数，每次执行 CyclicBarrier 一次障碍数会加一，如果达到了目标障碍数，才会执行 `cyclicBarrier.await()` 之后的语句。可以将 CyclicBarrier 理解为加 1 操作。

`public CyclicBarrier(int parties, Runnable barrierAction) ` 的构造方法第一个参数是目标障碍数，每次执行 CyclicBarrier 一次障碍数会加一，如果达到了目标障碍数，就会执行我们传入的 Runnable；

示例：

```java
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7, ()-> {
            System.out.println("已集齐七颗龙珠，成功召唤神龙");
        });

        for (int i = 1; i <= 7; i++) {
            // lambda表达式中拿不到i
            final int temp = i;
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + " 收集了第 " + temp + " 颗龙珠");
                try {
                    cyclicBarrier.await();  // 等待
                } catch (InterruptedException | BrokenBarrierException e) {
                    e.printStackTrace();
                }
                // 每个线程都会等达到目标障碍数后才执行下面的代码
                System.out.println(Thread.currentThread().getName() + "等待完毕");
            }).start();
        }
    }
}
```

输出：

```text
Thread-1 收集了第 2 颗龙珠
Thread-5 收集了第 6 颗龙珠
Thread-4 收集了第 5 颗龙珠
Thread-0 收集了第 1 颗龙珠
Thread-2 收集了第 3 颗龙珠
Thread-6 收集了第 7 颗龙珠
Thread-3 收集了第 4 颗龙珠
已集齐七颗龙珠，成功召唤神龙
Thread-3等待完毕
Thread-5等待完毕
Thread-0等待完毕
Thread-1等待完毕
Thread-6等待完毕
Thread-2等待完毕
Thread-4等待完毕

Process finished with exit code 0
```

**注意**：

- `cyclicBarrier.await()` 之后的代码是等到目标障碍数达到后再统一全部执行。
- `CyclicBarrier(int parties, Runnable barrierAction)` 中的第二个参数是 Runnable 类型的线程方法，等达到了目标障碍数，就会执行我们传入的 Runnable。
- 先执行 Runnable 方法，再执行 `cyclicBarrier.await()` 之后的代码



> CyclicBarrier和CountDownLatch的区别

- CyclicBarrier 只能够唤醒一个任务，CountDownLatch 可以唤起多个任务。
- CyclicBarrier 可以重置，重新使用，但是 CountDownLatch 的值等于0时，就不可重复用了。



### Semaphore（信号灯）

**Semaphore：信号量通常用于限制可以访问某些（物理或逻辑）资源的线程数**。

- Semaphore 的构造方法中传入的第一个参数是最大信号量（可以看成最大线程池），每个信号量初始化为一个最多只能分发一个许可证。

- 使用 `acquire()` 方法获得许可证，`release()` 方法释放许可。

- Semaphore 默认使用的非公平锁，在信号量都被使用的时候会阻塞等待线程释放信号量（默认不会出现抢占情况）。

**使用场景**：

- 限制资源，如抢位置、限流等。

示例：

```java
public class SemaphoreDemo {
    public static void main(String[] args) {
        // 线程数量：停车位 3个
        Semaphore semaphore = new Semaphore(3);

        // 有6辆车
        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                try {
                    semaphore.acquire();    //获得许可
                    System.out.println(Thread.currentThread().getName() + " 抢到了车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName() + " 离开了车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();    // 释放许可
                }
            }).start();
        }
    }
}
```

输出：

```text
Thread-0 抢到了车位
Thread-1 抢到了车位
Thread-2 抢到了车位
Thread-1 离开了车位
Thread-0 离开了车位
Thread-2 离开了车位
Thread-3 抢到了车位
Thread-4 抢到了车位
Thread-5 抢到了车位
Thread-4 离开了车位
Thread-3 离开了车位
Thread-5 离开了车位

Process finished with exit code 0
```

**原理**：

- `semaphore.acquire()` 获得资源，如果资源已经使用完了，就等待资源释放后再进行使用

- `semaphore.release()` 释放资源，会将当前的信号量 +1，然后唤醒其他等待的线程

**作用**： 

- 多个共享资源互斥的使用！ 并发限流，控制最大的线程数

## 9. 读写锁

如果不加锁的情况，多线程的读写会造成数据不可靠的问题，例如在一个线程还没写完的时候，另一个线程又进行写操作，就会导致数据混乱。

可以采用 **Synchronized** 这种重量锁和轻量锁 **Lock** 去保证数据的可靠。

但是更好的选择是采用更细粒度的锁：**ReadWriteLock** 读写锁来保证。

读写锁 `ReadWriteLock`：

- 更细粒度的控制
  - 排他锁：写相关时，只有一个线程写
  - 共享锁：读相关时，有多少线程同时读

读写锁 `ReadWriteLock` 有两个方法，其实现类为 `ReentrantReadWriteLock`：

- `Lock readLock();`
- `Lock writeLock();`

示例：

```java
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache2 myCache = new MyCache2();

        for (int i = 1; i <= 6; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.write(String.valueOf(temp), String.valueOf(temp));
            }, String.valueOf(i)).start();
        }

        for (int i = 1; i <= 6; i++) {
            final int temp = i;
            new Thread(() -> {
                myCache.read(String.valueOf(temp));
            }, String.valueOf(i)).start();
        }
    }

}
class MyCache2 {
    // 被volatile修饰的变量能够保证每个线程能够获取该变量的最新值，从而避免出现数据脏读的现象。
    private volatile Map<String, String> map = new HashMap<>();
    private ReadWriteLock lock = new ReentrantReadWriteLock();	// 读写锁

    public void write(String key, String value) {
        lock.writeLock().lock(); // 写锁
        try {
            System.out.println(Thread.currentThread().getName() + "线程开始写入");
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + "线程写入ok");

        }finally {
            lock.writeLock().unlock(); // 释放写锁
        }
    }

    public void read(String key) {
        lock.readLock().lock(); // 读锁
        try {
            System.out.println(Thread.currentThread().getName() + "线程开始读取");
            map.get(key);
            System.out.println(Thread.currentThread().getName() + "线程写读取ok");
        }finally {
            lock.readLock().unlock(); // 释放读锁
        }
    }
}
```

运行，可以发现在写操作时是一个线程写完，另一个线程再开始写的；而在读的时候，可以多个线程同时读：

```text
1线程开始写入
1线程写入ok
6线程开始写入
6线程写入ok
3线程开始写入
3线程写入ok
2线程开始写入
2线程写入ok
5线程开始写入
5线程写入ok
4线程开始写入
4线程写入ok
    
1线程开始读取
5线程开始读取
2线程开始读取
1线程写读取ok
3线程开始读取
2线程写读取ok
6线程开始读取
6线程写读取ok
5线程写读取ok
4线程开始读取
4线程写读取ok
3线程写读取ok

Process finished with exit code 0
```



## 10. 阻塞队列

阻塞队列 `BlockingQueue` ：

- 当阻塞队列是**空**的时候，从队列中**获取元素**的操作将会被阻塞
- 当阻塞队列是**满**的时候，往队列里**添加元素**的操作将会被阻塞

阻塞队列在数据结构中所起的作用如下图：

![image-20220629174558000](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220629174558000.png)

`Thread1`（生产线程）往阻塞队列中添加元素，`Thread2`（消费线程）从阻塞队列中移除元素；

产品没生产出来的时候，柜子里不存在产品，想要买产品，消费者阻塞（等待柜子装产品）；

产品生产已经足够多了，柜子装满了，此时再继续生产产品没地方放，生产者阻塞（等待柜子空出）。



> BlockingQueue 关系图

BlockingQueue 是 Queue 的子类：

![image-20220629183950790](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220629183950790.png)

BlockingQueue 有多个实现类：

- **ArrayBlockingQueue**：由数组结构组成的有界阻塞队列。
- **LinkedBlockingQueue**：由链表结构组成的有界 (但大小默认值为Integer.MAX_VALUE) 阻塞队列。
- **SynchronousQueue**：不存储元素的阻塞队列，也即单个元素的队列。
- PriorityBlockingQueue：支持优先级排序的无解阻塞队列。
- DelayQueue：使用优先级队列实现的延迟无界阻塞队列。
- LinkedTransferQueue：由链表结构组成的无界阻塞队列。
- LikedBlockingDeque：由链表结构组成的双端阻塞队列。

![image-20220629182826154](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220629182826154.png)



### 阻塞队列 BlockingQueue

BlockingQueue 的四组 API：

| 方式         | 抛出异常  | 不会抛出异常，有返回值 | 阻塞等待 (一直等) | 超时等待 (超时返回false/null)                   |
| ------------ | --------- | ---------------------- | ----------------- | ----------------------------------------------- |
| 添加         | add(E e)  | offer(E e)             | put(E e)          | boolean offer(E e, long timeout, TimeUnit unit) |
| 移出         | remove()  | poll()                 | take()            | E poll(long timeout, TimeUnit unit)             |
| 判断队首元素 | element() | peek()                 | -                 | -                                               |

示例：

```java
public class BlockingQueueDemo {
    public static void main(String[] args) throws InterruptedException {
//        test01();
//        test02();
//        test03();
        test04();
    }

    /**
     * 抛出异常
     */
    public static void test01() {
        // 需要初始化队列的大小，也可以说明类型
        ArrayBlockingQueue<Object> blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.add("a"));     // true
        System.out.println(blockingQueue.add("b"));     // true
        System.out.println(blockingQueue.add("c"));     // true
        // 如果多添加一个，会抛出异常：java.lang.IllegalStateException: Queue full
        // System.out.println(blockingQueue.add("d"));

        // remove() 无参数，先进先出
        System.out.println(blockingQueue.remove());     // a
        System.out.println(blockingQueue.remove());     // b
        System.out.println(blockingQueue.remove());     // c
        // 如果多移除一个，会抛出异常：java.util.NoSuchElementException
        // System.out.println(blockingQueue.remove());

        // 查看队首元素，使用element()：空队列时抛出异常：java.util.NoSuchElementException
        System.out.println(blockingQueue.element());
    }

    /**
     * 不抛出异常，有返回值
     */
    public static void test02() {
        ArrayBlockingQueue<Object> blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.offer("a"));       // true
        System.out.println(blockingQueue.offer("b"));       // true
        System.out.println(blockingQueue.offer("c"));       // true
        System.out.println(blockingQueue.offer("d"));       // false

        System.out.println(blockingQueue.poll());       // a
        System.out.println(blockingQueue.poll());       // b
        System.out.println(blockingQueue.poll());       // c
        System.out.println(blockingQueue.poll());       // null

        // 查看队首元素，使用peek()：空队列时返回 null
        System.out.println(blockingQueue.peek());       // null
    }

    /**
     * 阻塞，等待（一直等）
     */
    public static void test03() throws InterruptedException {
        ArrayBlockingQueue<Object> blockingQueue = new ArrayBlockingQueue<>(3);

        // put() 无返回值
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
        // blockingQueue.put("d");     // 一直阻塞

        System.out.println(blockingQueue.take());       // a
        System.out.println(blockingQueue.take());       // b
        System.out.println(blockingQueue.take());       // c
        // System.out.println(blockingQueue.take());       // 一直阻塞
    }

    /**
     * 超时等待
     */
    public static void test04() throws InterruptedException {
        ArrayBlockingQueue<Object> blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.offer("a"));       // true
        System.out.println(blockingQueue.offer("b"));       // true
        System.out.println(blockingQueue.offer("c"));       // true
        // 超过3秒就返回false
        System.out.println(blockingQueue.offer("d", 3, TimeUnit.SECONDS));

        System.out.println(blockingQueue.poll());       // a
        System.out.println(blockingQueue.poll());       // b
        System.out.println(blockingQueue.poll());       // c
        // 超过3秒就返回null
        System.out.println(blockingQueue.poll(3, TimeUnit.SECONDS));
    }
}
```



### 同步队列 SynchronousQueue

SynchronousQueue 实际上它不是一个真正的队列，因为 SynchronousQueue 没有容量。与其他 BlockingQueue（阻塞队列）不同，SynchronousQueue 是一个不存储元素的 BlockingQueue。只是它维护一组线程，这些线程在等待着把元素加入或移出队列。

SynchronousQueue 特点：

- `put()` 进去一个元素，就必须先 `take()` 出来，否则不能再 `put()` 进去值
- SynchronousQueue 的 `take()` 是使用了**Lock锁 保证线程安全**的
- 特别适用于数据传递和交换的场景。比如对于经典生产者-消费者线程模型：生产者和消费者之间等待对方的到来，然后交换数据，最后离开。

示例：

```java
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>();

        new Thread(() -> {
            try {
                System.out.println(Thread.currentThread().getName() + " put 1");
                blockingQueue.put("1");
                System.out.println(Thread.currentThread().getName() + " put 2");
                blockingQueue.put("2");
                System.out.println(Thread.currentThread().getName() + " put 3");
                blockingQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, "put Thread").start();

        new Thread(() -> {
            try {
                System.out.println(Thread.currentThread().getName() + " take " + blockingQueue.take());
                System.out.println(Thread.currentThread().getName() + " take " + blockingQueue.take());
                System.out.println(Thread.currentThread().getName() + " take " + blockingQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }, "take Thread").start();
    }
}
```

运行，可以看到每次队列中都只有一个元素，只有等该元素被移除后才能重新添加元素：

```text
put Thread put 1
take Thread take 1
put Thread put 2
take Thread take 2
put Thread put 3
take Thread take 3

Process finished with exit code 0
```



## 11. 线程池

线程池：三大方式、七大参数、四种拒绝策略

> 池化技术

程序的运行，本质：占用系统的资源！我们需要去优化资源的使用，资源的创建、销毁十分消耗资源。

所以出现了池化技术。类似的还有 JDBC 的连接池、内存池、对象池 等等。

**池化技术**：事先准备好一些资源，如果有人要用，就过来拿，用完之后放回去，以此来提高效率。



> 线程池的好处

1、降低资源的消耗；

2、提高响应的速度；

3、方便管理；

**线程复用、可以控制最大并发数、管理线程**

### 11.1 三大方法

- 单个线程：`ExecutorService threadPool = Executors.newSingleThreadExecutor();`
- 固定大小的线程池：`ExecutorService threadPool = Executors.newFixedThreadPool(5);`
- 可伸缩的线程池：`ExecutorService threadPool = Executors.newCachedThreadPool();`

示例：

```java
public class Demo01 {
    public static void main(String[] args) {
        ExecutorService threadPool = Executors.newSingleThreadExecutor();   // 单个线程
        ExecutorService threadPool2 = Executors.newFixedThreadPool(5);      // 创建一个固定大小的线程池
        ExecutorService threadPool3 = Executors.newCachedThreadPool();  // 可伸缩的线程池

        try {
            for (int i = 1; i <= 100; i++) {
                threadPool.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + "ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {     // 线程池用完必须要关闭
            threadPool.shutdown();
        }
    }
}
```

### 11.2 七大参数

调用 `Executors.newSingleThreadExecutor()` 、`Executors.newFixedThreadPool(5)` 、`Executors.newCachedThreadPool()` 时，在源码中实际上都调用了 `ThreadPoolExecutor` 。

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}

public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}

public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```



`ThreadPoolExecutor` 的七大参数：

```java
public ThreadPoolExecutor(int corePoolSize,  	// 核心线程池大小
                          int maximumPoolSize, 		// 最大的线程池大小
                          long keepAliveTime,  		// 空闲时存活实际（超时了没有人调用就会释放）
                          TimeUnit unit, 		// 超时单位
                          BlockingQueue<Runnable> workQueue,	 // 阻塞队列
                          ThreadFactory threadFactory, 		// 线程工厂 创建线程的 一般不用动
                          RejectedExecutionHandler handler 		// 拒绝策略
                         ) {
    if (corePoolSize < 0 ||
        maximumPoolSize <= 0 ||
        maximumPoolSize < corePoolSize ||
        keepAliveTime < 0)
        throw new IllegalArgumentException();
    if (workQueue == null || threadFactory == null || handler == null)
        throw new NullPointerException();
    this.corePoolSize = corePoolSize;
    this.maximumPoolSize = maximumPoolSize;
    this.workQueue = workQueue;
    this.keepAliveTime = unit.toNanos(keepAliveTime);
    this.threadFactory = threadFactory;
    this.handler = handler;
}
```



![](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5399b5ce260e990db30f26f84c45f81a.png)

阿里巴巴的Java操作手册中明确说明：对于 `Integer.MAX_VALUE` 初始值较大，所以一般情况我们要使用底层的 `ThreadPoolExecutor` 来创建线程池。

```java
public class Demo02 {
    public static void main(String[] args) {
        ExecutorService service = new ThreadPoolExecutor(
                2,
                5,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingQueue<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );

        try {
            for (int i = 1; i < 9; i++) {
                // 用线程池来创建线程
                service.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + " ok");
                });
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            service.shutdown();
        }
    }
}
```

### 11.3 四大拒绝策略

|                  策略                  |                             详解                             |
| :------------------------------------: | :----------------------------------------------------------: |
|     ThreadPoolExecutor.AbortPolicy     | 丢弃任务并抛出RejectedExecutionException异常。**是默认的策略** |
|    ThreadPoolExecutor.DiscardPolicy    |          丢弃任务，但是不抛出异常，这是不推荐的做法          |
| ThreadPoolExecutor.DiscardOldestPolicy |      抛弃队列中等待最久的任务 然后把当前任务加入队列中       |
|  ThreadPoolExecutor.CallerRunsPolicy   | 由主线程(main) 负责调用任务的 run() 方法从而绕过线程池直接执行（哪来的回哪里） |

### 11.4 如何设置线程池的大小

**1、CPU 密集型：根据电脑的 CPU 核数来选择 maximunPoolSize 的大小**

```java
	// 获取 cpu 的核数
        int max = Runtime.getRuntime().availableProcessors();
        ExecutorService service =new ThreadPoolExecutor(
                2,
                max,
                3,
                TimeUnit.SECONDS,
                new LinkedBlockingDeque<>(3),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy()
        );
```

**2、I/O 密集型**：在程序中有15个大型任务，I/O 十分占用资源；I/O 密集型就是判断程序中十分耗 I/O 的线程数量，大约是最大 I/O 数的一倍到两倍之间。

## 12. 四大函数式接口

新时代的程序员：**lambda 表达式、链式编程、函数式接口、Stream 流式计算**

- 函数式接口：只有一个方法的接口，例如 `Runnable` 接口：

  ```java
  @FunctionalInterface
  public interface Runnable {
      /**
       * When an object implementing interface <code>Runnable</code> is used
       * to create a thread, starting the thread causes the object's
       * <code>run</code> method to be called in that separately executing
       * thread.
       * <p>
       * The general contract of the method <code>run</code> is that it may
       * take any action whatsoever.
       *
       * @see     java.lang.Thread#run()
       */
      public abstract void run();
  }
  ```

- 在 `java.utils.function` 包中有许多函数式接口，有四个原生的：

  ![image-20220703101655716](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703101655716.png)

### 12.1 Function 函数型接口

![image-20220703102830360](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703102830360.png)

```java
public class FunctionDemo {
    public static void main(String[] args) {
        Function function = new Function<String, String>() {
            @Override
            public String apply(String s) {
                return s;
            }
        };
        System.out.println(function.apply("Hello Function"));
    }
}
```

只要是函数式接口，就可以用 Lambda 表达式简化：

```java
public class FunctionDemo {
    public static void main(String[] args) {
//        Function function = (str) -> {return  str;};
        // 单参数时，() 可省略； 单返回值时，{}和return 可省略
        Function function = str -> str;
        System.out.println(function.apply("Hello Function"));
    }
}
```



### 12.2 Predicate 断定型接口

![image-20220703103648801](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703103648801.png)

```java
public class PredicateDemo {
    public static void main(String[] args) {
//        Predicate predicate = new Predicate<String>() {
//            @Override
//            public boolean test(String str) {
//                return str.isEmpty();
//            }
//        };

//        Predicate<String> predicate = str -> str.isEmpty();

        // 更简洁的方法引用
        Predicate<String> predicate = String::isEmpty;

        System.out.println(predicate.test("predicate"));    // false
    }
}
```

### 12.3 Supplier 供给型接口

![image-20200812144653640](https://img-service.csdnimg.cn/img_convert/ff6aa0d95bf9f22b1f2dd5714f3db542.png)

```java
public class SupplierDemo {
    public static void main(String[] args) {
//        Supplier<String> supplier = new Supplier<String>() {
//            @Override
//            public String get() {
//                return "Hello Supplier";
//            }
//        };

        Supplier<String> supplier = () -> "Hello Supplier";
        System.out.printf(supplier.get());
    }
}
```

### 12.4 Consumer 消费型接口

![image-20200812144803229](https://img-service.csdnimg.cn/img_convert/772c811d116f54e0c739929ab392de25.png)

```java
public class ConsumerDemo {
    public static void main(String[] args) {
//        Consumer<String> consumer = new Consumer<String>() {
//            @Override
//            public void accept(String s) {
//                System.out.println(s);
//            }
//        };

//        Consumer<String> consumer = (str) -> {System.out.println(str);};

        // 更简洁的方法引用
        Consumer<String> consumer = System.out::println;
        consumer.accept("Hello Consumer");
    }
}
```

## 13. Stream 流式计算

Stream 是对集合功能的增强，它提供了各种非常便利、高效的聚合操作，可以大批量数据操作，同时再结合 Lambda 表达式，就可以极大的提高编程效率。

Stream流的概述：

- 在Java 8中，得益于Lambda所带来的函数式编程， 引入了一个全新的Stream流概念。
- 目的：用于简化集合和数组操作的API。

**Stream流支持链式编程**

**Stream流式思想的核心：**

1. 先得到集合或者数组的Stream流（就是一根传送带）
2. 把元素放上去
3. 然后就用这个Stream流简化的API来方便的操作元素。



> Stream 初体验

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
        names.stream()
            .filter(s -> s.startsWith("张"))
            .filter(s -> s.length() == 3)
            .forEach(s -> System.out.println(s));
    }
}
```

### 13.1 Stream流的获取

**Stream流的三类方法**：

- 获取Stream流
  - 创建一条流水线，并把数据放到流水线上准备进行操作
- 中间方法
  - 流水线上的操作。一次操作完毕之后，还可以继续进行其他操作
- 终结方法
  - 一个Stream流只能有一个终结方法，是流水线上的最后一个操作



Stream操作分为中间操作或者最终操作两种：

- 中间操作，返回Stream本身，这样就可以将多个操作依次串起来例如，`map、flatMap、filter、distinct、sorted、peek、limit、skip、parallel、sequential、unordered`
- 最终操作，返回一特定类型的计算结果例如，`forEach、forEachOrdered、toArray、reduce、collect、min、max、count、anyMatch、allMatch、noneMatch、findFirst、findAny、iterator`



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

示例：

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



### 13.2 Stream流常用API（中间操作方法）

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
        list.stream()
            .filter(s -> s.startsWith("张"))
            .forEach(s -> System.out.println(s));

        long size = list.stream()
            .filter(s -> s.length() == 3)
            .count();
        System.out.println(size);

       // list.stream().filter(s -> s.startsWith("张")).limit(2).forEach(s -> System.out.println(s));
        // 简化print，方法引用(::) (->前的对象要和print()中的对象相同)：
        list.stream()
            .filter(s -> s.startsWith("张"))
            .limit(2)
            .forEach(System.out::println);

        list.stream()
            .filter(s -> s.startsWith("张"))
            .skip(2)
            .forEach(System.out::println);

        // map加工方法: ->前：原材料  ->后：加工后的结果。
        // 给集合元素的前面都加上一个：黑马的：
        list.stream()
            .map(s -> "黑马的：" + s)
            .forEach(a -> System.out.println(a));

        // 需求：把所有的名称 都加工成一个学生对象。
        list.stream()
            .map(s -> new Student(s))
            .forEach(s -> System.out.println(s));
        //用引用符号:: 简化(对象要相同)：
//        list.stream().map(Student::new).forEach(System.out::println); // 构造器引用  方法引用

        // 合并流
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



### 13.3 Stream流案例

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
        one.add(new Employee("猪八戒", '男', 30000, 25000, null));
        one.add(new Employee("孙悟空", '男', 25000, 1000, "顶撞上司"));
        one.add(new Employee("沙僧", '男' ,20000, 20000, null));
        one.add(new Employee("小白龙", '男', 20000, 25000, null));

        List<Employee> two = new ArrayList<>();
        two.add(new Employee("武松", '男', 15000, 9000, null));
        two.add(new Employee("李逵", '男', 20000, 10000, null));
        two.add(new Employee("西门庆", '男', 50000, 100000, "被打"));
        two.add(new Employee("潘金莲", '女', 3500, 1000, "被打"));
        two.add(new Employee("武大郎", '女', 20000, 0, "下毒"));

        // 1、开发一部的最高工资员工
        // 用一个TopperFormer类型的map容器接收，max是stream流里获取最大值的方法（需要自己写规则），get是获取对象的方法
        TopperFormer t1=one.stream()
            .max((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(), o2.getSalary()+o2.getBonus()))
            .map(o -> new TopperFormer(o.getName(), o.getSalary()+o.getBonus()))
            .get();
        System.out.println("开发一部的最高工资员工为："+ t1);

        // 2、统计平均工资，要去掉最高最低工资
        // sorter是stream流里排序方法，需要自己制定规则，先用skip去掉最低工资，再用limit获取除了最高工资的元素，最后用forEach遍历，获取总工资
        one.stream()
            .sorted((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
            .skip(1)
            .limit(one.size() - 2)
            .forEach(o ->{
                    //o表示当前获得的对象
                    allMoney1 +=(o.getSalary() + o.getBonus());
                });

        // 浮点型运算用总工资除人数有可能会出现失真问题，或者除不尽的情况，用大数据对象处理
        BigDecimal bdAllMoney1 = BigDecimal.valueOf(allMoney1);
        BigDecimal bdSize1 = BigDecimal.valueOf(one.size() - 2);
        // 结果保留2位小数，除不尽采用四舍五入法
        BigDecimal average1 = bdAllMoney1.divide(bdSize1, 2, RoundingMode.HALF_UP);
        System.out.println("开发一部的平均工资是："+ average1);


        //开发二部获取最高工资员工和统计平均工资的方法跟上面相同
        TopperFormer t2=two.stream()
            .max((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
            .map(o->new TopperFormer(o.getName(),o.getSalary()+o.getBonus()))
            .get();
        System.out.println("开发二部的最高工资员工为："+ t2);

        one.stream()
            .sorted((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(),o2.getSalary()+o2.getBonus()))
            .skip(1)
            .limit(two.size() - 2)
            .forEach(o ->{
                    allMoney2 += (o.getSalary() + o.getBonus());
                });

        BigDecimal bdAllMoney2 = BigDecimal.valueOf(allMoney2);
        BigDecimal bdSize2 = BigDecimal.valueOf(two.size() - 2);
        BigDecimal average2 = bdAllMoney2.divide(bdSize2, 2, RoundingMode.HALF_UP);
        System.out.println("开发二部的平均工资是：" + average2);

        //3、合并两个集合流，再统计
        Stream<Employee> s1 = one.stream();
        Stream<Employee> s2 = two.stream();
        Stream<Employee> s3 = Stream.concat(s1, s2);
        s3.sorted((o1, o2) -> Double.compare(o1.getSalary() + o1.getBonus(), o2.getSalary() + o2.getBonus()))
            .skip(1)
            .limit(one.size() + two.size() - 2)
            .forEach(o ->{
                    allMoneyTotal += (o.getSalary() + o.getBonus());
                });
        BigDecimal bdAllMoneyTotal = BigDecimal.valueOf(allMoneyTotal);
        BigDecimal bdSizeTotal = BigDecimal.valueOf(one.size() + two.size() - 2);
        BigDecimal averageTotal = bdAllMoneyTotal.divide(bdSizeTotal, 2, RoundingMode.HALF_UP);
        System.out.println("两个开发部整体的平均工资是：" + averageTotal);
    }
}
```



### 13.4 收集Stream流

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

**注意注意注意：“流只能使用一次”**

**示例**：

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

        // 收集到List集合
        Stream<String> s1 = list.stream().filter(s -> s.startsWith("张"));
//      List<String> zhangList = s1.collect(Collectors.toList()); // 可变集合

        // JDK16开始，转为List集合可以直接调用toList()，但是不能修改集合
        List<String> zhangList=s1.toList();     // 不可变集合
//      zhangList.add("java1");       // 报错，用新的toList得到的集合不能被修改
        System.out.println(zhangList);

        // 收集到Set集合
        // 注意注意注意：流只能使用一次，所以要创建一个新流再收集
        Stream<String> s2 = list.stream().filter(s -> s.startsWith("张"));
        Set<String> zhangSet = s2.collect(Collectors.toSet());
        System.out.println(zhangSet);       // [张强, 张三丰, 张无忌] （Set集合会去重复）

        
        // 收集到Array数组里
        Stream<String> s3 = list.stream().filter(s -> s.startsWith("张"));
        Object[] arrs = s3.toArray();
        // String[] arrs = s3.toArray(String[]::new);  // 将流收集到字符串数组(或者其他类型的数组)
        System.out.println("Arrays数组内容：" + Arrays.toString(arrs));
    }
}
```

## 14. ForkJoin

Java 7开始引入了一种新的 Fork/Join 线程池，它可以执行一种特殊的任务：把一个大任务拆成多个小任务并行执行，最终汇总每个小任务结果后得到大任务的计算结果。

![image-20220703132244662](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703132244662.png)

它同 `ThreadPoolExecutor` 一样，也实现了 `Executor` 和 `ExecutorService` 接口。它使用了一个**无限队列**来保存需要执行的任务，而线程的数量则是通过构造函数传入，如果没有向构造函数中传入指定的线程数量，那么当前计算机可用的CPU数量会被设置为线程数量作为默认值。



> ForkJoin 特点： 工作窃取

实现原理：**双端队列**！从上面和下面都可以去拿到任务去执行！

![image-20220703132607509](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703132607509.png)

> 使用 ForkJoin

- ForkJoin框架为我们提供了 `RecursiveAction` 和 `RecursiveTask`。

- 只需要继承 ForkJoin 提供的抽象类的其中一个并且实现 `compute方法`。

- `RecursiveTask` 有返回值；而 `RecursiveAction` 无返回值。

`ForkJoinPool` 中的三个执行任务的方法：

- `execute(ForkJoinTask)` 异步执行 tasks，无返回值
- `invoke(ForkJoinTask)` 有Join, tasks 会被同步到主进程
- `submit(ForkJoinTask)` 异步执行，且带 Task 返回值，可通过 `task.get()` 获取

示例：

```java
/**
 * 任务：求和，当拆分差值大于10时，继续拆分。
 * 例如：30 拆分-> 1-15和16-30      差值15-1 > 10
 *       继续拆分-> 1-8和9-15  16-23和24-30   差值8-1 < 10
 *       不拆分了，进行计算
 */
public class ForkJoinDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建拆分任务对象MyTask
        MyTask myTask = new MyTask(0, 100);
        // 创建分支合并池对象
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        // 将任务对象丢进池里执行
        ForkJoinTask<Integer> forkJoinTask = forkJoinPool.submit(myTask);
        // 获取最终合并后的结果
        Integer result = forkJoinTask.get();
        System.out.println(result);
    }
}

class MyTask extends RecursiveTask<Integer> {
    // 拆分差值不能超过10，只计算相差在10以内的，否则就Fork
    private static final Integer VALUE = 10;
    private int begin;  // 拆分开始值
    private int end;    // 拆分结束值
    private int result;

    // 创建有参数的构造方法
    public MyTask(int begin, int end) {
        this.begin = begin;
        this.end = end;
    }

    // Fork/Join 过程
    @Override
    protected Integer compute() {
        // 判断拆分差值情况
        if (end - begin <= VALUE) {
            // 相加操作
            for (int i = begin; i <= end; i++) {
                result += i;
            }
        } else {
            // 继续Fork
            int middle = (begin + end) / 2;     // 得到中间值
            MyTask task01 = new MyTask(begin, middle);      // 拆分左边
            MyTask task02 = new MyTask(middle + 1, end);    // 拆分右边
            // 方法拆分，把任务塞进线程队列
            task01.fork();
            task02.fork();
            result = task01.join() + task02.join();     // 合并结果
        }
        return result;
    }
}
```



> Stream并行流

使用 Stream并行流也可以直接计算出结果，在数据量大的时候效率较高。

```java
public class StreamParallelDemo {
    public static void main(String[] args) {
        int sum = IntStream.rangeClosed(0, 100).parallel().reduce(0, Integer::sum);
        System.out.println(sum);
    }
}
```

## 15. 异步回调

Future 设计的初衷：对将来的某个事件结果进行建模！

其实就是类似前端发送 Ajax 异步请求给后端的过程。

![image-20220703155740306](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220703155740306.png)

平时都使用异步回调**CompletableFuture**。

### 没有返回值的runAsync

```java
public class Demo01 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            // 发起一个异步任务，睡眠2s是为了演示阻塞获取结果
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "线程");
        });

        // 获取异步任务的执行结果
        future.get();       // 阻塞获取执行结果

        System.out.println("main线程");
    }
}
```

### 有返回值的supplyAsync

```java
public class Demo02 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName() + "线程");
//            int i = 10 / 0;     // 制造错误
            // 成功时
            return 200;
        });

        System.out.println(
                future.whenComplete((t, u) -> {
                    System.out.println("t --> " + t);
                    System.out.println("u --> " + u);
                }).exceptionally((e) -> {   // 失败时
                    System.out.println(e.getMessage());
                    return 404;
                }).get()
        );
    }
}
```

当无错误时，输出：

```text
ForkJoinPool.commonPool-worker-9线程
t --> 200
u --> null
200

Process finished with exit code 0
```

当有错误时，输出：

```text
ForkJoinPool.commonPool-worker-9线程
t --> null
u --> java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
java.lang.ArithmeticException: / by zero
404

Process finished with exit code 0
```



`whenComplete` 有两个参数，一个是t 一个是u：

- T：代表 **正常返回的结果**

- U：代表 **抛出异常的错误信息**

## 16. JMM

### 16.1 对 Volatile 的理解

**Volatile** 是 Java 虚拟机提供 **轻量级的同步机制**

1. **保证可见性**
2. **不保证原子性**
3. **禁止指令重排**



**多处理器总线嗅探：**

  为了提高处理速度，处理器不直接和内存进行通信，而是先将系统内存的数据读到内部缓存后再进行操作，但操作不知道何时会写到内存。如果对声明了volatile的变量进行写操作，JVM就会向处理器发送一条lock前缀的指令，将这个变量所在缓存行的数据写回到系统内存。但是在**多处理器下**，为了保证各个处理器的缓存是一致的，就会实现缓存缓存一致性协议，**每个处理器通过嗅探在总线上传播的数据来检查自己的缓存值是不是过期了，如果处理器发现自己缓存行对应的内存地址呗修改，就会将当前处理器的缓存行设置无效状态**，当处理器对这个数据进行修改操作的时候，会重新从系统内存中把数据库读到处理器缓存中。

### 16.2 什么是 JMM

JMM：JAVA内存模型，不存在的东西，是一个概念，也是一个约定！

**关于 JMM 的一些同步的约定**：

1. 线程加锁前，必须**读取主存**中的最新值到**工作内存**中；
2. 线程解锁前，必须把共享变量**立刻刷回主存**；
3. 加锁和解锁是同一把锁；

线程中分为 **工作内存、主内存**。

**8 种操作**：

- **read（读取）**：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的 load 动作使用；

- **load（载入）**：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中；

- **use（使用）**：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令；

- **assign（赋值）**：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中；

- **store（存储）**：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用；

- **write（写入）**：作用于主内存中的变量，它把 store 操作从工作内存中得到的变量的值放入主内存的变量中；

- **lock（锁定）**：作用于主内存的变量，把一个变量标识为线程独占状态；

- **unlock（解锁）**：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定；

  ![image-20220704164310803](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704164310803.png)

  当线程 B 也在操作 Flag 时，把 Flag 改为了 false，这时线程 A 中的 Flag 还是 true。线程 A 不能及时可见。

  ![image-20220704164659060](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704164659060.png)

  
  
  **JMM 对这 8 种操作给了相应的规定**：
  
  - 不允许 read 和 load、store 和 write 操作之一单独出现。即使用了 read 必须 load，使用了 store必须 write；
  - 不允许线程丢弃他最近的 assign 操作，即工作变量的数据改变了之后，必须告知主存；
  - 不允许一个线程将没有 assign 的数据从工作内存同步回主内存；
  - 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施 use、store 操作之前，必须经过 assign 和 load 操作；
  - 一个变量同一时间只有一个线程能对其进行 lock。多次 lock 后，必须执行相同次数的 unlock 才能解锁；
  - 如果对一个变量进行 lock 操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新 load 或 assign 操作初始化变量的值；
  - 如果一个变量没有被 lock，就不能对其进行 unlock 操作。也不能 unlock 一个被其他线程锁住的变量；
  - 对一个变量进行 unlock 操作之前，必须把此变量同步回主内存；
  

## 17. Volatile

### 17.1 保证可见性

示例：

```java
public class VolatileDemo {

    private static Integer number = 0;

    public static void main(String[] args) throws InterruptedException {

        // 子线程对主内存的变化时不知道的
        new Thread(() -> {
            while (number == 0) {
		// 注意，不要在此使用System.out.println();   因为println加了synchronized关键字，所以也能保证对变量修改的可见性。
            }
        }).start();

        // 主线程先睡1s，让子线程先执行
        TimeUnit.SECONDS.sleep(1);

        number = 1;
        System.out.println(number);

    }
}
```

**注意**：注意，不要在 while 循环中使用 `System.out.println()`;   因为 `println` 加了 `synchronized` 关键字，所以也能保证对变量修改的可见性。

子线程先运行，1s 后主线程把 number 改为了 1，按道理子线程应该退出了循环，在实际上还一直在循环：

![image-20220704170540298](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704170540298.png)

把 number 变量加上 valatile 关键字：`private volatile static Integer number = 0;`，即可使变量保证可见。输出结果如下：

![image-20220704170817595](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704170817595.png)

### 17.2 不保证原子性

原子性：不可分割；

线程A在执行任务的时候，不能被打扰的，也不能被分割的，要么同时成功，要么同时失败。

示例：

```java
public class VolatileDemo02 {

    private static int number = 0;

    public static void add() {
        number++;
    }

    public static void main(String[] args) {

        // 理论上number结果为20000
        for (int i = 1; i <= 20; i++) {
            new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount() > 2) {
            // Java中默认有两个线程：main和gc
            // yield让步的意思，当存活的线程>2时，说明上面的子线程还没执行完，此时主线程让子线程先执行完
            // 这样保证子线程都执行完了，才输出下面的number
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + ", number=" + number);
    }
}
```

子线程全部执行完后才输出的 number，可是这时 number 并不为 20000：

```text
main, number=19976

Process finished with exit code 0
```

此时将 number 加上 volatile 关键字：`private volatile static int number = 0;`，发现 number 还是不为 20000，说明 **volatile 不保证原子性**：

```text
main, number=19820

Process finished with exit code 0
```



> 解决办法

在 `add()` 方法上加 `synchronized` 关键字或者在执行 `add()` 之前加锁，执行完后解锁，都可以解决这个问题。

**如果不加 lock 和 synchronized，怎么样保证原子性**？

先来通过反编译 Java 字节码 分析一下 `add()` 方法中 `number++` 的执行过程，可以发现确实只是简单的 +1，没有保证原子性：

![image-20220704174300804](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704174300804.png)

为了保证其具有原子性，需要使用 **原子类** 来解决：

![image-20220704174534023](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220704174534023.png)

```java
public class VolatileDemo02 {

    // 使用原子类的Integer
    private volatile static AtomicInteger number = new AtomicInteger();

    public static void add() {
//        number++;   // 不是原子操作
        number.getAndIncrement();       // 底层是CAS保证的原子性
    }

    public static void main(String[] args) {
        // 理论上number结果为20000
        for (int i = 1; i <= 20; i++) {
            new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount() > 2) {
            // Java中默认有两个线程：main和gc
            // yield让步的意思，当存活的线程>2时，说明上面的子线程还没执行完，此时主线程让子线程先执行完
            // 这样保证子线程都执行完了，才输出下面的number
            Thread.yield();
        }
        System.out.println(Thread.currentThread().getName() + ", number=" + number);
    }
}
```

这些类的底层都直接和操作系统挂钩，是在内存中修改值。

```java
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
```

`Unsafe` 类是一个很特殊的存在。在后面的 CAS 中再聊！

### 17.3 禁止指令重排

**什么是指令重排**？

我们写的程序，计算机并不是按照我们自己写的那样去执行的

源代码 –> 编译器优化重排 –> 指令并行也可能会重排 –> 内存系统也会重排 –> 执行



```java
int x = 1;	      //1
int y = 2;	      //2
x = x + 5;   	//3
y = x * x;          //4

// 我们期望的执行顺序是 1_2_3_4  计算机可能执行的顺序会变成2134 / 1324
// 可不可能是 4123？ 不可能
```

**处理器在进行指令重排的时候，会考虑数据之间的依赖性**



示例：

前提为 a b x y 这四个值的默认值都是 0。

| 线程A | 线程B |
| ----- | ----- |
| x = a | y = b |
| b = 1 | a = 2 |

正常的结果： x = 0; y =0;



在线程A中因为 x 和 b 无关，没有依赖性，所以可能会出现先执行 b=1，然后再执行 x=a；

在线程B中同理，可能会出现，先执行 a=2，然后执行 y=b；

| 线程A | 线程B |
| ----- | ----- |
| b=1   | a=2   |
| x=a   | y=b   |

那么就有可能结果如下：x=2; y=1；  这就是指令重排带来的影响。

**volatile 可以避免指令重排**：

- **volatile 中会加一道内存的屏障，这个内存屏障可以保证在这个屏障中的指令顺序**。



内存屏障 — CPU指令，作用：

1. 保证特定操作的执行顺序；
2. 可以保证某些变量的内存可见性（利用这些特性，就可以保证 volatile 实现的可见性）

![image-20200812220019582](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/85fa53d83ee4f89d5a7202e9e5a98987.png)

### 17.4 总结

- **volatile可以保证可见性**
- **不能保证原子性**
- **由于内存屏障，可以避免指令重排的现象产生**

在哪里用这个内存屏障用得最多呢？**单例模式**

## 18. 单例模式

### 18.1 什么是单例模式

单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

**注意**：

- 单例类只能有一个实例。
- 单例类必须自己创建自己的唯一实例。
- 单例类必须给所有其他对象提供这一实例。

**意图**：保证一个类仅有一个实例，并提供一个访问它的全局访问点。

**主要解决**：一个全局使用的类频繁地创建与销毁。

**何时使用**：当您想控制实例数目，节省系统资源的时候。

**如何解决**：判断系统是否已经有这个单例，如果有则返回，如果没有则创建。

**关键代码**：构造函数是私有的。

**优点**：

- 1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。
- 2、避免对资源的多重占用（比如写文件操作）。

**缺点**：没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

**注意事项：**`getInstance()` 方法中需要使用同步锁 `synchronized` (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。



简单的单例模式示例：

```java
public class Single {

    // 让构造函数为 private，这样该类就不会被实例化
    private Single() {}
    
    // 创建Single的一个对象
    private final static Single instance = new Single();

    // 获取唯一可用对象实例
    public static Single getInstance() {
        return instance;
    }

    public void showMessage() {
        System.out.println("Hello Singleton-Pattern");
    }

}
```

其他类获取实例：

```java
public class SingleDemo {
    public static void main(String[] args) {
        // 不合法的构造函数
        // 编译时错误：构造函数 Single() 是不可见的
        // Single single = new Single();

        // 获取唯一可用对象实例
        Single single = Single.getInstance();

        single.showMessage();       // Hello Singleton-Pattern
    }
}
```

### 18.2 饿汉式

**是否 Lazy 初始化**：否

**是否多线程安全**：是

**描述：**这种方式比较常用，但容易产生垃圾对象。

**优点**：没有加锁，执行效率会提高。

**缺点**：类加载时就初始化，浪费内存。

它基于 classloader 机制避免了多线程的同步问题，不过，instance  在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用 getInstance 方法，  但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化 instance 显然没有达到 lazy loading 的效果。

示例：就和简单的单例模式一样。

```java
public class Singleton {  
    private static Singleton instance = new Singleton();  
    private Singleton (){}  
    public static Singleton getInstance() {  
    	return instance;  
    }  
}
```

### 18.3 懒汉式—线程不安全

**是否 Lazy 初始化**：是

**是否多线程安全**：否

**描述**：这种方式是最基本的实现方式，这种实现最大的问题就是不支持多线程。因为没有加锁 synchronized，所以严格意义上它并不算单例模式。这种方式 lazy loading 很明显，不要求线程安全，在多线程不能正常工作。

示例：

```java
public class LazySingle {
    private LazySingle() {
        System.out.println(Thread.currentThread().getName() + " ok");
    }

    // 先不创建对象，需要使用的时候再创建
    private static LazySingle instance;

    public static LazySingle getInstance() {
        if (instance == null) {     // 实例为空的时候才创建对象，否则直接返回
            instance = new LazySingle();
        }
        return instance;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            new Thread(LazySingle::getInstance).start();
        }
    }
}
```

运行输出，可以发现输出了多个线程信息，说明无参构造器被调用了多次，对象被创建了多次，所以是线程不安全的。

```text
Thread-0 ok
Thread-5 ok
Thread-1 ok
Thread-2 ok

Process finished with exit code 0
```

### 18.4 懒汉式—线程安全

**是否 Lazy 初始化**：是

**是否多线程安全**：是

**描述**：这种方式具备很好的 lazy loading，能够在多线程中很好的工作，但是，效率很低，99% 情况下不需要同步。

**优点**：第一次调用才初始化，避免内存浪费。

**缺点**：必须加锁 synchronized 才能保证单例，但加锁会影响效率。

`getInstance()` 的性能对应用程序不是很关键（该方法使用不太频繁）。

示例，在 `getInstance()` 方法上加上 synchronized 即可：

```java
public class LazySingle {
    private LazySingle() {
        System.out.println(Thread.currentThread().getName() + " ok");
    }

    // 先不创建对象，需要使用的时候再创建
    private static LazySingle instance;

    public static synchronized LazySingle getInstance() {
        if (instance == null) {     // 实例为空的时候才创建对象，否则直接返回
            instance = new LazySingle();
        }
        return instance;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            new Thread(LazySingle::getInstance).start();
        }
    }
}
```

无论运行多少次，得到的结果都只有一个线程创建了对象：

```text
Thread-0 ok

Process finished with exit code 0
```

### 18.5 懒汉式—DCL

DCL，即 double-checked locking，双检锁/双重校验锁。

**JDK 版本**：JDK1.5 起

**是否 Lazy 初始化**：是

**是否多线程安全**：是

**描述**：这种方式采用双锁机制，安全且在多线程情况下能保持高性能。
`getInstance()` 的性能对应用程序很关键。

示例，在线程安全的懒汉式中，不使用 synchronized 锁方法，而是自己判断加锁：

```java
public class LazySingle {
    private LazySingle() {
        System.out.println(Thread.currentThread().getName() + " ok");
    }

    // 先不创建对象，需要使用的时候再创建
    private static LazySingle instance;

    public static LazySingle getInstance() {
        if (instance == null) {     // 实例为空
            synchronized (LazySingle.class) {   // 先上锁，锁整个class类
                if (instance == null) {     // 再次判断实例为空
                    instance = new LazySingle();        // 才创建对象
                }
            }
        }
        return instance;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            new Thread(LazySingle::getInstance).start();
        }
    }
}
```

输出：

```text
Thread-0 ok

Process finished with exit code 0
```



> 指令重排带来的问题

**注意**：使用了 DCL 后，虽然是线程安全的，但是在执行 `instance = new LazySingle();` 时，CPU 实际上执行了如下指令：

1. 分配内存空间
2. 执行构造方法，初始化对象
3. 把这个对象指向分配的空间

因为 `instance = new LazySingle();` 这行代码不是一个原子性操作，所以 CPU 在执行时可能会出现**指令重排**，假如线程1 执行顺序是1-3-2，它此时执行完3，还没执行完2；此时线程2 进来，发现对象已经分配空间了，就会误以为已经初始化完成了，那么 `instance != null` ，就会直接走 `return instance` ，但此时线程2 拿到的是没有实例化的对象。

解决问题的办法很简单，只要保证 `instance` 不会被指令重排即可，那么就可以使用 `volatile` 关键字，它就能避免指令重排：`private static volatile LazySingle instance;`



### 18.6 静态内部类

**是否 Lazy 初始化**：是

**是否多线程安全**：是

**描述**：这种方式能达到双检锁方式一样的功效，但实现更简单。

对**静态域使用延迟初始化**，应使用这种方式而不是双检锁方式。这种方式只适用于静态域的情况，双检锁方式可在**实例域需要延迟初始化**时使用。

这种方式同样利用了 classloader 机制来保证初始化 instance 时只有一个线程。

它跟饿汉式不同的是：

- 饿汉式只要  Singleton 类被装载了，那么 instance 就会被实例化（没有达到 lazy loading 效果），
- 而这种方式是  Singleton 类被装载了，instance 不一定被初始化。因为 SingletonHolder 类没有被主动使用，只有通过显式调用  `getInstance()` 方法时，才会显式装载 SingletonHolder 类，从而实例化 instance。

想象一下，如果实例化  instance 很消耗资源，所以想让它延迟加载，另外一方面，又不希望在 Singleton 类加载时就实例化，因为不能确保 Singleton 类还可能在其他的地方被主动使用从而被加载，那么这个时候实例化 instance 显然是不合适的。这个时候，这种方式相比饿汉式就显得很合理。

示例：

```java
public class Singleton {

    private Singleton () {}

    // 在Singleton类中再创建一个静态类SingletonHolder
    private static class SingletonHolder {
        // 再静态类中创建对象
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```



> 静态内部类是如何保证多线程安全的

看一下 `getInstance()` 方法，调用的是 `SingletonHoler.INSTANCE`，取的是 SingletonHoler 里的 INSTANCE 对象，跟 DCL 方法不同的是，`getInstance()` 方法并没有多次去 new 对象，故不管多少个线程去调用 `getInstance()` 方法，取的都是同一个 INSTANCE 对象，而不用去重新创建。当 `getInstance()` 方法被调用时，SingletonHoler 才在 Singleton 的运行时常量池里，把符号引用替换为直接引用，这时静态对象 INSTANCE 也真正被创建，然后再被 `getInstance()` 方法返回出去，这点同饿汉模式。那么 INSTANCE 在创建过程中又是如何保证线程安全的呢？

在《深入理解JAVA虚拟机》中，有这么一句话：虚拟机会保证一个类的 `<clinit>()` 方法在多线程环境中被正确地加锁、同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的 `<clinit>()` 方法，其他线程都需要阻塞等待，直到活动线程执行 `<clinit>()` 方法完毕。如果在一个类的 `<clinit>()` 方法中有耗时很长的操作，就可能造成多个进程阻塞(需要注意的是，其他线程虽然会被阻塞，但如果执行 `<clinit>()` 方法后，其他线程唤醒之后不会再次进入 `<clinit>()` 方法。同一个加载器下，一个类型只会初始化一次)，在实际应用中，这种阻塞往往是很隐蔽的。

故而，可以看出 INSTANCE 在创建过程中是线程安全的，所以说静态内部类形式的单例可保证线程安全，也能保证单例的唯一性，同时也延迟了单例的实例化。



`<clinit>()` 方法：在编译生成 class 文件时，会自动产生两个方法，一个是类的初始化方法 `<clinit>`,  另一个是实例的初始化方法 `<init>` 。

Java 类加载的初始化过程中，编译器**按语句在源文件中出现的顺序**，依次自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并产生 `<clinit>()` 方法。 如果类中没有静态语句和静态代码块，那可以不生成 `<clinit>()` 方法。

详细理解需要看 JVM。 



### 18.7 反射破坏安全性

上面的几种线程安全的单例模式，其实都不是完全安全的，因为 Java 中有一个强大的**反射机制**，它可以破坏任何安全性。

示例，就拿 DCL 方式来举例：

```java
public class LazySingle {
    private LazySingle() {

    }

    // 先不创建对象，需要使用的时候再创建
    private static volatile LazySingle instance;    // volatile避免指令重排

    public static LazySingle getInstance() {
        if (instance == null) {     // 实例为空
            synchronized (LazySingle.class) {   // 先上锁，锁整个class类
                if (instance == null) {     // 再次判断实例为空
                    instance = new LazySingle();        // 才创建对象
                }
            }
        }
        return instance;
    }

    public static void main(String[] args) throws Exception {
        LazySingle instance1 = LazySingle.getInstance();
        // 获取LazySingle的无参构造器
        Constructor<LazySingle> declaredConstructor = LazySingle.class.getDeclaredConstructor(null);
        declaredConstructor.setAccessible(true);    // 无视私有构造器
        LazySingle instance2 = declaredConstructor.newInstance();   // 通过反射创建的对象
        System.out.println(instance1.equals(instance2));	// false
    }
}
```

可以发现，通过反射获取的对象和通过 DLC 获取的对象不是同一个，说明反射破坏了单例模式的安全性。



> 解决方法

在构造器中给 LazySingle类 加锁（不是多线程可以不加），再判断 `instance` 是否为空：

```java
private LazySingle() {
        synchronized (LazySingle.class) {
            if (instance != null) {
                throw new RuntimeException("异常：不要试图使用反射破坏单例模式的安全性");
            }
        }
    }
```

再次运行，可以发现，当使用反射第二次创建对象时，就会抛出异常：

![image-20220705103913425](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705103913425.png)

但是，如果把反射方式放在 `getInstance()` 前面或者两次都使用反射来创建对象，还是会破坏单例模式的安全性。

因为反射是直接调用的构造方法，这两个通过反射创建的对象不是通过调用 `getInstance()` 方法 `new` 的，所以 instance 的指向一直为 null。每次都会创建。



> 解决方法

不使用内部的 instance 作为判断依据，设计一个外部加密标志位：

```java
    private static boolean AruNi_Lu = false;	// 不一定是AruNi_Lu，可以是加密的一串字符

    private LazySingle() {
        synchronized (LazySingle.class) {
            if (AruNi_Lu == false) {
                AruNi_Lu = true;
            } else {
                throw new RuntimeException("异常：不要试图使用反射破坏单例模式的安全性");
            }
        }
    }
```

此时就算两个对象都是由反射来创建的，也会抛出异常。



但是就算这样加密，但是通过反射还是有可能找到我们加密的这串字符，再第一个对象创建完后，把者串字符再重新置为 false，这样下一次通过反射创建对象就不会走异常了。

```java
        public static void main(String[] args) throws Exception {
//        LazySingle instance1 = LazySingle.getInstance();

        Field aruNi_lu = LazySingle.class.getDeclaredField("AruNi_Lu");
        aruNi_lu.setAccessible(true);

        // 获取LazySingle的无参构造器
        Constructor<LazySingle> declaredConstructor = LazySingle.class.getDeclaredConstructor(null);
        declaredConstructor.setAccessible(true);    // 无视私有构造器
        LazySingle instance1 = declaredConstructor.newInstance();
        aruNi_lu.set(instance1, false);
        LazySingle instance2 = declaredConstructor.newInstance();   // 通过反射创建的对象
        System.out.println(instance1.equals(instance2));
    }
```

所以不管什么解决方案，反射总是有方法破解！！！

那么怎么才能解决这个暴力的反射呢？

查看反射的 `newInstance()` 方法，在源码中可以发现一个异常，说的是 “不能用反射创建枚举对象”：

![image-20220705110707642](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705110707642.png)

所以枚举类型的单例模式，可以解决反射带来的问题。

### 18.8 枚举

**JDK 版本**：JDK1.5 起

**是否 Lazy 初始化**：否

**是否多线程安全**：是

**描述**：这种实现方式还没有被广泛采用，但这是实现单例模式的最佳方法。它更简洁，自动支持序列化机制，绝对防止多次实例化。

这种方式是 Effective Java 作者 Josh Bloch  提倡的方式，它不仅能避免多线程同步问题，而且还自动支持序列化机制，防止反序列化重新创建新的对象，绝对防止多次实例化。不过，由于 JDK1.5  之后才加入 enum 特性，用这种方式写不免让人感觉生疏，在实际工作中，也很少用。

这种方式不能通过 reflection attack 反射来调用私有构造方法。

示例：

```java
// enum本身就是一个Class 类
public enum EnumSingle {

    INSTANCE;

    public EnumSingle getInstance() {
        return INSTANCE;
    }

}

class Test {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(null);
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();
        System.out.println(instance1.equals(instance2));
    }
}
```

运行，发现如下错误：

![image-20220705112528441](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705112528441.png)

很明显这不是我们想要的异常，它显示没有这个方法，说明枚举类 EnumSingle 的构造方法不是无参数的。

我们先看一下生成的 .class 文件，发现确实是无参构造器啊，但结合异常来看，这并不是真实的：

![image-20220705112755170](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705112755170.png)

使用 `javap -p` 指令，查看反编译后的代码，发现还是无参构造：

![image-20200812220204965](https://img-service.csdnimg.cn/img_convert/8d1a2e29d5ae9bb9421ee94ed7468f89.png)

使用 jad 工具将 .class 类反编译回 .java 文件，可以发现是一个有参构造器：

```java
public final class EnumSingle extends Enum		// 就是一个EnumSingle类，继承了Enum类
{

    public static EnumSingle[] values()
    {
        return (EnumSingle[])$VALUES.clone();
    }

    public static EnumSingle valueOf(String name)
    {
        return (EnumSingle)Enum.valueOf(com/ogj/single/EnumSingle, name);
    }

    private EnumSingle(String s, int i)		// 有参构造器
    {
        super(s, i);
    }

    public EnumSingle getInstance()
    {
        return INSTANCE;
    }

    public static final EnumSingle INSTANCE;
    private static final EnumSingle $VALUES[];

    static 
    {
        INSTANCE = new EnumSingle("INSTANCE", 0);
        $VALUES = (new EnumSingle[] {
            INSTANCE
        });
    }
}
```

此时将反射获取构造器的代码修改：

```java
class Test {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        EnumSingle instance1 = EnumSingle.INSTANCE;
        // 枚举类不是无参构造器，而是有参构造器
        Constructor<EnumSingle> declaredConstructor = EnumSingle.class.getDeclaredConstructor(String.class, int.class);
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();
        System.out.println(instance1.equals(instance2));
    }
}
```

输出，这才是我们想要的：

![image-20220705113612873](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705113612873.png)



**经验之谈**：一般情况下，不建议使用线程不安全和线程安全的懒汉方式，建议使用饿汉方式。只有在要明确实现 lazy loading 效果时，才会使用静态内部类方式。如果涉及到反序列化创建对象时，可以尝试使用第枚举方式。如果有其他特殊的需求，可以考虑使用 DCL 双检锁方式。

## 19. CAS

### 19.1 什么是 CAS

在 JDK 5 之前Java语言是靠 synchronized 关键字保证同步的，这会导致有锁。

锁机制存在以下问题：

1. 在多线程竞争下，加锁、释放锁会导致比较多的上下文切换和调度延时，引起性能问题；
2. 一个线程持有锁会导致其它所有需要此锁的线程挂起；
3. 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能风险。

volatile 是不错的机制，但是 volatile 不能保证原子性。因此对于同步最终还是要回到锁机制上来。





CAS 全称是 compare and swap（比较并交换），是一种无锁算法。在不使用锁（没有线程被阻塞）的情况下实现多线程之间的变量同步。CAS 操作包含三个操作数 —  内存位置、预期数值和新值。

CAS  的实现逻辑是将内存位置处的数值与预期数值想比较，若相等，则将内存位置处的值替换为新值。若不相等，则不做任何操作。

在 Java 中，Java 并没有直接实现 CAS，CAS 相关的实现是通过 C++ 内联汇编的形式实现的。Java 代码需通过 JNI 才能调用。

CAS 是一条 CPU 的原子指令（cmpxchg指令），不会造成所谓的数据不一致问题，Unsafe 提供的 CAS 方法（如compareAndSwapXXX）底层实现即为 CPU 指令 cmpxchg

示例：

```java
public class CASDemo {
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2022);

        // public final boolean compareAndSet(int expect, int update)
        // 如果实际值和expect期望值相同，则更新为update；否则不更新
        System.out.println(atomicInteger.compareAndSet(2022, 2024));    // true
        System.out.println(atomicInteger.get());    // 2024

        atomicInteger.getAndIncrement();    // i++操作
        System.out.println(atomicInteger.compareAndSet(2025, 2030));    // true
        System.out.println(atomicInteger.get());    // 2030
    }
```

分析一波 `getAndIncrement()` 源码：

```java
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
```

点击 `unsafe` 对象，在 AtomicInteger 数据定义的部分，我们还获取了 unsafe 实例，并且定义了 valueOffset。再看到 static 块，懂类加载过程的都知道，static 块的加载发生于类加载的时候，是最先初始化的，这时候我们调用 unsafe 的 objectFieldOffset 从 Atomic 类文件中获取 value 的偏移量，那么 valueOffset 其实就是记录 value 值在内存中的偏移地址。

有了value在工作内存中的偏移地址之后，我们就可以用unsafe直接操作这个地址了，通过这个地址我们可以获取原值，也可以写入新值。

因为是修改工作内存中的 value 值，所以 value 是用 volatile 修饰的，保证了多线程之间的可见性和避免指令重排。

![image-20220705144418886](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705144418886.png)

点击 `getAndAddInt` ，通过分析发现，如果 object(AtomicInteger) 内的 value 和 expect 相等，就证明没有其他线程改变过这个变量，那么就更新它为 update，如果这一步的 CAS 没有成功，那就采用自旋的方式继续进行CAS操作。这个操作是一个 do-while 循环，通过 CAS 一直进行比较，直到成功才退出循环，这就是一个自旋锁。

![image-20220705151848415](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705151848415.png)

> 总结

CAS：比较当前工作内存中的值 和 主内存中的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环，使用的是自旋锁。

**缺点**：

- 循环会耗时；
- 一次性只能保证一个共享变量的原子性；
- 它会存在 ABA 问题



### 19.2 ABA 问题

> 什么是 ABA 问题

![image-20200812220441615](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/4b9db8d951df5271f214561766442910.png)

线程1：期望值是1，要变成2；

线程2：先把1变成了3，又把3变回了1；

所以对于线程1来说，A的值还是1，此1已经不是原来的1了，所以就出现了问题：

- 如果是基本类型不会产生影响，但是如果是引用类型，Java是值传递，就会有影响了，传递的值可能没变但是引用的对象可能变了，所以骗过了线程1；

示例：

```java
public class CASDemo02 {
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2022);

        // 线程1 先将2022改为2024，再将2024改回2022
        new Thread(() -> {
            System.out.println("线程1将2022改为2024：" + atomicInteger.compareAndSet(2022, 2024));
            System.out.println("线程1将2024改为2022：" + atomicInteger.compareAndSet(2024, 2022));
        }).start();

        // 线程2 将2022改为6666
        new Thread(() -> {
            // 睡1s，等线程1完成了ABA操作
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("线程2将2022改为6666：" + atomicInteger.compareAndSet(2022, 6666));
            System.out.println(atomicInteger.get());        // 6666
        }).start();
    }
}
```

输出：

```text
线程1将2022改为2024：true
线程1将2024改为2022：true
线程2将2022改为6666：true
6666

Process finished with exit code 0
```

可以发现，线程2还是将2022改为了6666，但是我们不希望这样，我们希望谁动过了这个值一定要告诉我们。

解决 ABA 问题需要使用原子引用。

## 20. 原子引用

原子引用其实和原子包装类是差不多的概念，就是将一个java类，用原子引用类进行包装起来，那么这个类就具备了原子性

新增一种机制，也就是修改版本号，类似于时间戳的概念

如果T1修改的时候，版本号为2，落后于现在的版本号3，所以要重新获取最新值，这里就提出了一个使用时间戳版本号，来解决 ABA 问题的思路。



### 20.1 AtomicStampedReference

时间戳原子引用，来这里应用于版本号的更新，也就是每次更新的时候，需要比较期望值和当前值，以及期望版本号和当前版本号。

在演示示例之前，先说一个坑：

1. 看 `compareAndSet` 的源码，里面是使用 `==` 进行比较的

   ![image-20220705165140150](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705165140150.png)

2. 由于 `new AtomicStampedReference<>` 的时候声明泛型 ，肯定是使用的装箱类，这个时候传入值类型将会自动装箱

3. 自动装箱的后果就是地址不一致，使用 `==` 判断的结果就为 `false`，则更新值都失败了

4. 总结：最好不使用原子类型，使用原子类型得保证比较时候传入的为同一个装箱类

如何才能保证传入的值是同一个装箱类？

- 拿 `Integer` 来说，在 Integer 中，使用了对象缓存机制，默认范围是 -128~127，如果超出了这个范围，就会创建新的对象，这个对象和之前的对象地址就不一样。所以需要控制数值范围。

- 在区间内的 `Integer` 类型的值可以直接使用 `==` 进行判断，但是在区间外的值需要使用 `equals()` 方法进行判断。



示例：

注意，给 `atomicStampedReference` 赋的值要在 `Integer` 区间内，否则 `compareAndSet` 会一直更新值失败，掉进上面的坑，因为 `compareAndSet` 源码是使用 `==` 号进行比较的。

```java
public class AtomicReferenceDemo {
    public static void main(String[] args) {
        // 时间戳原子引用：传递两个值，一个是初始值，一个是初始版本号
        AtomicStampedReference<Integer> atomicStampedReference = new AtomicStampedReference<>(22, 1);

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();   // 获取版本号
            System.out.println("线程1 第一次版本号为：" + stamp);

            // 睡眠1s，保证两线程获取到的第一版本号一样
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            // 传入4个参数：期望值、更新值、期望版本号、更新版本号
            atomicStampedReference.compareAndSet(22, 24,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1);
            System.out.println("线程1 第二次版本号为：" + atomicStampedReference.getStamp());

            atomicStampedReference.compareAndSet(24, 22,
                    atomicStampedReference.getStamp(), atomicStampedReference.getStamp() + 1);
            System.out.println("线程1 第三次版本号为：" + atomicStampedReference.getStamp());

        }).start();

        new Thread(() -> {
            int stamp = atomicStampedReference.getStamp();
            System.out.println("线程2 第一次版本号为：" + stamp);

            // 睡眠3s，线程2除了保证两线程获取到的第一版本号一样外，还要保证线程1已经完成了ABA操作
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            // 线程2中修改
            boolean res = atomicStampedReference.compareAndSet(22, 66,
                    stamp, stamp + 1);

            System.out.println("线程2 修改是否成功：" + res);
            System.out.println("线程2 中的版本号为：" + stamp + "；当前最新的实际版本号为：" + atomicStampedReference.getStamp());
            System.out.println("当前实际最新值为：" + atomicStampedReference.getReference());
        }).start();
    }
}
```

输出，可以发现，线程2 并没有成功修改值：

```text
线程1 第一次版本号为：1
线程2 第一次版本号为：1
线程1 第二次版本号为：2
线程1 第三次版本号为：3
线程2 修改是否成功：false
线程2 中的版本号为：1；当前最新的实际版本号为：3
当前实际最新值为：22

Process finished with exit code 0
```

## 21. 各种锁的理解

### 21.1 乐观锁、悲观锁

乐观锁与悲观锁是一种广义上的概念，体现了看待线程同步的不同角度。在Java和数据库中都有此概念对应的实际应用。

> 概念

对于同一个数据的并发操作，悲观锁认为自己在使用数据的时候一定有别的线程来修改数据，因此在获取数据的时候会先加锁，确保数据不会被别的线程修改。Java中，`synchronized` 关键字和 `Lock` 的实现类都是悲观锁。

而乐观锁认为自己在使用数据时不会有别的线程修改数据，所以不会添加锁，只是在更新数据的时候去判断之前有没有别的线程更新了这个数据。如果这个数据没有被更新，当前线程将自己修改的数据成功写入。如果数据已经被其他线程更新，则根据不同的实现方式执行不同的操作（例如报错或者自动重试）。

乐观锁在 Java 中是通过使用无锁编程来实现，最常采用的是 CAS 算法，Java 原子类中的递增操作就通过 CAS 自旋实现的。

根据从上面的概念描述可以发现：

- 悲观锁适合写操作多的场景，先加锁可以保证写操作时数据正确；
- 乐观锁适合读操作多的场景，不加锁的特点能够使其读操作的性能大幅提升。

### 21.2 公平锁、非公平锁

公平锁：

- 非常公平，不能插队，必须排队执行

Lock 默认都是非公平锁：

```java
/**
 * Creates an instance of {@code ReentrantLock}.
 * This is equivalent to using {@code ReentrantLock(false)}.
 */
public ReentrantLock() {
    sync = new NonfairSync();
}
```

非公平锁：

- 非常不公平，允许插队，可以改变顺序

可以在 new Lock 的时候给一个参数，使之变成公平锁：

```java
/**
 * Creates an instance of {@code ReentrantLock} with the
 * given fairness policy.
 *
 * @param fair {@code true} if this lock should use a fair ordering policy
 */
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

### 21.3 可重入锁

可重入锁又名递归锁，是指在**同一个线程在外层方法获取锁的时候，再进入该线程的内层方法会自动获取锁**（前提锁对象得是同一个对象或者class），不会因为之前已经获取过还没释放而阻塞。

Java 中 `ReentrantLock` 和 `synchronized` 都是可重入锁，可重入锁的一个优点是可以在一定程度上避免死锁。



> Synchonized 锁

```java
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        // 锁同一个 Phone 对象
        new Thread(phone::sendMsg,"线程A").start();
        new Thread(phone::sendMsg,"线程B").start();
    }
}

class Phone {
    public synchronized void sendMsg() {
        System.out.println(Thread.currentThread().getName()+"=> sendMsg");
        call();		// 这里也有一把锁
    }
    public synchronized void call() {
        System.out.println(Thread.currentThread().getName()+"=> call");
    }
}
```

通过输出结果可以发现，线程A执行 `sendMsg` 时，`call` 方法也获得了锁，执行完线程A的 `call` 后，才轮到线程B执行：

```text
线程A=> sendMsg
线程A=> call
线程B=> sendMsg
线程B=> call

Process finished with exit code 0
```



> Lock 锁

```java
public class Demo02 {
    public static void main(String[] args) {
        Phone2 phone2 = new Phone2();
        // 锁同一个 Phone 对象
        new Thread(phone2::sendMsg,"线程A").start();
        new Thread(phone2::sendMsg,"线程B").start();
    }
}

class Phone2 {
    Lock lock = new ReentrantLock();

    public void sendMsg() {
        lock.lock();    // 这把锁和 22 行的解锁对应
        // lock锁必须配对，否则就会死锁在里面
        try {
            System.out.println(Thread.currentThread().getName()+"=> sendMsg");
            call();		// 这里也有一把锁，拿到的是下面call中的锁
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    public void call() {
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName()+"=> call");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
// 输出同上
```



**注意**：

- lock 锁必须配对，相当于 lock 和 unlock 必须数量相同；
- 在外面加的锁，也可以在里面解锁；在里面加的锁，在外面也可以解锁；

### 21.4 自旋锁、自适应自旋锁

前置知识：阻塞或唤醒一个 Java 线程需要操作系统切换 CPU 状态来完成，这种状态转换需要耗费处理器时间。如果同步代码块中的内容过于简单，状态转换消耗的时间有可能比用户代码执行的时间还要长。

在许多场景中，同步资源的锁定时间很短，为了这一小段时间去切换线程，线程挂起和恢复现场的花费可能会让系统得不偿失。如果物理机器有多个处理器，能够让两个或以上的线程同时并行执行，我们就可以让后面那个请求锁的线程不放弃 CPU 的执行时间，看看持有锁的线程是否很快就会释放锁。

而为了让当前线程 “稍等一下”，我们需让当前线程进行自旋，如果在自旋完成后，前面锁定同步资源的线程已经释放了锁，那么当前线程就可以不必阻塞而是直接获取同步资源，从而避免切换线程的开销。这就是自旋锁（Spin Lock）。

![image-20220705173338234](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705173338234.png)

自旋锁本身是有缺点的，它不能代替阻塞。自旋等待虽然避免了线程切换的开销，但它要占用处理器时间。如果锁被占用的时间很短，自旋等待的效果就会非常好。反之，如果锁被占用的时间很长，那么自旋的线程只会白浪费处理器资源。所以，自旋等待的时间必须要有一定的限度，如果自旋超过了限定次数（默认是 10 次，可以使用 -XX:PreBlockSpin 来更改）没有成功获得锁，就应当挂起线程。

自旋锁的实现原理同样也是 CAS，AtomicInteger 中调用 unsafe 进行自增操作的源码中的 do-while 循环就是一个自旋操作，如果修改数值失败则通过循环来执行自旋，直至修改成功。

![image-20220705173523969](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705173523969.png)



> 适应性自旋锁

自旋锁在 JDK1.4.2 中引入，使用 -XX:+UseSpinning 来开启。JDK 6 中变为默认开启，并且引入了自适应的自旋锁（适应性自旋锁）。

自适应意味着自旋的时间（次数）不再固定，而是由前一次在同一个锁上的自旋时间及锁的拥有者的状态来决定。如果在同一个锁对象上，自旋等待刚刚成功获得过锁，并且持有锁的线程正在运行中，那么虚拟机就会认为这次自旋也是很有可能再次成功，进而它将允许自旋等待持续相对更长的时间。如果对于某个锁，自旋很少成功获得过，那在以后尝试获取这个锁时将可能省略掉自旋过程，直接阻塞线程，避免浪费处理器资源。



> 自设计自旋锁

自旋锁类：

```java
public class SpinlockDemo {
    // 默认  thread --> null
    AtomicReference<Thread> atomicReference=new AtomicReference<>();

    // 加锁
    public void myLock(){
        Thread thread = Thread.currentThread();

        // 自旋锁：thread 为 null 时解锁，否则自旋
        while (!atomicReference.compareAndSet(null, thread)) {

        }

        System.out.println(thread.getName() + " ===> 加锁");
    }


    // 解锁
    public void myUnlock(){
        Thread thread=Thread.currentThread();
        // 将thread设为null，解锁
        atomicReference.compareAndSet(thread, null);
        System.out.println(thread.getName()+"===> 解锁");
    }
}
```

测试类：

```java
public class TestSpinLock {
    public static void main(String[] args) throws InterruptedException {
        // 官方的锁
        Lock lock = new ReentrantLock();

        // 自己使用CAS实现自旋锁
        SpinlockDemo spinlockDemo = new SpinlockDemo();
        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnlock();
            }
        },"t1").start();

        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnlock();
            }
        },"t2").start();
    }
}
```

通过输出结果可以发现，线程t1加锁后，开始睡眠2s，在这期间按道理线程t2可以拿到锁，但是此时t1还没有释放锁，所以t2一直在自旋获取锁，等到t1解锁后，t2才停止自旋，获得了锁：

```text
t1 ===> 加锁
t1===> 解锁
t2 ===> 加锁
t2===> 解锁

Process finished with exit code 0
```

### 21.7 独享锁、共享锁

独享锁也叫排他锁，是指该锁一次只能被一个线程所持有。如果线程T对数据A加上排它锁后，则其他线程不能再对A加任何类型的锁。获得排它锁的线程即能读数据又能修改数据。JDK 中的 synchronized 和 JUC 中 Lock 的实现类就是互斥锁。

共享锁是指该锁可被多个线程所持有。如果线程T对数据A加上共享锁后，则其他线程只能对A再加共享锁，不能加排它锁。获得共享锁的线程只能读数据，不能修改数据。

独享锁与共享锁也是通过 AQS 来实现的，通过实现不同的方法，来实现独享或者共享。

下图为 `ReentrantReadWriteLock` 的部分源码：

![image-20220705203123157](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705203123157.png)

![image-20220705203142543](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705203142543.png)

看到 `ReentrantReadWriteLock` 有两把锁：`ReadLock` 和 `WriteLock`，由词知意，一个读锁一个写锁，合称 “读写锁”。再进一步观察可以发现 `ReadLock` 和 `WriteLock` 是靠内部类 `Sync` 实现的锁。`Sync` 是 AQS 的一个子类，这种结构在 `CountDownLatch`、`ReentrantLock`、`Semaphore` 里面也都存在。

在 `ReentrantReadWriteLock` 里面，读锁和写锁的锁主体都是 `Sync`，但读锁和写锁的加锁方式不一样。读锁是共享锁，写锁是独享锁。读锁的共享锁可保证并发读非常高效，而读写、写读、写写的过程互斥，因为读锁和写锁是分离的。所以 `ReentrantReadWriteLock` 的并发性相比一般的互斥锁有了很大提升。

### 21.6 死锁

死锁是指两个或两个以上的线程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去。

![image-20200812214548908](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/707c1f0130ce66a3ecb13ca178a881cc.png)

```java
public class DeadLockDemo {
    public static void main(String[] args) {
        String lockA = "lockA";
        String lockB = "lockB";

        new Thread(new MyThread(lockA, lockB), "T1").start();
        new Thread(new MyThread(lockB, lockA), "T2").start();	// 注意这里参数位置
    }
}

class MyThread implements Runnable {
    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA) {
            System.out.println(Thread.currentThread().getName() + lockA + " ===> get" + lockB);

            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            synchronized (lockB) {
                System.out.println(Thread.currentThread().getName() + lockB+" ===> get" + lockA);
            }
        }
    }
}
```

输出：

![image-20220705205020174](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220705205020174.png)



> 如何解开死锁

**1、使用jps定位进程号，jdk的bin目录下： 有一个jps**

命令：`jps -l`

![image-20200812214833647](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/32b977206fd43d9cd67cf7bf432b13e6.png)

**2、使用`jstack` 进程进程号 找到死锁信息**

![image-20200812214920583](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e56c37006badbc1bbbae99dba5438172.png)

**一般情况信息在最后：**

![image-20200812214957930](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/814d63935d3d21ed799afcc2eccd20c9.png)













