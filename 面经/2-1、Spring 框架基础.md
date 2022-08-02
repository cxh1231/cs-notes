# 2-1　Spring 框架基础

## 1、Spring 框架

`Spring` 是一款**开源的轻量级 Java 开发框架**，旨在提高开发人员的开发效率以及系统的可维护性。一般说 **Spring 框架**指的都是 `Spring Framework`，它是**很多模块的集合，**使用这些模块可以很方便地协助我们进行开发。

**Spring 最核心的思想就是不重新造轮子，开箱即用！**

## 2、Spring 框架的重要模块

`Spring` 各个模块的依赖关系如下：

![image-20220802101850532](https://img.zxdmy.com/2022/202208021018722.png)

### 2.1 核心模块（IoC 模块）

**Spring 框架的核心模块（基础模块），主要提供 IoC 依赖注入功能的支持**。

> **Spring** 其他所有的功能基本都需要依赖于该模块。

- **spring-core** ：**Spring 框架基本的核心工具类**。
- **spring-beans** ：**提供对 bean 的创建、配置和管理等功能的支持**。
- **spring-context** ：提供对国际化、事件传播、资源加载等功能的支持。
- **spring-expression** ：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用

### 2.2 AOP 模块

- **spring-aspects** ：**该模块为与 AspectJ 的集成提供支持。**
- **spring-aop** ：**提供了面向切面的编程实现**。
- **spring-instrument** ：提供了为 JVM 添加代理（agent）的功能。 具体来讲，它为 Tomcat 提供了一个织入代理，能够为 Tomcat 传递类文 件，就像这些文件是被类加载器加载的一样。没有理解也没关系，这个模块的使用场景非常有限。

### 2.3数据库连接与整合模块

- **spring-jdbc** ：**提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响**。
- **spring-tx** ：提供对事务的支持。
- **spring-orm** ： 提供对 Hibernate、JPA 、iBatis 等 ORM 框架的支持。
- **spring-oxm** ：提供一个抽象层支撑 OXM(Object-to-XML-Mapping)，例如：JAXB、Castor、XMLBeans、JiBX 和 XStream 等。
- **spring-jms** ：Java 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。

### 2.4 Web 模块

- **spring-web** ：**对 Web 功能的实现提供一些最基础的支持**。
- **spring-webmvc** ： **提供对 Spring MVC 的实现**。
- **spring-websocket** ： 提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。
- **spring-webflux** ：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步。

### 2.5 Test 模块

Spring 的测试模块对 JUnit（单元测试框架）、TestNG（类似 JUnit）、Mockito（主要用来 Mock 对象）、PowerMock（解决 Mockito 的问题比如无法模拟 final, static， private 方法）等等常用的测试框架支持的都比较好。

## 3、Spring、Spring MVC、Spring Boot 的区别

如前文，**Spring 包含了多种功能模块**。

> 其中最重要的是 `Spring-Core` 模块（主要提供 IoC 依赖注入功能的支持） ，**Spring 中的其他模块的功能实现基本都需要依赖于该模块**。

**Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力**。

> MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

**Spring Boot 简化了 Spring MVC 的各种配置，做到开箱即用。**

## 4、Spring IoC

`IoC`（全写：`Inverse of Control`），即 `控制反转` 的意思。

**IoC 是一种设计思想，而不是一个具体的技术实现。**

IoC 的思想就是**将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理**。不过， IoC 并非 Spring 特有，在其他语言中也有应用。

**控制反转 包含两层含义：**

- **控制** ：指的是对象创建（实例化、管理）的权力
- **反转** ：控制权交给外部环境（Spring 框架、IoC 容器）

![image-20220802104734352](https://img.zxdmy.com/2022/202208021047758.png)

**如上图，将对象之间的相互依赖关系，交给 IoC 容器管理，并由 IoC 容器完成对象的注入。**

> `IoC 容器` 就像工厂，当我们需要创建一个对象的时候，只需要配置好 `配置文件/注解`，无需考虑对象如何被创建，把应用从复杂的依赖关系中解放出来，极大简化开发难度。
>
> 比如：实际项目中的 Service 类可能依赖很多其他的类。如果需要实例化这个 Service，可能每次都需要搞清这个 Service 所有底层类的构造函数……。如果使用 IoC 的话，只需要配置好，然后在需要的地方引用即可，这极大增加了项目的可维护性，降低了开发难度。

在 Spring 中， `IoC 容器`是 Spring 用来实现 IoC 的载体， `IoC 容器`实际上就是个 `Map`（key，value），Map 中存放的是各种对象。

> Spring 时代一般通过 `XML` 文件来配置 `Bean`，后来开发人员觉得 `XML` 文件来配置不太好，于是 `SpringBoot 注解配置`就开始流行。

## 5、Spring Bean

简单来说，`Bean` 代指的就是那些被 `IoC 容器`所管理的 `对象`。

可以通过配置元数据定义对象，进而交给 IoC 容器管理。

配置元数据的方式可以是 XML、注解或Java配置类。

`org.springframework.beans`和 `org.springframework.context` 这两个包是 `IoC` 实现的基础。

### 5.1 将类声明为 Bean 的注解

一般使用 `@Component`、`@Repository` 、`@Service`、`@Controller`将类声明为 Bean，详情如下：

+ `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。
+ `@Repository` : 对应**持久层**即 Dao 层，主要用于数据库相关操作。
+ `@Service` : 对应**服务层**，主要涉及一些复杂的逻辑，需要用到 Dao 层。
+ `@Controller` : 对应 Spring MVC **控制层**，主要接受用户请求并调用 Service 层返回数据给前端页面。

#### @Component 和 @Bean 的区别

- `@Component` 注解作用于类，而`@Bean`注解作用于方法。
- `@Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。
- `@Bean` 注解比 `@Component` 注解的自定义性更强，而且很多地方我们只能通过 `@Bean` 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现。

### 5.2 注入 Bean 的注解

Spring 内置的 `@Autowired` 以及 JDK 内置的 `@Resource` 和 `@Inject` 都可以注入（自动装配） Bean，`@Autowired` 和`@Resource`使用的比较多。

|     注解     |  来源  |         优先注入方式         |
| :----------: | :----: | :--------------------------: |
| `@Autowired` | Spring | `byType`（根据类型进行匹配） |
| `@Resource`  |  JDK   | `byName`（根据名称进行匹配） |

详细如下。

#### @Autowired

**`@Autowired` 属于 Spring 内置的注解，默认的注入方式为`byType`（根据类型进行匹配），即优先根据接口类型去匹配并注入 Bean （接口的实现类）。**

但是：当一个接口存在多个实现类的话，`byType`这种方式就无法正确注入对象，会变成 `byName`（根据名称进行匹配），这个名称通常就是类名（首字母小写）。

比如： `SmsService` 接口有两个实现类: `SmsServiceImpl1`和 `SmsServiceImpl2`，且它们都已经被 Spring 容器所管理，那么下面的第一种方式就无法注入 Bean。

```java
// 报错，byName 和 byType 都无法匹配到 bean
@Autowired
private SmsService smsService;

// 正确注入 SmsServiceImpl1 对象对应的 bean，通过名称匹配
@Autowired
private SmsService smsServiceImpl1;

// 正确注入  SmsServiceImpl1 对象对应的 bean
// smsServiceImpl1 就是我们上面所说的名称
@Autowired
@Qualifier(value = "smsServiceImpl1")
private SmsService smsService;
```

> 对于一个接口多个实现类的情况，建议通过 `@Qualifier` 注解来显示指定名称。

#### @Resource

`@Resource` 属于 JDK 提供的注解，默认注入方式为 `byName`。如果无法通过名称匹配到对应的 Bean 的话，注入方式会变为 `byType`。

`@Resource` 注解有两个比较重要且日常开发常用的属性：`name`（名称）、`type`（类型）。

+ 如果不指定`name` 和 `type` ，则先 `byName` ，再 `byType` ；
+ 如果仅指定 `name` ，则注入方式为`byName`；
+ 如果仅指定 `type` ，则注入方式为`byType`；
+ 如果同时指定 `name`  和 `type` 属性，则注入方式为`byType`+`byName`（TM吃饱了撑的写这么多）

比如： `SmsService` 接口有两个实现类: `SmsServiceImpl1`和 `SmsServiceImpl2`：

```java
// 报错，byName 和 byType 都无法匹配到 bean
@Resource
private SmsService smsService;

// 正确注入 SmsServiceImpl1 对象对应的 bean
@Resource
private SmsService smsServiceImpl1;

// 正确注入 SmsServiceImpl1 对象对应的 bean（比较推荐这种方式）
@Resource(name = "smsServiceImpl1")
private SmsService smsService;
```

### 5.3 Bean 的作用域

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，连续 `getBean()` 两次，得到的是不同的 Bean 实例。
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。
- **application/global-session** （仅 Web 应用可用）： 每个 Web 应用在启动时创建一个 Bean（应用 Bean），，该 bean 仅在当前应用启动时间内有效。
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

**可以通过 `XML` 或 `注解方式` 配置 Bean 的作用域：**

XML：

```xml
<bean id="..." class="..." scope="singleton"></bean>
```

注解：

```java
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```

### 5.4 Bean 的生命周期

![image-20220802151417390](https://img.zxdmy.com/2022/202208021514870.png)

- Bean 容器找到配置文件中 Spring Bean 的定义。
- Bean 容器利用 Java Reflection API 创建一个 Bean 的实例。
- 如果涉及到一些属性值 利用 `set()`方法设置一些属性值。
- 如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入 Bean 的名字。
- 如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 如果 Bean 实现了 `BeanFactoryAware` 接口，调用 `setBeanFactory()`方法，传入 `BeanFactory`对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果 Bean 实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

### 5.5 单例 Bean 的线程安全问题

单例 bean 存在线程问题，主要是因为当多个线程操作同一个对象的时候是存在资源竞争的。不过，大部分 bean 实际都是无状态（没有实例变量）的（比如 Dao、Service），这种情况下， bean 是线程安全的。

常见的有两种解决办法：

1. 在 bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

不过，大部分 Bean 实际都是无状态（没有实例变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的。

## 6、Spring AOP

`AOP（Aspect-Oriented Programming，面向切面编程）`能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）**封装** 起来，便于**减少系统的重复代码，降低模块间的耦合度**，并有利于未来的可拓展性和可维护性。

AOP 切面编程设计到的一些专业术语：

|        术语         | 含义                                                         |
| :-----------------: | :----------------------------------------------------------- |
|   目标（Target）    | 被通知的对象                                                 |
|    代理（Proxy）    | 向目标对象应用通知之后创建的代理对象                         |
| 连接点（JoinPoint） | 目标对象的所属类中，定义的所有方法均为连接点                 |
| 切入点（Pointcut）  | 被切面拦截 / 增强的连接点（切入点一定是连接点，连接点不一定是切入点） |
|   通知（Advice）    | 增强的逻辑 / 代码，也即拦截到目标对象的连接点之后要做的事情  |
|   切面（Aspect）    | 切入点(Pointcut)+通知(Advice)                                |
|   Weaving（织入）   | 将通知应用到目标对象，进而生成代理对象的过程动作             |

Spring AOP 已经集成了 AspectJ ，AspectJ 基本是 Java 生态系统中最完整的 AOP 框架。

### 6.1 Spring AOP 与 AspectJ AOP 的区别

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。**

Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比 Spring AOP 快很多。

### 6.2 AspectJ 切面注解通知

- `@Before`：前置通知，目标对象的方法调用之前触发
- `@After`：后置通知，目标对象的方法调用之后触发
- `@AfterRunning`：返回通知，目标对象的方法调用完成，在返回结果值之后触发
- `@AfterThrowing`：异常通知，目标对象的方法运行中抛出 / 触发异常后触发。AfterReturning 和 AfterThrowing 两者互斥。如果方法调用成功无异常，则会有返回值；如果方法抛出了异常，则不会有返回值。
- `@Around`：环绕通知，围绕着方法执行，是所有通知类型中可操作范围最大的一种。

### 6.3 多个切面执行顺序

方法1、通常使用 `@Order` 注解直接定义切面顺序

```java
// 值越小优先级越高
@Order(3)
@Component
@Aspect
public class LoggingAspect implements Ordered {
}
```

方法2、实现`Ordered` 接口重写 `getOrder` 方法。

```java
@Component
@Aspect
public class LoggingAspect implements Ordered {

    // ....

    @Override
    public int getOrder() {
        // 返回值越小优先级越高
        return 1;
    }
}
```

## 7、Spring MVC

### 7.1 Spring MVC 核心组件

**MVC 是模型（Model）、视图（View）、控制器（Controller）的简写**，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

**Spring MVC 的核心组件有：**

+ **`DispatcherServlet`** ：**核心的中央处理器**
+ **`HandlerMapping`** ：**处理器映射器**
+ **`HandlerAdapter`** ：**处理器适配器**
+ **`Handler`** ：**请求处理器**
+ **`ViewResolver`** ：**视图解析器**

### 7.2 工作原理

**Spring MVC 原理如下图所示：**

![](https://img.zxdmy.com/2022/202207062051678.png)

1. **客户端（浏览器）发送请求，直接请求到 `DispatcherServlet`。**
2. **`DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。**
3. **解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由 `HandlerAdapter` 适配器处理。**
4. **`HandlerAdapter` 会根据 `Handler`来调用真正的处理器开处理请求，并处理相应的业务逻辑。**
5. 处理器处理完业务后，会返回一个 `ModelAndView` 对象，`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
6. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
7. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
8. 把 `View` 返回给请求者（浏览器）

## 8、Spring 事物

### Spring 管理事务的方式有几种？

- **编程式事务** ： 在代码中硬编码(不推荐使用) : 通过 `TransactionTemplate`或者 `TransactionManager` 手动管理事务，实际应用中很少使用，但是对于你理解 Spring 事务管理原理有帮助。
- **声明式事务** ： 在 XML 配置文件中配置或者直接基于注解（推荐使用） : 实际是通过 AOP 实现（基于`@Transactional` 的全注解方式使用最多）

### Spring中事务的传播行为

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。

- **propagation_require(默认)**

  **如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。**

- **propagation_require_new**

  **创建一个新的事务，如果当前存在事务，则把当前事务挂起。也就是说不管外部方法是否开启事务，`Propagation.REQUIRES_NEW`修饰的内部方法会新开启自己的事务，且开启的事务相互独立，互不干扰。**

- **propagation_nested**

  **如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行 , 如果没有 , 就等价于propagation_require**

- **propagation_mandatory**

  **如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。**

- propagation_supports

  如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。

- propagation_not_supports

  以非事务方式运行，如果当前存在事务，则把当前事务挂起。

- propagation_never

  以非事务方式运行，如果当前存在事务，则抛出异常。

### Spring 事务中的隔离级别有哪几种?

- default : 使用后端数据库默认的隔离级别
- read_uncommitted
- read_committed
- repeatable_read
- serializable





## 待整理

## 7、Spring 框架中用到的设计模式

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

## 依赖循环？三级缓存？



