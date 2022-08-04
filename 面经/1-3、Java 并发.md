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

## 2、线程的状态与生命周期

> java 程序中止了之后，线程还会继续运行吗？

### 2.1 线程状态

**线程在给定的时间点只能处于一种状态。**这些状态是虚拟机状态，不反映任何操作系统线程状态。

Java 线程在运行的生命周期中的指定时刻只可能处于下面 **6 种不同状态**的其中一个状态：

1. `NEW`：新建但是尚未启动的线程处于此状态，没有调用 start() 方法。
2. `RUNNABLE`：包含就绪（READY）和运行中（RUNNING）两种状态。线程调用 start() 方法会会进入就绪（READY）状态，等待获取 CPU 时间片。如果成功获取到 CPU 时间片，则会进入运行中（RUNNING）状态。
3. `BLOCKED`：线程在进入同步方法/同步块（synchronized）时被阻塞，等待同步锁的线程处于此状态。
4. `AITING`：无限期等待另一个线程执行特定操作的线程处于此状态，需要被显示的唤醒，否则会一直等待下去。例如对于 Object.wait()，需要等待另一个线程执行 Object.notify() 或 Object.notifyAll()；对于 Thread.join()，则需要等待指定的线程终止。
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

## 3、线程的实现与基本方法

### 3.1 线程的实现方式

通常来说，可以认为有三种方式：

1）继承 Thread 类；

2）实现 Runnable 接口；

3）实现 Callable 接口。

其中，Thread 其实也是实现了 Runable 接口。Runnable 和 Callable 的主要区别在于是否有返回值。

### 3.2 Thread 调用 start() 方法和调用 run() 方法的区别

run()：普通的方法调用，在主线程中执行，不会新建一个线程来执行。

start()：新启动一个线程，这时此线程处于就绪（可运行）状态，并没有运行，一旦得到 CPU 时间片，就开始执行 run() 方法。

### 3.3 sleep() 和 wait() 的区别

- 两者最主要的区别在于：**`sleep()` 方法没有释放锁，而 `wait()` 方法释放了锁** 。
- 两者都可以暂停线程的执行。
- `wait()` 通常被用于线程间交互/通信，`sleep() `通常被用于暂停执行。
- `wait()` 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify() `或者 `notifyAll()` 方法。`sleep() `方法执行完成后，线程会自动苏醒。或者可以使用 `wait(long timeout)` 超时后线程会自动苏醒。

+ 来源不同：
  + sleep() 来自 Thread 类，
  + wait() 来自 Object 类。
+ 对于同步锁的影响不同：
  + sleep() 不会该表同步锁的行为，如果当前线程持有同步锁，那么 sleep 是不会让线程释放同步锁的，
  + wait() 会释放同步锁，让其他线程进入 synchronized 代码块执行。
+ 使用范围不同：
  + sleep() 可以在任何地方使用，
  + wait() 只能在同步控制方法或者同步控制块里面使用，否则会抛 IllegalMonitorStateException。
+ 恢复方式不同：两者会暂停当前线程，但是在恢复上不太一样。
  + sleep() 在时间到了之后会重新恢复，
  + wait() 需要其他线程调用同一对象的 notify()/nofityAll() 才能重新恢复。

###  线程的 sleep() 方法和 yield() 方法有什么区别？

线程执行 sleep() 方法后进入超时等待（TIMED_WAITING）状态，而执行 yield() 方法后进入就绪（READY）状态。

sleep() 方法给其他线程运行机会时不考虑线程的优先级，因此会给低优先级的线程运行的机会；yield() 方法只会给相同优先级或更高优先级的线程以运行的机会。

### 线程的 join() 方法是干啥用的

用于等待当前线程终止。如果一个线程A执行了 threadB.join() 语句，其含义是：当前线程A等待 threadB 线程终止之后才从 threadB.join() 返回继续往下执行自己的代码。

### 为什么我们调用 start() 方法时会执行 run() 方法，为什么我们不能直接调用 run() 方法？

这是另一个非常经典的 Java 多线程面试问题，而且在面试中会经常被问到。很简单，但是很多人都会答不上来！

new 一个 Thread，线程进入了新建状态。调用 `start()`方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。 `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。 但是，直接执行 `run()` 方法，会把 `run()` 方法当成一个 main 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

