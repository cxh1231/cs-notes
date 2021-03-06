

# 1-4 Java 虚拟机

## 1、Java 内存（结构）

在 Java 虚拟机的自动内存管理机制下，无需手动 delete/free 操作，不容易出现内存泄露和内存溢出问题，全权交给虚拟机管理。但一旦出现内存泄露和溢出等问题，需要了解虚拟机的内存管理机制。

### 1.1 数据区域（基于JDK 1.8）

**Java 虚拟机**在执行 Java 程序的过程中会把它管理的内存划分成若干个不同的数据区域，如下图所示。

![image-20220729151844436](https://img.zxdmy.com/2022/202207291602860.png)

下面详细讲述各个内存区域。先从线程私有区域开始：

> **线程私有内存区域** 的 **生命周期和线程相同**，随着线程的创建而创建，随着线程的死亡而死亡。

#### 虚拟机栈

`Java 虚拟机栈` ，是 JVM 运行时数据区域的一个核心，除了一些 Native 方法调用是通过**本地方法栈** 实现的，**其他所有的 Java 方法调用都是通过虚拟机栈来实现的**（也需要和其他运行时数据区域比如程序计数器配合）。

**方法调用** 的数据需要通过栈进行传递，每一次方法调用都会有一个对应的 `栈帧` 被压入栈中，每一个方法调用结束后，都会有一个 `栈帧` 被弹出。

**虚拟机栈** 由一个个`栈帧`组成，而每个栈帧中都拥有：`局部变量表`、`操作数栈`、`动态链接`、`方法返回地址`。

+ **局部变量表**：存放编译期可知的**各种数据类型**（如8种基本数据类型）、**对象引用**（如指向对象起始地址的引用指针、指向一个代表对象的句柄或其他与此对象相关的位置）。
+ **操作数栈**：作为**方法调用**的中转站使用，用于存放方法执行过程中产生的中间计算结果，以及计算过程中产生的临时变量。
+ **动态链接**：服务一个方法需要调用其他方法的场景。

> 关于**动态链接**：Java 源文件被编译成字节码文件时，所有的变量和方法引用都作为 `符号引用` 保存在 Class 文件的`常量池`里。当一个方法要调用其他方法，需要将`常量池`中指向方法的`符号引用`转化为其在内存地址中的`直接引用`。**动态链接的作用就是为了将符号引用转换为调用方法的直接引用**。
>
> ![image-20220729153941829](https://img.zxdmy.com/2022/202207291539415.png)

**虚拟机栈** 和数据结构上的栈类似，也是 **先进后出** 的数据结构，只支持**出栈**和**入栈**两种操作。

![image-20220729153145253](https://img.zxdmy.com/2022/202207291531704.png)

程序运行中，**虚拟机栈** 可能会出现两种错误：

- `StackOverFlowError`： 若虚拟机栈的内存大小不允许动态扩展，那么当线程请求虚拟机栈的深度超过当前 Java 虚拟机栈的最大深度的时候，就抛出 `StackOverFlowError` 错误。
- `OutOfMemoryError`： 如果虚拟机栈的内存大小可以动态扩展， 但是虚拟机在动态扩展栈时无法申请到足够的内存空间，则抛出 `OutOfMemoryError` 异常。

#### 本地方法栈

**本地方法栈** 为虚拟机使用到的 `Native 方法` 服务。

本地方法被执行的时候，在**本地方法栈**也会创建一个`栈帧`，用于存放该本地方法的局部变量表、操作数栈、动态链接、出口信息。

方法执行完毕后相应的栈帧也会出栈并释放内存空间，也会出现 `StackOverFlowError` 和 `OutOfMemoryError` 两种错误。

>  在 `HotSpot 虚拟机` 中，**本地方法栈 和 Java 虚拟机栈 合二为一。**

#### 程序计数器

程序计数器是一块较小的内存空间，可以看作 `当前线程所执行的字节码的行号指示器`。

其作用如下：

+ `字节码解释器` 通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
+ 在 `多线程` 的情况下，程序计数器用于 **记录当前线程执行的位置**，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

> 正是为了线程切换时能恢复到正确的执行位置，所以每条线程都需要有一个独立的程序计数器，以保证各线程之间计数器互不影响，独立存储。

> 接下来是 线程共享区域：

#### 堆

**堆**，是JVM 所管理的内存中最大的一块，所有线程共享堆的空间，在虚拟机启动时创建。

**堆 的唯一目的是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。**

> 但是：所有的对象都分配到堆中，太绝对！
> 从 JDK 1.7 开始已经默认开启`逃逸分析`，如果某些方法中的`对象引用`没有被返回或者未被外面使用（也就是未逃逸出去），那么对象可以`直接在栈上分配内存`。

**Java 堆** 是 `垃圾收集器` 管理的主要区域，因此也被称作 **GC 堆（Garbage Collected Heap）**。

从**垃圾回收**的角度，由于`收集器`基本都采用`分代垃圾收集算法`，所以 **Java 堆** 还可以细分为新生代和老年代，再细致一点有：Eden、Survivor、Old 等空间。

> 进一步划分的目的是更好地回收内存，或者更快地分配内存。

在 JDK 7 版本及 JDK 7 版本之前，**堆内存** 被通常分为下面三部分：

1. 新生代内存（Young Generation）
2. 老年代内存（Old Generation）
3. 永久代内存（Permanent Generation）

**JDK 8 之后 PermGen(永久) 已被 Metaspace(元空间) 取代，元空间使用的是直接内存**。

下图所示的 `Eden` 区、两个 `Survivor` 区 S0 和 S1 都属于新生代，中间一层属于老年代，最下面一层属于永久代。

![image-20220729170939107](https://img.zxdmy.com/2022/202207291709859.png)

> 关于各个年代内存，涉及到垃圾回收算法的使用，详见后文。

程序运行时，堆 中最容易出现 `OutOfMemoryError` 错误，比如：

+ `java.lang.OutOfMemoryError: GC Overhead Limit Exceeded` ： 当 JVM 花太多时间执行垃圾回收并且只能回收很少的堆空间时，就会发生此错误。
+ `java.lang.OutOfMemoryError: Java heap space` :假如在创建新的对象时, 堆内存中的空间不足以存放新创建的对象, 就会引发此错误。
+ ……

#### 方法区（元空间实现）

**方法区** 属于是 JVM 运行时数据区域的一块逻辑区域，是各个线程共享的内存区域。

> 在不同的虚拟机实现上，方法区的实现是不同的。

当虚拟机要使用一个类时，它需要读取并解析 Class 文件获取相关信息，再将信息存入到方法区。

**方法区** 会存储已被虚拟机加载的 **类信息、字段信息、方法信息、常量、静态变量、即时编译器编译后的代码缓存等数据**。

**方法区和永久代、元空间 的关系**：`永久代`以及`元空间`是 HotSpot 虚拟机对虚拟机规范中`方法区`的两种实现方式。

+ **永久代** 是 JDK 1.8 之前方法区的实现，
+ **元空间** 是 JDK 1.8 及以后方法区的实现。

![image-20220729191012289](https://img.zxdmy.com/2022/202207291910278.png)

> **为什么要将永久代 (PermGen) 替换为元空间 (MetaSpace) 呢?**
>
> 1. 整个永久代有一个 JVM 本身设置的固定大小上限，无法进行调整，而元空间使用的是直接内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小。
> 2. 元空间里面存放的是类的元数据，这样加载多少类的元数据就不由 `MaxPermSize` 控制了, 而由系统的实际可用空间来控制，这样能加载的类就更多了。
> 3. 在 JDK8，合并 HotSpot 和 JRockit 的代码时, JRockit 从来没有一个叫永久代的东西, 合并之后就没有必要额外的设置这么一个永久代的地方了。







#### 直接内存 (非运行时数据区的一部分)





## 2、类的生命周期与加载



![image-20220729172031135](https://img.zxdmy.com/2022/202207291720677.png)







## 3、垃圾回收（GC）





## 4、JVM 参数与调优







## jvm 调优

## 什么时候会发生 GC

## 有哪些 GC 方式

## 服务器 oom 了怎么解决

##  G1 回收器和 CMS 区别

## G1 垃圾回收器有什么参数需要配置吗

## 比如 try catch 在 jvm 怎么实现的，

## GC算法：新生代、老年代