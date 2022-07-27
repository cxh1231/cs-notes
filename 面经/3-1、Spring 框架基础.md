# 3-1　Spring 框架基础

## 1、Spring 框架

Spring 是一款开源的轻量级 Java 开发框架，旨在提高开发人员的开发效率以及系统的可维护性。一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发。

Spring 最核心的思想就是不重新造轮子，开箱即用！

## 2、Spring 框架的重要模块

- **Spring Core**

  **Spring 其他所有的功能基本都需要依赖于该模块，主要提供 IoC 依赖注入功能的支持。**

- Spring Aspects

  该模块为与 AspectJ 的集成提供支持。

- **Spring AOP**

  提供了面向切面的编程实现。

- Spring Data Access

  - **spring-jdbc : 提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。**
  - spring-tx : 提供对事务的支持。
  - spring-orm : 提供对 Hibernate 等 ORM 框架的支持。
  - spring-oxm ： 提供对 Castor 等 OXM 框架的支持。
  - spring-jms : Java 消息服务。

- **Spring Web**

  - **spring-web ：对 Web 功能的实现提供一些最基础的支持。**
  - **spring-webmvc ： 提供对 Spring MVC 的实现。**
  - spring-websocket ： 提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。
  - spring-webflux ：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步.

- Spring Test

## 3、Spring、Spring MVC、Spring Boot 之间什么的区别

Spring MVC 是 Spring 中的一个很重要的模块，主要赋予 Spring 快速构建 MVC 架构的 Web 程序的能力。MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

Spring Boot 只是简化了配置，如果你需要构建 MVC 架构的 Web 程序，你还是需要使用 Spring MVC 作为 MVC 框架，只是说 Spring Boot 帮你简化了 Spring MVC 的很多配置，真正做到开箱即用！

## 4、Spring 的重要思想：IOC 与 AOP

### 4.1 IOC 与 AOP 的概念

**IoC（Inverse of Control:控制反转）** **是一种设计思想**，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spring 特有，在其他语言中也有应用(直言是面向对象编程的语言都可以实现该功能)。

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。

**AOP(Aspect-Oriented Programming:面向切面编程)**能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

### 4.2 Spring AOP 与 AspectJ AOP 的区别

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** 

Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

### AspectJ切面注解通知

- @Before: 前置通知, 在方法执行之前执行
- @After: 后置通知, 在方法执行之后执行 。
- @AfterRunning: 返回通知, 在方法返回结果之后执行
- @AfterThrowing: 异常通知, 在方法抛出异常之后
- @Around: 环绕通知, 围绕着方法执行

## 5、Spring Bean 相关问题

### 5.1 Spring Bean 概念

bean 代指的就是那些被 IoC 容器所管理的对象。

### Spring bean 的创建



### 5.2 将一个类声明为 bean 的注解

我们一般使用 `@Autowired` 注解自动装配 bean

+ `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。
+ `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
+ `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
+ `@Controller` : 对应 Spring MVC 控制层，主要功能是接受用户请求并调用 Service 层返回数据给前端页面。

### 5.3 bean 的作用域

- **singleton** : 唯一 bean 实例，Spring 中的 bean 默认都是单例的，对单例设计模式的应用。
- **prototype** : 每次请求都会创建一个新的 bean 实例。
- **request** : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP request 内有效。
- **session** : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP session 内有效。

### 5.4 @Component 和 @Bean 的区别

1. `@Component` 注解作用于类，而`@Bean`注解作用于方法。
2. `@Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。
3. `@Bean` 注解比 `@Component` 注解的自定义性更强，而且很多地方我们只能通过 `@Bean` 注解来注册 bean。比如当我们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现。

### 5.5 单例 Bean 的线程安全问题

单例 bean 存在线程问题，主要是因为当多个线程操作同一个对象的时候是存在资源竞争的。不过，大部分 bean 实际都是无状态（没有实例变量）的（比如 Dao、Service），这种情况下， bean 是线程安全的。

常见的有两种解决办法：

1. 在 bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

## 6、Spring 事务

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

## 7、Spring 框架中用到的设计模式

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

## 8、Spring MVC 工作原理

MVC 是模型(Model)、视图(View)、控制器(Controller)的简写，其核心思想是通过将业务逻辑、数据、显示分离来组织代码。

![](https://img.zxdmy.com/2022/202207062051678.png)

1. **客户端（浏览器）发送请求，直接请求到 `DispatcherServlet`。**
2. **`DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。**
3. **解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由 `HandlerAdapter` 适配器处理。**
4. **`HandlerAdapter` 会根据 `Handler`来调用真正的处理器开处理请求，并处理相应的业务逻辑。**
5. 处理器处理完业务后，会返回一个 `ModelAndView` 对象，`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
6. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
7. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
8. 把 `View` 返回给请求者（浏览器）





## 依赖循环？三级缓存？



