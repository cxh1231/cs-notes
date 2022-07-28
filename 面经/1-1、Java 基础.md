## 1、Java 基础知识

### 1.1 Java语言特点

面向对象（封装，继承，多态）；平台无关性；支持多线程；可靠性；安全性；支持网络编程并且很方便；编译与解释并存；

### 1.2 JDK  &  JVM  &  JRE 的区别

- **JVM（Java 虚拟机）**是`运行 Java 字节码的虚拟机`。JVM 有针对不同系统的特定实现，目的是使用相同的字节码，它们都会给出相同的结果。

- **JDK（Java Development Kit）** ，是`功能齐全的 Java SDK`。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。

- **JRE** 是 `Java 运行时环境`。它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。

> 如果只是为了运行一下 Java 程序的话，那只需要安装 JRE 就可。如果需要进行一些 Java 编程方面的工作，那就需要安装 JDK 。但不绝对！

### 1.3 字节码

在 Java 中，**`JVM` 可以理解的代码就叫做字节码**（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。

Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。

>  **Java 是编译与解释共存的语言** 。

### 1.4 编译与解释

- **编译型** ：`编译型语言 会通过编译器将源代码一次性翻译成可被该平台执行的机器码。`一般情况下，编译语言的执行速度比较快，开发效率比较低。常见的编译性语言有 C、C++、Go、Rust 等等。
- **解释型** ：`解释型语言 一句一句的将代码解释（interpret）为机器代码后再执行。`解释型语言开发效率比较快，执行速度比较慢。常见的解释性语言有 Python、JavaScript、PHP 等等。

Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class` 文件），这种字节码必须由 Java 解释器来解释执行。

### 1.5 Java 与 C++ 异同点

Java 和 C++ 都是面向对象的语言，都支持封装、继承和多态，但是：

- Java 不提供指针来直接访问内存，程序内存更加安全
- Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
- Java 有自动内存管理垃圾回收机制(GC)，不需要程序员手动释放无用内存。
- C ++同时支持方法重载和操作符重载，但是 Java 只支持方法重载（操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。

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

### 2.4 静态变量

`静态变量`可以被类的所有实例共享。无论一个类创建了多少个对象，它们都共享同一份静态变量。

通常情况下，静态变量会被 `final` 关键字修饰成为常量。

### 2.5 静态方法与非静态成员

+ `静态方法`是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。

+ `非静态成员`属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。

> **问：静态方法为什么不能调用非静态成员?**
>
> 在类的`非静态成员`不存在的时候`静态成员`就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。

### 2.6 静态方法与实例方法

**静态方法：**

+ 在外部被调用时，可以使用 `类名.方法名` 和 `对象.方法名` 两种方式（但推荐用前者）；
+ 在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员。

**实例方法：**

+ 被调用时，需要先实例化（创建对象），使用 `对象.方法名` 的方式来调用；
+ 在访问本类的成员时，没有任何限制。

### 2.7 重载与重写

**重载：**

指的是同样的一个方法，能够根据输入数据的不同，做出不同的处理，如：

```java
StringBuilder sb = new StringBuilder();
StringBuilder sb2 = new StringBuilder("HelloWorld");
```

即：`同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理`。

**重写：**

重写发生在运行期，是 `子类对父类的允许访问的方法的实现过程进行重新编写`。

+ 方法名、参数列表必须相同；
+ 子类方法`返回值`类型应比父类方法返回值类型更小（即可以是引用类型的子类）或相等；
+ 抛出的异常范围`小于等于`父类；
+ 访问修饰符范围`大于等于`父类；
+ 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 `static` 修饰的方法能够被再次声明；
+ 构造方法无法被重写。

即：`重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变。`

## 3、数据类型

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

![image-20220728142703256](1-1、Java 基础.assets/image-20220728142703256.png)

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

### 4.2 String 类

#### String 不可变

因为：

+ `String` 类中使用 `final` 关键字修饰字符数组来保存字符串，导致其不能被继承，进而避免了子类破坏 `String` 不可变。
+ 保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。

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

- `面向过程` 把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
- `面向对象` 会先抽象出对象，然后用对象执行方法的方式解决问题。

### 5.2 面向对象三大特征

- 封装 : 把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。
- 继承 : 不同类型的对象，相互之间经常有一定数量的共同点。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。
- 多态 : 表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。

### 5.3 对象

通过 new 运算符，new 创建对象实例（对象实例在堆内存中），对象引用指向对象实例（对象引用存放在栈内存中）。

一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。

### 5.4 构造方法

构造方法是一种特殊的方法，主要作用是完成对象的初始化工作。





### 5.5 接口和抽象类

**共同点** ：

- 都不能被实例化。
- 都可以包含抽象方法。
- 都可以有默认实现的方法（Java 8 可以用 `default` 关键字在接口中定义默认方法）。

**区别** ：

- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系（比如说我们抽象了一个发送短信的抽象类，）。
- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。





## 待整理





### 1.5 构造方法有哪些特点？是否可被 override?

构造方法特点如下：

- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 生成类的对象时自动执行，无需调用。

构造方法不能被重写,但是**可以重载**,所以你可以看到一个类中有多个构造函数的情况。

### 一个类的实例从new 开始的过程



### 2.2 接口和抽象类有什么共同点和区别？

**共同点**

- 都不能被实例化
- 都可以包含抽象方法
- 都可以有默认实现方法(接口1.8之后可以使用default定义)

**区别**

- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系

- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。

### Java的多态与实现



### Integer 的大小

## 4、泛型

### Java 泛型了解么？什么是类型擦除？

- **Java 泛型（generics）** 是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制
- Java 的泛型是伪泛型，这是因为 Java 在运行期间，所有的泛型信息都会被擦掉，这也就是通常所说类型擦除 。
- 泛型一般有三种使用方式: 泛型类、泛型接口、泛型方法。

## 5、反射

反射之所以被称为框架的灵魂，主要是因为它赋予了我们在运行时分析类以及执行类中方法的能力。通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些方法和属性。

### 反射机制优缺点

- **优点** ： 可以让咱们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利
- **缺点** ：让我们在运行时有了分析操作类的能力，这同样也增加了安全问题。比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

### 反射的应用场景

像 Spring/Spring Boot、MyBatis 等等,**这些框架中大量使用了动态代理，而动态代理的实现也依赖反射。**

**注解** 的实现也用到了反射 , 可以基于反射分析类，然后获取到类/属性/方法/方法的参数上的注解。你获取到注解之后，就可以做进一步的处理。

## 6、注解

常见的解析方法有两种：

- **编译期直接扫描** ：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理** ：像框架中自带的注解(比如 Spring 框架的 `@Value` 、`@Component`)都是通过反射来进行处理的。



## 7、异常

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- **`Error`** ：`Error` 属于程序无法处理的错误 。例如Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。

### 受检查异常和不受检查异常的区别

- Java 代码在编译过程中，如果**受检查异常**没有被 `catch`/`throw` 处理的话，就没办法通过编译常见的受检查异常有： IO 相关的异常、`ClassNotFoundException` 、`SQLException`...

- 即使不处理**不受检查异常**也可以正常通过编译 , 例如：`NullPointerException`、`NumberFormatException`（字符串转换为数字）、`ArrayIndexOutOfBoundsException`（数组越界）、`ClassCastException`（类型转换错误）、`ArithmeticException`（算术错误）等



### finally 中的代码一定会执行吗？

不一定

- 比如说 `finally` 之前虚拟机被终止运行的话，finally 中的代码就不会被执行

- 程序所在的线程死亡

- 关闭 CPU



## 8、I/O

### Java 序列化中如果有些字段不想进行序列化，怎么办？

对于不想进行序列化的变量，使用 `transient` 关键字修饰。

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。



### Java 中 IO 流分为几种?

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。



### 既然有了字节流,为什么还要有字符流?

字符流是由 Java 虚拟机将字节转换得到的，问题就出在这个过程还算是非常耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



### Java 中 3 种常见 IO 模型

- BIO : 同步阻塞IO

- NIO : 多路复用IO
- AIO : 异步IO