# 1-3　Java并发

## 1、线程与进程的基本概念

### 1.1 进程与线程

#### 进程

**进程** 是程序的一次执行过程，是系统运行程序的基本单位，进程是动态的。

系统运行一个程序即是一个进程从创建，运行到消亡的过程。

在 Java 中，当我们启动 `main 函数`时其实就是启动了一个 `JVM 的进程`，而 `main 函数所在的线程`就是这个进程中的一个线程，也称`主线程`。

#### 线程

线程与进程相似，但**线程**是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。

与进程不同的是，同类的多个线程共享进程的**堆**和**方法区**资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**。

所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

> **一个 Java 程序是 main 线程和多个其他线程同时运行。**

> 问：从 JVM 角度说进程和线程之间的关系（详见JVM部分）

### 1.2 并发与并行

- **并发**：两个及两个以上的作业在同一 **时间段** 内执行。
- **并行**：两个及两个以上的作业在同一 **时刻** 执行。

### 1.3 使用多线程原因

总体上：

- **从计算机底层来说：** 线程可以比作是轻量级的进程，是程序执行的最小单位，线程间的切换和调度的成本远远小于进程。另外，多核 CPU 时代意味着多个线程可以同时运行，这减少了线程上下文切换的开销。
- **从当代互联网发展趋势来说：** 现在的系统动不动就要求百万级甚至千万级的并发量，而多线程并发编程正是开发高并发系统的基础，利用好多线程机制可以大大提高系统整体的并发能力以及性能。

底层来说：

- **单核时代**：在单核时代多线程主要是为了提高单进程利用 CPU 和 IO 系统的效率。
- **多核时代**：多核时代多线程主要是为了提高进程利用多核 CPU 的能力。

> 并发编程的目的就是为了能提高程序的执行效率提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、死锁、线程不安全等等。

### 1.4 守护线程

**守护线程** 是运行在后台的一种特殊进程。

它独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。

在 Java 中垃圾回收线程就是特殊的守护线程。

## 2、线程的状态与生命周期

> java 程序中止了之后，线程还会继续运行吗？

### 2.1 线程 6 种状态

**线程在给定的时间点只能处于一种状态。**这些状态是虚拟机状态，不反映任何操作系统线程状态。

Java 线程在运行的生命周期中的指定时刻只可能处于下面 **6 种不同状态**的其中一个状态：

1. `NEW`：新建但是尚未启动的线程处于此状态，没有调用 start() 方法。
2. `RUNNABLE`：包含就绪（READY）和运行中（RUNNING）两种状态。线程调用 start() 方法会会进入就绪（READY）状态，等待获取 CPU 时间片。如果成功获取到 CPU 时间片，则会进入运行中（RUNNING）状态。
3. `BLOCKED`：线程在进入同步方法/同步块（synchronized）时被阻塞，等待同步锁的线程处于此状态。
4. `WAITING`：无限期等待另一个线程执行特定操作的线程处于此状态，需要被显示的唤醒，否则会一直等待下去。例如对于 Object.wait()，需要等待另一个线程执行 Object.notify() 或 Object.notifyAll()；对于 Thread.join()，则需要等待指定的线程终止。
5. `TIMED_WAITING`：在指定的时间内等待另一个线程执行某项操作的线程处于此状态。跟 WAITING 类似，区别在于该状态有超时时间参数，在超时时间到了后会自动唤醒，避免了无期限的等待。
6. `TERMINATED`：执行完毕已经退出的线程处于此状态。

