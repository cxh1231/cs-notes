## 1、Java 基础知识

### 1.1 Java语言特点

面向对象（封装，继承，多态）；

平台无关性（一次编写，到处运行）；

支持多线程；可靠性；安全性；支持网络编程并且很方便；编译与解释并存。

### 1.2 JDK  &  JVM  &  JRE 的区别

- **JVM（Java 虚拟机）**是`运行 Java 字节码的虚拟机`。JVM 有针对不同系统的特定实现，目的是使用相同的字节码，它们都会给出相同的结果。

- **JDK（Java Development Kit）** ，是`功能齐全的 Java SDK`。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。

- **JRE** 是 `Java 运行时环境`。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。

![image-20220809144931268](https://img.zxdmy.com/2022/202208091449793.png)

> 如果只是为了运行一下 Java 程序的话，那只需要安装 JRE 就可。如果需要进行一些 Java 编程方面的工作，那就需要安装 JDK 。但不绝对！

### 1.3 字节码

在 Java 中，**`JVM` 可以理解的代码就叫做字节码**（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。

之所以被称之为字节码，是因为**字节码文件由十六进制值组成**，而JVM以两个十六进制值为一组，即以字节为单位进行读取。

Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。

>  **Java 是编译与解释共存的语言** 。

### 1.4 编译与解释

- **编译型** ：`编译型语言 会通过编译器将源代码一次性翻译成可被该平台执行的机器码。`一般情况下，编译语言的执行速度比较快，开发效率比较低。常见的编译性语言有 C、C++、Go、Rust 等等。
- **解释型** ：`解释型语言 一句一句的将代码解释（interpret）为机器代码后再执行。`解释型语言开发效率比较快，执行速度比较慢。常见的解释性语言有 Python、JavaScript、PHP 等等。

Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class` 文件），这种字节码必须由 Java 解释器来解释执行。

一个 `.java` 文件从编译到运行的过程，如下图所示。

![image-20220809145137249](https://img.zxdmy.com/2022/202208091451707.png)

### 1.5 Java 与 C++ 异同点

Java 和 C++ 都是面向对象的语言，都支持封装、继承和多态，但是：

- `Java` **不提供指针**来直接访问内存，程序内存更加安全
- `Java` 的**类是单继承**的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
- `Java` 有**自动内存管理垃圾回收机制(GC)**，不需要程序员手动释放无用内存。
- C ++同时支持方法重载和操作符重载，但是 `Java` **只支持方法重载**（操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。
- `Java` **不支持操作符重载**；C++支持；
- `Java` **不支持缺省参数函数**；C++支持；
- `Java` **不支持 C++ 中的自动强制类型转换**，如果需要，必须由程序显式进行强制类型转换。

### 1.6  Oracle JDK 与 OpenJDK

+ `OpenJDK` 是一个参考模型并且是`完全开源`的，而 `Oracle JDK` 是 OpenJDK 的一个实现，并`不是完全开源`的；
+ `Oracle JDK` 比 OpenJDK `更稳定`（Oracle 内部团队进行单独研发）；
+ `Oracle JDK` 与 OpenJDK 相比提供了`更好的性能`；
+ Oracle JDK 使用 BCL/OTN 协议获得许可，而 OpenJDK 根据 GPL v2 许可获得许可。

## 2、基本语法

### 2.1 标识符与关键字

**标识符就是一个名字** ，而**关键字是被赋予特殊含义的标识符**。

> 所有的关键字都是小写的。

### 2.2 自增自减运算符

**前缀：先自增/减，再赋值**

当 `b = ++a` 时，先自增（自己增加 1），再赋值（赋值给 b）

**后缀：先赋值，再自增/减**

当 `b = a++` 时，先赋值(赋值给 b)，再自增（自己增加 1）

### 2.3 成员变量与局部变量

|        | 成员变量                                                     | 局部变量                                                    |
| :----: | ------------------------------------------------------------ | ----------------------------------------------------------- |
|  语法  | 属于类的，可以被 `public`,`private`,`static` 等修饰符所修饰，能被 `final` 所修饰 | 代码块或方法中定义的变量或方法的参数，只能被 `final` 所修饰 |
|  存储  | `static` 修饰，属于类，堆内存；否则属于实例，栈内存          | 栈内存                                                      |
|  生存  | 与对象生存周期一致                                           | 方法调用结束则消亡                                          |
| 默认值 | 以类型的默认值而赋值（ `final` 修饰的成员变量必须显式地赋值） | 不会自动赋值                                                |

### 2.4 Static 关键字

“`static`”关键字表明一个**成员变量** 或 **成员方法**可以在没有所属的类的实例变量的情况下被访问，即**在没有创建对象的情况下也可以去调用方法**。

> Java中 `static` 方法不能被覆盖。
>
> 因为**方法覆盖是基于运行时动态绑定**的，而static方法是编译时静态绑定的。

### 2.5 静态变量

`静态变量` 可以被类的所有实例共享。无论一个类创建了多少个对象，它们都共享同一份静态变量。

通常情况下，静态变量会被 `final` 关键字修饰成为常量。

### 2.6 静态方法与非静态成员

+ `静态方法`是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。

+ `非静态成员`属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。

> **问：静态方法为什么不能调用非静态成员?**
>
> 在类的`非静态成员`不存在的时候`静态成员`就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。
>
> **静态方法可以调用静态资源。**

### 2.7 静态方法与实例方法

**静态方法：**

+ 在外部被调用时，可以使用 `类名.方法名` 和 `对象.方法名` 两种方式（但推荐用前者）；
+ 在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员。

**实例方法：**

+ 被调用时，需要先实例化（创建对象），使用 `对象.方法名` 的方式来调用；
+ 在访问本类的成员时，没有任何限制。

### 2.8 重载与重写

![image-20220809163417776](https://img.zxdmy.com/2022/202208091634574.png)

#### 重载

指的是同样的一个方法，能够根据输入数据的不同，做出不同的处理，如：

```java
StringBuilder sb = new StringBuilder();
StringBuilder sb2 = new StringBuilder("HelloWorld");
```

即：`同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理`。

> 不能根据返回值类型来区分重载的方法。

#### 重写

重写发生在运行期，是 `子类对父类的允许访问的方法的实现过程进行重新编写`。

+ 方法名、参数列表必须相同；
+ 子类方法`返回值`类型应比父类方法返回值类型更小（即可以是引用类型的子类）或相等；
+ 抛出的异常范围`小于等于`父类；
+ 访问修饰符范围`大于等于`父类；
+ 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 `static` 修饰的方法能够被再次声明；
+ 构造方法无法被重写。

即：`重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变。`

### 2.9 switch 语法

Java5 以前 `switch(expr)` 中，expr 只能是 `byte`、`short`、`int`、`char`。

从 Java 5 开始，Java 中引入了枚举类型， expr 也可以是 `enum` 类型。

从 Java 7 开始，expr还可以是字符串(`String`)。

**但是长整型(long)在目前所有的版本中都是不可以的。**

### 2.10 访问修饰符public、private、protected

- `public` ：**对所有类可见**。使用对象：类、接口、变量、方法
- `protected` ：**对同一包内的类和所有子类可见**。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。
- `default` (即默认，什么也不写）: **在同一包内可见**，不使用任何修饰符。使用对象：类、接口、变量、方法。
- `private` ：**在同一类内可见**。使用对象：变量、方法。 **注意：不能修饰类（外部类）**

![image-20220809154320321](https://img.zxdmy.com/2022/202208091543673.png)

### 2.11 代码块的执行顺序

Java 中的代码块包含以下三种类型。

1. **静态代码块**（静态快、静态初始化块）：在类加载 JVM 时运行；
2. **构造代码块**（构造初始化块）：创建对象时被调用，在类中定义；
3. **普通代码块**（代码块、初始化块）：在方法体中定义。

代码块执行顺序： **静态代码块 —> 构造代码块 —> 构造函数—> 普通代码块**，示例如下：

User.java：

```java
public class User {

   public String name;

   // 构造代码块
   {
      name = "张三";
      System.out.println("User：构造代码块");
   }

   // 无参构造代码块
   public User() {
      System.out.println("User：无参构造代码块");
   }

   // 有参构造代码块
   public User(String name) {
      this.name = name;
      System.out.println("User：有参构造代码块");
   }

   // 静态代码块
   static {
      System.out.println("User：静态代码块");
   }

   // 普通代码块
   public String getName() {
      return name;
   }
}
```

执行结果：

```java
public class Main {
   public static void main(String[] args) {
      User user = new User("用户李四");
      System.out.println(user.getName());

      System.out.println("----------");

      User user2 = new User();
      System.out.println(user2.getName());
   }
}
/**
 User：静态代码块
 User：构造代码块
 User：有参构造代码块
 用户李四
 ----------
 User：构造代码块
 User：无参构造代码块
 张三
 */
```

继承中代码块执行顺序：**父类静态块 —> 子类静态块 —> 父类代码块 —> 父类构造器 —> 子类代码块 —> 子类构造器**，示例如下：

父类是上面的User，子类为Student.java：

```java
public class Student extends User {

   // 构造代码块
   {
      name = "李四";
      System.out.println("Student：构造代码块");
   }

   // 无参构造代码块
   public Student() {
      System.out.println("Student：无参构造代码块");
   }

   // 有参构造代码块
   public Student(String name) {
      System.out.println("Student：有参构造代码块");
   }


   // 静态代码块
   static {
      System.out.println("Student：静态代码块");
   }
}
```

执行结果：

```java
public class Main {
   public static void main(String[] args) {
      Student student = new Student("学生王五");
      System.out.println(student.getName());

      System.out.println("----------");

      Student student2 = new Student();
      System.out.println(student2.getName());
   }
}
/**
 User：静态代码块
 Student：静态代码块
 User：构造代码块
 User：无参构造代码块
 Student：构造代码块
 Student：有参构造代码块
 学生王五
 ----------
 User：构造代码块
 User：无参构造代码块
 Student：构造代码块
 Student：无参构造代码块
 李四
 */
```

## 3、数据类型

Java 语言的数据类型分为两种：`基本数据类型`和`引用数据类型`。

![img](https://img.zxdmy.com/2022/202208091535838.png)

### 3.1 八种基本数据类型

- 4 种整数型：`byte`、`short`、`int`、`long`
- 2 种浮点型：`float`、`double`
- 1 种字符类型：`char`
- 1 种布尔型：`boolean`

| 基本类型  | 位数 | 字节 | 默认值  | 取值范围                                   |
| :-------: | :--: | :--: | :-----: | ------------------------------------------ |
|  `byte`   |  8   |  1   |    0    | -128 ~ 127                                 |
|  `short`  |  16  |  2   |    0    | -32768 ~ 32767                             |
|   `int`   |  32  |  4   |    0    | -2147483648 ~ 2147483647                   |
|  `long`   |  64  |  8   |   0L    | -9223372036854775808 ~ 9223372036854775807 |
|  `char`   |  16  |  2   | 'u0000' | 0 ~ 65535                                  |
|  `float`  |  32  |  4   |   0f    | 1.4E-45 ~ 3.4028235E38                     |
| `double`  |  64  |  8   |   0d    | 4.9E-324 ~ 1.7976931348623157E308          |
| `boolean` |  1   |      |  false  | true、false                                |

>  `boolean`的 字节数，官方文档未明确定义。
>
>  `long` 类型的数据要在数值后面加 **L** 。

这**八种基本类型**都有对应的包装类分别为：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 。

> Integer 的大小：16 字节（含int 4 字节，其他是指针等信息）。

### 3.2 基本类型和包装类型的区别

- 包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。
- 包装类型可用于泛型，而基本类型不可以。
- 基本数据类型的**局部变量存放在 Java 虚拟机栈中的局部变量表中**，基本数据类型的**成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中**。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。
- 相比于对象类型， 基本数据类型占用的空间非常小。

### 3.3 包装类型的缓存机制（常量池）与比较

Java 基本类型的包装类的大部分都实现了常量池技术。

`Byte`、`Short`、`Integer`、`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。

所以 `==` 比较大小，有问题，强制使用 `equals` 。

```java
Integer a = 33;
        Integer b1 = 33;
        Integer b2 = new Integer(33);
        System.out.println(a == b1); // true
        System.out.println(a == b2); // false
        Integer c = 1024;
        Integer d = 1024;
        System.out.println(c == d); // false
        System.out.println(c.equals(d)); // true

        Float i1 = 333f;
        Float i2 = 333f;
        System.out.println(i1 == i2); // false

        Double i3 = 1.2;
        Double i4 = 1.2;
        System.out.println(i3 == i4); // false
```

下图是阿里巴巴Java开发手册给出的说明。

![image-20220728142703256](https://img.zxdmy.com/2022/202208101056842.png)

### 3.4 自动装箱与拆箱

- **装箱**：将`基本类型`用它们对应的`引用类型`包装起来；调用了包装类的`valueOf()`方法，
- **拆箱**：将`包装类型`转换为`基本数据类型`；调用了 `xxxValue()`方法。

```java
Integer i = 10; //装箱
        int n = i;   	//拆箱
```

即：

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`
- `int n = i` 等价于 `int n = i.intValue()`;

> 频繁拆装箱会严重影响系统的性能，应该尽量避免不必要的拆装箱操作。

### 3.5 浮点运算精度问题

由于计算机保存浮点数的机制问题，其使用二进制保存数字，宽度有限，无限循环小数只能被截断，所以导致小数精度丢失。

示例：

```java
float a = 2.0f - 1.9f;
float b = 1.8f - 1.7f;
System.out.println(a);		// 0.100000024
System.out.println(b);		// 0.099999905
System.out.println(a == b);	// false
```

解决精度丢失问题，可以使用`BigDecimal` 实现对浮点数的运算，不会造成精度丢失。

示例：

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
BigDecimal c = new BigDecimal("0.8");

BigDecimal x = a.subtract(b);
BigDecimal y = b.subtract(c);

System.out.println(x); /* 0.1 */
System.out.println(y); /* 0.1 */
System.out.println(Objects.equals(x, y)); /* true */
```

> BigDecimal 传入的值，建议使用 String 类型。

### 3.6 大数表示

超过 `Long.MAX_VALUE` 的数，可以使用 `BigInteger` 来表示运算。但运算效率相对较低。

> `BigInteger` 内部使用 `int[]` 数组来存储任意大小的整形数据。

## 4、Java 常用对象（类）

### 4.1 Object 类

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```java
// native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
public final native Class<?> getClass();

// native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
public native int hashCode();

// 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
public boolean equals(Object obj);

// naitive 方法，用于创建并返回当前对象的一份拷贝。
protected native Object clone() throws CloneNotSupportedException;

//返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
public String toString();

// native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
public final native void notify();

// native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
public final native void notifyAll();

// native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
public final native void wait(long timeout) throws InterruptedException;

// 多了 nanos 参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 毫秒。。
public final void wait(long timeout, int nanos) throws InterruptedException;

// 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
public final void wait() throws InterruptedException;

// 实例被垃圾回收器回收的时候触发的操作
protected void finalize() throws Throwable { };
```

#### hashcode()

`hashCode()` 的作用是获取哈希码，也称为散列码；它实际上是返回一个int整数。

这个哈希码的作用是确定该对象在哈希表中的索引位置。

####  equals() 和 ==

**`==`** 对于基本类型和引用类型的作用效果是不同的：

- 对于基本数据类型来说，`==` 比较的是值。
- 对于引用数据类型来说，`==` 比较的是对象的内存地址。

> 因为 Java 只有 `值传递`，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是 `引用类型变量存的值是对象的地址`。

`equals()` 不能用于判断**基本数据类型**的变量，只能用来判断两个对象是否相等。

`equals()` 方法存在两种使用情况：

- **类没有重写 `equals()`方法** ：通过`equals()`比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 `Object`类`equals()`方法。
- **类重写了 `equals()`方法** ：一般都重写 `equals()`方法来比较两个对象中的**属性**是否相等；若它们的**属性相等**，则返回 true(即，认为这两个对象相等)。

比如，String 类就对 equals() 方法进行了重写，以判断字符串的对象的值是否相同。

#### hashCode()与equals()的关系

- 如果两个对象相等，则`hashcode`一定也是相同的；
- 如果两个对象相等，对两个对象分别调用`equals`方法都返回`true`；
- 如果两个对象有相同的`hashcode`值，它们也不一定是相等的。

#### 重写 equals 方法必须重写 hashcode 方法

判断的时候先根据hashcode进行的判断，相同的情况下再根据equals()方法进行判断。如果只重写了equals方法，而不重写hashcode的方法，会造成hashcode的值不同，而equals()方法判断出来的结果为true。

在Java中的一些容器中，不允许有两个完全相同的对象，插入的时候，如果判断相同则会进行覆盖。

这时候如果只重写了equals 的方法，而不重写hashcode的方法，Object中hashcode是根据对象的存储地址转换而形成的一个哈希值。

这时候就有可能因为没有重写hashcode方法，造成相同的对象散列到不同的位置而造成对象的不能覆盖的问题。

### 4.2 String 类

#### String 不可变

**为什么不可变**：

+ `String` 类中使用 `final` 关键字修饰字符数组来保存字符串，导致其不能被继承，进而避免了子类破坏 `String` 不可变。所以线程安全。
+ 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。

**为什么设计成不可变**：

1. 便于实现字符串池（String pool）
2. 使多线程安全
3. 避免安全问题
4. 加快字符串处理速度

#### 字符串常量池

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

示例如下：

```java
// 在堆中创建字符串对象”ab“
// 将字符串对象”ab“的引用保存在字符串常量池中
String a = "ab";
// 直接返回字符串常量池中字符串对象”ab“的引用
String b = "ab";
System.out.println(a == b);// true
```

#### 字符串对象创建

`String s = new String("abc")` 会创建几个字符串对象？会创建 1 或 2 个字符串对象。

1. 如果字符串常量池中**不存在**字符串对象“abc”的引用，那么会在堆中创建 2 个字符串对象“abc”。

```java
String s = new String("abc");
```

1. 如果字符串常量池中**已存在**字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。

```java
// 字符串常量池中已存在字符串对象“abc”的引用
String s1 = "abc";
// 下面这段代码只会在堆中创建 1 个字符串对象“abc”
String s = new String("abc");
```

#### String.intern() 方法

`String.intern()` 是一个 native（本地）方法，其作用是**将指定的字符串对象的引用保存在字符串常量池中**，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中**创建一个指向该字符串对象的引用并返回**。

示例：

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```

#### 常量折叠

对于编译器可以确定值的字符串，也就是**常量字符串** ，jvm 会将其存入字符串常量池。并且，**字符串常量拼接得到的字符串常量**在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。

比如，对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 。

> 并不是所有的常量都会进行折叠，**只有编译器在程序编译期就可以确定值的常量才可以**：
>
> - 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量。
> - `final` 修饰的基本数据类型和字符串变量
> - 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、>>、>>> ）

#### String 的 + 运算

Java 语言本身并不支持运算符重载，`+` 和 `+=` 是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

对于示例代码：

```java
String str1 = "he";
String str2 = "llo";
String str3 = "world";
String str4 = str1 + str2 + str3;
```

**字符串对象** 通过 `+` 的形式拼接字符串，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。

即：

```java
String str4 = new StringBuilder().append(str1).append(str2).toString();
```

> 注意：在循环内使用 `+` 或 `+=` 进行字符串拼接，编译器会创建多个 `StringBuilder` 对象（即在循环内创建）。

#### String.equals() 方法与字符串比较

`String` 中的 `equals` 方法是被重写过的，比较的是 String 字符串的值是否相等。

`Object` 的 `equals` 方法是比较的对象的内存地址。

对于字符串的各种创建与比较，直接上代码，详见如下示例。

**示例：常量与常量比较**

```java
String a = "ab"; // 放在常量池中
String b = "ab"; // 从常量池中查找
System.out.println(a == b); // true
System.out.println(a.equals(b)); // true
```

**示例：引用与引用比较**

```java
String a = new String("ab"); // a 为一个引用
String b = new String("ab"); // b为另一个引用,对象的内容一样
System.out.println(a == b); // false，比较的是内存地址
System.out.println(a.equals(b)); // true，比较的是对象的值
```

**示例：常量与引用的比较**

```java
String a = "ab"; // 放在常量池中
String b = new String("ab");
System.out.println(a == b); // false
System.out.println(a.equals(b)); // true
```

**示例：+ 运算后比较**

```java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";	// 常量池
String str4 = str1 + str2;		// 创建了新的对象
System.out.println(str3 == str4); //false

String str5 = "string";
System.out.println(str3 == str5); //true
System.out.println(str4 == str5); //false
```

**示例：final 修饰后比较**

```java
final String str1 = "str";
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2;  // 常量池中的对象
System.out.println(c == d); // true
```

> 被 `final` 关键字修改之后的 `String` 会被编译器当做常量来处理，编译器在程序编译期就可以确定它的值，其效果就相当于访问常量。

**示例：运行时才确定的值的比较**

```java
final String str1 = "str";    	// 编译时确定
final String str2 = getStr();	// 运行时确定

String c = "str" + "ing";	// 常量池中的对象
String d = str1 + str2; 	// 在堆上创建的新的对象
System.out.println(c == d);	// false

public static String getStr() {
      return "ing";
}
```

**示例：intern() 方法后获取的引用的比较**

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();

// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true

// 在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();

// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false

// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```



#### String、StringBuffer、StringBuilder 的区别

可变性：

+ `String` 是不可变的。
+ `StringBuilder` 与 `StringBuffer` 没有使用 `final` 和 `private` 关键字修饰，提供了修改字符串的方法比如 `append` 。

线程安全性：

+ `String` 中的对象是不可变的，也就可以理解为常量，线程安全。
+ `StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。
+ `StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

性能：

+ 对 `String` 类型进行改变的时候，会生成新的 `String` 对象，然后将指针指向新的 `String` 对象。
+ `StringBuffer` 会对 `StringBuffer` 对象本身进行操作，而不生成新的对象并改变对象引用。
+ 相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

**对于三者使用的总结：**

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



## 5、面向对象

### 5.1 面向对象与面向过程

- `面向过程` 把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。比如单片机、嵌入式开发一般采用面向过程开发，性能比较高。
- `面向对象` 会先抽象出对象，然后用对象执行方法的方式解决问题。有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护。易维护、易复用、易扩展。但性能比面向过程低。

### 5.2 面向对象三大特征

#### 封装

**封装** : 把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。

+ 比如`实体类`。

#### 继承

**继承** : 不同类型的对象，相互之间经常有一定数量的共同点。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。

+ 子类拥有父类对象所有的属性和方法，但无法访问，子类的对象可以访问。
+ 子类可以拥有自己属性和方法；
+ 子类可以用自己的方式实现父类的方法。

示例：

```java
// 父类
public class Animal { 
    private String name;  
    private int id; 
    // 构造方法
    public Animal(String myName, int myid) { 
        name = myName; 
        id = myid;
    } 
    public void eat(){ 
        System.out.println(name+"正在吃"); 
    }
    public void sleep() {
		System.out.println(name + "正在睡觉");
	}
}
// 子类
public class Mouse extends Animal { 
    // 继承父类的构造方法
    public Mouse(String myName, int myid) { 
        super(myName, myid); 
    }
}
// 使用
public static void main(String[] args) {
	Animal mouse = new Mouse("张三", 0);
    // 或：Mouse mouse = new Mouse("张三", 0);
	mouse.eat();
    mouse.sleep();
}
// 结果
张三正在吃
张三正在睡觉
```

#### 多态

**多态** : 表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。

+ `多态是方法的多态`，属性没有多态；
+ `方法可重写`，不能实现重写的也就没有多态可言；
+ 父类引用指向子类对象：`Parent p = new Child();`

> 即三个必要条件：继承、重写和向上转型

示例：

```java
//父类
class Shape {
	void draw() {
	}
}
//继承
class Circle extends Shape {
	// 多态：重写父类方法
	void draw() {
		System.out.println("Circle.draw()");
	}
}
//继承
class Square extends Shape {
	// 多态：重写父类方法
	void draw() {
		System.out.println("Square.draw()");
	}
}
// 使用
Shape shape = new Circle();
shape.draw(); // 输出：Circle.draw()
Shape shape2 = new Square();
shape2.draw(); // 输出：Square.draw()
Circle circle = new Circle();
circle.draw(); // 输出：Circle.draw()
```

多态又分为 **编译时多态（又称静态多态）** 和 **运行时多态（又称动态多态）**。

**重载（overload）就是编译时多态**的一个例子，编译时多态在编译时就已经确定，运行的时候调用的是确定的方法。

**通常所说的多态指的都是运行时多态，也就是编译时不确定究竟调用哪个具体方法，一直延迟到运行时才能确定。**

### 5.3 对象

#### 简介

**对象**是类的一个实例，有状态和行为。

> 对象实例在堆内存中，对象引用存放在栈内存中。

一个对象引用可以指向 0 个或 1 个对象，一个对象可以有 n 个引用指向它。

- **对象相等** 一般比较的是内存中存放的`内容`是否相等。
- **引用相等** 一般比较的是他们指向的`内存地址`是否相等。

如表达式 `A a1 = new A();` ：

+ `A` 是类；
+ `a1` 是对象引用，a1不是对象；
+ `new A()` 是对象，a1引用 指向 new A() 这个对象。

> 一个类的实例从new 开始的过程？TODO

#### 创建

java中提供了以下四种创建对象的方式:

- `new` 创建新对象：创建`对象实例`，`对象引用` 指向对象实例。
- 通过`反射`机制
- 采用 `clone` 机制
- 通过 `序列化` 机制

#### 不可变对象

**不可变对象** 指对象一旦被创建，状态就不能再改变，任何修改都会创建一个新的对象。

如 `String`、`Integer`及其它包装类就是不可变对象。

**不可变对象最大的好处是线程安全**。

> 可以创建一个包含可变对象的不可变对象。
>
> 比如`final Person[] persons = new Persion[]{}`，`persons`是不可变对象的引用，但其数组中的Person实例却是可变的。
>
> 这种情况下需要特别谨慎，不要共享可变对象的引用。
>
> 这种情况下，如果数据需要变化时，就返回原对象的一个拷贝。

### 5.4 构造方法

**构造方法** 是一种特殊的方法，主要作用是 **完成对象的初始化工作**。

- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 生成类的对象时自动执行，无需调用。

如果类不写构造方法，则Java默认有一个`无参构造方法`。如果写了构造方法，则Java不再默认添加无参构造方法。

**构造方法不能被 override（重写），但是可以 overload（重载）**。

> 推荐：如果重载了有参的构造方法，要把无参的构造方法也写出来。

### 5.5 接口和抽象类

**共同点** ：

- 都不能被实例化。
- 都可以包含抽象方法。
- 都可以有默认实现的方法（Java 8 可以用 `default` 关键字在接口中定义默认方法）。

**区别** ：

- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系（比如说我们抽象了一个发送短信的抽象类，）。
- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。

### 5.6 引用拷贝、深拷贝与浅拷贝

- **引用拷贝**：引用拷贝就是两个不同的引用指向同一个对象。
- **浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说`拷贝对象和原对象共用同一个内部对象`。
- **深拷贝** ：深拷贝会`完全复制`整个对象，包括这个对象所包含的内部对象。

![image-20220729110217967](https://img.zxdmy.com/2022/202207291102912.png)

### 5.7 方法调用的值传递与引用传递

+ **值传递**：指的是在方法调用时，**传递的参数是按值的拷贝传递**，传递的是值的拷贝，也就是说传递后就互不相关了。
+ **引用传递**：指的是在方法调用时，传递的参数是按引用进行传递，其实**传递的是引用的地址**，也就是变量所对应的内存空间的地址。传递的是值的引用，也就是说传递前和传递后都指向同一个引用（也就是同一个内存空间）。

**Java 中只有值传递**。

基本类型作为参数被传递时肯定是值传递；

引用类型作为参数被传递时也是值传递，只不过“值”为对应的引用，并不是对象本身。

## 6、泛型

### 6.1 泛型与类型擦除

#### 泛型

**Java 泛型（Generics）** 是 JDK 5 中引入的一个新特性。使用泛型参数，可以增强代码的可读性以及稳定性。

**泛型就是将类型参数化，其在编译时才确定具体的参数。**

**泛型只存在于编译阶段，而不存在于运行阶段。**

#### 类型擦除

Java 的泛型是伪泛型，这是因为 Java 在运行期间，所有的泛型信息都会被擦掉，这也就是通常所说 **类型擦除** 。

使用泛型的时候加上的类型参数，编译器在编译的时候去掉类型参数。

### 6.2 泛型的使用方式

#### 泛型类

泛型类在实例化时，必须指定 `T` 的具体类型。

定义与使用：

```java
// 定义泛型类
public class Generic<T>{

    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}
// 使用
Generic<Integer> genericInteger = new Generic<Integer>(123456);
```

#### 泛型接口

定义与使用：

```java
// 定义
public interface Generator<T> {
    public T method();
}
// 实现泛型接口，不指定类型：
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}
// 实现泛型接口，指定类型：
class GeneratorImpl<T> implements Generator<String>{
    @Override
    public String method() {
        return "hello";
    }
}
```

#### 泛型方法

定义与使用：

```java
// 定义
public static <E> void printArray(E[] inputArray) {
	for (E element : inputArray) {
		System.out.printf("%s ", element);
	}
	System.out.println();
}
// 使用
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray(intArray);
printArray(stringArray);
```

### 6.3 泛型的好处

**类型安全**：

- 泛型的主要目标是提高 Java 程序的类型安全
- 编译时期就可以检查出因 Java 类型不正确导致的 ClassCastException 异常
- 符合越早出错代价越小原则

**消除强制类型转换**：

- 泛型的一个附带好处是，使用时直接得到目标类型，消除许多强制类型转换
- 所得即所需，这使得代码更加可读，并且减少了出错机会

**潜在的性能收益**：

- 由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改
- 所有工作都在编译器中完成
- 编译器生成的代码跟不使用泛型（和强制类型转换）时所写的代码几乎一致，只是更能确保类型安全而已

### 6.4 泛型的限定通配符与非限定通配符

#### 限定通配符

**限定通配符** 对类型进行了限制。

有两种限定通配符：

1. `<? extends T>`，其**类型必须是 `T` 的子类来设定类型的上界**，即可以接受任何继承自 T 的类型；
2. `<? super T>`，其**类型必须是 `T` 的父类来设定类型的下界**，即可以接受任何 T 的父类的类型。

泛型类型必须用限定内的类型来进行初始化，否则会导致编译错误。

#### 非限定通配符

**非限定通配符** `?` ，可以用任意类型来替代。

如`List<?>` 的意思是这个集合是一个可以持有任意类型的集合，它可以是`List<A>`，也可以是`List<B>`,或者`List<C>`。

> 不可以把 `List<String>` 传递给一个接受 `List<Object>` 参数的方法。

> 不可以在 `Array` 中使用泛型，而经过使用 `List` 代替。

## 7、反射

**反射** 赋予了框架在`运行时分析类以及执行类中方法的能力`，即通过反射可以获取任何一个类的所有属性和方法，进而调用这些方法和属性，是框架的灵魂。

### 7.1 反射机制优缺点

- **优点** ：可以让代码更加灵活，为各种框架的开箱即用功能提供了便利;
- **缺点** ：在运行时有了分析操作类的能力，这同样也 `增加了安全问题`。比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

### 7.2 反射的使用示例

**可以通过如下方法获取反射中的 Class 对象。**

+ `Class.forName(“类的路径”)`：知道该类的全路径名，可使用此方法

```java
Class clz = Class.forName("java.lang.String");
```

+ `类名.class`：在编译前就知道操作的 Class

```java
Class clz = String.class;
```

+ `对象名.getClass()`

```java
String str = new String("Hello");
Class clz = str.getClass();
```

+ 如果是基本类型的包装类，可以调用包装类的`Type属性`来获得该包装类的Class对象。

**下面是一个完整的示例：**

```java
import java.lang.reflect.Method;

public class Book {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public static void main(String[] args) {
        // 正常调用
        Book book = new Book();
        book.setName("西游记");
        System.out.println(book.getName());

        // 使用反射调用
        try {
            // 1. 获取类的 Class 对象实例
            Class<?> clazz = Class.forName("test2.Book");
            // 2. 使用对象的 newInstance 方法获取反射类对象
            Object bookObj = clazz.newInstance();
            // 3. 获取[setName]方法的 Method 对象
            Method setPriceMethod = clazz.getMethod("setName", String.class);
            // 4. 利用 invoke 方法调用[setName]方法
            setPriceMethod.invoke(bookObj, "三国演义");
            // 5. 获取[getName]方法的 Method 对象，并通过 invoke 方法调用[getName]方法
            System.out.println(clazz.getMethod("getName").invoke(bookObj));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 7.3 反射的应用场景

业务代码一般用不到反射机制。

+ 像 Spring/Spring Boot、MyBatis 等，**这些框架中大量使用了动态代理，而动态代理的实现也依赖反射。**

+ **注解** 的实现也用到了反射。基于反射分析类，获取到类/属性/方法/方法的参数上的注解，然后做进一步的处理。

- 反射让开发人员可以**通过外部类的全路径名创建对象**，并使用这些类，实现一些扩展的功能。
- 反射让开发人员可以**枚举出类的全部成员**，包括构造函数、属性、方法。以帮助开发者写出正确的代码。
- 测试时可以**利用反射 API 访问类的私有成员**，以保证测试代码覆盖率。

#### 场景1：JDBC 的数据库的连接

在JDBC 的操作中，第一步就是通过 `Class.forName()` 加载数据库的驱动程序 （通过反射加载，前提是引入相关了Jar包）

```java
 Class.forName("com.mysql.jdbc.Driver"); // 使用CLASS 类加载驱动程序 ,反射机制的体现 
```

#### 场景2：Spring 框架的 xml 配置模式

Spring 通过 XML 配置模式装载 Bean 的过程中，通过对 XML 内容进行解析，得到对应实体类的字节码字符串，以及相关的属性信息，然后**使用反射机制，根据这个字符串获得某个类的Class实例**，进而动态配置实例的属性。



## 8、注解

`Annotation` （注解） 是 Java5 开始引入的新特性，可以看作是一种特殊的注释，主要用于修饰类、方法或者变量。

> 注解本质是一个继承了`Annotation` 的特殊接口。

**注解只有被解析之后才会生效**，常见的解析方法有两种：

- **编译期直接扫描** ：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理** ：像框架中自带的注解（比如 Spring 框架的 `@Value` 、`@Component`）都是通过反射来进行处理的。

## 9、I/O 与序列化

### 9.1 序列化与反序列化

#### 简介

需要持久化 Java 对象，或在网络传输 Java 对象，就需要序列化。比如将 Java 对象保存在文件、Redis中。

即：**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**

序列化与反序列化的基本过程：

- **序列化**： 将数据结构或对象 `转换成二进制字节流` 的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流 `转换成数据结构或者对象` 的过程

> Java 序列化的都是对象（Object），也就是实例化后的类(Class)。

#### 优点

1. **对象序列化可以实现分布式对象。**
2. **java 对象序列化不仅保留一个对象的数据，而且递归保存对象引用的每个对象的数据。**即进行对象的`深复制`。
3. **序列化可以将内存中的类写入文件或数据库中。**
4. 对象、文件、数据，有许多不同的格式，很难统一传输和保存。**序列化后就变成字节流，可以进行通用的格式传输或保存**。

#### 序列化实现方式

> 实现 `Serializable` 接口或者 `Externalizable` 接口。

1、**可以通过实现 `java.io.Serializable` 接口以启用其序列化功能**

Phone.java：

```java
import java.io.Serializable;

public class Phone implements Serializable {

    private String name;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Main.java：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        // 创建 Phone 对象
        Phone phone = new Phone();
        phone.setName("华为");

        // 序列化输出至文件
        try {
            FileOutputStream fos = new FileOutputStream("phone.txt");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            oos.writeObject(phone);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 从文件中反序列化输入
        File file = new File("phone.txt");
        try {
            FileInputStream fis = new FileInputStream(file);
            ObjectInputStream ois = new ObjectInputStream(fis);
            Phone phone2 = (Phone) ois.readObject();
            System.out.println(phone2.getName()); // 输出：华为
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

2、**可以通过实现 `Externalizable` 接口实现序列化**

Computer.java：

```java
import java.io.Externalizable;
import java.io.IOException;
import java.io.ObjectInput;
import java.io.ObjectOutput;

public class Computer implements Externalizable {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        out.writeObject(name);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        name = (String) in.readObject();
    }
}
```

Main.java：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        // 创建Computer对象
        Computer computer = new Computer();
        computer.setName("联想");

        // 序列化输出至文件
        try {
            FileOutputStream fos = new FileOutputStream("computer.txt");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            oos.writeObject(computer);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 从文件中反序列化输入
        File file = new File("computer.txt");
        try {
            FileInputStream fis = new FileInputStream(file);
            ObjectInputStream ois = new ObjectInputStream(fis);
            Computer computer2 = (Computer) ois.readObject();
            System.out.println(computer2.getName()); // 输出：联想
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

**两种序列化的对比**

|                     实现Serializable接口                     |  实现Externalizable接口  |
| :----------------------------------------------------------: | :----------------------: |
|                    系统自动存储必要的信息                    |  程序员决定存储哪些信息  |
| Java内建支持，易于实现，只需要实现该接口即可，无需任何代码支持 | 必须实现接口内的两个方法 |
|                           性能略差                           |         性能略好         |

#### serialVersionUID

Java 的序列化机制是通过 **在运行时判断类的`serialVersionUID`来验证版本一致性** 的。

在进行反序列化时，JVM会把传来的字节流中的 `serialVersionUID` 与本地相应实体（类）的 `serialVersionUID` 进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。

**显示指定 serialVersionUID 值的原因**：

1. 若不指定，则 `JVM` 在序列化时会根据属性自动生成一个`serialVersionUID`。当反序列化时，JVM 会再根据属性自动生成一个**新版** `serialVersionUID`。这个`新版serialVersionUID`与序列化时生成的`旧版serialVersionUID`进行比较。
   1. 如果序列化的这个类不做任何修改，那新旧`serialVersionUID`一致，没有问题；
   2. 但是，如果这个类进行了迭代更新，新旧`serialVersionUID`将不致，旧对象反序列化就会报错。
2. 如果显示指定，JVM在序列化和反序列化时仍然都会生成一个serialVersionUID，但**值为我们显示指定的值**，这样在反序列化时新旧版本的serialVersionUID就一致了。

![image-20220809202546014](https://img.zxdmy.com/2022/202208092025532.png)

### 9.2  transient  关键字（阻止序列化）

对于不想进行序列化的变量，使用 `transient` 关键字修饰。

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化，即 **静态变量不会被序列化**。

### 9.3 Java 中 IO 流的分类

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

![image-20220809203111696](https://img.zxdmy.com/2022/202208092031968.png)

![image-20220729145125664](https://img.zxdmy.com/2022/202207291451500.png)

### 9.4 既然有了字节流，为什么还要有字符流？

> 不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？

字符流是由 Java 虚拟机将字节转换得到的，问题就出在这个过程还算是非常耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。

### 9.4 三种常见 IO 模型：BIO、NIO、AIO

#### BIO

**同步阻塞IO**：**同步并阻塞**，在服务器中实现的模式为**一个连接一个线程**。也就是说，客户端有连接请求的时候，服务器就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然这也可以通过线程池机制改善。BIO**一般适用于连接数目小且固定的架构**，这种方式对于服务器资源要求比较高，而且并发局限于应用中，是JDK1.4之前的唯一选择，但好在程序直观简单，易理解。

#### NIO

**多路复用IO**：**同步并非阻塞**，在服务器中实现的模式为**一个请求一个线程**，也就是说，客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到有连接IO请求时才会启动一个线程进行处理。**NIO一般适用于连接数目多且连接比较短（轻操作）的架构**，并发局限于应用中，编程比较复杂，从JDK1.4开始支持。

#### AIO

**异步IO**：异步并非阻塞，在服务器中实现的模式为**一个有效请求一个线程**，也就是说，客户端的IO请求都是通过操作系统先完成之后，再通知服务器应用去启动线程进行处理。AIO一般适用于连接数目多且连接比较长（重操作）的架构，充分调用操作系统参与并发操作，编程比较复杂，从JDK1.7开始支持。

## 10、异常

### 10.1 异常类别

![image-20220729145309400](https://img.zxdmy.com/2022/202207291453708.png)

![image-20220809202657051](https://img.zxdmy.com/2022/202208092026349.png)

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

**`Exception`** ：程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 ：

- Checked Exception (受检查异常，必须处理) ，
- Unchecked Exception (不受检查异常，可以不处理)。

**`Error`** ：程序无法处理的错误。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止：

- Java 虚拟机运行错误（`Virtual MachineError`），
- 虚拟机内存不够错误(`OutOfMemoryError`)，
- 类定义错误（`NoClassDefFoundError`）等 。

### 10.2 受检查异常与不受检查异常

#### 受检查异常（一般异常）

`Checked Exception` 即 **受检查异常（一般异常）** 。Java 代码在编译过程中，如果**受检查异常**没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。比如：

-  IO 相关的异常

-  `ClassNotFoundException`

-  `SQLException` ...

#### 不受检查异常（运行时异常）

`Unchecked Exception` 即 **不受检查异常（运行时异常）** ，Java 代码在编译过程中 ，即使不处理`不受检查异常`也可以正常通过编译。比如：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ArithmeticException`（算术错误）……

### 10.3 try-catch-finally

- `try` 块：用于捕获异常。其后可接多个 `catch` 块，若没有 `catch` 块，则必须接一个 `finally` 块。
- `catch` 块：用于处理 `try` 捕获到的异常。
- `finally` 块：无论是否捕获或处理异常，`finally` 块里的语句都会被执行。
   - 当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。

#### 不要在 finally 语句块中使用 return

因为：try 语句遇到 return 时，其返回值会暂存本地变量中，然后执行 finally 语句，然后由于遇到了 finally 中的 return，就执行该 return，进而忽略了 try 中的 return，变为了 finally 语句中的 return 返回值。

示例：

```java
public static int getInt() {
    int a = 10;
    try {
        System.out.println(a / 0);
        a = 20;
    } catch (ArithmeticException e) {
        a = 30;
        return a;
    } finally {
        a = 400;
        //如果这样，就又重新形成了一条返回路径，由于只能通过一个 return 返回，所以这里直接返回 400
        return a; 
    }
}
// 返回结果：400
```

### 10.4 finally 的代码一定会执行吗？

不一定，有以下几种情况，不会执行：

-  `finally` 之前虚拟机被终止运行（`System.exit(1);`）

-  程序所在的线程死亡

-  关闭 CPU

### 10.5 异常使用注意事项

+ 不要把异常定义为静态变量，因为这样会导致异常栈信息错乱。
+ 抛出的异常信息一定要有意义。
+ 抛出更加具体的异常
+ 使用日志打印异常之后就不要再抛出异常