**总结： 调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。**

## 4、线程安全与线程死锁

### 4.1 线程安全

当多个线程同时访问一个对象时，如果不用考虑这些线程在运行时环境下的调度和交替执行，也不需要进行额外的同步，或者在调用方进行任何其他的协调操作，调用这个对象的行为都可以获得正确的结果，那就称这个对象是线程安全的。——《Java并发编程实战》

> 什么是线程安全的？线程不安全会造成什么结果？

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



## 5、synchronized 关键字

### 5.1 synchronized 简介

`synchronized` 关键字是同步的意思，主要**解决多个线程之间访问资源的同步性**，`synchronized`关键字可以保证被它修饰的方法或者代码块**在任意时刻只能有一个线程执行**。

早期的 `synchronized` 属于 **重量级锁**，效率低下。但 Java 6 ，Java官方对其进行优化，引入了如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。

### 5.2 synchronized 的使用方式

`synchronized` 关键字最主要的三种使用方式：

1. 修饰实例方法
2. 修饰静态方法
3. 修饰代码块

#### 修饰实例方法

> 锁当前对象实例

给当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁** 。

```java
synchronized void method() {
    //业务代码
}
```

> 注意：**构造方法不能使用 synchronized 关键字修饰。**构造方法本身就属于线程安全的，不存在同步的构造方法一说。

#### 修饰静态方法

> 锁当前类

给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。

```java
synchronized static void method() {
    //业务代码
}
```

#### 修饰代码块

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

## 6、volatile 关键字









volatile 关键字了解吗？

volatile 关键字，在单例模式里有什么作用

## 7、线程池

### 7.1 使用线程池的原因

> **池化技术** 的思想，在项目中已被广泛应用，比如数据库连接处、HTTP 连接池等等，池化技术的思想主要是为了减少每次获取资源的消耗，提供对资源的利用率。

**线程池** 提供了一种限制和管理资源（包括执行一个任务）的方式。

每个**线程池** 维护了一些基本统计信息，例如已完成任务的数量

**使用线程池的好处**：

- **降低资源消耗**。通过`重复利用已创建的线程`降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以`不需要等到线程创建就能立即执行`。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，`使用线程池可以进行统一的分配，调优和监控` 。

### 7.2 Executor 框架结构

`Executor` 框架是 Java5 之后引进的，通过 `Executor` 来启动线程比使用 `Thread` 的 `start` 方法更好，除了更易管理，效率更好（用线程池实现，节约开销）外，还有关键的一点：有助于避免 this 逃逸问题。

> 补充：this 逃逸是指在构造函数返回之前其他线程就持有该对象的引用. 调用尚未构造完全的对象的方法可能引发令人疑惑的错误。

`Executor` 框架不仅包括了**线程池的管理**，还提供了**线程工厂**、**队列**以及**拒绝策略**等，`Executor` 框架让并发编程变得更加简单。

#### 结构1：任务（Runnable /Callable）

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

#### Executor 的执行流程

如下图所示：