![image-20220727203523157](https://img.zxdmy.com/2022/202207272035303.png)

### 2.2 线程生命周期

**线程在生命周期**是**随着代码的执行在不同状态之间切换**。

Java 线程状态变迁如下图所示：

![image-20220727203711300](https://img.zxdmy.com/2022/202207272037213.png)

### 2.3 上下文切换

**线程在执行过程中会有自己的运行条件和状态（也称上下文）。**

当出现如下情况的时候，线程会从占用 CPU 状态中退出：

- **主动让出 CPU**，比如调用了 `sleep()`, `wait()` 等。
- **时间片用完**，因为操作系统要防止一个线程或者进程长时间占用CPU导致其他线程或者进程饿死。
- 调用了阻塞类型的**系统中断**，比如请求 IO，线程被阻塞。
- **被终止或结束**运行

> 前三种都会发生**线程切换**，线程切换意味着需要保存当前线程的上下文，留待线程下次占用 CPU 的时候**恢复现场**，并加载下一个将要占用 CPU 的**线程上下文**。

### 2.4 线程终止（死亡）的 3 种方式

1. **正常结束**：`run()` 或者 `call()` 方法执行完成后，线程正常结束；
2. **异常结束**：线程抛出一个未捕获的 `Exception` 或 `Error`，导致线程异常结束；
3. **调用 stop()**：直接调用线程的 `stop()` 方法来结束该线程，但是一般不推荐使用该种方式，**因为该方法通常容易导致死锁**。

## 3、线程的实现与基本方法

### 3.1 线程的 3 种实现方式

通常来说，可以认为有三种方式：

1. 继承 Thread 类；
2. 实现 Callable 接口；
3. 实现 Runnable 接口。

其中，Thread 其实是实现了 Runable 接口。

`Runnable` 和 `Callable` 的区别：

|   区别   |           Callable           | Runnable |
| :------: | :--------------------------: | :------: |
| 重写方法 |            call()            |  run()   |
|   异常   |           可以抛出           |    无    |
|  返回值  |           有返回值           |    无    |
| 返回对象 | Future对象，表示异步计算结果 |    无    |

###  3.2 Thread 类调用 start() 方法的流程

1. 当 `new` 一个 `Thread` 时，线程即进入 **新建** （NEW）状态；
2. 当 `Thread` 调用 `start()` 方法时，会启动一个线程，并进入 **就绪** （READY）状态；
3. 当 该线程分配到 CPU 时间片时，就开始 **运行**（RUNNING）。

> `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。

### 3.3 Thread 类调用 start() 和 run() 方法的区别

**先说结论**

`Thread.run()` 方法：就是**普通的方法调用**，即在 Main 线程中调用 `run()` 方法，不新建线程；

`Thread.star()` 方法：启动一个**新线程执行 run() 方法**。次数该线程处于就绪（可运行）状态，并没有运行，一旦得到 CPU 时间片，就开始执行 `run()` 方法。

**代码示例：**

`Thread.run()` 方法：

```java
public class ThreatTest {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("A");
        });
        // 调用 run 方法
        thread.run();
        System.out.println("B");
    }
}
/**
A
B
*/
```

`Thread.star()` 方法：

```java
public class ThreatTest {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("A");
        });
        // 调用 start 方法
        thread.start();
        System.out.println("B");
    }
}
/**
B
A
*/
```

### 3.4 sleep() 和 wait() 方法的区别

两者都可以**暂停线程的执行**，使线程进入**等待**（WAITING）或**超时等待**（TIMED_WAITING）状态。

但其详细区别如下：

|      区别点      |             sleep()              |                            wait()                            |
| :--------------: | :------------------------------: | :----------------------------------------------------------: |
|       来源       |      `Thread` 类的静态方法       |                    `Object` 类的成员方法                     |
|    **锁释放**    |             没有释放             |                           释放了锁                           |
|       用途       |             暂停执行             |                      线程间交互 / 通信                       |
| 唤醒 / 恢复方式  |         时间结束自动恢复         | 其他线程调用同一对象的 `notify()/nofityAll()` 才能重新恢复，或使用 `wait(long timeout)` 超时后线程会自动苏醒。 |
|     使用范围     |             任何地方             | **同步控制方法或者同步控制块内**，否则抛出 `IllegalMonitorStateException`异常 |
| **同步锁的影响** | 没有影响，已拥有的**锁不会释放** |   **释放同步锁**，让其他线程进入 `synchronized` 代码块执行   |

代码示例：

```java
class SleepTest implements Runnable {