![image-20220804181040377](https://img.zxdmy.com/2022/202208041810573.png)

1. **主线程**创建实现 `Runnable` 或 `Callable` 接口的**任务对象**；
2. 可以把实现 `Runnable`接口的 **任务对象** 直接交给 `ExecutorService` 的`execute(Runnable command)` 方法执行；
3. 或者可以把实现 `Runnable/Callable` 接口的 **任务对象** 提交给 `ExecutorService` 的`submit(Runnable task)` 或  `submit(Callable <T> task)` 执行，并返回一个实现`Future`接口的`FutureTask` 对象。
4. 最后，主线程可以执行 `FutureTask.get()`方法来等待任务执行完成。
5. 主线程也可以执行 `FutureTask.cancel（boolean mayInterruptIfRunning）`来取消此任务的执行。

### 7.3 ThreadPoolExecutor 类

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

1. **`corePoolSize`** ：核心线程数，定义最小可以同时运行的线程数量。
2. **`maximumPoolSize`** ：线程池的最大线程数。当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
3. **`workQueue`**：任务队列，用来储存等待执行任务的队列。新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。
4. **`keepAliveTime`** ：当线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁；
5. **`unit`** ：`keepAliveTime` 参数的时间单位。
6. **`threadFactory`** ：线程工厂，`executor` 创建新线程的时候会用到，一般默认即可。。
7. **`handler`** :饱和策略（拒绝策略），当提交的任务过多而不能及时处理时，定制策略来处理任务：
  + `ThreadPoolExecutor.AbortPolicy` ：（默认策略）抛出 `RejectedExecutionException`来拒绝新任务的处理。
  + `ThreadPoolExecutor.CallerRunsPolicy` ：（推荐使用）调用执行自己的线程运行任务，也就是直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。如果您的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
  + `ThreadPoolExecutor.DiscardPolicy` ：不处理新任务，直接丢弃掉。
  + `ThreadPoolExecutor.DiscardOldestPolicy` ：此策略将丢弃最早的未处理的任务请求。

> 不允许使用 `Executors` 去创建，而是通过 `ThreadPoolExecutor` 构造函数的方式创建线程池的原因：规避资源耗尽的风险。
>
> `Executors` 返回线程池对象的弊端如下：
>
> - `FixedThreadPool` 和 `SingleThreadExecutor` ：允许**请求的队列长度**为 `Integer.MAX_VALUE` ，可能堆积大量的请求，从而导致 OOM。
> - `CachedThreadPool` 和 `ScheduledThreadPool` ：允许**创建的线程数量**为 `Integer.MAX_VALUE` ，可能会创建大量线程，从而导致 OOM。







### 线程池的核心属性

threadFactory（线程工厂）：用于创建工作线程的工厂。

corePoolSize（核心线程数）：当线程池运行的线程少于 corePoolSize 时，将创建一个新线程来处理请求，即使其他工作线程处于空闲状态。

workQueue（队列）：用于保留任务并移交给工作线程的阻塞队列。

maximumPoolSize（最大线程数）：线程池允许开启的最大线程数。

handler（拒绝策略）：往线程池添加任务时，将在下面两种情况触发拒绝策略：1）线程池运行状态不是 RUNNING；2）线程池已经达到最大线程数，并且阻塞队列已满时。

keepAliveTime（保持存活时间）：如果线程池当前线程数超过 corePoolSize，则多余的线程空闲时间超过 keepAliveTime 时会被终止。

### 线程池的运行（运作）流程

![image-20220727202905097](https://img.zxdmy.com/2022/202207272029608.png)

### 线程池的拒绝策略

AbortPolicy：中止策略。默认的拒绝策略，直接抛出 RejectedExecutionException。调用者可以捕获这个异常，然后根据需求编写自己的处理代码。

DiscardPolicy：抛弃策略。什么都不做，直接抛弃被拒绝的任务。

DiscardOldestPolicy：抛弃最老策略。抛弃阻塞队列中最老的任务，相当于就是队列中下一个将要被执行的任务，然后重新提交被拒绝的任务。如果阻塞队列是一个优先队列，那么“抛弃最旧的”策略将导致抛弃优先级最高的任务，因此最好不要将该策略和优先级队列放在一起使用。

CallerRunsPolicy：调用者运行策略。在调用者线程中执行该任务。该策略实现了一种调节机制，该策略既不会抛弃任务，也不会抛出异常，而是将任务回退到调用者（调用线程池执行任务的主线程），由于执行任务需要一定时间，因此主线程至少在一段时间内不能提交任务，从而使得线程池有时间来处理完正在执行的任务。




线程池核心线程空闲时怎么销毁？















多线程同步怎么做？







Java 实现线程安全的方式，每一种方式的特点

## Java 常见锁

公平锁和非公平锁的区别，优缺点

java 里的各种锁

公平锁非公平锁

可重入与不可重入锁

可中断与不可中断锁、