    @Override
    public void run() {
        try {
            System.out.println("sleep 开始并睡眠 5 秒");
            // 调用 sleep 方法，休眠5秒
            Thread.sleep(5000);
            System.out.println("sleep 结束睡眠");
            // 唤醒 wait
            synchronized (WaitTest.class) {
                System.out.println("唤醒 wait");
                WaitTest.class.notify();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class WaitTest implements Runnable {

    @Override
    public void run() {
        synchronized (WaitTest.class) {
            try {
                System.out.println("wait 开始并等待");
                // 调用 wait 方法，持续等待
                WaitTest.class.wait();
                // 调用 wait 方法，等待时间超过 5 秒，自动唤醒
                // this.wait(5000);
                System.out.println("wait 被唤醒并结束");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class SleepWaitTest {
    public static void main(String[] args) {
        // sleep 使用示例
        Thread thread = new Thread(new SleepTest());
        thread.start();
        // wait 使用示例
        Thread thread1 = new Thread(new WaitTest());
        thread1.start();
    }
}
/**
sleep 开始并睡眠 5 秒
wait 开始并等待
sleep 结束睡眠
唤醒 wait
wait 被唤醒并结束
*/
```

###  3.5 Thread 类的 sleep() 和 yield() 方法的区别

`Thread.sleep()` 方法：线程进入 **超时等待**（TIMED_WAITING）状态；换言之，即暂停当前线程让出 CPU，给其他线程运行的机会，不考虑优先级。

`Thread.yield()` 方法：线程进入 **就绪**（READY）状态。只给相同或更高优先级的线程运行机会。也就是说，该方法只能保证当前线程放弃 CPU 占用，但不能保证其他线程能占用 CPU，当前线程也可能进入暂停状态后马上被执行。

### 3.6 线程的 join() 方法的使用

用于等待当前线程终止，即使当前**主线程**进入 **等待**（WAITING）状态。

> 注意：这里的主线程，不是 Main 方法，而是调用了其他子线程的线程。

一句话：**让当前线程等待子线程执行结束之后再运行当前线程**。

即：如果一个线程A执行了 `threadB.join()` 语句，其含义是：当前线程A等待 `threadB` 线程终止之后才从 `threadB.join()` 的位置返回，继续往下执行线程 A 的代码。

代码示例：

```java
class MyThread_A extends Thread {

    public void run() {
        System.out.println("A 线程开始执行");
        // 创建子线程 B 并执行
        MyThread_B thread_b = new MyThread_B();
        thread_b.start();
        try {
            // 调用子线程 B 的 join 方法，等待子线程 B 结束
            thread_b.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("A 线程执行结束");
    }
}

class MyThread_B extends Thread {

    public void run() {
        for (int i = 1; i < 4; i++) {
            // 休眠 2 秒后打印
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println("B - " + i);
        }
    }
}

public class JoinTest {
    public static void main(String[] args) throws InterruptedException {
        MyThread_A thread_a = new MyThread_A();
        thread_a.start();
    }
}
/**
A 线程开始执行
B - 1
B - 2
B - 3
A 线程执行结束
*/
```

> `thread.join()` 方法只会使主线程进入等待池并等待 `thread` 线程执行完毕后才会被唤醒，并不影响同一时刻处在运行状态的其他线程。
>
> 比如上面的 Main 方法中，再来个 C 线程，则不影响 C 线程的运行。

## 4、线程安全与线程死锁

### 4.1 线程安全

当多个线程同时访问一个对象时，如果不用考虑这些线程在运行时环境下的调度和交替执行，也不需要进行额外的同步，或者在调用方进行任何其他的协调操作，调用这个对象的行为都可以获得正确的结果，那就称这个对象是线程安全的。——《Java并发编程实战》

> 什么是线程安全的？线程不安全会造成什么结果？
>
> Java 实现线程安全的方式，每一种方式的特点？

### 4.2 线程死锁

**线程死锁**：多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

![image-20220801154134903](https://img.zxdmy.com/2022/202208011541635.png)

**产生死锁的四个必要条件：**

1. **互斥**条件：该资源任意一个时刻只由一个线程占用。
2. **请求与保持**条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
3. **不剥夺**条件：线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4. **循环等待**条件：若干线程之间形成一种头尾相接的循环等待资源关系。

### 4.3 线程死锁的预防和避免

**预防： 破坏死锁的产生的必要条件即可**

1. **破坏请求与保持条件** ：一次性申请所有的资源。
2. **破坏不剥夺条件** ：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
3. **破坏循环等待条件** ：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

**避免：借助算法对资源分配进行评估使其进入`安全状态`**

+ 比如：银行家算法

> **安全状态** 指的是系统能够按照某种线程推进顺序（P1、P2、P3.....Pn）来为每个线程分配所需资源，直到满足每个线程对资源的最大需求，使每个线程都可顺利完成。称<P1、P2、P3.....Pn>序列为安全序列。

## 5、锁类型简介



## 5、加锁：synchronized 关键字

### 5.1 synchronized 简介

`synchronized` 关键字是同步的意思，主要**解决多个线程之间访问资源的同步性**，`synchronized`关键字可以保证被它修饰的方法或者代码块**在任意时刻只能有一个线程执行**。

早期的 `synchronized` 属于 **重量级锁**，效率低下。但 Java 6 ，Java官方对其进行优化，引入了如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。

### 5.2 synchronized 的使用方式

`synchronized` 关键字最主要的三种使用方式：

1. 修饰实例方法
2. 修饰静态方法
3. 修饰代码块

#### 方式1：修饰实例方法

> 锁当前对象实例

给当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁** 。

```java
synchronized void method() {
    //业务代码
}
```

> 注意：**构造方法不能使用 synchronized 关键字修饰。**构造方法本身就属于线程安全的，不存在同步的构造方法一说。

#### 方式2：修饰静态方法

> 锁当前类

给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。

```java
synchronized static void method() {
    //业务代码
}
```

#### 方式3：修饰代码块

> 锁指定对象/类

对括号里指定的对象/类加锁：

- `synchronized(object)` 表示进入同步代码库前要获得 **给定对象的锁**。
- `synchronized(类.class)` 表示进入同步代码前要获得 **给定 Class 的锁**

```java
synchronized(this) {
    //业务代码
}
```

> 尽量不要使用 `synchronized(String a)`，因为 JVM 中，字符串常量池具有缓存功能。

### 5.3 synchronized 的使用示例

下面是一个使用的示例。

```java
class MyThread extends Thread {

    String name;

    MyThread(String name) {
        this.name = name;
    }

    public void run() {
        // 同步代码块
        synchronized (MyThread.class) {
            System.out.println(this.name + " 线程开始");
            // 休眠 2 秒后打印
            for (int i = 1; i < 4; i++) {
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                System.out.println(this.name + " - " + i);
            }
            System.out.println(this.name + " 线程结束");
        }
    }
}

public class SynchronizedTest {
    public static void main(String[] args) {
        MyThread thread_a = new MyThread("A");
        MyThread thread_b = new MyThread("B");
        thread_a.start();
        thread_b.start();
    }
}
/**
A 线程开始
A - 1
A - 2
A - 3
A 线程结束
B 线程开始
B - 1
B - 2
B - 3
B 线程结束
*/
```

### 5.3 synchronized 的底层原理

> `synchronized` 关键字底层原理属于 JVM 层面。

#### 修饰同步语句块

如代码：

```java
public class SynchronizedDemo {
    public void method() {
        synchronized (this) {
            System.out.println("synchronized 代码块");
        }
    }
}
```

其在 JVM 中的字节码为：

![image-20220801162432996](https://img.zxdmy.com/2022/202208011624904.png)

可以看出：**`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。**

当执行 `monitorenter` 指令时，线程试图获取锁也就是获取 **对象监视器 `monitor`** 的持有权。

在执行`monitorenter`时，会尝试获取对象的锁，如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。

![image-20220801163059853](https://img.zxdmy.com/2022/202208011631456.png)

对象锁的的拥有者线程才可以执行 `monitorexit` 指令来释放锁。在执行 `monitorexit` 指令后，将锁计数器设为 0，表明锁被释放，其他线程可以尝试获取锁。

![image-20220801163103539](https://img.zxdmy.com/2022/202208011631595.png)



如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。

#### 修饰方法

如代码：

```java
public class SynchronizedDemo2 {
    public synchronized void method() {
        System.out.println("synchronized 方法");
    }
}
```

其在 JVM 中的字节码为：

![image-20220801162455652](https://img.zxdmy.com/2022/202208011624507.png)

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

如果是实例方法，JVM 会尝试获取实例对象的锁。如果是静态方法，JVM 会尝试获取当前 class 的锁

#### 总结

`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。

**不过两者的本质都是对对象监视器 monitor 的获取。**

> synchronized 的锁优化

## 6、加锁：Lock 接口

### 简介

`java.util.concurrent.locks.Lock` 是一个类似于`synchronized` 关键字的线程同步机制。

`Lock` 比 `synchronized` 块更加灵活。`Lock` 是个**接口**，有个**实现类**是 `ReentrantLock`。

### synchronized 与 Lock 的异同

|  区别点  |            synchronized 关键词             |      Lock / ReentrantLock 接口       |
| :------: | :----------------------------------------: | :----------------------------------: |
|  锁类型  |                  可重入锁                  |               可重入锁               |
|   用法   |          给 类、方法、代码块 加锁          |          只能给 代码块 加锁          |
| 锁的取舍 | 不需要手动获取和释放锁，异常释放锁，无死锁 | 需要手动加锁和释放锁，处理不当会死锁 |
|   实现   |                  基于 JVM                  |               基于 API               |

### Lock / ReentrantLock 的高级功能

#### 等待可中断



#### 可实现公平锁



#### 可实现选择性通知













## 6、变量：volatile 关键字

### 6.1 CPU 缓存模型

**CPU Cache 缓存 的内存数据用于解决 CPU 处理速度和内存不匹配的问题**，内存缓存的是硬盘数据用于解决硬盘访问速度过慢的问题。

![image-20220805152854756](https://img.zxdmy.com/2022/202208051528133.png)

**CPU Cache 工作方式**：

1. 复制一份数据到 CPU Cache 中
2. 直接从 CPU Cache 中读取数据
3. 当运算完成后，将运算得到的数据写回 Main Memory 中。

> 上述工作方式存在**内存缓存不一致性的问题**：**通过制定缓存一致协议或者其他手段来解决**

### 6.2 Java 内存模型（JMM）

**Java 内存模型** 抽象了线程和主内存之间的关系，就比如说 **线程之间的共享变量必须存储在主内存** 中。

![image-20220805153304337](https://img.zxdmy.com/2022/202208051533910.png)

> - **主内存** ：所有线程创建的实例对象都存放在主内存中，不管该实例对象是成员变量还是方法中的本地变量(也称局部变量)
> - **本地内存** ：每个线程都有一个私有的本地内存来存储共享变量的副本，并且，每个线程只能访问自己的本地内存，无法访问其他线程的本地内存。本地内存是 JMM 抽象出来的一个概念，存储了主内存中的共享变量副本。

**Java 内存区域和内存模型的区别**：

- **Java 内存区域** 定义了JVM 在运行时如何分区存储程序数据，就比如说堆主要用于存放对象实例。
- **Java 内存模型** 抽象了线程和主内存之间的关系，就比如说线程之间的共享变量必须存储在主内存中，其目的是为了屏蔽系统和硬件的差异，避免一套代码在不同的平台下产生的效果不一致。

### 6.3 volatile：保证变量可见性

**`volatile` 关键字可以保证变量的可见性**，如果我们将变量声明为 **`volatile`** ，这就指示 JVM，这个**变量是共享且不稳定的，每次使用它都到主存中进行读取**。

![image-20220805153458705](https://img.zxdmy.com/2022/202208051535091.png)

### 6.4 volatile：防止 JVM 指令重排

为了提升执行速度/性能，**计算机在执行程序代码的时候，会对指令就行重排序**，即系统在执行代码的时候并不一定是按照所写代码的顺序依次执行。

**指令重排序可以保证串行语义一致，但是没有义务保证多线程间的语义也一致**。

**`volatile` 关键字可以防止 JVM 的指令重排序。**

即：将变量声明为 **`volatile`** ，在对这个变量进行读写操作的时候，编译器会通过 **内存屏障** 来禁止指令重排序。

> volatile 关键字，在单例模式里有什么作用

## 7、变量：ThreadLocal

`ThreadLocal`，即**线程本地变量**。

如果创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的一个本地拷贝，多个线程操作这个变量的时候，实际是操作自己本地内存里面的变量，从而起到线程隔离的作用，避免了线程安全问题。



## 8、线程池

### 8.1 使用线程池的原因

> **池化技术** 的思想，在项目中已被广泛应用，比如数据库连接处、HTTP 连接池等等，池化技术的思想主要是为了减少每次获取资源的消耗，提供对资源的利用率。

**线程池** 提供了一种限制和管理资源（包括执行一个任务）的方式。

每个**线程池** 维护了一些基本统计信息，例如已完成任务的数量

**使用线程池的好处**：

- **降低资源消耗**。通过`重复利用已创建的线程`降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以`不需要等到线程创建就能立即执行`。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，`使用线程池可以进行统一的分配，调优和监控` 。

### 8.2 Executor 框架结构

`Executor` 框架是 Java5 之后引进的，通过 `Executor` 来启动线程比使用 `Thread` 的 `start` 方法更好，除了更易管理，效率更好（用线程池实现，节约开销）外，还有关键的一点：有助于避免 this 逃逸问题。

> 补充：this 逃逸是指在构造函数返回之前其他线程就持有该对象的引用. 调用尚未构造完全的对象的方法可能引发令人疑惑的错误。

`Executor` 框架不仅包括了**线程池的管理**，还提供了**线程工厂**、**队列**以及**拒绝策略**等，`Executor` 框架让并发编程变得更加简单。

#### 结构1：任务（Runnable / Callable）

执行任务需要实现 **`Runnable` 接口** 或 **`Callable`接口**。**`Runnable` 接口 **或 **`Callable` 接口** 的实现类都可以被 **`ThreadPoolExecutor`** 或 **`ScheduledThreadPoolExecutor`** 执行。

> **`Runnable` 接口和 `Callable` 接口的区别：**
>
> + **`Runnable` 接口** 不会返回结果或抛出检查异常，但是 **`Callable` 接口** 可以。
> + 工具类 `Executors` 可以实现将 `Runnable` 对象转换成 `Callable` 对象。

#### 结构2：任务的执行（Executor）

任务执行机制的**核心接口**是 **`Executor`** ，以及继承自 `Executor` 接口的 `ExecutorService` 接口。

![image-20220804180215789](https://img.zxdmy.com/2022/202208041802505.png)

**`ThreadPoolExecutor`** 和 **`ScheduledThreadPoolExecutor`** 这两个**关键类**实现了 `ExecutorService` 接口。

> 上图的各个底层类关系中， `ThreadPoolExecutor` 类在实际使用线程池的过程中，使用频率非常高。

#### 结构3：异步计算的结果（Future）

**`Future`** 接口以及 `Future` 接口的实现类 **`FutureTask`** 类都可以代表异步计算的结果。

当我们把 **`Runnable`接口** 或 **`Callable` 接口** 的实现类提交给 **`ThreadPoolExecutor`** 或 **`ScheduledThreadPoolExecutor`** 执行，调用 `submit()` 方法时会返回一个 **`FutureTask`** 对象。

> **`ExecutorService` 接口有两种执行线程任务的方法：`execute()` 和 `submit()`，其区别如下**：
>
> + `execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
> + `submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get(long timeout，TimeUnit unit)`方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。

### 8.3 线程池流程

#### Executor 的执行流程

如下图所示：

![image-20220804181040377](https://img.zxdmy.com/2022/202208041810573.png)

1. **主线程**创建实现 `Runnable` 或 `Callable` 接口的**任务对象**；
2. 可以把实现 `Runnable`接口的 **任务对象** 直接交给 `ExecutorService` 的`execute(Runnable command)` 方法执行；
3. 或者可以把实现 `Runnable/Callable` 接口的 **任务对象** 提交给 `ExecutorService` 的`submit(Runnable task)` 或  `submit(Callable <T> task)` 执行，并返回一个实现`Future`接口的`FutureTask` 对象。
4. 最后，主线程可以执行 `FutureTask.get()`方法来等待任务执行完成。
5. 主线程也可以执行 `FutureTask.cancel（boolean mayInterruptIfRunning）`来取消此任务的执行。

#### 线程池的运行（运作）流程

![image-20220727202905097](https://img.zxdmy.com/2022/202207272029608.png)

### 8.4 ThreadPoolExecutor 类

> 线程池实现类 `ThreadPoolExecutor` 是 `Executor` 框架最核心的类。

#### ThreadPoolExecutor 类分析

`ThreadPoolExecutor` 类中提供的四个构造方法。其中最基础的构造方法如下：

```java
/**
 * 用给定的初始参数创建一个新的ThreadPoolExecutor。
 */
public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
                          int maximumPoolSize,//线程池的最大线程数
                          long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
                          TimeUnit unit,//时间单位
                          BlockingQueue<Runnable> workQueue,//任务队列，
                          ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
                          RejectedExecutionHandler handler//拒绝策略，
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

其他构造方法均基于此，对一些默认参数进行指定。

**构造方法的 7 个参数：**

1. **`corePoolSize`** ：**核心线程数**，定义最小可以同时运行的线程数量。
2. **`maximumPoolSize`** ：线程池的**最大线程数**。当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
3. **`workQueue`**：任务队列，用来储存等待执行任务的队列。新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。
4. **`keepAliveTime`** ：当线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁；
5. **`unit`** ：`keepAliveTime` 参数的时间单位。
6. **`threadFactory`** ：线程工厂，`executor` 创建新线程的时候会用到，一般默认即可。
7. **`handler`** ：**饱和策略（拒绝策略）**，当提交的任务过多而不能及时处理时，定制策略来处理任务：
    + `ThreadPoolExecutor.AbortPolicy` ：（默认策略）抛出 `RejectedExecutionException`来拒绝新任务的处理。
    + `ThreadPoolExecutor.CallerRunsPolicy` ：（推荐使用）调用执行自己的线程运行任务，也就是直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。如果您的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
    + `ThreadPoolExecutor.DiscardPolicy` ：不处理新任务，直接丢弃掉。
    + `ThreadPoolExecutor.DiscardOldestPolicy` ：此策略将丢弃最早的未处理的任务请求。

> **不允许使用 `Executors` 去创建，而是通过 `ThreadPoolExecutor` 构造函数的方式创建线程池的原因：规避资源耗尽的风险**。
>
> `Executors` 返回线程池对象的弊端如下：
>
> - `FixedThreadPool` 和 `SingleThreadExecutor` ：允许**请求的队列长度**为 `Integer.MAX_VALUE` ，可能堆积大量的请求，从而导致 OOM。
> - `CachedThreadPool` 和 `ScheduledThreadPool` ：允许**创建的线程数量**为 `Integer.MAX_VALUE` ，可能会创建大量线程，从而导致 OOM。

#### ThreadPoolExecutor 类的使用

**1、创建任务的实现类**

以`Runnable` 接口为例：

```java
import java.util.Date;

public class MyRunnable implements Runnable {

    private String userName;

    public MyRunnable(String username) {
        this.userName = username;
    }


    @Override
    public void run() {
        System.out.println("当前线程：" + Thread.currentThread().getName() + " 开始时间：" + new Date() + " 当前用户：" + this.userName);
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("当前线程：" + Thread.currentThread().getName() + " 结束时间：" + new Date());
    }
}
```

以`Callable` 接口为例：

```java
import java.util.Date;
import java.util.concurrent.Callable;

public class MyCallable implements Callable<String> {

    private String userName;

    public MyCallable(String username) {
        this.userName = username;
    }

    @Override
    public String call() throws Exception {
        System.out.println("当前线程：" + Thread.currentThread().getName() + " 开始时间：" + new Date() + " 当前用户：" + this.userName);
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("当前线程：" + Thread.currentThread().getName() + " 结束时间：" + new Date());
        return "用户 " + this.userName + " 执行完成";
    }
}
```

**2、创建线程池和任务并执行**

```java
package xianchengchi;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        // 创建线程池
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(2, 4, 100,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(10),
                new ThreadPoolExecutor.DiscardPolicy()

        );
        // MyCallable 执行结果返回列表
        // List<Future<String>> futureList = new ArrayList<>();

        // 循环
        for (int i = 0; i < 20; i++) {
            // 通过 Runnable创建任务并执行：
            MyRunnable myRunnable = new MyRunnable("张三" + i);
            threadPoolExecutor.execute(myRunnable);

            // 通过 Callable创建任务并执行
            // MyCallable myCallable = new MyCallable("李四" + i);
            // Future<String> future = threadPoolExecutor.submit(myCallable);
            // futureList.add(future);
        }
        // 打印 MyCallable 的执行结果
        // for (Future<String> future : futureList) {
        //     try {
        //         System.out.println(future.get());
        //     } catch (Exception e) {
        //         e.printStackTrace();
        //     }
        // }

        // 任务执行完成后，结束线程池
        threadPoolExecutor.shutdown();
        while (!threadPoolExecutor.isTerminated()) {
        }
        System.out.println("Finished all threads");
    }
}

```

执行结果：

```java
当前线程：pool-1-thread-3 开始时间：Fri Aug 05 10:49:23 CST 2022 当前用户：张三12
当前线程：pool-1-thread-4 开始时间：Fri Aug 05 10:49:23 CST 2022 当前用户：张三13
当前线程：pool-1-thread-1 开始时间：Fri Aug 05 10:49:23 CST 2022 当前用户：张三0
当前线程：pool-1-thread-2 开始时间：Fri Aug 05 10:49:23 CST 2022 当前用户：张三1
当前线程：pool-1-thread-4 结束时间：Fri Aug 05 10:49:28 CST 2022
当前线程：pool-1-thread-3 结束时间：Fri Aug 05 10:49:28 CST 2022
当前线程：pool-1-thread-1 结束时间：Fri Aug 05 10:49:28 CST 2022
当前线程：pool-1-thread-2 结束时间：Fri Aug 05 10:49:28 CST 2022
当前线程：pool-1-thread-1 开始时间：Fri Aug 05 10:49:28 CST 2022 当前用户：张三3
当前线程：pool-1-thread-3 开始时间：Fri Aug 05 10:49:28 CST 2022 当前用户：张三2
当前线程：pool-1-thread-2 开始时间：Fri Aug 05 10:49:28 CST 2022 当前用户：张三5
当前线程：pool-1-thread-4 开始时间：Fri Aug 05 10:49:28 CST 2022 当前用户：张三4
当前线程：pool-1-thread-2 结束时间：Fri Aug 05 10:49:33 CST 2022
当前线程：pool-1-thread-3 结束时间：Fri Aug 05 10:49:33 CST 2022
当前线程：pool-1-thread-2 开始时间：Fri Aug 05 10:49:33 CST 2022 当前用户：张三6
当前线程：pool-1-thread-3 开始时间：Fri Aug 05 10:49:33 CST 2022 当前用户：张三7
当前线程：pool-1-thread-1 结束时间：Fri Aug 05 10:49:33 CST 2022
当前线程：pool-1-thread-4 结束时间：Fri Aug 05 10:49:33 CST 2022
当前线程：pool-1-thread-1 开始时间：Fri Aug 05 10:49:33 CST 2022 当前用户：张三8
当前线程：pool-1-thread-4 开始时间：Fri Aug 05 10:49:33 CST 2022 当前用户：张三9
当前线程：pool-1-thread-3 结束时间：Fri Aug 05 10:49:38 CST 2022
当前线程：pool-1-thread-1 结束时间：Fri Aug 05 10:49:38 CST 2022
当前线程：pool-1-thread-2 结束时间：Fri Aug 05 10:49:38 CST 2022
当前线程：pool-1-thread-1 开始时间：Fri Aug 05 10:49:38 CST 2022 当前用户：张三11
当前线程：pool-1-thread-4 结束时间：Fri Aug 05 10:49:38 CST 2022
当前线程：pool-1-thread-3 开始时间：Fri Aug 05 10:49:38 CST 2022 当前用户：张三10
当前线程：pool-1-thread-1 结束时间：Fri Aug 05 10:49:43 CST 2022
当前线程：pool-1-thread-3 结束时间：Fri Aug 05 10:49:43 CST 2022
Finished all threads

Process finished with exit code 0
```

如上是执行流程的输出结果，如下图所示。

![image-20220805104411091](https://img.zxdmy.com/2022/202208051044558.png)

> **核心线程池** 对应参数：`corePoolSize = 2`，大于则加入等待队列
>
> **线程池** 对应参数：`maximumPoolSize = 4`，等待队列满了，则创建新的线程池，进行执行

#### ThreadPoolExecutor 类的几个方法

`execute()` vs `submit()`

- `execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
- `submit()`方法用于提交需要返回值的任务。

`shutdown()`VS`shutdownNow()`

- **`shutdown（）`** :关闭线程池，线程池的状态变为 `SHUTDOWN`。线程池不再接受新任务了，但是队列里的任务得执行完毕。
- **`shutdownNow（）`** :关闭线程池，线程的状态变为 `STOP`。线程池会终止当前正在运行的任务，并停止处理排队的任务并返回正在等待执行的 List。

`isTerminated()` VS `isShutdown()`

- **`isShutDown`** 当调用 `shutdown()` 方法后返回为 true。
- **`isTerminated`** 当调用 `shutdown()` 方法后，并且所有提交的任务完成后返回为 true

> 线程池核心线程空闲时怎么销毁？
>
> 超过corePoolSize的空闲线程由线程池回收，线程池Worker启动跑第一个任务之后就一直循环遍历线程池任务队列，超过指定超时时间获取不到任务就remove Worker，最后由垃圾回收器回收。
>
> https://zhuanlan.zhihu.com/p/510240849

### 8.5 更多线程池（ThreadExecutor）

#### FixedThreadPool

**实现源码：**

```java
/**
* 创建一个可重用固定数量线程的线程池
*/
public static ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory) {
    return new ThreadPoolExecutor(nThreads, 
                                  nThreads,
                                  0L, 
                                  TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>(),
                                  threadFactory);
}
```

> 创建的 `FixedThreadPool` 的 `corePoolSize` 和 `maximumPoolSize` 都被设置为 nThreads。

**不足：**

1. 当线程池中的线程数达到 `corePoolSize` 后，新任务将在无界队列中等待，因此线程池中的线程数不会超过 corePoolSize；
2. 由于使用无界队列时 `maximumPoolSize` 将是一个无效参数，因为不可能存在任务队列满的情况。所以，通过创建 `FixedThreadPool`的源码可以看出创建的 `FixedThreadPool` 的 `corePoolSize` 和 `maximumPoolSize` 被设置为同一个值。
3. 由于 1 和 2，使用无界队列时 `keepAliveTime` 将是一个无效参数；
4. 运行中的 `FixedThreadPool`（未执行 `shutdown()`或 `shutdownNow()`）不会拒绝任务，在任务比较多的时候会导致 OOM（内存溢出）

#### SingleThreadExecutor

**实现源码**：

```java
/**
 *返回只有一个线程的线程池
 */
public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 
                                1,
                                0L, 
                                TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>(),
                                threadFactory));
}
```

**不足：**

`SingleThreadExecutor` 使用无界队列 `LinkedBlockingQueue` 作为线程池的工作队列（队列的容量为 Intger.MAX_VALUE）。`SingleThreadExecutor` 使用无界队列作为线程池的工作队列会对线程池带来的影响与 `FixedThreadPool` 相同。说简单点就是可能会导致 OOM，

#### CachedThreadPool

**实现源码**：

```java
/**
 * 创建一个线程池，根据需要创建新线程，但会在先前构建的线程可用时重用它。
 */
public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory) {
    return new ThreadPoolExecutor(0, 
                                  Integer.MAX_VALUE,
                                  60L, 
                                  TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>(),
                                  threadFactory);
}
```

**不足：**

`CachedThreadPool`允许创建的线程数量为 `Integer.MAX_VALUE` ，可能会创建大量线程，从而导致 OOM。

### 8.6 线程池大小确定



多线程同步怎么做？



## 9、Fork & Join 框架

#### 简介

Fork/Join框架是Java7提供的一个用于并行执行任务的框架，是一个**把大任务分割成若干个小任务**，最终**汇总每个小任务结果后得到大任务结果**的框架。这体现出来的是 **分而治之** 特点。

还有一个运行时的算法是 **工作窃取算法** ：把大任务拆分成小任务，放到不同队列执行，交由不同的线程分别执行时。有的线程优先把自己负责的任务执行完了，其他线程还在慢慢悠悠处理自己的任务，这时候为了充分提高效率，就需要工作盗窃算法，即 **某个线程从其他队列中窃取任务进行执行**，一般就是指做得快的线程（盗窃线程）抢慢的线程的任务来做，同时为了减少锁竞争，通常使用双端队列，即快线程和慢线程各在一端。

![img](https://img.zxdmy.com/2022/202208151049925.png)

#### 示例：打印斐波那契数列

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.concurrent.RecursiveTask;
public class Fibonacci extends RecursiveTask<Integer>{

    private int n;
    public Fibonacci(int n) {
        this.n = n;
    }
    @Override
    protected Integer compute() {
        if (n <= 1){
            return n;
        }
        Fibonacci f1 = new Fibonacci(n - 1);
        f1.fork();
        Fibonacci f2 = new Fibonacci(n - 2);
        f2.fork();
        return f1.join() + f2.join();
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ForkJoinPool pool = new ForkJoinPool();
        for (int i = 0; i< 10; i++) {
            ForkJoinTask task = pool.submit(new Fibonacci(i));
            System.out.println(task.get());
        }
    }
}
```

## 10、CAS 同步原语

#### 简介

`CAS`：全称 `Compare and swap`，即**比较并交换**，它是一条 **CPU 同步原语**。

CAS 是一种硬件对并发的支持，针对多处理器操作而设计的一种特殊指令，用于 **管理对共享数据的并发访问**。

CAS 是一种 **无锁** 的 **非阻塞** 算法的实现。

CAS 包含了 3 个**操作数**：

+ 需要读写的内存值 V
+ 旧的预期值 A
+ 要修改的更新值 B

当且仅当 V 的值等于 A 时，CAS 通过原子方式用新值 B 来更新 V 的 值，否则不会执行任何操作（他的功能是判断内存某个位置的值是否为预期值，如果是则更改为新的值，这个过程是原子的）。

CAS 并发原语体现在 Java 语言中的 `sum.misc.Unsafe` 类中的各个方法。调用 Unsafe 类中的 CAS 方法， JVM 会帮助我们实现出 CAS 汇编指令。这是一种完全依赖于硬件的功能，通过它实现了原子操作。再次强调，由于 CAS是一种系统原语，**原语属于操作系统用于范畴，是由若干条指令组成的，用于完成某个功能的一个过程，并且原语的执行必须是连续的**，**在执行过程中不允许被中断**，CAS 是一条 CPU 的原子指令，不会造成数据不一致问题。

#### 缺陷与解决方法

CAS 主要存在三个缺陷：

+ **ABA 问题**

**问题描述**：

并发环境下，假设初始条件是A，去修改数据时，发现是A就会执行修改。但是看到的虽然是A，中间可能发生了A变B，B又变回A的情况。此时A已经非彼A，数据即使成功修改，也可能有问题。

**解决方案**：

可以通过`AtomicStampedReference` **解决ABA问题**，它是一个带有标记的原子引用类，通过控制变量值的版本来保证CAS的正确性。

+ **循环时间长开销**

**问题描述**：

自旋CAS，如果一直循环执行，一直不成功，会给CPU带来非常大的执行开销。

**解决方案**：

很多时候，CAS思想体现，是有个自旋次数的，就是为了避开这个耗时问题~

+ **只能保证一个变量的原子操作**

**问题描述**：

CAS 保证的是对一个变量执行操作的原子性，如果对多个变量操作时，CAS 目前无法直接保证操作的原子性的。

**解决方案**：

1、使用互斥锁来保证原子性；

2、将多个变量封装成对象，通过 `AtomicReference` 来保证原子性。







## Java 常见锁

公平锁和非公平锁的区别，优缺点

java 里的各种锁

公平锁非公平锁

可重入与不可重入锁

可中断与不可中断锁、